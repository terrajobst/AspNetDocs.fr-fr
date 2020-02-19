---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Accès aux données de votre modèle à partir d’un contrôleur | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : une version mise à jour de ce didacticiel est disponible ici et utilise ASP.NET MVC 5 et Visual Studio 2013. C’est plus sécurisé, bien plus simple à suivre et à faire une démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 7c4aa34567ac4fb31d1ed874cf65986c4e779e66
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456164"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Accès aux données de votre modèle à partir d’un contrôleur

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) et utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sûr, bien plus simple à suivre et qui illustre davantage de fonctionnalités.

Dans cette section, vous allez créer une classe de `MoviesController` et écrire du code qui récupère les données de film et les affiche dans le navigateur à l’aide d’un modèle de vue.

**Générez l’application** avant de passer à l’étape suivante.

Cliquez avec le bouton droit sur le dossier *Controllers* et créez un contrôleur de `MoviesController`. Les options ci-dessous n’apparaissent pas tant que vous ne générez pas votre application. Sélectionnez les options suivantes :

- Nom du contrôleur : **MoviesController**. (Il s’agit de la valeur par défaut. )
- Modèle : **contrôleur MVC avec des actions et des vues en lecture/écriture, à l’aide de Entity Framework**.
- Classe de modèle : **Movie (MvcMovie. Models)** .
- Classe de contexte de données : **MovieDBContext (MvcMovie. Models)** .
- Vues : **Razor (cshtml)** . (Valeur par défaut).

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Cliquez sur **Ajouter**. Visual Studio Express crée les fichiers et dossiers suivants :

- *Un fichier MoviesController.cs* dans le dossier *Controllers* du projet.
- Un dossier *movies* dans le dossier *views* du projet.
- *Créez. cshtml, DELETE. cshtml, Details. cshtml, Edit. cshtml*et *index. cshtml* dans le nouveau dossier *Views\Movies* .

ASP.NET MVC 4 a créé automatiquement les méthodes et les vues d’action CRUD (créer, lire, mettre à jour et supprimer) pour vous (la création automatique de méthodes et de vues d’action CRUD est appelée génération de modèles automatique). Vous disposez maintenant d’une application Web entièrement fonctionnelle qui vous permet de créer, de répertorier, de modifier et de supprimer des entrées de film.

Exécutez l’application et accédez au contrôleur `Movies` en ajoutant */movies* à l’URL dans la barre d’adresses de votre navigateur. Étant donné que l’application s’appuie sur le routage par défaut (défini dans le fichier *global. asax* ), la demande de navigateur `http://localhost:xxxxx/Movies` est routée vers la méthode d’action de `Index` par défaut du contrôleur `Movies`. En d’autres termes, la demande de navigateur `http://localhost:xxxxx/Movies` est identique à celle de la demande du navigateur `http://localhost:xxxxx/Movies/Index`. Le résultat est une liste vide de films, car vous n’en avez pas encore ajouté.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Création d’un film

Sélectionnez le lien **Créer nouveau**. Entrez des détails sur un film, puis cliquez sur le bouton **créer** .

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Si vous cliquez sur le bouton **créer** , le formulaire est publié sur le serveur, où les informations sur le film sont enregistrées dans la base de données. Vous êtes ensuite redirigé vers l’URL */movies* , où vous pouvez voir le film nouvellement créé dans la liste.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Créez deux ou trois autres entrées. Essayez les liens **Edit**, **Details** et **Delete**, qui sont tous opérationnels.

## <a name="examining-the-generated-code"></a>Examen du code généré

Ouvrez le fichier *Controllers\MoviesController.cs* et examinez la méthode `Index` générée. Une partie du contrôleur Movie avec la méthode `Index` est illustrée ci-dessous.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

La ligne suivante de la classe `MoviesController` instancie un contexte de base de données de films, comme décrit précédemment. Vous pouvez utiliser le contexte de la base de données de films pour interroger, modifier et supprimer des films.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Une demande au contrôleur `Movies` retourne toutes les entrées de la table `Movies` de la base de données Movie, puis passe les résultats à la vue `Index`.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modèles fortement typés et le mot clé @model

Plus haut dans ce didacticiel, vous avez vu comment un contrôleur peut passer des données ou des objets à un modèle de vue à l’aide de l’objet `ViewBag`. Le `ViewBag` est un objet dynamique qui fournit un moyen pratique à liaison tardive pour passer des informations à une vue.

ASP.NET MVC offre également la possibilité de passer des données ou des objets fortement typés à un modèle de vue. Cette approche fortement typée permet une meilleure vérification au moment de la compilation de votre code et de la fonctionnalité IntelliSense plus riche dans l’éditeur Visual Studio. Le mécanisme de génération de modèles automatique dans Visual Studio utilisait cette approche avec les modèles de classe et de vue de `MoviesController` lors de la création des méthodes et des vues.

Dans le fichier *Controllers\MoviesController.cs* , examinez la méthode `Details` générée. Une partie du contrôleur Movie avec la méthode `Details` est illustrée ci-dessous.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Si un `Movie` est trouvé, une instance du modèle de `Movie` est passée au mode Détails. Examinez le contenu du fichier *Views\Movies\Details.cshtml* .

En incluant une instruction `@model` en haut du fichier de modèle de vue, vous pouvez spécifier le type d’objet attendu par la vue. Quand vous avez créé le contrôleur pour les films, Visual Studio a inclus automatiquement l’instruction `@model` suivante en haut du fichier *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Cette directive `@model` vous permet d’accéder au film que le contrôleur a passé à la vue en utilisant un objet `Model` qui est fortement typé. Par exemple, dans le modèle *Details. cshtml* , le code passe chaque champ Movie à la `DisplayNameFor` et [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) les applications auxiliaires HTML avec l’objet `Model` fortement typé. Les méthodes Create et Edit et les modèles de vue transmettent également un objet de modèle de film.

Examinez le modèle de vue *index. cshtml* et la méthode `Index` dans le fichier *MoviesController.cs* . Notez que le code crée un objet [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) lorsqu’il appelle la méthode d’assistance `View` dans la méthode d’action `Index`. Le code transmet ensuite cette liste de `Movies` du contrôleur à la vue :

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Lorsque vous avez créé le contrôleur de film, Visual Studio Express inclus automatiquement l’instruction `@model` suivante en haut du fichier *index. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Cette `@model` directive vous permet d’accéder à la liste des films que le contrôleur a transmis à la vue à l’aide d’un objet `Model` fortement typé. Par exemple, dans le modèle *index. cshtml* , le code parcourt les films en exécutant une instruction `foreach` sur l’objet `Model` fortement typé :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Étant donné que l’objet `Model` est fortement typé (en tant qu’objet `IEnumerable<Movie>`), chaque objet `item` de la boucle est de type `Movie`. Entre autres avantages, cela signifie que vous bénéficiez d’une vérification au moment de la compilation du code et de la prise en charge complète d’IntelliSense dans l’éditeur de code :

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Utilisation de SQL Server LocalDB

Entity Framework Code First a détecté que la chaîne de connexion de base de données qui a été fournie pointait vers une base de données `Movies` qui n’existait pas encore, donc Code First créé la base de données automatiquement. Vous pouvez vérifier qu’il a été créé en examinant le dossier de données de l' *application\_* . Si vous ne voyez pas le fichier *movies. mdf* , cliquez sur le bouton **Afficher tous les fichiers** dans la barre d’outils **Explorateur de solutions** , cliquez sur le bouton **Actualiser** , puis développez le dossier données de l' *application\_* .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Double-cliquez sur *movies. mdf* pour ouvrir l' **Explorateur de bases de données**, puis développez le dossier **tables** pour afficher le tableau films.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Si l’Explorateur de bases de données n’apparaît pas, dans le menu **Outils** , sélectionnez **se connecter à la base de**données, puis annulez la boîte de dialogue **choisir la source de données** . Cette opération force l’ouverture de l’Explorateur de bases de données.

> [!NOTE]
> Si vous utilisez VWD existant ou Visual Studio 2010 et que vous recevez une erreur semblable à l’un des éléments suivants :
> 
> - La base de données «C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF ne peut pas être ouvert, car il s’agit de la version 706. Ce serveur prend en charge la version 655 et les versions antérieures. Un chemin de mise à niveau vers une version antérieure n'est pas pris en charge.
> - &quot;exception InvalidOperation n’était pas gérée par le code utilisateur&quot; le SqlConnection fourni ne spécifie pas de catalogue initial.
> 
> Vous devez installer la [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) [et la base de données](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)locale. Vérifiez la chaîne de connexion `MovieDBContext` spécifiée sur la page précédente.

Cliquez avec le bouton droit sur la table `Movies`, puis sélectionnez **afficher les données** de la table pour afficher les données que vous avez créées.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Cliquez avec le bouton droit sur la table `Movies` et sélectionnez **ouvrir la définition de table** pour afficher la structure de la table que Entity Framework code First créé pour vous.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Notez que le schéma de la table `Movies` est mappé à la classe `Movie` que vous avez créée précédemment. Entity Framework Code First créé automatiquement ce schéma pour vous en fonction de votre classe `Movie`.

Lorsque vous avez terminé, fermez la connexion en cliquant avec le bouton droit sur *MovieDBContext* et en sélectionnant **Fermer la connexion**. (Si vous ne fermez pas la connexion, vous risquez de recevoir une erreur la prochaine fois que vous exécuterez le projet).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Vous disposez maintenant de la base de données et d’une page de liste simple pour afficher le contenu à partir de celle-ci. Dans le didacticiel suivant, nous allons examiner le reste du code de génération de modèles automatique et ajouter une méthode de `SearchIndex` et une vue de `SearchIndex` qui vous permet de rechercher des films dans cette base de données.

> [!div class="step-by-step"]
> [Précédent](adding-a-model.md)
> [Suivant](examining-the-edit-methods-and-edit-view.md)
