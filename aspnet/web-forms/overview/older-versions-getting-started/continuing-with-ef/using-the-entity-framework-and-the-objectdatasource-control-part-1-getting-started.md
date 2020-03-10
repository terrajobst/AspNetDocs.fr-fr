---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'À l’aide de l’Entity Framework 4,0 et du contrôle ObjectDataSource, partie 1 : Prise en main | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le Prise en main avec la série de didacticiels Entity Framework. Si Yo...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 2f14707eb058d438495dd2bc4c17b976c471fc97
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547291"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>À l’aide de l’Entity Framework 4,0 et du contrôle ObjectDataSource, partie 1 : Prise en main

par [Tom Dykstra](https://github.com/tdykstra)

> Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le [prise en main avec la](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série de didacticiels Entity Framework 4,0. Si vous n’avez pas terminé les didacticiels précédents, comme point de départ pour ce didacticiel, vous pouvez [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous auriez créée. Vous pouvez également [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créée par la série de didacticiels complète.
> 
> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET Web Forms à l’aide de la Entity Framework 4,0 et de Visual Studio 2010. L’exemple d’application est un site Web pour une université contoso fictive. Il comprend des fonctionnalités telles que l’admission des étudiants, la création des cours et les affectations des formateurs.
> 
> Ce didacticiel présente des exemples C#dans. L' [exemple téléchargeable](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) contient du code C# à la fois dans et Visual Basic.
> 
> ## <a name="database-first"></a>Database First
> 
> Il existe trois façons de travailler avec des données dans le Entity Framework : *Database First*, *Model First*et *Code First*. Ce didacticiel est destiné à Database First. Pour plus d’informations sur les différences entre ces flux de travail et des conseils sur la façon de choisir le meilleur pour votre scénario, consultez [Entity Framework des flux de travail de développement](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> À l’instar de la série Prise en main, cette série de didacticiels utilise le modèle ASP.NET Web Forms et suppose que vous savez utiliser ASP.NET Web Forms dans Visual Studio. Si ce n’est pas le cas, consultez [prise en main avec ASP.NET 4,5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Si vous préférez utiliser l’infrastructure MVC ASP.NET, consultez [prise en main avec le Entity Framework à l’aide de ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versions de logiciels
> 
> | **Indiqué dans le didacticiel** | **Fonctionne également avec** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express pour le Web. Ce didacticiel n’a pas été testé avec les versions ultérieures de Visual Studio. Il existe de nombreuses différences dans les sélections de menus, les boîtes de dialogue et les modèles. |
> | .NET 4 | .NET 4,5 offre une compatibilité descendante avec .NET 4, mais le didacticiel n’a pas été testé avec .NET 4,5. |
> | Entity Framework 4 | Ce didacticiel n’a pas été testé avec les versions ultérieures de Entity Framework. À partir de Entity Framework 5, EF utilise par défaut le `DbContext API` introduit avec EF 4,1. Le contrôle EntityDataSource a été conçu pour utiliser l’API `ObjectContext`. Pour plus d’informations sur l’utilisation du contrôle EntityDataSource avec l’API `DbContext`, consultez ce billet de [blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Questions
> 
> Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), sur le [forum Entity Framework et LINQ to Entities](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), ou sur [StackOverflow.com](http://stackoverflow.com/).

Le contrôle `EntityDataSource` vous permet de créer une application très rapidement, mais elle vous oblige généralement à conserver une quantité significative de logique métier et d’accès aux données dans vos pages *. aspx* . Si vous vous attendez à ce que votre application se développe en complexité et qu’elle nécessite une maintenance continue, vous pouvez investir plus de temps pour le développement en amont afin de créer une structure d’application *multicouche ou* en *couches* plus facile à gérer. Pour implémenter cette architecture, vous séparez la couche de présentation de la couche de logique métier (BLL) et de la couche d’accès aux données (DAL). L’une des façons d’implémenter cette structure est d’utiliser le contrôle `ObjectDataSource` au lieu du contrôle `EntityDataSource`. Lorsque vous utilisez le contrôle `ObjectDataSource`, vous implémentez votre propre code d’accès aux données, puis vous l’appelez dans des pages *. aspx* à l’aide d’un contrôle qui possède un grand nombre des mêmes fonctionnalités que les autres contrôles de source de données. Cela vous permet de combiner les avantages d’une approche multiniveau avec les avantages de l’utilisation d’un contrôle de Web Forms pour l’accès aux données.

Le contrôle `ObjectDataSource` offre également davantage de flexibilité. Étant donné que vous écrivez votre propre code d’accès aux données, il est plus facile de faire plus que simplement lire, insérer, mettre à jour ou supprimer un type d’entité spécifique, qui sont les tâches que le contrôle de `EntityDataSource` est conçu pour effectuer. Par exemple, vous pouvez effectuer la journalisation chaque fois qu’une entité est mise à jour, archiver des données chaque fois qu’une entité est supprimée, ou vérifier et mettre à jour automatiquement les données associées en fonction des besoins lors de l’insertion d’une ligne avec une valeur de clé étrangère.

## <a name="business-logic-and-repository-classes"></a>Logique métier et classes de référentiel

Un contrôle `ObjectDataSource` fonctionne en appelant une classe que vous créez. La classe comprend des méthodes qui récupèrent et mettent à jour des données, et vous fournissez les noms de ces méthodes au contrôle `ObjectDataSource` dans le balisage. Pendant le rendu ou le traitement des publications (postback), le `ObjectDataSource` appelle les méthodes que vous avez spécifiées.

Outre les opérations CRUD de base, la classe que vous créez à utiliser avec le contrôle `ObjectDataSource` peut avoir besoin d’exécuter la logique métier lorsque le `ObjectDataSource` lit ou met à jour les données. Par exemple, lorsque vous mettez à jour un service, vous devrez peut-être vérifier qu’aucun autre service n’a le même administrateur, car une personne ne peut pas être administrateur de plusieurs départements.

Dans une documentation `ObjectDataSource`, telle que la [vue d’ensemble de la classe ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), le contrôle appelle une classe appelée *objet métier* qui comprend à la fois la logique métier et la logique d’accès aux données. Dans ce didacticiel, vous allez créer des classes distinctes pour la logique métier et pour la logique d’accès aux données. La classe qui encapsule la logique d’accès aux données est appelée un *référentiel*. La classe de logique métier comprend à la fois des méthodes de logique métier et des méthodes d’accès aux données, mais les méthodes d’accès aux données appellent le référentiel pour effectuer des tâches d’accès aux données.

Vous allez également créer une couche d’abstraction entre vos couches BLL et DAL qui facilite le test unitaire automatisé de la couche BLL. Cette couche d’abstraction est implémentée en créant une interface et en utilisant l’interface quand vous instanciez le référentiel dans la classe de logique métier. Cela vous permet de fournir la classe de logique métier avec une référence à n’importe quel objet qui implémente l’interface de référentiel. Pour un fonctionnement normal, vous fournissez un objet de référentiel qui fonctionne avec l’Entity Framework. À des fins de test, vous fournissez un objet de référentiel qui fonctionne avec les données stockées d’une manière que vous pouvez facilement manipuler, comme les variables de classe définies en tant que Collections.

L’illustration suivante montre la différence entre une classe de logique métier qui comprend une logique d’accès aux données sans référentiel et une classe qui utilise un référentiel.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Vous commencerez par créer des pages Web dans lesquelles le contrôle `ObjectDataSource` est lié directement à un référentiel, car il effectue uniquement des tâches d’accès aux données de base. Dans le didacticiel suivant, vous allez créer une classe de logique métier avec une logique de validation et lier le contrôle `ObjectDataSource` à cette classe au lieu de la classe de référentiel. Vous allez également créer des tests unitaires pour la logique de validation. Dans le troisième didacticiel de cette série, vous allez ajouter des fonctionnalités de tri et de filtrage à l’application.

Les pages que vous créez dans ce didacticiel fonctionnent avec le jeu d’entités `Departments` du modèle de données que vous avez créé dans le [prise en main série de didacticiels](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Mise à jour de la base de données et du modèle de données

Vous allez commencer ce didacticiel en apportant deux modifications à la base de données, qui nécessitent toutes deux des modifications correspondantes au modèle de données que vous avez créé dans le [prise en main avec les didacticiels Entity Framework et Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) . Dans l’un de ces didacticiels, vous avez apporté manuellement des modifications au concepteur pour synchroniser le modèle de données avec la base de données après une modification de la base de données. Dans ce didacticiel, vous allez utiliser l’outil de **mise à jour du modèle de base** de données du concepteur pour mettre à jour le modèle de données automatiquement.

### <a name="adding-a-relationship-to-the-database"></a>Ajout d’une relation à la base de données

Dans Visual Studio, ouvrez l’application Web Contoso University que vous avez créée dans le [prise en main à l’aide de la](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série de didacticiels Entity Framework et Web Forms, puis ouvrez le schéma de base de données `SchoolDiagram`.

Si vous examinez la table `Department` dans le schéma de base de données, vous verrez qu’elle contient une colonne `Administrator`. Cette colonne est une clé étrangère de la table `Person`, mais aucune relation de clé étrangère n’est définie dans la base de données. Vous devez créer la relation et mettre à jour le modèle de données afin que le Entity Framework puisse gérer automatiquement cette relation.

Dans le schéma de base de données, cliquez avec le bouton droit sur la table `Department`, puis sélectionnez **relations**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

Dans la zone **relations de clé étrangère** , cliquez sur **Ajouter**, puis sur les points de suspension pour la **spécification de tables et de colonnes**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

Dans la boîte de dialogue **tables et colonnes** , définissez la table et le champ de clé primaire sur `Person` et `PersonID`, puis définissez la table et le champ de clé étrangère sur `Department` et `Administrator`. (Dans ce cas, le nom de la relation passe de `FK_Department_Department` à `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Cliquez sur **OK** dans la zone **tables et colonnes** , cliquez sur **Fermer** dans la zone **relations de clé étrangère** , puis enregistrez les modifications. Si vous êtes invité à enregistrer les `Person` et les tables `Department`, cliquez sur **Oui**.

> [!NOTE]
> Si vous avez supprimé `Person` lignes qui correspondent à des données qui se trouvent déjà dans la colonne `Administrator`, vous ne pourrez pas enregistrer cette modification. Dans ce cas, utilisez l’éditeur de table dans **Explorateur de serveurs** pour vous assurer que la valeur `Administrator` de chaque ligne de `Department` contient l’ID d’un enregistrement qui existe réellement dans la table `Person`.
> 
> Une fois que vous avez enregistré la modification, vous ne pouvez pas supprimer une ligne de la table `Person` si cette personne est un administrateur de service. Dans une application de production, vous devez fournir un message d’erreur spécifique lorsqu’une contrainte de base de données empêche une suppression ou si vous spécifiez une suppression en cascade. Pour obtenir un exemple de la façon de spécifier une suppression en cascade, consultez [les Entity Framework et ASP.net – prise en main Partie 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).

### <a name="adding-a-view-to-the-database"></a>Ajout d’une vue à la base de données

Dans la nouvelle page *Departments. aspx* que vous allez créer, vous souhaitez fournir une liste déroulante d’instructeurs, dont les noms sont au format « Last, First », afin que les utilisateurs puissent sélectionner des administrateurs de service. Pour faciliter cette opération, vous allez créer une vue dans la base de données. La vue se compose uniquement des données requises par la liste déroulante : le nom complet (correctement mis en forme) et la clé d’enregistrement.

Dans **Explorateur de serveurs**, développez *School. mdf*, cliquez avec le bouton droit sur le dossier **views** , puis sélectionnez **Ajouter une nouvelle vue**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Cliquez sur **Fermer** lorsque la boîte de dialogue **Ajouter une table** s’affiche et collez l’instruction SQL suivante dans le volet SQL :

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Enregistrez la vue en tant que `vInstructorName`.

### <a name="updating-the-data-model"></a>Mise à jour du modèle de données

Dans le dossier *dal* , ouvrez le fichier *SchoolModel. edmx* , cliquez avec le bouton droit sur l’aire de conception, puis sélectionnez **mettre à jour le modèle à partir de la base de données**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

Dans la boîte **de dialogue choisir vos objets de base de données** , sélectionnez l’onglet **Ajouter** et sélectionnez la vue que vous venez de créer.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Cliquez sur **Finish**.

Dans le concepteur, vous voyez que l’outil a créé une entité `vInstructorName` et une nouvelle association entre les entités `Department` et `Person`.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> Dans les fenêtres **sortie** et **liste d’erreurs** , vous pouvez voir un message d’avertissement vous informant que l’outil a créé automatiquement une clé primaire pour la nouvelle vue de `vInstructorName`. Ce comportement est normal.

Quand vous faites référence à la nouvelle entité `vInstructorName` dans le code, vous ne souhaitez pas utiliser la Convention de base de données de préfixe de « v » en minuscules. Par conséquent, vous allez renommer l’entité et le jeu d’entités dans le modèle.

Ouvrez l' **Explorateur de modèles**. Vous voyez `vInstructorName` listé comme un type d’entité et une vue.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

Sous **SchoolModel** (non **SchoolModel. Store**), cliquez avec le bouton droit sur **vInstructorName** et sélectionnez **Propriétés**. Dans la fenêtre **Propriétés** , modifiez la propriété **nom** en « InstructorName » et définissez la propriété nom du **jeu d’entités** sur « InstructorNames ».

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Enregistrez et fermez le modèle de données, puis régénérez le projet.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Utilisation d’une classe de référentiel et d’un contrôle ObjectDataSource

Créez un nouveau fichier de classe dans le dossier *dal* , nommez-le *SchoolRepository.cs*et remplacez le code existant par le code suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Ce code fournit une seule méthode `GetDepartments` qui retourne toutes les entités du jeu d’entités `Departments`. Comme vous savez que vous allez accéder à la propriété de navigation `Person` pour chaque ligne retournée, vous spécifiez un chargement hâtif pour cette propriété à l’aide de la méthode `Include`. La classe implémente également l’interface `IDisposable` pour garantir la libération de la connexion de base de données lorsque l’objet est supprimé.

> [!NOTE]
> Une pratique courante consiste à créer une classe de référentiel pour chaque type d’entité. Dans ce didacticiel, vous utilisez une classe de référentiel pour plusieurs types d’entités. Pour plus d’informations sur le modèle de référentiel, consultez les publications sur le blog de l' [équipe Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) et le [blog de Julie Lerman](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).

La méthode `GetDepartments` retourne un objet `IEnumerable` plutôt qu’un objet `IQueryable` afin de garantir que la collection retournée est utilisable même après la suppression de l’objet de référentiel. Un objet `IQueryable` peut provoquer l’accès à la base de données à chaque fois qu’il est utilisé, mais l’objet de l’espace de stockage peut être supprimé au moment où un contrôle DataBound tente d’afficher les données. Vous pouvez retourner un autre type de collection, tel qu’un objet `IList` au lieu d’un objet `IEnumerable`. Toutefois, le retour d’un objet `IEnumerable` garantit que vous pouvez effectuer des tâches de traitement de liste en lecture seule typiques, telles que des boucles `foreach` et des requêtes LINQ, mais vous ne pouvez pas ajouter ou supprimer des éléments dans la collection, ce qui peut impliquer que ces modifications sont conservées dans la base de données.

Créez une page *Departments. aspx* qui utilise la page maître *site. Master* , puis ajoutez le balisage suivant dans le contrôle `Content` nommé `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Ce balisage crée un contrôle `ObjectDataSource` qui utilise la classe de référentiel que vous venez de créer, et un contrôle `GridView` pour afficher les données. Le contrôle `GridView` spécifie des commandes **modifier** et **supprimer** , mais vous n’avez pas encore ajouté de code pour les prendre en charge.

Plusieurs colonnes utilisent des contrôles `DynamicField` afin que vous puissiez tirer parti de la fonctionnalité de validation et de mise en forme automatique des données. Pour que cela fonctionne, vous devez appeler la méthode `EnableDynamicData` dans le gestionnaire d’événements `Page_Init`. (les contrôles`DynamicControl` ne sont pas utilisés dans le champ `Administrator`, car ils ne fonctionnent pas avec les propriétés de navigation.)

Les attributs `Vertical-Align="Top"` deviennent importants ultérieurement lorsque vous ajoutez une colonne qui a un contrôle `GridView` imbriqué à la grille.

Ouvrez le fichier *Departments.aspx.cs* et ajoutez l’instruction `using` suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Ajoutez ensuite le gestionnaire suivant pour l’événement `Init` de la page :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

Dans le dossier *dal* , créez un fichier de classe nommé *Department.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Ce code ajoute des métadonnées au modèle de données. Il spécifie que la propriété `Budget` de l’entité `Department` représente en réalité la devise, bien que son type de données soit `Decimal`et qu’elle spécifie que la valeur doit être comprise entre 0 et $1 000 000,00. Il spécifie également que la propriété `StartDate` doit être mise en forme en tant que date au format mm/jj/aaaa.

Exécutez la page *Departments. aspx* .

[![image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Notez que même si vous n’avez pas spécifié de chaîne de format dans le balisage de page *Departments. aspx* pour les colonnes de **Date de début** ou de **budget** , la devise par défaut et la mise en forme des dates ont été appliquées par les contrôles `DynamicField`, à l’aide des métadonnées que vous avez fournies dans le fichier *Department.cs* .

## <a name="adding-insert-and-delete-functionality"></a>Ajout de la fonctionnalité d’insertion et de suppression

Ouvrez *SchoolRepository.cs*, ajoutez le code suivant afin de créer une méthode `Insert` et une méthode `Delete`. Le code comprend également une méthode nommée `GenerateDepartmentID` qui calcule la valeur de clé d’enregistrement disponible suivante pour une utilisation par la méthode `Insert`. Cela est nécessaire, car la base de données n’est pas configurée pour calculer cette valeur automatiquement pour la table `Department`.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Méthode Attach

La méthode `DeleteDepartment` appelle la méthode `Attach` afin de rétablir le lien maintenu dans le gestionnaire d’état d’objet du contexte de l’objet entre l’entité en mémoire et la ligne de base de données qu’elle représente. Cela doit se produire avant que la méthode appelle la méthode `SaveChanges`.

Le terme *contexte* de l’objet fait référence à la classe Entity Framework qui dérive de la classe `ObjectContext` que vous utilisez pour accéder à vos jeux d’entités et entités. Dans le code de ce projet, la classe est nommée `SchoolEntities`et une instance de celle-ci est toujours nommée `context`. Le *Gestionnaire d’état d’objet* du contexte de l’objet est une classe qui dérive de la classe `ObjectStateManager`. Le contact de l’objet utilise le gestionnaire d’état d’objet pour stocker les objets d’entité et effectuer le suivi de la synchronisation de chacun d’entre eux avec la ou les lignes de table correspondantes dans la base de données.

Lorsque vous lisez une entité, le contexte de l’objet la stocke dans le gestionnaire d’état d’objet et assure le suivi de la synchronisation de cette représentation de l’objet avec la base de données. Par exemple, si vous modifiez une valeur de propriété, un indicateur est défini pour indiquer que la propriété que vous avez modifiée n’est plus synchronisée avec la base de données. Ensuite, lorsque vous appelez la méthode `SaveChanges`, le contexte de l’objet sait quoi faire dans la base de données, car le gestionnaire d’état d’objet sait exactement ce qui est différent de l’état actuel de l’entité et de l’état de la base de données.

Toutefois, ce processus ne fonctionne généralement pas dans une application Web, car l’instance de contexte de l’objet qui lit une entité, ainsi que tout ce qui se trouve dans son gestionnaire d’état d’objet, est supprimée après le rendu d’une page. L’instance de contexte de l’objet qui doit appliquer les modifications est une nouvelle instance qui est instanciée pour le traitement de la publication (postback). Dans le cas de la méthode `DeleteDepartment`, le `ObjectDataSource` contrôle recrée la version d’origine de l’entité pour vous à partir de valeurs dans l’état d’affichage, mais cette entité `Department` recréée n’existe pas dans le gestionnaire d’état d’objet. Si vous avez appelé la méthode `DeleteObject` sur cette entité recréée, l’appel échoue parce que le contexte de l’objet ne sait pas si l’entité est synchronisée avec la base de données. Toutefois, l’appel de la méthode `Attach` rétablit le même suivi entre l’entité recréée et les valeurs de la base de données qui a été initialement effectuée automatiquement lors de la lecture de l’entité dans une instance antérieure du contexte de l’objet.

Il peut arriver que vous ne souhaitiez pas que le contexte de l’objet effectue le suivi des entités dans le gestionnaire d’état d’objet, et vous pouvez définir des indicateurs pour empêcher l’exécution de ce dernier. Des exemples sont présentés dans les didacticiels ultérieurs de cette série.

### <a name="the-savechanges-method"></a>Méthode SaveChanges

Cette classe de référentiel simple illustre les principes de base de l’exécution d’opérations CRUD. Dans cet exemple, la méthode `SaveChanges` est appelée immédiatement après chaque mise à jour. Dans une application de production, vous souhaiterez peut-être appeler la méthode `SaveChanges` à partir d’une méthode distincte pour mieux contrôler le moment où la base de données est mise à jour. (À la fin du didacticiel suivant, vous trouverez un lien vers un livre blanc qui aborde le modèle d’unité de travail qui constitue une approche pour coordonner les mises à jour associées.) Notez également que dans l’exemple, la méthode `DeleteDepartment` n’inclut pas de code pour gérer les conflits d’accès concurrentiel. le code à cette fin sera ajouté dans un didacticiel ultérieur de cette série.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Récupération des noms de formateur à sélectionner lors de l’insertion

Les utilisateurs doivent être en mesure de sélectionner un administrateur dans une liste déroulante lors de la création de nouveaux services. Par conséquent, ajoutez le code suivant à *SchoolRepository.cs* pour créer une méthode permettant de récupérer la liste des instructeurs à l’aide de la vue que vous avez créée précédemment :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Création d’une page pour l’insertion de services

Créez une page *DepartmentsAdd. aspx* qui utilise la page *site. Master* , puis ajoutez le balisage suivant dans le contrôle `Content` nommé `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Ce balisage crée deux contrôles `ObjectDataSource`, un pour insérer de nouvelles entités `Department` et une pour récupérer les noms des enseignants pour le contrôle `DropDownList` utilisé pour la sélection des administrateurs de service. Le balisage crée un contrôle de `DetailsView` pour l’entrée de nouveaux services, et spécifie un gestionnaire pour l’événement `ItemInserting` du contrôle afin que vous puissiez définir la valeur de clé étrangère `Administrator`. À la fin, est un contrôle de `ValidationSummary` pour afficher les messages d’erreur.

Ouvrez *DepartmentsAdd.aspx.cs* et ajoutez l’instruction `using` suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Ajoutez la variable de classe et les méthodes suivantes :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

La méthode `Page_Init` active les fonctionnalités Dynamic Data. Le gestionnaire de l’événement `Init` du contrôle `DropDownList` enregistre une référence au contrôle, et le gestionnaire de l’événement `Inserting` du contrôle `DetailsView` utilise cette référence pour obtenir la valeur `PersonID` de l’instructeur sélectionné et mettre à jour la propriété de clé étrangère `Administrator` de l’entité `Department`.

Exécutez la page, ajoutez des informations pour un nouveau service, puis cliquez sur le lien **Insérer** .

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Entrez les valeurs d’un autre nouveau service. Entrez un nombre supérieur à 1 000 000,00 dans le champ **budget** et l’onglet suivant. Un astérisque apparaît dans le champ et, si vous maintenez le pointeur de la souris sur celui-ci, vous pouvez voir le message d’erreur que vous avez entré dans les métadonnées de ce champ.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Cliquez sur **Insérer**pour afficher le message d’erreur affiché par le contrôle `ValidationSummary` au bas de la page.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Fermez ensuite le navigateur et ouvrez la page *Departments. aspx* . Ajoutez la fonctionnalité de suppression à la page *Departments. aspx* en ajoutant un attribut `DeleteMethod` au contrôle `ObjectDataSource` et un attribut `DataKeyNames` au contrôle `GridView`. Les balises d’ouverture de ces contrôles ressemblent maintenant à l’exemple suivant :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Exécutez la page.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Supprimez le service que vous avez ajouté lors de l’exécution de la page *DepartmentsAdd. aspx* .

## <a name="adding-update-functionality"></a>Ajout de la fonctionnalité de mise à jour

Ouvrez *SchoolRepository.cs* et ajoutez la méthode `Update` suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Quand vous cliquez sur **mettre à jour** dans la page *services. aspx* , le contrôle `ObjectDataSource` crée deux entités `Department` à passer à la méthode `UpdateDepartment`. L’un contient les valeurs d’origine qui ont été stockées dans l’état d’affichage, tandis que l’autre contient les nouvelles valeurs qui ont été entrées dans le contrôle `GridView`. Le code de la méthode `UpdateDepartment` passe l’entité `Department` qui contient les valeurs d’origine à la méthode `Attach` afin d’établir le suivi entre l’entité et ce qui se trouve dans la base de données. Ensuite, le code passe l’entité `Department` qui a les nouvelles valeurs à la méthode `ApplyCurrentValues`. Le contexte de l’objet compare les valeurs anciennes et nouvelles. Si une nouvelle valeur est différente d’une ancienne valeur, le contexte de l’objet modifie la valeur de la propriété. La méthode `SaveChanges` met alors à jour uniquement les colonnes modifiées dans la base de données. (Toutefois, si la fonction de mise à jour de cette entité a été mappée à une procédure stockée, la ligne entière serait mise à jour indépendamment des colonnes qui ont été modifiées.)

Ouvrez le fichier *Departments. aspx* et ajoutez les attributs suivants au contrôle `DepartmentsObjectDataSource` :

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Cela entraîne le stockage des anciennes valeurs dans l’état d’affichage afin qu’elles puissent être comparées aux nouvelles valeurs de la méthode `Update`.
- `OldValuesParameterFormatString="orig{0}"`   
 Cela indique au contrôle que le nom du paramètre de valeurs d’origine est `origDepartment`.

Le balisage de la balise d’ouverture du contrôle `ObjectDataSource` ressemble maintenant à l’exemple suivant :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Ajoutez un attribut `OnRowUpdating="DepartmentsGridView_RowUpdating"` au contrôle `GridView`. Vous allez l’utiliser pour définir la valeur de la propriété `Administrator` en fonction de la ligne sélectionnée par l’utilisateur dans une liste déroulante. La balise d’ouverture `GridView` ressemble maintenant à l’exemple suivant :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Ajoutez un contrôle `EditItemTemplate` pour la colonne `Administrator` au contrôle `GridView`, immédiatement après le contrôle `ItemTemplate` pour cette colonne :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Ce contrôle de `EditItemTemplate` est similaire au contrôle `InsertItemTemplate` dans la page *DepartmentsAdd. aspx* . La différence réside dans le fait que la valeur initiale du contrôle est définie à l’aide de l’attribut `SelectedValue`.

Avant le contrôle `GridView`, ajoutez un contrôle `ValidationSummary` comme vous l’avez fait dans la page *DepartmentsAdd. aspx* .

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Ouvrez *Departments.aspx.cs* et immédiatement après la déclaration de classe partielle, ajoutez le code suivant pour créer un champ privé afin de référencer le contrôle `DropDownList` :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Ajoutez ensuite des gestionnaires pour l’événement `Init` du contrôle `DropDownList` et l’événement `RowUpdating` du contrôle `GridView` :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Le gestionnaire de l’événement `Init` enregistre une référence au contrôle `DropDownList` dans le champ de classe. Le gestionnaire de l’événement `RowUpdating` utilise la référence pour obtenir la valeur entrée par l’utilisateur et l’appliquer à la propriété `Administrator` de l’entité `Department`.

Utilisez la page *DepartmentsAdd. aspx* pour ajouter un nouveau service, puis exécutez la page *Departments. aspx* , puis cliquez sur **modifier** sur la ligne que vous avez ajoutée.

> [!NOTE]
> Vous ne pouvez pas modifier les lignes que vous n’avez pas ajoutées (c’est-à-dire qui se trouvent déjà dans la base de données) en raison de données non valides dans la base de données ; les administrateurs des lignes créées avec la base de données sont les élèves. Si vous essayez de modifier l’une d’entre elles, vous obtiendrez une page d’erreur qui signale une erreur telle que `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`

[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Si vous entrez un montant de **budget** non valide, puis cliquez sur **mettre à jour**, vous voyez le même astérisque et le même message d’erreur que ceux que vous avez vus dans la page *Departments. aspx* .

Modifiez une valeur de champ ou sélectionnez un autre administrateur, puis cliquez sur **mettre à jour**. La modification s’affiche.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Cela termine la présentation de l’utilisation du contrôle `ObjectDataSource` pour les opérations CRUD (création, lecture, mise à jour, suppression) de base avec le Entity Framework. Vous avez créé une application multiniveau simple, mais la couche logique métier est toujours étroitement couplée à la couche d’accès aux données, ce qui complique les tests unitaires automatisés. Dans le didacticiel suivant, vous allez apprendre à implémenter le modèle de référentiel pour faciliter les tests unitaires.

> [!div class="step-by-step"]
> [Next](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
