---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Création de nouvelles procédures stockées pour les TableAdapters du DataSet typé (VB) | Microsoft Docs
author: rick-anderson
description: Dans les didacticiels précédents, nous avons créé des instructions SQL dans notre code et transmis les instructions à la base de données à exécuter. Une autre approche consiste à utiliser des...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: a7cc890038e5bb4eb61c7c3b808154c196ab2423
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74605584"
---
# <a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Création de procédures stockées pour les TableAdapters de dataset typé (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip) ou [Télécharger le PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> Dans les didacticiels précédents, nous avons créé des instructions SQL dans notre code et transmis les instructions à la base de données à exécuter. Une autre approche consiste à utiliser des procédures stockées, dans lesquelles les instructions SQL sont prédéfinies au niveau de la base de données. Dans ce didacticiel, nous allons apprendre à faire en sorte que l’Assistant TableAdapter génère de nouvelles procédures stockées pour nous.

## <a name="introduction"></a>Introduction

La couche DAL (Data Access Layer) de ces didacticiels utilise des DataSets typés. Comme indiqué dans le didacticiel [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) , les datasets typés consistent en des DataTables fortement typés et des TableAdapters. Les DataTables représentent les entités logiques dans le système tandis que l’interface TableAdapters avec la base de données sous-jacente pour effectuer le travail d’accès aux données. Cela comprend le remplissage des DataTables avec des données, l’exécution de requêtes qui retournent des données scalaires, et l’insertion, la mise à jour et la suppression d’enregistrements de la base de données.

Les commandes SQL exécutées par les TableAdapters peuvent être des instructions SQL ad hoc, telles que des `SELECT columnList FROM TableName`ou des procédures stockées. Les TableAdapters dans notre architecture utilisent des instructions SQL ad hoc. De nombreux développeurs et administrateurs de base de données, cependant, préfèrent des procédures stockées sur des instructions SQL ad hoc pour des raisons de sécurité, de facilité de maintenance et de mise à jour. D’autres ardently préfèrent les instructions SQL ad hoc pour leur flexibilité. Dans mon propre travail, je préfère les procédures stockées par rapport aux instructions SQL ad hoc, mais j’ai choisi d’utiliser des instructions SQL ad hoc pour simplifier les didacticiels précédents.

Lors de la définition d’un TableAdapter ou de l’ajout de nouvelles méthodes, l’Assistant TableAdapter s simplifie la création de procédures stockées ou l’utilisation de procédures stockées existantes, comme c’est le cas pour utiliser des instructions SQL ad hoc. Dans ce didacticiel, nous allons examiner comment l’Assistant TableAdapter s génère automatiquement des procédures stockées. Dans le didacticiel suivant, nous allons examiner comment configurer les méthodes de TableAdapter s pour utiliser des procédures stockées existantes ou créées manuellement.

> [!NOTE]
> Consultez l’entrée de blog de Rob Howard [Don t Use Stored Procedures pourtant ?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) and [Frans Bouma](https://weblogs.asp.net/fbouma/) s. les [procédures stockées sont incorrectes, M Kay ?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) pour un débat vivant sur les avantages et les inconvénients des procédures stockées et de SQL ad hoc.

## <a name="stored-procedure-basics"></a>Présentation des procédures stockées

Les fonctions sont une construction commune à tous les langages de programmation. Une fonction est une collection d’instructions qui sont exécutées lorsque la fonction est appelée. Les fonctions peuvent accepter des paramètres d’entrée et éventuellement retourner une valeur. Les *[procédures stockées](http://en.wikipedia.org/wiki/Stored_procedure)* sont des constructions de base de données qui partagent de nombreuses similitudes avec les fonctions dans les langages de programmation. Une procédure stockée est constituée d’un ensemble d’instructions T-SQL qui sont exécutées lorsque la procédure stockée est appelée. Une procédure stockée peut accepter zéro à plusieurs paramètres d’entrée et peut retourner des valeurs scalaires, des paramètres de sortie ou, le plus souvent, des jeux de résultats à partir de requêtes de `SELECT`.

> [!NOTE]
> Les procédures stockées sont souvent appelées sprocs ou SPs.

Les procédures stockées sont créées à l’aide de l’instruction t-SQL [`CREATE PROCEDURE`](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) . Par exemple, le script T-SQL suivant crée une procédure stockée nommée `GetProductsByCategoryID` qui prend un seul paramètre nommé `@CategoryID` et retourne les champs `ProductID`, `ProductName`, `UnitPrice`et `Discontinued` des colonnes de la table `Products` qui ont une valeur `CategoryID` correspondante :

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Une fois que cette procédure stockée a été créée, elle peut être appelée à l’aide de la syntaxe suivante :

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> Dans le didacticiel suivant, nous allons examiner la création de procédures stockées par le biais de l’IDE de Visual Studio. Pour ce didacticiel, toutefois, nous allons laisser l’Assistant TableAdapter générer automatiquement les procédures stockées pour nous.

En plus de renvoyer simplement des données, les procédures stockées sont souvent utilisées pour exécuter plusieurs commandes de base de données dans l’étendue d’une transaction unique. Une procédure stockée nommée `DeleteCategory`, par exemple, peut prendre un paramètre `@CategoryID` et exécuter deux instructions `DELETE` : tout d’abord, la première pour supprimer les produits associés et l’autre supprimer la catégorie spécifiée. Plusieurs instructions au sein d’une procédure stockée ne sont *pas* automatiquement encapsulées dans une transaction. Des commandes T-SQL supplémentaires doivent être émises pour s’assurer que les commandes multiples de la procédure stockée sont traitées comme une opération atomique. Nous verrons comment inclure dans un wrapper les commandes d’une procédure stockée dans l’étendue d’une transaction dans le didacticiel suivant.

Lorsque vous utilisez des procédures stockées dans une architecture, les méthodes de la couche d’accès aux données appellent une procédure stockée particulière plutôt que d’émettre une instruction SQL ad hoc. Cela centralise l’emplacement des instructions SQL exécutées (sur la base de données) au lieu de les définir dans l’architecture de l’application. Cette centralisation facilite la recherche, l’analyse et le paramétrage des requêtes, et fournit une image plus claire quant à l’emplacement et à la façon dont la base de données est utilisée.

Pour plus d’informations sur les principes de base des procédures stockées, consultez les ressources dans la section de lecture supplémentaire à la fin de ce didacticiel.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Étape 1 : création des pages Web des scénarios de la couche d’accès aux données avancée

Avant de commencer notre discussion sur la création d’une couche DAL à l’aide de procédures stockées, commençons par prendre un moment pour créer les pages ASP.NET dans notre projet de site Web dont nous aurons besoin pour cela et les prochains didacticiels. Commencez par ajouter un nouveau dossier nommé `AdvancedDAL`. Ajoutez ensuite les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page à la page maître `Site.master` :

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`

![Ajouter les pages ASP.NET pour les didacticiels avancés sur les scénarios de couche d’accès aux données](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figure 1**: ajouter les pages ASP.net pour les didacticiels avancés sur les scénarios de couche d’accès aux données

Comme dans les autres dossiers, `Default.aspx` dans le dossier `AdvancedDAL` répertorie les didacticiels de la section. N’oubliez pas que le contrôle utilisateur `SectionLevelTutorialListing.ascx` fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser de l’Explorateur de solutions sur la page s Mode Création.

[![ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**Figure 2**: ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))

Enfin, ajoutez ces pages en tant qu’entrées au fichier `Web.sitemap`. Plus précisément, ajoutez le balisage suivant après l’utilisation des données par lot `<siteMapNode>`:

[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

Après avoir mis à jour `Web.sitemap`, prenez un moment pour afficher le site Web des didacticiels via un navigateur. Le menu à gauche contient maintenant des éléments pour les didacticiels de scénarios DAL avancés.

![Le plan de site comprend désormais des entrées pour les didacticiels de scénarios DAL avancés](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**Figure 3**: le plan de site comprend désormais des entrées pour les didacticiels de scénarios dal avancés

## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Étape 2 : configuration d’un TableAdapter pour créer des procédures stockées

Pour illustrer la création d’une couche d’accès aux données qui utilise des procédures stockées plutôt que des instructions SQL ad hoc, créez un nouveau DataSet typé dans le dossier `~/App_Code/DAL` nommé `NorthwindWithSprocs.xsd`. Étant donné que nous avons parcouru ce processus en détail dans les didacticiels précédents, nous allons passer rapidement en revue les étapes ci-dessous. Si vous êtes bloqué ou si vous avez besoin d’autres instructions pas à pas pour créer et configurer un jeu de données typé, reportez-vous au didacticiel [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) .

Ajoutez un nouveau DataSet au projet en cliquant avec le bouton droit sur le dossier `DAL`, en sélectionnant Ajouter un nouvel élément, puis en sélectionnant le modèle DataSet, comme illustré à la figure 4.

[![ajouter un nouveau DataSet typé au projet nommé NorthwindWithSprocs. xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**Figure 4**: ajouter un nouveau DataSet typé au projet nommé `NorthwindWithSprocs.xsd` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))

Cette opération crée le nouveau DataSet typé, ouvre son concepteur, crée un nouveau TableAdapter et lance l’Assistant Configuration de TableAdapter. La première étape de l’Assistant Configuration de TableAdapter nous invite à sélectionner la base de données à utiliser. La chaîne de connexion à la base de données Northwind doit être répertoriée dans la liste déroulante. Sélectionnez cette option et cliquez sur suivant.

À partir de cet écran suivant, nous pouvons choisir comment le TableAdapter doit accéder à la base de données. Dans les didacticiels précédents, nous avons sélectionné la première option, utiliser des instructions SQL. Pour ce didacticiel, sélectionnez la deuxième option, créer des procédures stockées, puis cliquez sur suivant.

[![demander au TableAdapter de créer de nouvelles procédures stockées](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**Figure 5**: demander au TableAdapter de créer de nouvelles procédures stockées ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))

À l’instar de l’utilisation d’instructions SQL ad hoc, vous êtes invité à fournir l’instruction `SELECT` pour la requête principale du TableAdapter à l’étape suivante. Mais au lieu d’utiliser l’instruction `SELECT` entrée ici pour effectuer directement une requête ad hoc, l’Assistant TableAdapter s crée une procédure stockée qui contient cette `SELECT` requête.

Utilisez la requête `SELECT` suivante pour ce TableAdapter :

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

[![entrez la requête SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**Figure 6**: entrez la requête `SELECT` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))

> [!NOTE]
> La requête ci-dessus diffère légèrement de la requête principale de la `ProductsTableAdapter` dans le `Northwind` DataSet typé. Rappelez-vous que les `ProductsTableAdapter` dans le `Northwind` DataSet typé incluent deux sous-requêtes corrélées pour ramener le nom de la catégorie et le nom de la société pour chaque catégorie et fournisseur du produit. Dans le didacticiel [mise à jour imminente du TableAdapter pour utiliser les jointures](updating-the-tableadapter-to-use-joins-vb.md) , nous allons ajouter ces données associées à ce TableAdapter.

Prenez un moment pour cliquer sur le bouton Options avancées. À partir de là, nous pouvons spécifier si l’Assistant doit également générer des instructions INSERT, Update et Delete pour le TableAdapter, s’il faut utiliser l’accès concurrentiel optimiste et si la table de données doit être actualisée après les insertions et les mises à jour. L’option générer des instructions INSERT, Update et DELETE est activée par défaut. Laissez-la activée. Pour ce didacticiel, n’activez pas la case à cocher utiliser les options d’accès concurrentiel optimiste.

Lorsque les procédures stockées sont créées automatiquement par l’Assistant TableAdapter, l’option actualiser la table de données semble être ignorée. Que cette case à cocher soit activée ou non, les procédures stockées d’insertion et de mise à jour résultantes récupèrent l’enregistrement qui vient d’être inséré ou juste à jour, comme nous le verrons à l’étape 3.

![Laissez la case à cocher générer des instructions INSERT, Update et Delete activée](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**Figure 7**: conserver l’option générer les instructions INSERT, Update et Delete activée

> [!NOTE]
> Si l’option utiliser l’accès concurrentiel optimiste est activée, l’Assistant ajoute des conditions supplémentaires à la clause `WHERE` qui empêchent la mise à jour des données si d’autres champs ont été modifiés. Reportez-vous au didacticiel sur l’implémentation de l' [accès concurrentiel optimiste](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) pour plus d’informations sur l’utilisation de la fonctionnalité de contrôle d’accès concurrentiel optimiste intégré au TableAdapter.

Après avoir entré la requête `SELECT` et confirmé que l’option générer des instructions INSERT, Update et DELETE est cochée, cliquez sur suivant. L’écran suivant, illustré à la figure 8, vous invite à entrer les noms des procédures stockées que l’Assistant va créer pour sélectionner, insérer, mettre à jour et supprimer des données. Modifiez les noms de ces procédures stockées pour `Products_Select`, `Products_Insert`, `Products_Update`et `Products_Delete`.

[![renommer les procédures stockées](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figure 8**: renommer les procédures stockées ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))

Pour voir le code T-SQL que l’Assistant TableAdapter utilisera pour créer les quatre procédures stockées, cliquez sur le bouton Aperçu du script SQL. Dans la boîte de dialogue Aperçu du script SQL, vous pouvez enregistrer le script dans un fichier ou le copier dans le presse-papiers.

![Afficher un aperçu du script SQL utilisé pour générer les procédures stockées](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figure 9**: aperçu du script SQL utilisé pour générer les procédures stockées

Après avoir nommé les procédures stockées, cliquez sur suivant pour nommer les méthodes correspondantes du TableAdapter. Comme lorsque vous utilisez des instructions SQL ad hoc, nous pouvons créer des méthodes qui remplissent un DataTable existant ou en retournent un nouveau. Nous pouvons également spécifier si le TableAdapter doit inclure le modèle DB-direct pour l’insertion, la mise à jour et la suppression des enregistrements. Laissez les trois cases à cocher activées, mais renommez le retour d’une méthode DataTable en `GetProducts` (comme indiqué dans la figure 10).

[![nom des méthodes Fill et GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**Figure 10**: nommer les méthodes `Fill` et `GetProducts` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))

Cliquez sur suivant pour afficher un résumé des étapes que l’Assistant va effectuer. Terminez l’Assistant en cliquant sur le bouton Terminer. Une fois l’Assistant terminé, vous êtes redirigé vers le concepteur de DataSet, qui doit maintenant inclure l' `ProductsDataTable`.

[![le concepteur de DataSet affiche la ProductsDataTable nouvellement ajoutée](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**Figure 11**: le concepteur de DataSet affiche la `ProductsDataTable` récemment ajoutée ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))

## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Étape 3 : examen des procédures stockées nouvellement créées

L’Assistant TableAdapter utilisé à l’étape 2 a créé automatiquement les procédures stockées pour sélectionner, insérer, mettre à jour et supprimer des données. Ces procédures stockées peuvent être affichées ou modifiées par le biais de Visual Studio en accédant au Explorateur de serveurs et en explorant le dossier des procédures stockées de la base de données. Comme le montre la figure 12, la base de données Northwind contient quatre nouvelles procédures stockées : `Products_Delete`, `Products_Insert`, `Products_Select`et `Products_Update`.

![Les quatre procédures stockées créées à l’étape 2 se trouvent dans le dossier des procédures stockées de la base de données.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**Figure 12**: les quatre procédures stockées créées à l’étape 2 se trouvent dans le dossier des procédures stockées de la base de données.

> [!NOTE]
> Si vous ne voyez pas la Explorateur de serveurs, accédez au menu Affichage et choisissez l’option Explorateur de serveurs. Si vous ne voyez pas les procédures stockées relatives au produit ajoutées à l’étape 2, essayez de cliquer avec le bouton droit sur le dossier procédures stockées, puis choisissez actualiser.

Pour afficher ou modifier une procédure stockée, double-cliquez sur son nom dans la Explorateur de serveurs ou, sinon, cliquez avec le bouton droit sur la procédure stockée et choisissez Ouvrir. La figure 13 illustre la procédure stockée `Products_Delete`, quand elle est ouverte.

[![procédures stockées peuvent être ouvertes et modifiées dans Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**Figure 13**: les procédures stockées peuvent être ouvertes et modifiées dans Visual Studio ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))

Le contenu des procédures stockées `Products_Delete` et `Products_Select` est relativement simple. En revanche, les procédures stockées `Products_Insert` et `Products_Update` garantissent une inspection plus étroite, car elles exécutent toutes les deux une instruction `SELECT` après leur `INSERT` et les instructions `UPDATE`. Par exemple, le code SQL suivant constitue la procédure stockée `Products_Insert` :

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

La procédure stockée accepte comme paramètres d’entrée les colonnes de `Products` qui ont été retournées par la requête `SELECT` spécifiée dans l’Assistant TableAdapter s et ces valeurs sont utilisées dans une instruction `INSERT`. À la suite de l’instruction `INSERT`, une requête `SELECT` est utilisée pour retourner les valeurs de colonne `Products` (y compris le `ProductID`) de l’enregistrement qui vient d’être ajouté. Cette fonctionnalité d’actualisation est utile lors de l’ajout d’un nouvel enregistrement à l’aide du modèle de mise à jour par lot, car elle met automatiquement à jour les propriétés des instances de `ProductRow` récemment ajoutées `ProductID` avec les valeurs incrémentées automatiquement affectées par la base de données.

Le code suivant illustre cette fonctionnalité. Il contient une `ProductsTableAdapter` et `ProductsDataTable` créés pour le jeu de données typé `NorthwindWithSprocs`. Un nouveau produit est ajouté à la base de données en créant une instance de `ProductsRow`, en fournissant ses valeurs et en appelant la méthode TableAdapter s `Update`, en passant le `ProductsDataTable`. En interne, la méthode `Update` du TableAdapter énumère les instances de `ProductsRow` dans le DataTable passé (dans cet exemple, il n’y a qu’un seul-celui que nous venons d’ajouter) et exécute la commande d’insertion, de mise à jour ou de suppression appropriée. Dans ce cas, la procédure stockée `Products_Insert` est exécutée, ce qui ajoute un nouvel enregistrement à la table `Products` et retourne les détails de l’enregistrement qui vient d’être ajouté. La valeur de `ProductID` de l’instance de `ProductsRow` est ensuite mise à jour. Une fois la méthode `Update` terminée, nous pouvons accéder à la valeur de l’enregistrement nouvellement ajouté `ProductID` via la propriété `ProductsRow` s `ProductID`.

[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

La procédure stockée `Products_Update` comprend de manière similaire une instruction `SELECT` après son instruction `UPDATE`.

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

Notez que cette procédure stockée comprend deux paramètres d’entrée pour `ProductID`: `@Original_ProductID` et `@ProductID`. Cette fonctionnalité permet des scénarios dans lesquels la clé primaire peut être modifiée. Par exemple, dans une base de données Employee, chaque enregistrement d’employé peut utiliser le numéro de sécurité sociale Employee s comme clé primaire. Pour modifier le numéro de sécurité sociale d’un employé existant, vous devez fournir le nouveau numéro de sécurité sociale et le numéro d’origine. Pour la table `Products`, ces fonctionnalités ne sont pas nécessaires, car la colonne `ProductID` est une colonne `IDENTITY` et ne doit jamais être modifiée. En fait, l’instruction `UPDATE` dans la procédure stockée `Products_Update` n’inclut pas la colonne `ProductID` dans sa liste de colonnes. Ainsi, bien que `@Original_ProductID` soit utilisé dans la clause `UPDATE` des instructions `WHERE`, il est superflu pour la table `Products` et peut être remplacé par le paramètre `@ProductID`. Lors de la modification des paramètres d’une procédure stockée, il est important que la ou les méthodes TableAdapter qui utilisent cette procédure stockée soient également mises à jour.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Étape 4 : modification des paramètres des procédures stockées et mise à jour du TableAdapter

Étant donné que le paramètre `@Original_ProductID` est superflu, laissez-le supprimer entièrement de la procédure stockée `Products_Update`. Ouvrez la procédure stockée `Products_Update`, supprimez le paramètre `@Original_ProductID` et, dans la clause `WHERE` de l’instruction `UPDATE`, modifiez le nom du paramètre utilisé de `@Original_ProductID` en `@ProductID`. Après avoir apporté ces modifications, le code T-SQL de la procédure stockée doit ressembler à ce qui suit :

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

Pour enregistrer ces modifications dans la base de données, cliquez sur l’icône Enregistrer dans la barre d’outils ou appuyez sur CTRL + S. À ce stade, la procédure stockée `Products_Update` n’attend pas de paramètre d’entrée `@Original_ProductID`, mais le TableAdapter est configuré pour passer ce paramètre. Vous pouvez voir les paramètres que le TableAdapter enverra à la procédure stockée `Products_Update` en sélectionnant le TableAdapter dans le concepteur de DataSet, en accédant au Fenêtre Propriétés et en cliquant sur les ellipses dans la collection de `Parameters` `UpdateCommand` s. La boîte de dialogue Éditeur de collections Parameters s’affiche dans la figure 14.

![L’éditeur de collections Parameters répertorie les paramètres utilisés passés à la procédure stockée Products_Update](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**Figure 14**: l’éditeur de collections Parameters répertorie les paramètres utilisés passés à la procédure stockée `Products_Update`

Vous pouvez supprimer ce paramètre de cet emplacement en sélectionnant simplement le paramètre `@Original_ProductID` dans la liste des membres, puis en cliquant sur le bouton supprimer.

Vous pouvez également actualiser les paramètres utilisés pour toutes les méthodes en cliquant avec le bouton droit sur le TableAdapter dans le concepteur et en choisissant Configurer. Cela permet d’afficher l’Assistant Configuration de TableAdapter, qui répertorie les procédures stockées utilisées pour la sélection, l’insertion, la mise à jour et la suppression, ainsi que les paramètres que les procédures stockées attendent de recevoir. Si vous cliquez sur la liste déroulante mettre à jour, vous pouvez voir les paramètres d’entrée `Products_Update` procédures stockées attendues, qui n’incluent plus `@Original_ProductID` (voir figure 15). Cliquez simplement sur Terminer pour mettre à jour automatiquement la collection de paramètres utilisée par le TableAdapter.

[![vous pouvez également utiliser l’Assistant Configuration de TableAdapter pour actualiser ses collections de paramètres de méthode](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figure 15**: vous pouvez également utiliser l’Assistant Configuration de TableAdapter pour actualiser ses collections de paramètres de méthode ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))

## <a name="step-5-adding-additional-tableadapter-methods"></a>Étape 5 : ajout de méthodes TableAdapter supplémentaires

Comme illustré à l’étape 2, lors de la création d’un TableAdapter, il est facile de générer automatiquement les procédures stockées correspondantes. Il en va de même lorsque vous ajoutez des méthodes supplémentaires à un TableAdapter. Pour illustrer cela, ajoutez une méthode `GetProductByProductID(productID)` à la `ProductsTableAdapter` créée à l’étape 2. Cette méthode prendra comme entrée une valeur `ProductID` et renverra des détails sur le produit spécifié.

Commencez par cliquer avec le bouton droit sur le TableAdapter et sélectionnez Ajouter une requête dans le menu contextuel.

![Ajouter une nouvelle requête au TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figure 16**: ajouter une nouvelle requête au TableAdapter

Cette opération démarre l’Assistant Configuration de requêtes TableAdapter, qui vous invite d’abord à indiquer comment le TableAdapter doit accéder à la base de données. Pour créer une nouvelle procédure stockée, choisissez l’option créer une nouvelle procédure stockée, puis cliquez sur suivant.

[![choisissez l’option créer une nouvelle procédure stockée](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**Figure 17**: choisissez l’option créer une nouvelle procédure stockée ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))

L’écran suivant nous demande d’identifier le type de requête à exécuter, s’il renvoie un ensemble de lignes ou une valeur scalaire unique, ou exécute une instruction `UPDATE`, `INSERT`ou `DELETE`. Étant donné que la méthode `GetProductByProductID(productID)` retourne une ligne, laissez l’option SELECT qui retourne la ligne sélectionnée et appuyez sur suivant.

[![choisissez l’option Sélectionner la ligne Returns](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**Figure 18**: choisissez l’option Sélectionner la ligne retournée ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))

L’écran suivant affiche la requête principale du TableAdapter, qui répertorie uniquement le nom de la procédure stockée (`dbo.Products_Select`). Remplacez le nom de la procédure stockée par l’instruction `SELECT` suivante, qui retourne tous les champs de produit pour un produit spécifié :

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]

[![remplacer le nom de la procédure stockée par une requête SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**Figure 19**: remplacer le nom de la procédure stockée par une requête `SELECT` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))

L’écran suivant vous demande de nommer la procédure stockée qui sera créée. Entrez le nom `Products_SelectByProductID`, puis cliquez sur suivant.

[![nommez la nouvelle procédure stockée Products_SelectByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figure 20**: nommer la nouvelle procédure stockée `Products_SelectByProductID` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))

La dernière étape de l’Assistant nous permet de modifier les noms de méthode générés et d’indiquer s’il faut utiliser le modèle remplir un DataTable, retourner un modèle de DataTable, ou les deux. Pour cette méthode, laissez les deux options cochées, mais renommez les méthodes pour `FillByProductID` et `GetProductByProductID`. Cliquez sur suivant pour afficher un résumé des étapes que l’Assistant va effectuer, puis cliquez sur Terminer pour terminer l’Assistant.

[![renommer les méthodes de TableAdapter s en FillByProductID et GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**Figure 21**: renommer les méthodes des TableAdapter s en `FillByProductID` et `GetProductByProductID` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))

Une fois l’Assistant terminé, le TableAdapter a une nouvelle méthode disponible, `GetProductByProductID(productID)` qui, lorsqu’il est appelé, exécutera la procédure stockée `Products_SelectByProductID` qui vient d’être créée. Prenez un moment pour afficher cette nouvelle procédure stockée à partir de la Explorateur de serveurs en explorant le dossier procédures stockées et en ouvrant `Products_SelectByProductID` (si vous ne le voyez pas, cliquez avec le bouton droit sur le dossier procédures stockées, puis choisissez actualiser).

Notez que la procédure stockée `SelectByProductID` prend `@ProductID` en tant que paramètre d’entrée et exécute l’instruction `SELECT` que nous avons entrée dans l’Assistant.

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Étape 6 : création d’une classe de couche de logique métier

Tout au long de la série de didacticiels, nous nous efforçons de maintenir une architecture en couches dans laquelle la couche de présentation a effectué tous ses appels à la couche BLL (Business Logic Layer). Pour adhérer à cette décision de conception, nous devons tout d’abord créer une classe BLL pour le nouveau DataSet typé avant de pouvoir accéder aux données du produit à partir de la couche de présentation.

Créez un nouveau fichier de classe nommé `ProductsBLLWithSprocs.vb` dans le dossier `~/App_Code/BLL` et ajoutez-y le code suivant :

[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

Cette classe imite la sémantique de la classe `ProductsBLL` des didacticiels précédents, mais utilise les objets `ProductsTableAdapter` et `ProductsDataTable` du jeu de données `NorthwindWithSprocs`. Par exemple, au lieu d’avoir une instruction `Imports NorthwindTableAdapters` au début du fichier de classe comme `ProductsBLL`, la classe `ProductsBLLWithSprocs` utilise `Imports NorthwindWithSprocsTableAdapters`. De même, les objets `ProductsDataTable` et `ProductsRow` utilisés dans cette classe sont préfixés avec l’espace de noms `NorthwindWithSprocs`. La classe `ProductsBLLWithSprocs` fournit deux méthodes d’accès aux données, `GetProducts` et `GetProductByProductID`, ainsi que des méthodes permettant d’ajouter, de mettre à jour et de supprimer une instance de produit unique.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Étape 7 : utilisation du DataSet`NorthwindWithSprocs`à partir de la couche de présentation

À ce stade, nous avons créé une couche DAL qui utilise des procédures stockées pour accéder aux données de la base de données sous-jacente et les modifier. Nous avons également créé une couche BLL rudimentaire avec des méthodes pour récupérer tous les produits ou un produit particulier, ainsi que des méthodes permettant d’ajouter, de mettre à jour et de supprimer des produits. Pour arrondir ce didacticiel, nous allons créer une page ASP.NET qui utilise la classe BLL s `ProductsBLLWithSprocs` pour afficher, mettre à jour et supprimer des enregistrements.

Ouvrez la page `NewSprocs.aspx` dans le dossier `AdvancedDAL` et faites glisser un contrôle GridView de la boîte à outils vers le concepteur, en lui attribuant un nom `Products`. À partir de la balise active GridView s, choisissez de le lier à un nouvel ObjectDataSource nommé `ProductsDataSource`. Configurez le ObjectDataSource pour utiliser la classe `ProductsBLLWithSprocs`, comme illustré à la figure 22.

[![configurer ObjectDataSource pour utiliser la classe ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**Figure 22**: configurer ObjectDataSource pour utiliser la classe `ProductsBLLWithSprocs` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))

La liste déroulante de l’onglet sélectionner comporte deux options : `GetProducts` et `GetProductByProductID`. Étant donné que nous souhaitons afficher tous les produits dans le GridView, choisissez la méthode `GetProducts`. Les listes déroulantes dans les onglets mettre à jour, insérer et supprimer ne disposent que d’une seule méthode. Assurez-vous que la méthode appropriée de chacune de ces listes déroulantes est sélectionnée, puis cliquez sur Terminer.

Une fois l’exécution de l’Assistant ObjectDataSource terminée, Visual Studio ajoute BoundFields et un CheckBoxField au GridView pour les champs de données de produit. Activez les fonctionnalités de modification et de suppression de GridView s en cochant les options activer la modification et activer la suppression présentes dans la balise active.

[![la page contient un GridView avec prise en charge de l’édition et de la suppression activée](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**Figure 23**: la page contient un GridView pour lequel la prise en charge de l’édition et de la suppression est activée ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))

Comme nous l’avons vu dans les didacticiels précédents, à la fin de l’Assistant ObjectDataSource s, Visual Studio affecte à la propriété `OldValuesParameterFormatString` la valeur d’origine\_{0}. Cela doit être rétabli à sa valeur par défaut de {0} pour que les fonctionnalités de modification des données fonctionnent correctement étant donné les paramètres attendus par les méthodes de notre couche BLL. Par conséquent, veillez à définir la propriété `OldValuesParameterFormatString` sur {0} ou supprimer entièrement la propriété de la syntaxe déclarative.

À la fin de l’Assistant Configuration de la source de données, en activant la modification et en supprimant la prise en charge dans le GridView et en retournant la propriété ObjectDataSource s `OldValuesParameterFormatString` sa valeur par défaut, le balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

À ce stade, nous pourrions nettoyer le GridView en personnalisant l’interface de modification pour inclure la validation, en faisant en sorte que les colonnes `CategoryID` et `SupplierID` restituent en tant que DropDownList, et ainsi de suite. Nous pouvons également ajouter une confirmation côté client au bouton supprimer, et je vous encourage à prendre le temps de mettre en œuvre ces améliorations. Comme ces rubriques ont été abordées dans les didacticiels précédents, nous ne les aborderons pas ici.

Que vous amélioriez ou non le contrôle GridView, testez les fonctionnalités principales de page dans un navigateur. Comme le montre la figure 24, la page répertorie les produits dans un GridView qui fournit des fonctionnalités de modification et de suppression par ligne.

[![les produits peuvent être affichés, modifiés et supprimés du contrôle GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**Figure 24**: les produits peuvent être affichés, modifiés et supprimés à partir du contrôle GridView ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))

## <a name="summary"></a>Récapitulatif

Les TableAdapters d’un DataSet typé peuvent accéder aux données de la base de données à l’aide d’instructions SQL ad hoc ou de procédures stockées. Lorsque vous utilisez des procédures stockées, vous pouvez utiliser des procédures stockées existantes ou l’Assistant TableAdapter peut être invité à créer de nouvelles procédures stockées basées sur une requête de `SELECT`. Dans ce didacticiel, nous avons abordé la création automatique des procédures stockées pour nous.

Si les procédures stockées générées automatiquement permettent de gagner du temps, il existe certains cas où la procédure stockée créée par l’Assistant n’est pas alignée sur ce que nous aurions créé. Par exemple, la procédure stockée `Products_Update`, qui attendait à la fois `@Original_ProductID` et `@ProductID` paramètres d’entrée, même si le paramètre `@Original_ProductID` était superflu.

Dans de nombreux scénarios, les procédures stockées ont peut-être déjà été créées, ou nous pouvons les générer manuellement de façon à ce qu’elles aient un degré de contrôle plus fin sur les commandes des procédures stockées. Dans les deux cas, nous souhaitons demander au TableAdapter d’utiliser des procédures stockées existantes pour ses méthodes. Nous allons voir comment procéder dans le didacticiel suivant.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Création et gestion des procédures stockées](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Récupération de données scalaires à partir d’une procédure stockée](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Notions de base des procédures stockées SQL Server](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Procédures stockées : vue d’ensemble](http://www.sqlteam.com/item.asp?ItemID=563)
- [Écriture d’une procédure stockée](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Hilton Geisenow. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
> [Suivant](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
