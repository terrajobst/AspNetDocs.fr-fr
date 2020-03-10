---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: Accès aux données de votre modèle à partir d’un contrôleur (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 37f45d8f12e3ab5c485718bcf2c59934ad272118
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615254"
---
# <a name="accessing-your-models-data-from-a-controller-vb"></a>Accès aux données de votre modèle à partir d’un contrôleur (VB)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :
> 
> - [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez C#, passez à la [ C# version](../cs/accessing-your-models-data-from-a-controller.md) de ce didacticiel.

Dans cette section, vous allez créer une classe de `MoviesController` et écrire du code qui récupère les données de film et les affiche dans le navigateur à l’aide d’un modèle de vue. Veillez à créer votre application avant de continuer.

Cliquez avec le bouton droit sur le dossier *Controllers* et créez un contrôleur de `MoviesController`. Sélectionnez les options suivantes :

- Nom du contrôleur : **MoviesController**. (Il s'agit du paramètre par défaut.)
- Modèle : **contrôleur avec des actions et des vues en lecture/écriture, à l’aide de Entity Framework**.
- Classe de modèle : **Movie (MvcMovie. Models)** .
- Classe de contexte de données : **MovieDBContext (MvcMovie. Models)** .
- Vues : **Razor (cshtml)** . (Valeur par défaut).

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

Cliquez sur **Ajouter**. Visual Web Developer crée les fichiers et dossiers suivants :

- *Un fichier MoviesController. vb* dans le dossier *Controllers* du projet.
- Un dossier *movies* dans le dossier *views* du projet.
- *Créez. vbhtml, DELETE. vbhtml, Details. vbhtml, Edit. vbhtml*et *index. vbhtml* dans le nouveau dossier *Views\Movies* .

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

Le mécanisme de génération de modèles automatique ASP.NET MVC 3 a créé automatiquement les méthodes et les vues d’action CRUD (créer, lire, mettre à jour et supprimer). Vous disposez maintenant d’une application Web entièrement fonctionnelle qui vous permet de créer, de répertorier, de modifier et de supprimer des entrées de film.

Exécutez l’application et accédez au contrôleur `Movies` en ajoutant */movies* à l’URL dans la barre d’adresses de votre navigateur. Étant donné que l’application s’appuie sur le routage par défaut (défini dans le fichier *global. asax* ), la demande de navigateur `http://localhost:xxxxx/Movies` est routée vers la méthode d’action de `Index` par défaut du contrôleur `Movies`. En d’autres termes, la demande de navigateur `http://localhost:xxxxx/Movies` est identique à celle de la demande du navigateur `http://localhost:xxxxx/Movies/Index`. Le résultat est une liste vide de films, car vous n’en avez pas encore ajouté.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Création d’un film

Sélectionnez le lien **Créer nouveau**. Entrez des détails sur un film, puis cliquez sur le bouton **créer** .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Si vous cliquez sur le bouton **créer** , le formulaire est publié sur le serveur, où les informations sur le film sont enregistrées dans la base de données. Vous êtes ensuite redirigé vers l’URL */movies* , où vous pouvez voir le film nouvellement créé dans la liste.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Créez deux ou trois autres entrées. Essayez les liens **Edit**, **Details** et **Delete**, qui sont tous opérationnels.

## <a name="examining-the-generated-code"></a>Examen du code généré

Ouvrez le fichier *Controllers\MoviesController.vb* et examinez la méthode `Index` générée. Une partie du contrôleur Movie avec la méthode `Index` est illustrée ci-dessous.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

La ligne suivante de la classe `MoviesController` instancie un contexte de base de données de films, comme décrit précédemment. Vous pouvez utiliser le contexte de la base de données de films pour interroger, modifier et supprimer des films.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Une demande au contrôleur `Movies` retourne toutes les entrées de la table `Movies` de la base de données Movie, puis passe les résultats à la vue `Index`.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modèles fortement typés et le mot clé @model

Plus haut dans ce didacticiel, vous avez vu comment un contrôleur peut passer des données ou des objets à un modèle de vue à l’aide de l’objet `ViewBag`. Le `ViewBag` est un objet dynamique qui fournit un moyen pratique à liaison tardive pour passer des informations à une vue.

ASP.NET MVC offre également la possibilité de passer des données ou des objets fortement typés à un modèle de vue. Cette approche fortement typée permet une meilleure vérification au moment de la compilation de votre code et de la fonctionnalité IntelliSense plus riche dans l’éditeur de Visual Web Developer. Nous utilisons cette approche avec la classe `MoviesController` et le modèle de vue *index. vbhtml* .

Notez que le code crée un objet [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) lorsqu’il appelle la méthode d’assistance `View` dans la méthode d’action `Index`. Le code transmet ensuite cette liste de `Movies` du contrôleur à la vue :

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

En incluant une instruction `@ModelType` en haut du fichier de modèle de vue, vous pouvez spécifier le type d’objet attendu par la vue. Lorsque vous avez créé le contrôleur de films, Visual Web Developer a inclus automatiquement l’instruction `@model` suivante en haut du fichier *index. vbhtml* :

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

Cette `@ModelType` directive vous permet d’accéder à la liste des films que le contrôleur a transmis à la vue à l’aide d’un objet `Model` fortement typé. Par exemple, dans le modèle *index. vbhtml* , le code parcourt les films en exécutant une instruction `foreach` sur l’objet `Model` fortement typé :

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Étant donné que l’objet `Model` est fortement typé (en tant qu’objet `IEnumerable<Movie>`), chaque objet `item` de la boucle est de type `Movie`. Entre autres avantages, cela signifie que vous bénéficiez d’une vérification au moment de la compilation du code et de la prise en charge complète d’IntelliSense dans l’éditeur de code :

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Utilisation de SQL Server Compact

Entity Framework Code First a détecté que la chaîne de connexion de base de données qui a été fournie pointait vers une base de données `Movies` qui n’existait pas encore, donc Code First créé la base de données automatiquement. Vous pouvez vérifier qu’il a été créé en examinant le dossier de données de l' *application\_* . Si vous ne voyez pas le fichier *movies. sdf* , cliquez sur le bouton **Afficher tous les fichiers** dans la barre d’outils **Explorateur de solutions** , cliquez sur le bouton **Actualiser** , puis développez le dossier données de l' *application\_* .

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Double-cliquez sur *movies. sdf* pour ouvrir **Explorateur de serveurs**. Développez ensuite le dossier **tables** pour afficher les tables qui ont été créées dans la base de données.

> [!NOTE]
> Si vous recevez une erreur lorsque vous double-cliquez sur *movies. sdf*, vérifiez que vous avez installé **Visual Studio 2010 SP1 Tools pour SQL Server Compact 4,0**. (Pour obtenir des liens vers le logiciel, consultez la liste des conditions préalables dans la partie 1 de cette série de didacticiels.) Si vous installez la version maintenant, vous devrez fermer et rouvrir Visual Web Developer.

[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

Il existe deux tables, une pour le jeu d’entités `Movie`, puis la table `EdmMetadata`. La table `EdmMetadata` est utilisée par le Entity Framework pour déterminer quand le modèle et la base de données ne sont pas synchronisés.

Cliquez avec le bouton droit sur la table `Movies`, puis sélectionnez **afficher les données** de la table pour afficher les données que vous avez créées.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Cliquez avec le bouton droit sur la table `Movies` et sélectionnez **modifier le schéma**de la table.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Notez que le schéma de la table `Movies` est mappé à la classe `Movie` que vous avez créée précédemment. Entity Framework Code First créé automatiquement ce schéma pour vous en fonction de votre classe `Movie`.

Lorsque vous avez terminé, fermez la connexion. (Si vous ne fermez pas la connexion, vous risquez de recevoir une erreur la prochaine fois que vous exécuterez le projet).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

Vous disposez maintenant de la base de données et d’une page de liste simple pour afficher le contenu à partir de celle-ci. Dans le didacticiel suivant, nous allons examiner le reste du code de génération de modèles automatique et ajouter une méthode de `SearchIndex` et une vue de `SearchIndex` qui vous permet de rechercher des films dans cette base de données.

> [!div class="step-by-step"]
> [Précédent](adding-a-model.md)
> [Suivant](examining-the-edit-methods-and-edit-view.md)
