---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Ajout d’un nouveau champ | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5974e53e4610dccc7812df261dc97a9b0327de85
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582907"
---
# <a name="adding-a-new-field"></a>Ajout d’un nouveau champ

par [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

Dans cette section, vous allez utiliser Migrations Entity Framework Code First pour migrer des modifications apportées aux classes de modèle afin que la modification soit appliquée à la base de données.

Par défaut, lorsque vous utilisez Entity Framework Code First pour créer automatiquement une base de données, comme vous l’avez fait précédemment dans ce didacticiel, Code First ajoute une table à la base de données pour vous aider à déterminer si le schéma de la base de données est synchronisé avec les classes de modèle à partir desquelles il a été généré. S’ils ne sont pas synchronisés, le Entity Framework génère une erreur. Cela facilite le suivi des problèmes au moment du développement, que vous ne pouvez trouver dans le cas contraire (par des erreurs obscures) au moment de l’exécution.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuration de Migrations Code First pour les modifications de modèle

Accédez à Explorateur de solutions. Cliquez avec le bouton droit sur le fichier *movies. mdf* , puis sélectionnez **supprimer** pour supprimer la base de données films. Si vous ne voyez pas le fichier *movies. mdf* , cliquez sur l’icône **Afficher tous les fichiers** présentée ci-dessous dans le cadre rouge.

![](adding-a-new-field/_static/image1.png)

Générez l’application pour vous assurer qu’aucune erreur n’est détectée.

Dans le menu **Outils**, cliquez sur **Gestionnaire de package NuGet**, puis sur **Console du Gestionnaire de package**.

![Ajouter un homme à en-tête pack](adding-a-new-field/_static/image2.png)

Dans la fenêtre **console du gestionnaire de package** à l’invite de `PM>`, entrez

Activer-migrations-ContextTypeName MvcMovie. Models. MovieDBContext

![](adding-a-new-field/_static/image3.png)

La commande **Enable-migrations** (illustrée ci-dessus) crée un fichier *Configuration.cs* dans un nouveau dossier *migrations* .

![](adding-a-new-field/_static/image4.png)

Visual Studio ouvre le fichier *Configuration.cs* . Remplacez la méthode `Seed` dans le fichier *Configuration.cs* par le code suivant :

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Pointez sur la ligne ondulée rouge sous `Movie`, puis cliquez sur `Show Potential Fixes` puis sur **using** **MvcMovie. Models.**

![](adding-a-new-field/_static/image5.png)

En procédant ainsi, vous ajoutez l’instruction using suivante :

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Migrations Code First appelle la méthode `Seed` après chaque migration (autrement dit, en appelant **Update-Database** dans la console du gestionnaire de package) et cette méthode met à jour les lignes qui ont déjà été insérées, ou les insère si elles n’existent pas encore.
> 
> La méthode [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) dans le code suivant effectue une opération « upsert » :
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Étant donné que la méthode [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) s’exécute avec chaque migration, vous ne pouvez pas simplement insérer des données, car les lignes que vous essayez d’ajouter sont déjà présentes après la première migration qui crée la base de données. L’opération «[upsert](http://en.wikipedia.org/wiki/Upsert)» empêche les erreurs qui se produisent si vous essayez d’insérer une ligne qui existe déjà, mais elle remplace toutes les modifications apportées aux données que vous avez pu effectuer lors du test de l’application. Avec les données de test dans certaines tables, vous pouvez ne pas souhaiter que cela se produise : dans certains cas, lorsque vous modifiez des données pendant un test, vous souhaitez conserver les modifications après les mises à jour de la base de données. Dans ce cas, vous devez effectuer une opération d’insertion conditionnelle : Insérez une ligne uniquement si elle n’existe pas déjà.   
> 
> Le premier paramètre passé à la méthode [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) spécifie la propriété à utiliser pour vérifier si une ligne existe déjà. Pour les données de test de film que vous fournissez, la propriété `Title` peut être utilisée à cet effet puisque chaque titre de la liste est unique :
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Ce code suppose que les titres sont uniques. Si vous ajoutez manuellement un titre en double, vous obtenez l’exception suivante la prochaine fois que vous effectuez une migration.   
> 
> *La séquence contient plusieurs éléments*  
> 
> Pour plus d’informations sur la méthode [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) , consultez se [préoccuper de la méthode EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/).

**Appuyez sur Ctrl-Shift-B pour générer le projet.** (Les étapes suivantes échouent si vous n’effectuez pas de génération à ce stade.)

L’étape suivante consiste à créer une classe de `DbMigration` pour la migration initiale. Cette migration crée une nouvelle base de données, c’est pourquoi vous avez supprimé le fichier *Movie. mdf* à l’étape précédente.

Dans la fenêtre **console du gestionnaire de package** , entrez la commande `add-migration Initial` pour créer la migration initiale. Le nom « initial » est arbitraire et est utilisé pour nommer le fichier de migration créé.

![](adding-a-new-field/_static/image6.png)

Migrations Code First crée un autre fichier de classe dans le dossier *migrations* (portant le nom *{date}\_initial.cs* ), et cette classe contient le code qui crée le schéma de la base de données. Le nom du fichier de migration est précédé d’un horodateur pour faciliter le classement. Examinez le fichier *{Date}\_initial.cs* , qui contient les instructions permettant de créer la table `Movies` pour la base de données Movie. Lorsque vous mettez à jour la base de données dans les instructions ci-dessous, ce fichier *{Date}\_initial.cs* s’exécute et crée le schéma de base de données. La méthode **Seed** s’exécutera ensuite pour remplir la base de données avec les données de test.

Dans la **console du gestionnaire de package**, entrez la commande `update-database` pour créer la base de données et exécuter la méthode `Seed`.

![](adding-a-new-field/_static/image7.png)

Si vous recevez une erreur indiquant qu’une table existe déjà et qu’elle ne peut pas être créée, cela est probablement dû au fait que vous avez exécuté l’application après avoir supprimé la base de données et avant d’avoir exécuté `update-database`. Dans ce cas, supprimez de nouveau le fichier *movies. mdf* et relancez la commande `update-database`. Si vous recevez toujours une erreur, supprimez le dossier et le contenu des migrations, puis commencez par suivre les instructions en haut de cette page (effacez le fichier *movies. mdf* , puis passez à activer-migrations). Si vous recevez toujours une erreur, ouvrez Explorateur d’objets SQL Server et supprimez la base de données de la liste.

Exécutez l’application et accédez à l’URL */movies* . Les données de départ sont affichées.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Ajout d’une propriété Rating au modèle Movie

Commencez par ajouter une nouvelle propriété `Rating` à la classe `Movie` existante. Ouvrez le fichier *Models\Movie.cs* et ajoutez la propriété `Rating` comme celle-ci :

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

La classe `Movie` complète ressemble maintenant au code suivant :

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Générez l’application (Ctrl + Maj + B).

Étant donné que vous avez ajouté un nouveau champ à la classe `Movie`, vous devez également mettre à jour la *liste* verte de liaison pour inclure cette nouvelle propriété. Mettez à jour l’attribut `bind` pour `Create` et `Edit` méthodes d’action pour inclure la propriété `Rating` :

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Vous devez aussi mettre à jour les modèles de vue pour afficher, créer et modifier la nouvelle propriété `Rating` dans la vue du navigateur.

Ouvrez le fichier *\Views\Movies\Index.cshtml* et ajoutez un en-tête de colonne `<th>Rating</th>` juste après la colonne **Price** . Ajoutez ensuite une `<td>` colonne près de la fin du modèle pour afficher la valeur de `@item.Rating`. Voici à quoi ressemble le modèle de vue *index. cshtml* mis à jour :

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Ensuite, ouvrez le fichier *\Views\Movies\Create.cshtml* et ajoutez le champ `Rating` avec le balisage en surbrillance suivant. Cela génère le rendu d’une zone de texte afin que vous puissiez spécifier une évaluation lors de la création d’un nouveau film.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Vous avez maintenant mis à jour le code de l’application pour prendre en charge la nouvelle propriété `Rating`.

Exécutez l’application et accédez à l’URL */movies* . Lorsque vous procédez ainsi, l’une des erreurs suivantes s’affiche :

![](adding-a-new-field/_static/image9.png)  
  
Le modèle sauvegardant le contexte’MovieDBContext’a changé depuis la création de la base de données. Envisagez d’utiliser Migrations Code First pour mettre à jour la base de données (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Vous voyez cette erreur, car la classe de modèle `Movie` mise à jour dans l’application est maintenant différente du schéma de la table `Movie` de la base de données existante. (Il n’existe pas de colonne `Rating` dans la table de base de données.)

Plusieurs approches sont possibles pour résoudre l’erreur :

1. Laissez Entity Framework supprimer et recréer automatiquement la base de données sur la base du nouveau schéma de classe de modèle. Cette approche est très utile au début du cycle de développement quand vous effectuez un développement actif sur une base de données de test. Elle permet de faire évoluer rapidement le schéma de modèle et de base de données ensemble. L’inconvénient, cependant, est que vous perdez les données existantes dans la base de données, de sorte que vous *ne souhaitez pas* utiliser cette approche sur une base de données de production. L’utilisation d’un initialiseur pour amorcer automatiquement une base de données avec des données de test est souvent un moyen efficace pour développer une application. Pour plus d’informations sur les initialiseurs de base de données Entity Framework, consultez le [didacticiel ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modifiez explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle. L’avantage de cette approche est que vous conservez vos données. Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.
3. Utilisez Migrations Code First pour mettre à jour le schéma de base de données.

Pour ce didacticiel, nous allons utiliser les migrations Code First.

Mettez à jour la méthode Seed afin qu’elle fournisse une valeur pour la nouvelle colonne. Ouvrez le fichier Migrations\Configuration.cs et ajoutez un champ Rating à chaque objet film.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Générez la solution, puis ouvrez la fenêtre de **console du gestionnaire de package** et entrez la commande suivante :

`add-migration Rating`

La commande `add-migration` indique à l’infrastructure de migration d’examiner le modèle de film actuel avec le schéma de la base de code de la vidéo actuelle et de créer le code nécessaire pour migrer la base de code vers le nouveau modèle. L' *évaluation* du nom est arbitraire et sert à nommer le fichier de migration. Il est utile d’utiliser un nom explicite pour l’étape de migration.

Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMigration` classe dérivée, et dans la méthode `Up` vous pouvez voir le code qui crée la nouvelle colonne.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Générez la solution, puis entrez la commande `update-database` dans la fenêtre **console du gestionnaire de package** .

L’illustration suivante montre la sortie dans la fenêtre de la **console du gestionnaire de package** (l' *évaluation* de l’horodatage en attente sera différente.)

![](adding-a-new-field/_static/image11.png)

Réexécutez l’application et accédez à l’URL/movies. Vous pouvez voir le nouveau champ évaluation.

![](adding-a-new-field/_static/image12.png)

Cliquez sur le lien **Create New** pour ajouter un nouveau film. Notez que vous pouvez ajouter une évaluation.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Cliquez sur **Créer**. Le nouveau film, y compris l’évaluation, s’affiche désormais dans la liste des films :

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Maintenant que le projet utilise des migrations, vous n’avez pas besoin de supprimer la base de données lorsque vous ajoutez un nouveau champ ou que vous mettez à jour le schéma. Dans la section suivante, nous allons apporter davantage de modifications au schéma et utiliser les migrations pour mettre à jour la base de données.

Vous devez également ajouter le champ `Rating` aux modèles de vue Edit, Details et DELETE.

Vous pouvez entrer à nouveau la commande « Update-Database » dans la fenêtre de **console du gestionnaire de package** et aucun code de migration n’est exécuté, car le schéma correspond au modèle. Toutefois, l’exécution de « Update-Database » réexécute la méthode `Seed` et, si vous avez modifié l’une des données de départ, les modifications sont perdues, car la méthode `Seed` Upserts les données. Pour plus d’informations sur la méthode `Seed`, consultez le [didacticiel ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)populaire de Tom Dykstra.

Dans cette section, vous avez vu comment vous pouvez modifier des objets de modèle et maintenir la synchronisation de la base de données avec les modifications. Vous avez également appris à remplir une base de données nouvellement créée avec des exemples de données afin de pouvoir essayer des scénarios. Il s’agissait simplement d’une présentation rapide de Code First, consultez [création d’un modèle de données Entity Framework pour une Application ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) pour obtenir un didacticiel plus complet sur le sujet. Voyons ensuite comment vous pouvez ajouter une logique de validation plus riche aux classes de modèle et permettre l’application de certaines règles d’entreprise.

> [!div class="step-by-step"]
> [Précédent](adding-search.md)
> [Suivant](adding-validation.md)
