---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Ajout d’un nouveau champ à la table et au modèleC#Movie () | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 40b02a2f608f07091ce6b5339688a1e6290e2e37
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540802"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Ajout d’un nouveau champ à la table et au modèle Movie (C#)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../../getting-started/introduction/getting-started.md) et utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sûr, bien plus simple à suivre et qui illustre davantage de fonctionnalités.
> 
> 
> Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :
> 
> - [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet Visual Web Developer C# avec le code source est disponible pour accompagner cette rubrique. [Téléchargez la C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez Visual Basic, passez à la [version Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de ce didacticiel.

Dans cette section, vous allez apporter des modifications aux classes de modèle et découvrir comment vous pouvez mettre à jour le schéma de base de données pour qu’il corresponde aux modifications du modèle.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Ajout d’une propriété Rating au modèle Movie

Commencez par ajouter une nouvelle propriété `Rating` à la classe `Movie` existante. Ouvrez le fichier *Movie.cs* et ajoutez la propriété `Rating` comme celle-ci :

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

La classe `Movie` complète ressemble maintenant au code suivant :

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Recompilez l’application à l’aide de la commande de menu **Déboguer** &gt;**générer un film** .

Maintenant que vous avez mis à jour la classe `Model`, vous devez également mettre à jour les modèles de vue *\Views\Movies\Index.cshtml* et *\Views\Movies\Create.cshtml* afin de prendre en charge la nouvelle propriété `Rating`.

Ouvrez le fichier *\Views\Movies\Index.cshtml* et ajoutez un en-tête de colonne `<th>Rating</th>` juste après la colonne **Price** . Ajoutez ensuite une `<td>` colonne près de la fin du modèle pour afficher la valeur de `@item.Rating`. Voici à quoi ressemble le modèle de vue *index. cshtml* mis à jour :

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

Ensuite, ouvrez le fichier *\Views\Movies\Create.cshtml* et ajoutez le balisage suivant à la fin du formulaire. Cela génère le rendu d’une zone de texte afin que vous puissiez spécifier une évaluation lors de la création d’un nouveau film.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Gestion des différences de schéma de base de données et de modèle

Vous avez maintenant mis à jour le code de l’application pour prendre en charge la nouvelle propriété `Rating`.

Exécutez maintenant l’application et accédez à l’URL */movies* . Lorsque vous procédez ainsi, l’erreur suivante s’affiche :

![](adding-a-new-field/_static/image1.png)

Vous voyez cette erreur, car la classe de modèle `Movie` mise à jour dans l’application est maintenant différente du schéma de la table `Movie` de la base de données existante. (Il n’existe pas de colonne `Rating` dans la table de base de données.)

Par défaut, lorsque vous utilisez Entity Framework Code First pour créer automatiquement une base de données, comme vous l’avez fait précédemment dans ce didacticiel, Code First ajoute une table à la base de données pour vous aider à déterminer si le schéma de la base de données est synchronisé avec les classes de modèle à partir desquelles il a été généré. S’ils ne sont pas synchronisés, le Entity Framework génère une erreur. Cela facilite le suivi des problèmes au moment du développement, que vous ne pouvez trouver dans le cas contraire (par des erreurs obscures) au moment de l’exécution. La fonctionnalité de vérification de la synchronisation permet d’afficher le message d’erreur que vous venez de constater.

Il existe deux approches pour résoudre l’erreur :

1. Laissez Entity Framework supprimer et recréer automatiquement la base de données sur la base du nouveau schéma de classe de modèle. Cette approche est très pratique lors du développement actif sur une base de données de test, car elle vous permet de faire évoluer rapidement le schéma de base de données et le modèle. L’inconvénient, cependant, est que vous perdez les données existantes dans la base de données, de sorte que vous *ne souhaitez pas* utiliser cette approche sur une base de données de production.
2. Modifiez explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle. L’avantage de cette approche est que vous conservez vos données. Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.

Pour ce didacticiel, nous allons utiliser la première approche : vous aurez la Entity Framework Code First recréer automatiquement la base de données chaque fois que le modèle sera modifié.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Recréation automatique de la base de données lors des modifications du modèle

Nous allons mettre à jour l’application afin que Code First supprime et recrée automatiquement la base de données chaque fois que vous modifiez le modèle de l’application.

> [!NOTE] 
> 
> **Avertissement** Vous devez activer cette méthode de suppression et de recréation automatique de la base de données uniquement lorsque vous utilisez une base de données de développement ou de test, et *jamais* sur une base de données de production qui contient des données réelles. Son utilisation sur un serveur de production peut entraîner une perte de données.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *modèles* , sélectionnez **Ajouter**, puis **classe**.

![](adding-a-new-field/_static/image2.png)

Nommez la classe « MovieInitializer ». Mettez à jour la classe `MovieInitializer` pour qu’elle contienne le code suivant :

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

La classe `MovieInitializer` spécifie que la base de données utilisée par le modèle doit être supprimée et automatiquement recréée si les classes de modèle changent. Le code comprend une méthode `Seed` pour spécifier des données par défaut à ajouter automatiquement à la base de données à chaque fois qu’il est créé (ou recréé). Cela permet de remplir la base de données avec des exemples de données, sans que vous ayez besoin de la remplir manuellement chaque fois que vous apportez une modification au modèle.

Maintenant que vous avez défini la classe `MovieInitializer`, vous devez la connecter afin que chaque fois que l’application s’exécute, elle vérifie si les classes de modèle sont différentes du schéma dans la base de données. Si c’est le cas, vous pouvez exécuter l’initialiseur pour recréer la base de données afin qu’elle corresponde au modèle, puis remplir la base de données avec les exemples de données.

Ouvrez le fichier *global. asax* situé à la racine du projet `MvcMovies` :

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

Le fichier *global. asax* contient la classe qui définit l’application entière pour le projet et contient une `Application_Start` gestionnaire d’événements qui s’exécute lorsque l’application démarre pour la première fois.

Nous allons ajouter deux instructions using en haut du fichier. Le premier fait référence à l’espace de noms Entity Framework, tandis que le second référence l’espace de noms dans lequel notre classe `MovieInitializer` vit :

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

Recherchez ensuite la méthode `Application_Start` et ajoutez un appel à `Database.SetInitializer` au début de la méthode, comme indiqué ci-dessous :

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

L’instruction `Database.SetInitializer` que vous venez d’ajouter indique que la base de données utilisée par l’instance `MovieDBContext` doit être automatiquement supprimée et recréée si le schéma et la base de données ne correspondent pas. Comme vous l’avez vu, il remplit également la base de données avec les exemples de données qui sont spécifiés dans la classe `MovieInitializer`.

Fermez le fichier *global. asax* .

Réexécutez l’application et accédez à l’URL */movies* . Lorsque l’application démarre, elle détecte que la structure de modèle ne correspond plus au schéma de base de données. Il recrée automatiquement la base de données pour qu’elle corresponde à la nouvelle structure de modèle et remplit la base de données avec les exemples de films :

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Cliquez sur le lien **Create New** pour ajouter un nouveau film. Notez que vous pouvez ajouter une évaluation.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

Cliquez sur **Créer**. Le nouveau film, y compris l’évaluation, s’affiche désormais dans la liste des films :

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

Dans cette section, vous avez vu comment vous pouvez modifier des objets de modèle et maintenir la synchronisation de la base de données avec les modifications. Vous avez également appris à remplir une base de données nouvellement créée avec des exemples de données afin de pouvoir essayer des scénarios. Voyons ensuite comment vous pouvez ajouter une logique de validation plus riche aux classes de modèle et permettre l’application de certaines règles d’entreprise.

> [!div class="step-by-step"]
> [Précédent](examining-the-edit-methods-and-edit-view.md)
> [Suivant](adding-validation-to-the-model.md)
