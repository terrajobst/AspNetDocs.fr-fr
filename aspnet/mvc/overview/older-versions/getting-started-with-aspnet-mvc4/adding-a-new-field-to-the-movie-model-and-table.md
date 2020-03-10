---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Ajout d’un nouveau champ au modèle de film et à la table | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : une version mise à jour de ce didacticiel est disponible ici et utilise ASP.NET MVC 5 et Visual Studio 2013. C’est plus sécurisé, bien plus simple à suivre et à faire une démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d966b95163f64b20a17d2327a12c5d6c44a4a66b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615086"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table"></a>Ajout d’un nouveau champ à la table et au modèle Movie

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) et utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sûr, bien plus simple à suivre et qui illustre davantage de fonctionnalités.

Dans cette section, vous allez utiliser Migrations Entity Framework Code First pour migrer des modifications apportées aux classes de modèle afin que la modification soit appliquée à la base de données.

Par défaut, lorsque vous utilisez Entity Framework Code First pour créer automatiquement une base de données, comme vous l’avez fait précédemment dans ce didacticiel, Code First ajoute une table à la base de données pour vous aider à déterminer si le schéma de la base de données est synchronisé avec les classes de modèle à partir desquelles il a été généré. S’ils ne sont pas synchronisés, le Entity Framework génère une erreur. Cela facilite le suivi des problèmes au moment du développement, que vous ne pouvez trouver dans le cas contraire (par des erreurs obscures) au moment de l’exécution.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuration de Migrations Code First pour les modifications de modèle

Si vous utilisez Visual Studio 2012, double-cliquez sur le fichier *movies. mdf* à partir de Explorateur de solutions pour ouvrir l’outil de base de données. Visual Studio Express pour le Web affiche explorateur de base de données, Visual Studio 2012 affiche Explorateur de serveurs. Si vous utilisez Visual Studio 2010, utilisez Explorateur d’objets SQL Server.

Dans l’outil de base de données (explorateur de base de données, Explorateur de serveurs ou Explorateur d’objets SQL Server), cliquez avec le bouton droit sur `MovieDBContext`, puis sélectionnez **supprimer** pour supprimer la base de données de films.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Revenez à Explorateur de solutions. Cliquez avec le bouton droit sur le fichier *movies. mdf* , puis sélectionnez **supprimer** pour supprimer la base de données films.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Générez l’application pour vous assurer qu’aucune erreur n’est détectée.

Dans le menu **Outils**, cliquez sur **Gestionnaire de package NuGet**, puis sur **Console du Gestionnaire de package**.

![Ajouter un homme à en-tête pack](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

Dans la fenêtre **console du gestionnaire de package** , à l’invite de `PM>`, entrez « Enable-migrations-ContextTypeName MvcMovie. Models. MovieDBContext ».

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

La commande **Enable-migrations** (illustrée ci-dessus) crée un fichier *Configuration.cs* dans un nouveau dossier *migrations* .

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio ouvre le fichier *Configuration.cs* . Remplacez la méthode `Seed` dans le fichier *Configuration.cs* par le code suivant :

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Cliquez avec le bouton droit sur la ligne ondulée rouge sous `Movie` puis sélectionnez **résoudre** , puis **Utilisez** **MvcMovie. Models.**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

En procédant ainsi, vous ajoutez l’instruction using suivante :

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Migrations Code First appelle la méthode `Seed` après chaque migration (autrement dit, en appelant **Update-Database** dans la console du gestionnaire de package) et cette méthode met à jour les lignes qui ont déjà été insérées, ou les insère si elles n’existent pas encore.

**Appuyez sur Ctrl-Shift-B pour générer le projet.** (Les étapes suivantes échouent si votre version n’est pas générée à ce stade.)

L’étape suivante consiste à créer une classe de `DbMigration` pour la migration initiale. Cette migration vers crée une nouvelle base de données, c’est pourquoi vous avez supprimé le fichier *Movie. mdf* à l’étape précédente.

Dans la fenêtre **console du gestionnaire de package** , entrez la commande « Add-migration initial » pour créer la migration initiale. Le nom « initial » est arbitraire et est utilisé pour nommer le fichier de migration créé.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migrations Code First crée un autre fichier de classe dans le dossier *migrations* (portant le nom *{date}\_initial.cs* ), et cette classe contient le code qui crée le schéma de la base de données. Le nom du fichier de migration est précédé d’un horodateur pour faciliter le classement. Examinez le fichier *{Date}\_initial.cs* , qui contient les instructions pour créer le tableau films pour la base de données Movie. Lorsque vous mettez à jour la base de données dans les instructions ci-dessous, ce fichier *{Date}\_initial.cs* s’exécute et crée le schéma de base de données. La méthode **Seed** s’exécutera ensuite pour remplir la base de données avec les données de test.

Dans la **console du gestionnaire de package**, entrez la commande Update-Database pour créer la base de données et exécuter la méthode **Seed** .

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Si vous recevez une erreur indiquant qu’une table existe déjà et qu’elle ne peut pas être créée, cela est probablement dû au fait que vous avez exécuté l’application après avoir supprimé la base de données et avant d’avoir exécuté `update-database`. Dans ce cas, supprimez de nouveau le fichier *movies. mdf* et relancez la commande `update-database`. Si vous recevez toujours une erreur, supprimez le dossier et le contenu des migrations, puis commencez par suivre les instructions en haut de cette page (effacez le fichier *movies. mdf* , puis passez à activer-migrations).

Exécutez l’application et accédez à l’URL */movies* . Les données de départ sont affichées.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Ajout d’une propriété Rating au modèle Movie

Commencez par ajouter une nouvelle propriété `Rating` à la classe `Movie` existante. Ouvrez le fichier *Models\Movie.cs* et ajoutez la propriété `Rating` comme celle-ci :

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

La classe `Movie` complète ressemble maintenant au code suivant :

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Générez l’application à l’aide de la commande de menu **générer** &gt;**générer un film** ou en appuyant sur Ctrl-Shift-B.

Maintenant que vous avez mis à jour la classe `Model`, vous devez également mettre à jour les modèles de vue *\Views\Movies\Index.cshtml* et *\Views\Movies\Create.cshtml* pour afficher la nouvelle propriété `Rating` dans la vue du navigateur.

Ouvrez le fichier<em>\Views\Movies\Index.cshtml</em> et ajoutez un en-tête de colonne `<th>Rating</th>` juste après la colonne <strong>Price</strong> . Ajoutez ensuite une `<td>` colonne près de la fin du modèle pour afficher la valeur de `@item.Rating`. Voici à quoi ressemble le modèle de vue <em>index. cshtml</em> mis à jour :

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Ensuite, ouvrez le fichier *\Views\Movies\Create.cshtml* et ajoutez le balisage suivant à la fin du formulaire. Cela génère le rendu d’une zone de texte afin que vous puissiez spécifier une évaluation lors de la création d’un nouveau film.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Vous avez maintenant mis à jour le code de l’application pour prendre en charge la nouvelle propriété `Rating`.

Exécutez maintenant l’application et accédez à l’URL */movies* . Lorsque vous procédez ainsi, l’une des erreurs suivantes s’affiche :

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Vous voyez cette erreur, car la classe de modèle `Movie` mise à jour dans l’application est maintenant différente du schéma de la table `Movie` de la base de données existante. (Il n’existe pas de colonne `Rating` dans la table de base de données.)

Plusieurs approches sont possibles pour résoudre l’erreur :

1. Laissez Entity Framework supprimer et recréer automatiquement la base de données sur la base du nouveau schéma de classe de modèle. Cette approche est très pratique lors du développement actif sur une base de données de test. elle vous permet de faire évoluer rapidement le schéma de base de données et le modèle. L’inconvénient, cependant, est que vous perdez les données existantes dans la base de données, de sorte que vous *ne souhaitez pas* utiliser cette approche sur une base de données de production. L’utilisation d’un initialiseur pour amorcer automatiquement une base de données avec des données de test est souvent un moyen productif de développer une application. Pour plus d’informations sur les initialiseurs de base de données Entity Framework, consultez le [didacticiel ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)de Tom Dykstra.
2. Modifiez explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle. L’avantage de cette approche est que vous conservez vos données. Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.
3. Utilisez Migrations Code First pour mettre à jour le schéma de base de données.

Pour ce didacticiel, nous allons utiliser les migrations Code First.

Mettez à jour la méthode Seed afin qu’elle fournisse une valeur pour la nouvelle colonne. Ouvrez le fichier Migrations\Configuration.cs et ajoutez un champ Rating à chaque objet film.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Générez la solution, puis ouvrez la fenêtre de **console du gestionnaire de package** et entrez la commande suivante :

`add-migration AddRatingMig`

La commande `add-migration` indique à l’infrastructure de migration d’examiner le modèle de film actuel avec le schéma de la base de code de la vidéo actuelle et de créer le code nécessaire pour migrer la base de code vers le nouveau modèle. Le AddRatingMig est arbitraire et est utilisé pour nommer le fichier de migration. Il est utile d’utiliser un nom explicite pour l’étape de migration.

Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMigration` classe dérivée, et dans la méthode `Up` vous pouvez voir le code qui crée la nouvelle colonne.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Générez la solution, puis entrez la commande « Update-Database » dans la fenêtre de **console du gestionnaire de package** .

L’illustration suivante montre la sortie dans la fenêtre de la **console du gestionnaire de package** (le datage en préAddRatingMig de date est différent.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Réexécutez l’application et accédez à l’URL/movies. Vous pouvez voir le nouveau champ évaluation.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Cliquez sur le lien **Create New** pour ajouter un nouveau film. Notez que vous pouvez ajouter une évaluation.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Cliquez sur **Créer**. Le nouveau film, y compris l’évaluation, s’affiche désormais dans la liste des films :

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Vous devez également ajouter le champ `Rating` aux modèles de vue Edit, Details et SearchIndex.

Vous pouvez entrer à nouveau la commande « Update-Database » dans la fenêtre de la **console du gestionnaire de package** et aucune modification n’est apportée, car le schéma correspond au modèle.

Dans cette section, vous avez vu comment vous pouvez modifier des objets de modèle et maintenir la synchronisation de la base de données avec les modifications. Vous avez également appris à remplir une base de données nouvellement créée avec des exemples de données afin de pouvoir essayer des scénarios. Voyons ensuite comment vous pouvez ajouter une logique de validation plus riche aux classes de modèle et permettre l’application de certaines règles d’entreprise.

> [!div class="step-by-step"]
> [Précédent](examining-the-edit-methods-and-edit-view.md)
> [Suivant](adding-validation-to-the-model.md)
