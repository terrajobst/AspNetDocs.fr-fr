---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: Configuration des paramètres de connexion et de niveau de commande de la couche d’accès aux données (VB) | Microsoft Docs
author: rick-anderson
description: Les TableAdapters dans un DataSet typé prennent automatiquement en charge la connexion à la base de données, l’émission de commandes et le remplissage d’un DataTable avec les résultats...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: fa2868fc0dd8acd76f600b47d92adb984ce8d105
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78551806"
---
# <a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>Configuration des paramètres de niveau connexion et commande de la couche d’accès aux données (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip) ou [Télécharger le PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> Les TableAdapters dans un DataSet typé prennent automatiquement en charge la connexion à la base de données, l’émission de commandes et le remplissage d’un DataTable avec les résultats. Toutefois, dans certains cas, nous voulons nous occuper de ces détails. dans ce didacticiel, nous allons apprendre à accéder aux paramètres de connexion à la base de données et au niveau de la commande dans le TableAdapter.

## <a name="introduction"></a>Introduction

Tout au long de la série de didacticiels, nous avons utilisé des DataSets typés pour implémenter la couche d’accès aux données et les objets métier de notre architecture en couches. Comme indiqué dans le [premier didacticiel](../introduction/creating-a-data-access-layer-vb.md), les DataTables de DataSet typés servent de référentiels de données tandis que les TableAdapters jouent le rôle de wrappers pour communiquer avec la base de données afin de récupérer et de modifier les données sous-jacentes. Les TableAdapters encapsulent la complexité impliquée dans l’utilisation de la base de données et nous évitent d’avoir à écrire du code pour se connecter à la base de données, à émettre une commande ou à remplir les résultats dans un DataTable.

Dans certains cas, toutefois, lorsque nous avons besoin d’Burrow dans les profondeurs du TableAdapter et d’écrire du code qui fonctionne directement avec les objets ADO.NET. Dans le didacticiel sur les [modifications de base de données encapsulées dans une transaction](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) , nous avons par exemple ajouté des méthodes au TableAdapter pour le début, la validation et la restauration des transactions ADO.net. Ces méthodes utilisaient un objet interne, créé manuellement `SqlTransaction`, qui a été assigné aux objets TableAdapter s `SqlCommand`.

Dans ce didacticiel, nous allons examiner comment accéder aux paramètres de connexion à la base de données et au niveau de la commande dans le TableAdapter. En particulier, nous ajouterons des fonctionnalités au `ProductsTableAdapter` qui permet d’accéder aux paramètres de chaîne de connexion sous-jacente et de délai de commande.

## <a name="working-with-data-using-adonet"></a>Utilisation des données à l’aide de ADO.NET

L’infrastructure Microsoft .NET contient une multitude de classes conçues spécifiquement pour fonctionner avec des données. Ces classes, qui se trouvent dans l' [espace de noms`System.Data`](https://msdn.microsoft.com/library/system.data.aspx), sont appelées classes *ADO.net* . Certaines des classes sous le parapluie ADO.NET sont liées à un fournisseur de *données*particulier. Vous pouvez considérer un fournisseur de données comme un canal de communication qui permet d’acheminer les informations entre les classes ADO.NET et le magasin de données sous-jacent. Il existe des fournisseurs généralisés, tels que OleDb et ODBC, ainsi que des fournisseurs qui sont spécialement conçus pour un système de base de données particulier. Par exemple, bien qu’il soit possible de se connecter à une base de données Microsoft SQL Server à l’aide du fournisseur OleDb, le fournisseur SqlClient est bien plus efficace, car il a été conçu et optimisé spécifiquement pour SQL Server.

Lors de l’accès par programme aux données, le modèle suivant est couramment utilisé :

1. Établissez une connexion à la base de données.
2. Émettez une commande.
3. Pour les requêtes `SELECT`, travaillez avec les enregistrements résultants.

Il existe des classes ADO.NET distinctes pour effectuer chacune de ces étapes. Pour vous connecter à une base de données à l’aide du fournisseur SqlClient, par exemple, utilisez la [classe`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Pour émettre une commande `INSERT`, `UPDATE`, `DELETE`ou `SELECT` à la base de données, utilisez la [classe`SqlCommand`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

À l’exception des [modifications de la base de données d’encapsulation dans un](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) didacticiel sur les transactions, nous n’avons pas eu à écrire de code ADO.net de bas niveau nous-même, car le code généré automatiquement par les TableAdapters comprend les fonctionnalités nécessaires pour se connecter à la base de données, émettre des commandes, récupérer des données et remplir ces données dans des DataTables. Toutefois, il peut arriver que vous deviez personnaliser ces paramètres de bas niveau. Au cours des étapes suivantes, nous allons examiner comment exploiter les objets ADO.NET utilisés en interne par les TableAdapters.

## <a name="step-1-examining-with-the-connection-property"></a>Étape 1 : examen avec la propriété de connexion

Chaque classe TableAdapter a une propriété `Connection` qui spécifie les informations de connexion à la base de données. Le type de données et la valeur `ConnectionString` de cette propriété sont déterminés par les sélections effectuées dans l’Assistant Configuration de TableAdapter. Rappelez-vous que lors de la première ajout d’un TableAdapter à un DataSet typé, cet Assistant vous demande la source de la base de données (voir la figure 1). La liste déroulante de cette première étape comprend les bases de données spécifiées dans le fichier de configuration ainsi que toutes les autres bases de données des connexions de données Explorateur de serveurs s. Si la base de données que nous souhaitons utiliser n’existe pas dans la liste déroulante, vous pouvez spécifier une nouvelle connexion à la base de données en cliquant sur le bouton nouvelle connexion et en fournissant les informations de connexion nécessaires.

[![la première étape de l’Assistant Configuration de TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**Figure 1**: première étape de l’Assistant Configuration de TableAdapter ([cliquez pour afficher l’image en taille réelle](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))

Essayons quelques instants pour inspecter le code de la propriété `Connection` TableAdapter s. Comme indiqué dans le didacticiel [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) , nous pouvons afficher le code du TableAdapter généré automatiquement en accédant à la fenêtre de affichage de classes, en explorant la classe appropriée, puis en double-cliquant sur le nom du membre.

Accédez à la fenêtre de Affichage de classes en accédant au menu Affichage et en choisissant Affichage de classes (ou en tapant Ctrl + Maj + C). Dans la partie supérieure de la fenêtre de Affichage de classes, descendez jusqu’à l’espace de noms `NorthwindTableAdapters` et sélectionnez la classe `ProductsTableAdapter`. Cette opération affiche les membres `ProductsTableAdapter` s dans la moitié inférieure du Affichage de classes, comme illustré à la figure 2. Double-cliquez sur la propriété `Connection` pour afficher son code.

![Double-cliquez sur la propriété de connexion dans le Affichage de classes pour afficher son code généré automatiquement](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**Figure 2**: double-cliquez sur la propriété de connexion dans le affichage de classes pour afficher son code généré automatiquement

La propriété `Connection` du TableAdapter et le code associé à la connexion sont les suivants :

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

Lorsque la classe TableAdapter est instanciée, la variable membre `_connection` est égale à `Nothing`. Lorsque vous accédez à la propriété `Connection`, il commence par vérifier si la variable membre `_connection` a été instanciée. Si ce n’est pas le cas, la méthode `InitConnection` est appelée, ce qui instancie `_connection` et définit sa propriété `ConnectionString` sur la valeur de chaîne de connexion spécifiée à partir de la première étape de l’Assistant Configuration de TableAdapter.

La propriété `Connection` peut également être assignée à un objet `SqlConnection`. Cela associe le nouvel objet `SqlConnection` à chacun des objets TableAdapter s `SqlCommand`.

## <a name="step-2-exposing-connection-level-settings"></a>Étape 2 : exposition des paramètres au niveau de la connexion

Les informations de connexion doivent rester encapsulées dans le TableAdapter et ne pas être accessibles aux autres couches de l’architecture de l’application. Toutefois, il peut y avoir des scénarios dans lesquels les informations au niveau de la connexion du TableAdapter doivent être accessibles ou personnalisables pour une requête, un utilisateur ou une page ASP.NET.

Nous allons étendre le `ProductsTableAdapter` dans le jeu de données `Northwind` pour inclure une propriété `ConnectionString` qui peut être utilisée par la couche de logique métier pour lire ou modifier la chaîne de connexion utilisée par le TableAdapter.

> [!NOTE]
> Une *chaîne de connexion* est une chaîne qui spécifie les informations de connexion à la base de données, telles que le fournisseur à utiliser, l’emplacement de la base de données, les informations d’identification d’authentification et d’autres paramètres liés à la base de données. Pour obtenir la liste des modèles de chaîne de connexion utilisés par divers magasins de données et fournisseurs, consultez [connectionStrings.com](http://www.connectionstrings.com/).

Comme indiqué dans le didacticiel [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) , les classes générées automatiquement du DataSet typé peuvent être étendues à l’aide de classes partielles. Tout d’abord, créez un sous-dossier dans le projet nommé `ConnectionAndCommandSettings` sous le dossier `~/App_Code/DAL`.

![Ajoutez un sous-dossier nommé ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**Figure 3**: ajouter un sous-dossier nommé `ConnectionAndCommandSettings`

Ajoutez un nouveau fichier de classe nommé `ProductsTableAdapter.ConnectionAndCommandSettings.vb` et entrez le code suivant :

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

Cette classe partielle ajoute une propriété `Public` nommée `ConnectionString` à la classe `ProductsTableAdapter` qui permet à n’importe quelle couche de lire ou de mettre à jour la chaîne de connexion pour la connexion sous-jacente du TableAdapter.

Une fois cette classe partielle créée (et enregistrée), ouvrez la classe `ProductsBLL`. Accédez à l’une des méthodes existantes et tapez `Adapter`, puis appuyez sur la touche point pour afficher IntelliSense. Vous devez voir la nouvelle propriété `ConnectionString` disponible dans IntelliSense, ce qui signifie que vous pouvez lire ou ajuster cette valeur par programmation à partir de la couche BLL.

## <a name="exposing-the-entire-connection-object"></a>Exposition de l’objet de connexion entier

Cette classe partielle expose une seule propriété de l’objet de connexion sous-jacent : `ConnectionString`. Si vous souhaitez que l’intégralité de l’objet de connexion soit disponible au-delà des limites du TableAdapter, vous pouvez également modifier le niveau de protection de `Connection` propriété s. Le code généré automatiquement que nous avons examiné à l’étape 1 a montré que la propriété TableAdapter s `Connection` est marquée comme `Friend`, ce qui signifie qu’elle est accessible uniquement par les classes du même assembly. Cela peut toutefois être modifié par le biais de la propriété `ConnectionModifier` du TableAdapter.

Ouvrez le jeu de données `Northwind`, cliquez sur l' `ProductsTableAdapter` dans le concepteur, puis accédez à la Fenêtre Propriétés. Vous verrez que le `ConnectionModifier` défini sur sa valeur par défaut, `Assembly`. Pour rendre la propriété `Connection` disponible en dehors de l’assembly du DataSet typé, affectez à la propriété `ConnectionModifier` la valeur `Public`.

[![le niveau d’accessibilité de la propriété de connexion peut être configuré à l’aide de la propriété ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**Figure 4**: le niveau d’accessibilité de la propriété `Connection` peut être configuré via la propriété `ConnectionModifier` ([cliquez pour afficher l’image en taille réelle](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))

Enregistrez le jeu de données, puis revenez à la classe `ProductsBLL`. Comme précédemment, accédez à l’une des méthodes existantes et tapez `Adapter` puis appuyez sur la touche point pour afficher IntelliSense. La liste doit inclure une propriété `Connection`, ce qui signifie que vous pouvez désormais lire par programmation ou assigner des paramètres au niveau de la connexion à partir de la couche BLL.

## <a name="step-3-examining-the-command-related-properties"></a>Étape 3 : examen des propriétés relatives aux commandes

Un TableAdapter se compose d’une requête principale qui, par défaut, a des instructions `INSERT`, `UPDATE`et `DELETE` générées automatiquement. Cette requête principale s `INSERT`, `UPDATE`et `DELETE` instructions sont implémentées dans le code du TableAdapter en tant qu’objet adaptateur de données ADO.NET via la propriété `Adapter`. Comme avec sa propriété `Connection`, le type de données de la propriété `Adapter` s est déterminé par le fournisseur de données utilisé. Étant donné que ces didacticiels utilisent le fournisseur SqlClient, la propriété `Adapter` est de type [`SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

La propriété `Adapter` du TableAdapter possède trois propriétés de type `SqlCommand` qu’il utilise pour émettre les instructions `INSERT`, `UPDATE`et `DELETE` :

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Un objet `SqlCommand` est chargé d’envoyer une requête particulière à la base de données et possède des propriétés telles que : [`CommandText`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), qui contient l’instruction SQL ou la procédure stockée ad hoc à exécuter ; et [`Parameters`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), qui est une collection d’objets `SqlParameter`. Comme nous l’avons vu dans le didacticiel [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) , vous pouvez personnaliser ces objets de commande à l’aide de l’fenêtre Propriétés.

En plus de sa requête principale, le TableAdapter peut inclure un nombre variable de méthodes qui, lorsqu’il est appelé, distribue une commande spécifiée à la base de données. L’objet de commande principal de la requête et les objets de commande pour toutes les méthodes supplémentaires sont stockés dans la propriété `CommandCollection` TableAdapter s.

Nous allons prendre un moment pour examiner le code généré par le `ProductsTableAdapter` dans le jeu de données `Northwind` pour ces deux propriétés, ainsi que les variables membres et les méthodes d’assistance suivantes :

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

Le code pour les propriétés `Adapter` et `CommandCollection` imite fidèlement celui de la propriété `Connection`. Il existe des variables membres qui contiennent les objets utilisés par les propriétés. Les propriétés `Get` accesseurs commencent par vérifier si la variable membre correspondante est `Nothing`. Si c’est le cas, une méthode d’initialisation est appelée, ce qui crée une instance de la variable membre et assigne les propriétés principales relatives à la commande.

## <a name="step-4-exposing-command-level-settings"></a>Étape 4 : exposition des paramètres au niveau de la commande

Dans l’idéal, les informations au niveau de la commande doivent rester encapsulées dans la couche d’accès aux données. Toutefois, si ces informations sont nécessaires dans d’autres couches de l’architecture, elles peuvent être exposées par le biais d’une classe partielle, tout comme avec les paramètres de niveau de connexion.

Étant donné que le TableAdapter ne possède qu’une seule `Connection` propriété, le code pour exposer les paramètres de niveau connexion est relativement simple. Les choses sont un peu plus compliquées lors de la modification des paramètres au niveau de la commande, car le TableAdapter peut avoir plusieurs objets de commande : un `InsertCommand`, `UpdateCommand`et `DeleteCommand`, ainsi qu’un nombre variable d’objets de commande dans la propriété `CommandCollection`. Lors de la mise à jour des paramètres au niveau de la commande, ces paramètres doivent être propagés à tous les objets de commande.

Par exemple, imaginez que certaines requêtes dans le TableAdapter prenaient beaucoup de temps à s’exécuter. Lorsque vous utilisez le TableAdapter pour exécuter l’une de ces requêtes, nous pouvons souhaiter augmenter la [propriété`CommandTimeout`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx)de l’objet de commande. Cette propriété spécifie le nombre de secondes d’attente pour que la commande s’exécute et la valeur par défaut est 30.

Pour permettre l’ajustement de la propriété `CommandTimeout` par la couche BLL, ajoutez la méthode `Public` suivante au `ProductsDataTable` à l’aide du fichier de classe partielle créé à l’étape 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`) :

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

Cette méthode peut être appelée à partir de la couche BLL ou de la couche de présentation pour définir le délai d’attente de la commande pour tous les problèmes de commandes par cette instance de TableAdapter.

> [!NOTE]
> Les propriétés `Adapter` et `CommandCollection` sont marquées comme `Private`, ce qui signifie qu’elles ne sont accessibles qu’à partir du code dans le TableAdapter. Contrairement à la propriété `Connection`, ces modificateurs d’accès ne sont pas configurables. Par conséquent, si vous devez exposer des propriétés au niveau de la commande à d’autres couches de l’architecture, vous devez utiliser l’approche de classe partielle décrite ci-dessus pour fournir une méthode ou propriété `Public` qui lit ou écrit dans les objets de commande `Private`.

## <a name="summary"></a>Récapitulatif

Les TableAdapters dans un DataSet typé servent à encapsuler les détails et la complexité d’accès aux données. À l’aide de TableAdapters, nous n’avons pas à vous soucier de l’écriture de code ADO.NET pour la connexion à la base de données, l’émission d’une commande ou le remplissage des résultats dans un DataTable. Tout est géré automatiquement pour nous.

Toutefois, il peut arriver que vous deviez personnaliser les caractéristiques ADO.NET de bas niveau, telles que la modification de la chaîne de connexion ou la connexion par défaut ou les valeurs du délai d’attente de la commande. Le TableAdapter possède des propriétés de `Connection`, `Adapter`et `CommandCollection` générées automatiquement, mais celles-ci sont soit `Friend`, soit `Private`, par défaut. Ces informations internes peuvent être exposées en étendant le TableAdapter à l’aide de classes partielles pour inclure `Public` des méthodes ou des propriétés. Vous pouvez également configurer le modificateur d’accès à la propriété `Connection` du TableAdapter par le biais de la propriété TableAdapter s `ConnectionModifier`.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Burnadette Leigh, S Ren Jacob Lauritsen, Teresa Murphy et Hilton Geisenow. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](working-with-computed-columns-vb.md)
> [Suivant](protecting-connection-strings-and-other-configuration-information-vb.md)
