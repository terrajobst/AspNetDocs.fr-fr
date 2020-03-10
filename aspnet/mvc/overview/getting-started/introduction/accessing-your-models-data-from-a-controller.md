---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Accès aux données de votre modèle à partir d’un contrôleur | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 5d882d765133d32d3acdba9ffb5d43b69119a273
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615919"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Accès aux données de votre modèle à partir d’un contrôleur

par [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

Dans cette section, vous allez créer une classe de `MoviesController` et écrire du code qui récupère les données de film et les affiche dans le navigateur à l’aide d’un modèle de vue.

**Générez l’application** avant de passer à l’étape suivante. Si vous ne générez pas l’application, vous obtiendrez une erreur lors de l’ajout d’un contrôleur.

Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Controllers* , puis cliquez sur **Ajouter**, puis sur **contrôleur**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

Dans la boîte de dialogue **Ajouter une structure** , cliquez sur **contrôleur MVC 5 avec affichages, à l’aide de Entity Framework**, puis cliquez sur **Ajouter**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Sélectionnez **Movie (MvcMovie. Models)** pour la classe de modèle.
- Sélectionnez **MovieDBContext (MvcMovie. Models)** pour la classe de contexte de données.
- Pour le nom du contrôleur, entrez **MoviesController**.

  L’image ci-dessous montre la boîte de dialogue terminé.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Cliquez sur **Ajouter**. (Si vous recevez une erreur, vous n’avez probablement pas généré l’application avant de commencer à ajouter le contrôleur.) Visual Studio crée les fichiers et dossiers suivants :

- *Un fichier MoviesController.cs* dans le dossier *Controllers* .
- Un dossier *Views\Movies* .
- *Créez. cshtml, DELETE. cshtml, Details. cshtml, Edit. cshtml*et *index. cshtml* dans le nouveau dossier *Views\Movies* .

Visual Studio a créé automatiquement les méthodes d’action et les vues [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (créer, lire, mettre à jour et supprimer) pour vous (la création automatique de méthodes et de vues d’action CRUD porte le nom de génération de modèles automatique). Vous disposez maintenant d’une application Web entièrement fonctionnelle qui vous permet de créer, de répertorier, de modifier et de supprimer des entrées de film.

Exécutez l’application et cliquez sur le lien de la **vidéo MVC** (ou accédez au contrôleur `Movies` en ajoutant */movies* à l’URL dans la barre d’adresses de votre navigateur). Étant donné que l’application s’appuie sur le routage par défaut (défini dans le fichier *App\_Start\RouteConfig.cs* ), la demande de navigateur `http://localhost:xxxxx/Movies` est routée vers la méthode d’action par défaut `Index` du contrôleur de `Movies`. En d’autres termes, la demande de navigateur `http://localhost:xxxxx/Movies` est identique à celle de la demande du navigateur `http://localhost:xxxxx/Movies/Index`. Le résultat est une liste vide de films, car vous n’en avez pas encore ajouté.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Création d’un film

Sélectionnez le lien **Créer nouveau**. Entrez des détails sur un film, puis cliquez sur le bouton **créer** .

![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Vous ne pourrez peut-être pas entrer des virgules ou des virgules dans le champ Price. pour prendre en charge la validation jQuery pour les paramètres régionaux autres que l’anglais qui utilisent une virgule (&quot;,&quot;) pour une virgule décimale et des formats de date autres que l’anglais des États-Unis, vous devez inclure *global. js* et votre fichier *cultures/globaliser. cultures. js* spécifique (à partir de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) et JavaScript pour utiliser `Globalize.parseFloat`. Je vais vous montrer comment procéder dans le didacticiel suivant. Pour le moment, entrez simplement des nombres entiers tels que 10.

Si vous cliquez sur le bouton **créer** , le formulaire est publié sur le serveur, où les informations sur le film sont enregistrées dans la base de données. Vous êtes ensuite redirigé vers l’URL */movies* , où vous pouvez voir le film nouvellement créé dans la liste.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Créez deux ou trois autres entrées. Essayez les liens **Edit**, **Details** et **Delete**, qui sont tous opérationnels.

## <a name="examining-the-generated-code"></a>Examen du code généré

Ouvrez le fichier *Controllers\MoviesController.cs* et examinez la méthode `Index` générée. Une partie du contrôleur Movie avec la méthode `Index` est illustrée ci-dessous.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Une demande au contrôleur `Movies` retourne toutes les entrées de la table `Movies` puis passe les résultats à la vue `Index`. La ligne suivante de la classe `MoviesController` instancie un contexte de base de données de films, comme décrit précédemment. Vous pouvez utiliser le contexte de la base de données de films pour interroger, modifier et supprimer des films.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modèles fortement typés et le mot clé @model

Plus haut dans ce didacticiel, vous avez vu comment un contrôleur peut passer des données ou des objets à un modèle de vue à l’aide de l’objet `ViewBag`. Le `ViewBag` est un objet dynamique qui fournit un moyen pratique à liaison tardive pour passer des informations à une vue.

MVC offre également la possibilité de passer des objets *fortement* typés à un modèle de vue. Cette approche fortement typée permet une meilleure vérification au moment de la compilation de votre code et de la [fonctionnalité IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) plus riche dans l’éditeur Visual Studio. Le mécanisme de génération de modèles automatique dans Visual Studio a utilisé cette approche (c’est-à-dire passer un modèle *fortement* typé) avec la classe `MoviesController` et les modèles de vue lorsqu’elle a créé les méthodes et les vues.

Dans le fichier *Controllers\MoviesController.cs* , examinez la méthode `Details` générée. La méthode `Details` est illustrée ci-dessous.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Le paramètre `id` est généralement passé en tant que données d’itinéraire, par exemple `http://localhost:1234/movies/details/1` définit le contrôleur sur le contrôleur Movie, l’action à `details` et le `id` sur 1. Vous pouvez également transmettre l’ID avec une chaîne de requête comme suit :

`http://localhost:1234/movies/details?id=1`

Si un `Movie` est trouvé, une instance du modèle `Movie` est passée à la vue `Details` :

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Examinez le contenu du fichier *Views\Movies\Details.cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

En incluant une instruction `@model` en haut du fichier de modèle de vue, vous pouvez spécifier le type d’objet attendu par la vue. Quand vous avez créé le contrôleur pour les films, Visual Studio a inclus automatiquement l’instruction `@model` suivante en haut du fichier *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Cette directive `@model` vous permet d’accéder au film que le contrôleur a passé à la vue en utilisant un objet `Model` qui est fortement typé. Par exemple, dans le modèle *Details. cshtml* , le code passe chaque champ Movie à la `DisplayNameFor` et [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) les applications auxiliaires HTML avec l’objet `Model` fortement typé. Les méthodes `Create` et `Edit` et les modèles de vue transmettent également un objet de modèle de film.

Examinez le modèle de vue *index. cshtml* et la méthode `Index` dans le fichier *MoviesController.cs* . Notez que le code crée un objet [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) lorsqu’il appelle la méthode d’assistance `View` dans la méthode d’action `Index`. Le code transmet ensuite cette liste de `Movies` de la méthode d’action `Index` à la vue :

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Lorsque vous avez créé le contrôleur de film, Visual Studio inclut automatiquement l’instruction `@model` suivante en haut du fichier *index. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Cette `@model` directive vous permet d’accéder à la liste des films que le contrôleur a transmis à la vue à l’aide d’un objet `Model` fortement typé. Par exemple, dans le modèle *index. cshtml* , le code parcourt les films en exécutant une instruction `foreach` sur l’objet `Model` fortement typé :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Étant donné que l’objet `Model` est fortement typé (en tant qu’objet `IEnumerable<Movie>`), chaque objet `item` de la boucle est de type `Movie`. Entre autres avantages, cela signifie que vous bénéficiez d’une vérification au moment de la compilation du code et de la prise en charge complète d’IntelliSense dans l’éditeur de code :

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Utilisation de SQL Server LocalDB

Entity Framework Code First a détecté que la chaîne de connexion de base de données qui a été fournie pointait vers une base de données `Movies` qui n’existait pas encore, donc Code First créé la base de données automatiquement. Vous pouvez vérifier qu’il a été créé en examinant le dossier de données de l' *application\_* . Si vous ne voyez pas le fichier *movies. mdf* , cliquez sur le bouton **Afficher tous les fichiers** dans la barre d’outils **Explorateur de solutions** , cliquez sur le bouton **Actualiser** , puis développez le dossier données de l' *application\_* .

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Double-cliquez sur *movies. mdf* pour ouvrir l' **Explorateur de serveurs**, puis développez le dossier **tables** pour afficher le tableau films. Notez l’icône de clé en regard de ID. Par défaut, EF fait d’une propriété nommée ID la clé primaire. Pour plus d’informations sur EF et MVC, consultez le excellent didacticiel de Tom Dykstra sur [MVC et EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Cliquez avec le bouton droit sur la table `Movies`, puis sélectionnez **afficher les données** de la table pour afficher les données que vous avez créées.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Cliquez avec le bouton droit sur la table `Movies` et sélectionnez **ouvrir la définition de table** pour afficher la structure de la table que Entity Framework code First créé pour vous.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Notez que le schéma de la table `Movies` est mappé à la classe `Movie` que vous avez créée précédemment. Entity Framework Code First créé automatiquement ce schéma pour vous en fonction de votre classe `Movie`.

Lorsque vous avez terminé, fermez la connexion en cliquant avec le bouton droit sur *MovieDBContext* et en sélectionnant **Fermer la connexion**. (Si vous ne fermez pas la connexion, vous risquez de recevoir une erreur la prochaine fois que vous exécuterez le projet).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Vous disposez maintenant d’une base de données et de pages pour afficher, modifier, mettre à jour et supprimer les données. Dans le didacticiel suivant, nous allons examiner le reste du code de génération de modèles automatique et ajouter une méthode de `SearchIndex` et une vue de `SearchIndex` qui vous permet de rechercher des films dans cette base de données. Pour plus d’informations sur l’utilisation de Entity Framework avec MVC, consultez [création d’un modèle de données Entity Framework pour une Application ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Précédent](creating-a-connection-string.md)
> [Suivant](examining-the-edit-methods-and-edit-view.md)
