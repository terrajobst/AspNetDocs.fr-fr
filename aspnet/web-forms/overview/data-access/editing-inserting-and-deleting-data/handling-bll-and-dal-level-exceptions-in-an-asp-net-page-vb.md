---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: Gestion des exceptions de niveau BLL et DAL dans une page ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous verrons comment afficher un message d’erreur convivial et informatif si une exception se produit pendant une opération d’insertion, de mise à jour ou de suppression de...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: ee277596ade18d2603892d134b47c2c8697836bb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621052"
---
# <a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>Gestion des exceptions de niveau BLL et DAL dans une page ASP.NET (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe) ou [Télécharger le PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> Dans ce didacticiel, nous verrons comment afficher un message d’erreur convivial et informatif si une exception se produit pendant une opération d’insertion, de mise à jour ou de suppression d’un contrôle Web de données ASP.NET.

## <a name="introduction"></a>Introduction

L’utilisation des données d’une application Web ASP.NET à l’aide d’une architecture d’application à plusieurs niveaux implique les trois étapes générales suivantes :

1. Déterminez la méthode de la couche de logique métier qui doit être appelée et les valeurs de paramètre à passer. Les valeurs des paramètres peuvent être codées en dur, affectées par programmation ou entrées par l’utilisateur.
2. Appelez la méthode.
3. Traitez les résultats. Lors de l’appel d’une méthode BLL qui retourne des données, cela peut impliquer la liaison des données à un contrôle Web de données. Pour les méthodes BLL qui modifient des données, il peut s’agir de l’exécution d’une action basée sur une valeur de retour ou de la gestion en douceur d’une exception qui a eu naissance à l’étape 2.

Comme nous l’avons vu dans le [didacticiel précédent](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md), ObjectDataSource et les contrôles Web de données fournissent des points d’extensibilité pour les étapes 1 et 3. Le GridView, par exemple, déclenche son événement `RowUpdating` avant d’assigner ses valeurs de champ à la collection de `UpdateParameters` de l’ObjectDataSource. son `RowUpdated` événement est déclenché après l’exécution de l’ObjectDataSource.

Nous avons déjà examiné les événements qui se déclenchent au cours de l’étape 1 et vu comment ils peuvent être utilisés pour personnaliser les paramètres d’entrée ou pour annuler l’opération. Dans ce didacticiel, nous allons attirer l’attention sur les événements qui se déclenchent une fois l’opération terminée. Grâce à ces gestionnaires d’événements de publication, nous pouvons, entre autres, déterminer si une exception s’est produite pendant l’opération et la gérer de manière appropriée, en affichant un message d’erreur convivial et informatif à l’écran plutôt que par défaut au ASP.NET standard Page d’exception.

Pour illustrer l’utilisation de ces événements de publication, nous allons créer une page qui répertorie les produits dans un GridView modifiable. Lors de la mise à jour d’un produit, si une exception est levée, notre page ASP.NET affiche un message succinct au-dessus du contrôle GridView qui explique qu’un problème s’est produit. Commençons !

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Étape 1 : création d’un contrôle GridView modifiable de produits

Dans le didacticiel précédent, nous avons créé un GridView modifiable avec seulement deux champs, `ProductName` et `UnitPrice`. Cela nécessitait la création d’une surcharge supplémentaire pour la méthode `UpdateProduct` de la classe `ProductsBLL`, qui acceptait uniquement trois paramètres d’entrée (le nom du produit, le prix unitaire et l’ID), contrairement à un paramètre pour chaque champ produit. Pour ce didacticiel, nous allons réutiliser cette technique, en créant un GridView modifiable qui affiche le nom du produit, la quantité par unité, le prix unitaire et les unités en stock, mais uniquement le nom, le prix unitaire et les unités en stock à modifier.

Pour s’adapter à ce scénario, nous avons besoin d’une autre surcharge de la méthode `UpdateProduct`, qui accepte quatre paramètres : le nom du produit, le prix unitaire, les unités en stock et l’ID. Ajoutez la méthode suivante à la classe `ProductsBLL` :

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

Une fois cette méthode terminée, nous sommes prêts à créer la page ASP.NET qui permet de modifier ces quatre champs de produit particuliers. Ouvrez la page `ErrorHandling.aspx` dans le dossier `EditInsertDelete` et ajoutez un contrôle GridView à la page par le biais du concepteur. Liez le contrôle GridView à un nouvel ObjectDataSource, en mappant la méthode `Select()` à la méthode `GetProducts()` de la classe `ProductsBLL` et à la méthode `Update()` à la surcharge `UpdateProduct` créée.

[![utilisez la surcharge de la méthode UpdateProduct qui accepte quatre paramètres d’entrée](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**Figure 1**: utilisation de la surcharge de méthode `UpdateProduct` qui accepte quatre paramètres d’entrée ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))

Cela créera un ObjectDataSource avec une collection de `UpdateParameters` avec quatre paramètres et un GridView avec un champ pour chacun des champs de produit. Le balisage déclaratif de ObjectDataSource assigne la propriété `OldValuesParameterFormatString` la valeur `original_{0}`, ce qui entraînera une exception puisque notre classe BLL ne s’attend pas à ce que le paramètre d’entrée nommé `original_productID` soit passé. N’oubliez pas de supprimer ce paramètre de la syntaxe déclarative (ou affectez-lui la valeur par défaut, `{0}`).

Ensuite, réduisez la taille du contrôle GridView pour inclure uniquement les `ProductName`, `QuantityPerUnit`, `UnitPrice`et `UnitsInStock` BoundFields. N’hésitez pas à appliquer les mises en forme de niveau champ que vous jugez nécessaires (par exemple, pour modifier les propriétés de `HeaderText`).

Dans le didacticiel précédent, nous avons examiné comment formater le `UnitPrice` BoundField en tant que devise en mode lecture seule et en mode édition. Faisons la même chose ici. Rappelez-vous que ce paramètre nécessitait la définition de la propriété `DataFormatString` de BoundField pour `{0:c}`, de sa propriété `HtmlEncode` à `false`, et de ses `ApplyFormatInEditMode` à `true`, comme illustré à la figure 2.

[![configurer le BoundField UnitPrice pour qu’il s’affiche en tant que devise](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**Figure 2**: configurer le `UnitPrice` BoundField pour qu’il s’affiche en tant que devise ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))

La mise en forme de la `UnitPrice` en tant que devise dans l’interface d’édition requiert la création d’un gestionnaire d’événements pour l’événement de `RowUpdating` du GridView qui analyse la chaîne au format monétaire en une valeur `decimal`. Rappelez-vous que le gestionnaire d’événements `RowUpdating` du dernier didacticiel a également été vérifié pour s’assurer que l’utilisateur a fourni une valeur de `UnitPrice`. Toutefois, pour ce didacticiel, nous allons autoriser l’utilisateur à omettre le prix.

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

Notre GridView comprend un `QuantityPerUnit` BoundField, mais ce BoundField ne doit être utilisé qu’à des fins d’affichage et ne doit pas être modifiable par l’utilisateur. Pour ce faire, il vous suffit de définir la propriété BoundFields' `ReadOnly` sur `true`.

[![rendre le QuantityPerUnit BoundField en lecture seule](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**Figure 3**: rendre le `QuantityPerUnit` BoundField en lecture seule ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))

Enfin, activez la case à cocher Activer la modification à partir de la balise active de GridView. Une fois ces étapes terminées, le concepteur de la page `ErrorHandling.aspx` doit ressembler à la figure 4.

[![supprimer tout sauf les BoundFields nécessaires et activer la case à cocher Activer la modification](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**Figure 4**: supprimer tout sauf les BoundFields nécessaires et activer la case à cocher Activer la modification ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))

À ce stade, nous disposons d’une liste de tous les champs de `ProductName`, `QuantityPerUnit`, `UnitPrice`et `UnitsInStock` des produits. Toutefois, seuls les champs `ProductName`, `UnitPrice`et `UnitsInStock` peuvent être modifiés.

[![les utilisateurs peuvent désormais facilement modifier les noms, les prix et les unités des produits dans les champs de stock](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**Figure 5**: les utilisateurs peuvent désormais modifier facilement les noms, les prix et les unités des produits dans les champs boursiers ([cliquez pour afficher l’image en plein écran](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))

## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Étape 2 : gestion en douceur des exceptions au niveau de la couche DAL

Tandis que notre GridView modifiable fonctionne très bien quand les utilisateurs entrent des valeurs autorisées pour le nom, le prix et les unités en stock édités, la saisie de valeurs illégales entraîne une exception. Par exemple, l’omission de la valeur `ProductName` entraîne la levée d’un [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) , car `ProductName` la propriété `AllowDBNull` de la classe `ProductsRow` a la valeur `false`; Si la base de données est inactive, un `SqlException` est levé par le TableAdapter lors de la tentative de connexion à la base de données. Sans entreprendre aucune action, ces exceptions se propagent de la couche d’accès aux données vers la couche de logique métier, puis vers la page ASP.NET et enfin vers le runtime ASP.NET.

Selon la façon dont votre application Web est configurée et si vous visitez l’application à partir de `localhost`, une exception non gérée peut générer une page d’erreur de serveur générique, un rapport d’erreur détaillé ou une page Web conviviale. Consultez [gestion des erreurs d’application Web dans ASP.net](http://www.15seconds.com/issue/030102.htm) et l' [élément customErrors](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) pour plus d’informations sur la façon dont le runtime ASP.net répond à une exception non interceptée.

La figure 6 illustre l’écran rencontré lors d’une tentative de mise à jour d’un produit sans spécifier la valeur `ProductName`. Il s’agit du rapport d’erreurs détaillé par défaut qui s’affiche lors de la `localhost`.

[![l’omission du nom du produit affichera les détails de l’exception](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**Figure 6**: l’omission du nom du produit affichera les détails de l’exception ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))

Bien que ces détails d’exception soient utiles lors du test d’une application, la présentation d’un utilisateur final avec un tel écran en face d’une exception est moins que idéale. Un utilisateur final risque de ne pas savoir ce qu’est un `NoNullAllowedException` ou la raison pour laquelle il a été provoqué. Une meilleure approche consiste à présenter à l’utilisateur un message plus convivial qui explique qu’il y a eu des problèmes lors de la tentative de mise à jour du produit.

Si une exception se produit lors de l’exécution de l’opération, les événements de publication dans ObjectDataSource et le contrôle Web de données fournissent un moyen de le détecter et d’annuler l’exception de propagation jusqu’au runtime ASP.NET. Pour notre exemple, nous allons créer un gestionnaire d’événements pour l’événement `RowUpdated` de GridView qui détermine si une exception s’est déclenchée et, le cas échéant, affiche les détails de l’exception dans un contrôle Web étiquette.

Commencez par ajouter une étiquette à la page ASP.NET, en affectant à sa propriété `ID` la valeur `ExceptionDetails` et en effaçant sa propriété `Text`. Pour attirer l’attention de l’utilisateur sur ce message, définissez sa propriété `CssClass` sur `Warning`, qui est une classe CSS ajoutée au fichier `Styles.css` dans le didacticiel précédent. Rappelez-vous que cette classe CSS provoque l’affichage du texte de l’étiquette dans une police rouge, italique, en gras, très grande.

[![ajouter un contrôle Web Label à la page](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**Figure 7**: ajouter un contrôle Web étiquette à la page ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))

Comme nous voulons que ce contrôle Web label soit visible juste après une exception, affectez la valeur false à sa propriété `Visible` dans le gestionnaire d’événements `Page_Load` :

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

Avec ce code, sur la première page, consultez les publications (postbacks) suivantes. la propriété `Visible` de la `ExceptionDetails` contrôle sera définie sur `false`. En face d’une exception de niveau DAL ou BLL, que nous pouvons détecter dans le gestionnaire d’événements `RowUpdated` du GridView, nous définissons la propriété `Visible` du contrôle `ExceptionDetails` sur true. Étant donné que les gestionnaires d’événements de contrôle Web se produisent après le `Page_Load` gestionnaire d’événements dans le cycle de vie de la page, l’étiquette est affichée. Toutefois, lors de la prochaine publication (postback), le gestionnaire d’événements `Page_Load` rétablira la propriété `Visible` à `false`, en le masquant à partir de l’affichage.

> [!NOTE]
> Vous pouvez également supprimer la nécessité de définir la propriété `Visible` du contrôle `ExceptionDetails` dans `Page_Load` en affectant sa propriété `Visible` `false` dans la syntaxe déclarative et en désactivant son état d’affichage (en définissant sa propriété `EnableViewState` sur `false`). Nous utiliserons cette autre approche dans un prochain didacticiel.

Une fois le contrôle Label ajouté, l’étape suivante consiste à créer le gestionnaire d’événements pour l’événement `RowUpdated` de GridView. Sélectionnez le contrôle GridView dans le concepteur, accédez à la Fenêtre Propriétés, puis cliquez sur l’icône représentant un éclair, qui répertorie les événements du contrôle GridView. Il doit déjà y avoir une entrée pour l’événement `RowUpdating` du GridView, car nous avons créé un gestionnaire d’événements pour cet événement plus tôt dans ce didacticiel. Créez également un gestionnaire d’événements pour l’événement `RowUpdated`.

![Créer un gestionnaire d’événements pour l’événement RowUpdated du GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**Figure 8**: créer un gestionnaire d’événements pour l’événement `RowUpdated` du GridView

> [!NOTE]
> Vous pouvez également créer le gestionnaire d’événements dans les listes déroulantes en haut du fichier de classe code-behind. Sélectionnez le contrôle GridView dans la liste déroulante à gauche et l’événement `RowUpdated` dans celui de droite.

La création de ce gestionnaire d’événements ajoute le code suivant à la classe code-behind de la page ASP.NET :

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

Le deuxième paramètre d’entrée du gestionnaire d’événements est un objet de type [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), qui a trois propriétés intéressantes pour la gestion des exceptions :

- `Exception` une référence à l’exception levée ; Si aucune exception n’a été levée, cette propriété aura la valeur `null`
- `ExceptionHandled` une valeur booléenne qui indique si l’exception a été gérée dans le gestionnaire d’événements `RowUpdated` ; Si `false` (valeur par défaut), l’exception est levée à nouveau, avec un percolation jusqu’au runtime ASP.NET
- `KeepInEditMode` si la valeur `true` la ligne de GridView modifiée reste en mode édition ; Si `false` (valeur par défaut), la ligne GridView revient en mode lecture seule

Notre code, puis doit vérifier si `Exception` n’est pas `null`, ce qui signifie qu’une exception a été levée lors de l’exécution de l’opération. Si c’est le cas, nous voulons :

- Afficher un message convivial dans l’étiquette `ExceptionDetails`
- Indiquer que l’exception a été gérée
- Conserver la ligne GridView en mode édition

Ce code remplit les objectifs suivants :

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

Ce gestionnaire d’événements commence par vérifier si `e.Exception` est `null`. Si ce n’est pas le cas, la propriété `Visible` de l’étiquette `ExceptionDetails` est définie sur `true` et sa propriété `Text` sur « un problème est survenu lors de la mise à jour du produit ». Les détails de l’exception réelle qui a été levée se trouvent dans la propriété `InnerException` de l’objet `e.Exception`. Cette exception interne est examinée et, si elle est d’un type particulier, un message utile supplémentaire est ajouté à la propriété `Text` de l’étiquette `ExceptionDetails`. Enfin, les propriétés `ExceptionHandled` et `KeepInEditMode` sont toutes deux définies sur `true`.

La figure 9 illustre une capture d’écran de cette page lorsque vous omettez le nom du produit. La figure 10 montre les résultats lors de l’entrée d’une valeur de `UnitPrice` illégale (-50).

[![le BoundField ProductName doit contenir une valeur](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**Figure 9**: le `ProductName` BoundField doit contenir une valeur ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))

[![valeurs UnitPrice négatives ne sont pas autorisées](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**Figure 10**: les valeurs `UnitPrice` négatives ne sont pas autorisées ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))

En affectant à la propriété `e.ExceptionHandled` la valeur `true`, le gestionnaire d’événements `RowUpdated` a indiqué qu’il a géré l’exception. Par conséquent, l’exception ne se propage pas jusqu’au runtime ASP.NET.

> [!NOTE]
> Les figures 9 et 10 montrent un moyen normal de gérer les exceptions levées en raison d’une entrée utilisateur non valide. Dans l’idéal, toutefois, une telle entrée non valide n’atteindra jamais la couche de logique métier en premier lieu, car la page ASP.NET doit s’assurer que les entrées de l’utilisateur sont valides avant d’appeler la méthode `UpdateProduct` de la classe `ProductsBLL`. Dans notre prochain didacticiel, nous verrons comment ajouter des contrôles de validation aux interfaces de modification et d’insertion pour vous assurer que les données soumises à la couche de logique métier sont conformes aux règles d’entreprise. Les contrôles de validation empêchent non seulement l’appel de la méthode `UpdateProduct` tant que les données fournies par l’utilisateur ne sont pas valides, mais fournissent également une expérience utilisateur plus informatif pour identifier les problèmes d’entrée de données.

## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Étape 3 : gestion en douceur des exceptions de niveau BLL

Lors de l’insertion, de la mise à jour ou de la suppression de données, la couche d’accès aux données peut lever une exception en cas d’erreur liée aux données. La base de données est peut-être hors connexion, une colonne de table de base de données requise peut ne pas avoir de valeur spécifiée ou une contrainte au niveau de la table a peut-être été violée. En plus des exceptions strictement liées aux données, la couche de logique métier peut utiliser des exceptions pour indiquer le moment où les règles d’entreprise ont été violées. Dans le didacticiel [création d’une couche de logique métier](../introduction/creating-a-business-logic-layer-vb.md) , par exemple, nous avons ajouté une vérification de règle d’entreprise à la surcharge d' `UpdateProduct` d’origine. Plus précisément, si l’utilisateur marque un produit comme étant abandonné, nous avions besoin que le produit ne soit pas celui fourni par son fournisseur. Si cette condition a été violée, une `ApplicationException` a été levée.

Pour la surcharge de `UpdateProduct` créée dans ce didacticiel, nous allons ajouter une règle d’entreprise qui empêche le champ `UnitPrice` d’être défini sur une nouvelle valeur supérieure à deux fois la valeur de `UnitPrice` d’origine. Pour ce faire, ajustez la surcharge `UpdateProduct` pour qu’elle effectue cette vérification et lève une `ApplicationException` si la règle est violée. La méthode mise à jour est la suivante :

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

Avec cette modification, toute mise à jour de prix qui est plus que deux fois le prix existant entraîne la levée d’une `ApplicationException`. Tout comme l’exception levée à partir de la couche DAL, cette `ApplicationException` en forme de couche BLL peut être détectée et gérée dans le gestionnaire d’événements `RowUpdated` du GridView. En fait, le `RowUpdated` code du gestionnaire d’événements, tel qu’il est écrit, détecte correctement cette exception et affiche la valeur de la propriété `Message` du `ApplicationException`. La figure 11 illustre une capture d’écran lorsqu’un utilisateur tente de mettre à jour le prix de chai à $50,00, ce qui correspond à plus de deux fois son prix actuel de $19,95.

[![le prix de l’interdiction des règles d’entreprise augmente le prix d’un produit supérieur à double](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**Figure 11**: le prix de l’interdiction des règles d’entreprise augmente de plus que le double du prix d’un produit ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))

> [!NOTE]
> Idéalement, nos règles de logique métier seraient refactorées à partir des surcharges de méthode `UpdateProduct` et dans une méthode commune. Cela reste un exercice pour le lecteur.

## <a name="summary"></a>Récapitulatif

Lors de l’insertion, de la mise à jour et de la suppression d’opérations, le contrôle Web de données et l’ObjectDataSource impliquaient des événements de pré-et de postconnexion qui bookend l’opération réelle. Comme nous l’avons vu dans ce didacticiel et dans le précédent, lors de l’utilisation d’un GridView modifiable, l’événement `RowUpdating` du GridView se déclenche, suivi de l’événement de `Updating` de l’ObjectDataSource, à partir duquel la commande de mise à jour est effectuée vers l’objet sous-jacent de l’ObjectDataSource. Une fois l’opération terminée, l’événement de `Updated` de l’ObjectDataSource se déclenche, suivi de l’événement de `RowUpdated` du GridView.

Nous pouvons créer des gestionnaires d’événements pour les événements de préniveau afin de personnaliser les paramètres d’entrée ou les événements de la publication afin d’inspecter et de répondre aux résultats de l’opération. Les gestionnaires d’événements de publication sont couramment utilisés pour détecter si une exception s’est produite pendant l’opération. En face d’une exception, ces gestionnaires d’événements de niveau de publication peuvent éventuellement gérer l’exception par eux-mêmes. Dans ce didacticiel, nous avons vu comment gérer une telle exception en affichant un message d’erreur convivial.

Dans le didacticiel suivant, nous verrons comment réduire la probabilité d’exceptions dues à des problèmes de mise en forme des données (par exemple, la saisie d’un `UnitPrice`négatif). Plus précisément, nous verrons comment ajouter des contrôles de validation aux interfaces de modification et d’insertion.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Liz Shulok. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [Suivant](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
