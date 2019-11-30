---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: Création de classes de modèle avec LINQ to SQL (VB) | Microsoft Docs
author: microsoft
description: L’objectif de ce didacticiel est d’expliquer une méthode de création de classes de modèle pour une application MVC ASP.NET. Dans ce didacticiel, vous allez apprendre à générer le modèle c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 88a5f1037d93ef3bdc95bf60b6005ebb254ab440
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588624"
---
# <a name="creating-model-classes-with-linq-to-sql-vb"></a>Création de classes de modèle avec LINQ to SQL (VB)

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> L’objectif de ce didacticiel est d’expliquer une méthode de création de classes de modèle pour une application MVC ASP.NET. Dans ce didacticiel, vous allez apprendre à créer des classes de modèle et à effectuer l’accès à la base de données en tirant parti de Microsoft LINQ to SQL.

L’objectif de ce didacticiel est d’expliquer une méthode de création de classes de modèle pour une application MVC ASP.NET. Dans ce didacticiel, vous allez apprendre à créer des classes de modèle et à effectuer l’accès à la base de données en tirant parti de Microsoft LINQ to SQL.

Dans ce didacticiel, nous créons une application de base de données de vidéo de base. Nous commençons par créer l’application de base de données de films de la manière la plus rapide et la plus simple possible. Nous effectuons tous nos accès aux données directement à partir de nos actions de contrôleur.

Ensuite, vous allez apprendre à utiliser le modèle de référentiel. L’utilisation du modèle de référentiel nécessite un peu plus de travail. Toutefois, l’adoption de ce modèle présente l’avantage de vous permettre de créer des applications adaptables pour les modifier et pouvant être facilement testées.

## <a name="what-is-a-model-class"></a>Qu’est-ce qu’une classe de modèle ?

Un modèle MVC contient toute la logique d’application qui n’est pas contenue dans une vue MVC ou un contrôleur MVC. En particulier, un modèle MVC contient toutes les logiques d’accès aux données et à l’entreprise de votre application.

Vous pouvez utiliser diverses technologies pour implémenter votre logique d’accès aux données. Par exemple, vous pouvez créer vos classes d’accès aux données à l’aide des classes Microsoft Entity Framework, NHibernate, SubSonic ou ADO.NET.

Dans ce didacticiel, j’utilise LINQ to SQL pour interroger et mettre à jour la base de données. LINQ to SQL vous offre une méthode très simple d’interaction avec une base de données Microsoft SQL Server. Toutefois, il est important de comprendre que l’infrastructure MVC ASP.NET n’est pas liée à LINQ to SQL de quelque manière que ce soit. ASP.NET MVC est compatible avec toutes les technologies d’accès aux données.

## <a name="create-a-movie-database"></a>Créer une base de données de films

Dans ce didacticiel, afin d’illustrer comment vous pouvez créer des classes de modèle, nous créons une application de base de données de films simple. La première étape consiste à créer une nouvelle base de données. Cliquez avec le bouton droit sur le dossier de données de l’application\_dans la fenêtre Explorateur de solutions et sélectionnez l’option de menu **Ajouter, nouvel élément**. Sélectionnez le modèle de base de données SQL Server, donnez-lui le nom MoviesDB. mdf, puis cliquez sur le bouton **Ajouter** (voir figure 1).

[![de l’ajout d’une nouvelle base de données SQL Server](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Figure 01**: ajout d’une nouvelle base de données SQL Server ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))

Après avoir créé la base de données, vous pouvez ouvrir la base de données en double-cliquant sur le fichier MoviesDB. mdf dans le dossier de données de l’application\_. Double-cliquez sur le fichier MoviesDB. mdf pour ouvrir la fenêtre Explorateur de serveurs (voir figure 2).

|   | La fenêtre Explorateur de serveurs s’appelle la fenêtre de explorateur de base de données lors de l’utilisation de Visual Web Developer. |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![à l’aide de la fenêtre Explorateur de serveurs](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Figure 02**: utilisation de la fenêtre Explorateur de serveurs ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))

Nous devons ajouter une table à notre base de données qui représente nos films. Cliquez avec le bouton droit sur le dossier tables et sélectionnez l’option de menu **Ajouter une nouvelle table**. La sélection de cette option de menu ouvre le Concepteur de tables (voir figure 3).

[![à l’aide de la fenêtre Explorateur de serveurs](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Figure 03**: concepteur de tables ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))

Nous devons ajouter les colonnes suivantes à la table de base de données :

| **Nom de la colonne** | **Type de données** | **Autoriser les valeurs null** |
| --- | --- | --- |
| ID | int | False |
| Titre | Nvarchar (200) | False |
| MetaDirectory | Nvarchar (50) | False |

Vous devez effectuer deux opérations spéciales dans la colonne ID. Tout d’abord, vous devez marquer la colonne ID en tant que colonne de clé primaire en sélectionnant la colonne dans la Concepteur de tables et en cliquant sur l’icône d’une clé. LINQ to SQL vous oblige à spécifier vos colonnes de clé primaire lors de l’exécution d’insertions ou de mises à jour sur la base de données.

Ensuite, vous devez marquer la colonne ID en tant que colonne d’identité en affectant la valeur yes à la propriété **is Identity** (voir figure 3). Une colonne d’identité est une colonne à laquelle un nouveau numéro est affecté automatiquement chaque fois que vous ajoutez une nouvelle ligne de données à une table.

Une fois ces modifications effectuées, enregistrez la table sous le nom tblMovie. Vous pouvez enregistrer la table en cliquant sur le bouton enregistrer.

## <a name="create-linq-to-sql-classes"></a>Créer des classes de LINQ to SQL

Notre modèle MVC contient des classes LINQ to SQL qui représentent la table de base de données tblMovie. Le moyen le plus simple de créer ces LINQ to SQL des classes consiste à cliquer avec le bouton droit sur le dossier modèles, à sélectionner **Ajouter, nouvel élément**, à sélectionner le modèle LINQ to SQL classes, à attribuer aux classes le nom Movie. dbml, puis à cliquer sur le bouton **Ajouter** (voir figure 4).

[![de la création de classes LINQ to SQL](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Figure 04**: création de classes LINQ to SQL ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))

Immédiatement après avoir créé le film LINQ to SQL classes, le Concepteur Objet Relationnel s’affiche. Vous pouvez faire glisser des tables de base de données de la fenêtre Explorateur de serveurs sur le Concepteur Objet Relationnel pour créer des classes LINQ to SQL qui représentent des tables de base de données particulières. Nous devons ajouter la table de base de données tblMovie sur le Concepteur Objet Relationnel (voir figure 4).

[![à l’aide de la Concepteur Objet Relationnel](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Figure 05**: utilisation du concepteur objet relationnel ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))

Par défaut, le Concepteur Objet Relationnel crée une classe portant le même nom que la table de base de données que vous faites glisser sur le concepteur. Toutefois, nous ne souhaitons pas appeler notre classe tblMovie. Par conséquent, cliquez sur le nom de la classe dans le concepteur et remplacez le nom de la classe par Movie.

Enfin, n’oubliez pas de cliquer sur le bouton **Enregistrer** (l’image de la disquette) pour enregistrer les Classes de LINQ to SQL. Dans le cas contraire, les classes LINQ to SQL ne seront pas générées par le Concepteur Objet Relationnel.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Utilisation de LINQ to SQL dans une action de contrôleur

Maintenant que nous avons nos classes LINQ to SQL, nous pouvons utiliser ces classes pour récupérer des données de la base de données. Dans cette section, vous allez apprendre à utiliser des classes de LINQ to SQL directement dans une action de contrôleur. Nous allons afficher la liste des films à partir de la table de base de données tblMovies dans une vue MVC.

Tout d’abord, nous devons modifier la classe HomeController. Cette classe se trouve dans le dossier Controllers de votre application. Modifiez la classe pour qu’elle ressemble à la classe dans la liste 1.

**Liste 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

L’action index () dans la liste 1 utilise une LINQ to SQL classe DataContext (MovieDataContext) pour représenter la base de données MoviesDB. La classe MoveDataContext a été générée par l’Concepteur Objet Relationnel Visual Studio.

Une requête LINQ est exécutée sur le DataContext pour récupérer tous les films de la table de base de données tblMovies. La liste des films est assignée à une variable locale nommée movies. Enfin, la liste des films est transmise à la vue par le biais de données d’affichage.

Pour afficher les films, nous devons ensuite modifier la vue index. Vous pouvez trouver la vue index dans le dossier Views\Home\. Mettez à jour la vue index afin qu’elle ressemble à la vue dans la liste 2.

**Liste 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Notez que la vue d’index modifiée comprend une directive &lt;% @ Import namespace%&gt; en haut de la vue. Cette directive importe l’espace de noms MvcApplication1. Nous avons besoin de cet espace de noms pour pouvoir utiliser les classes de modèle, en particulier la classe Movie, dans la vue.

La vue dans la liste 2 contient une boucle for each qui itère au sein de tous les éléments représentés par la propriété ViewData. Model. La valeur de la propriété titre est affichée pour chaque film.

Notez que la valeur de la propriété ViewData. Model est castée en IEnumerable. Cela est nécessaire pour effectuer une boucle dans le contenu de ViewData. Model. Une autre option consiste à créer une vue fortement typée. Quand vous créez une vue fortement typée, vous effectuez un cast de la propriété ViewData. Model en un type particulier dans la classe code-behind d’une vue.

Si vous exécutez l’application après avoir modifié la classe HomeController et la vue index, vous obtiendrez une page vierge. Vous obtiendrez une page vide, car il n’y a pas d’enregistrement Movie dans la table de base de données tblMovies.

Pour ajouter des enregistrements à la table de base de données tblMovies, cliquez avec le bouton droit sur la table de base de données tblMovies dans la fenêtre Explorateur de serveurs (fenêtre explorateur de base de données dans Visual Web Developer) et sélectionnez l’option de menu **afficher les données**de la table. Vous pouvez insérer des enregistrements de films à l’aide de la grille qui s’affiche (voir figure 5).

[![insertion de films](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Figure 06**: insertion de films ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))

Une fois que vous avez ajouté des enregistrements de base de données à la table tblMovies et que vous exécutez l’application, vous voyez la page de la figure 7. Tous les enregistrements de base de données de films s’affichent dans une liste à puces.

[![affichage des films avec la vue index](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Figure 07**: affichage des films avec la vue d’index ([cliquez pour afficher l’image en taille réelle](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))

## <a name="using-the-repository-pattern"></a>Utilisation du modèle de référentiel

Dans la section précédente, nous avons utilisé LINQ to SQL classes directement dans une action de contrôleur. Nous avons utilisé la classe MovieDataContext directement à partir de l’action du contrôleur index (). Cela ne pose aucun problème dans le cas d’une application simple. Toutefois, travailler directement avec LINQ to SQL dans une classe de contrôleur crée des problèmes lorsque vous devez créer une application plus complexe.

L’utilisation de LINQ to SQL au sein d’une classe de contrôleur rend difficile le changement futur des technologies d’accès aux données. Par exemple, vous pouvez décider de passer de Microsoft LINQ to SQL à l’utilisation de la Entity Framework Microsoft comme technologie d’accès aux données. Dans ce cas, vous devez réécrire chaque contrôleur qui accède à la base de données au sein de votre application.

L’utilisation de LINQ to SQL dans une classe de contrôleur rend également difficile la génération de tests unitaires pour votre application. Normalement, vous ne souhaitez pas interagir avec une base de données lors de l’exécution de tests unitaires. Vous souhaitez utiliser vos tests unitaires pour tester la logique de votre application et non votre serveur de base de données.

Pour générer une application MVC qui est plus adaptable aux modifications ultérieures et qui peut être testée plus facilement, vous devez envisager d’utiliser le modèle de référentiel. Lorsque vous utilisez le modèle de référentiel, vous créez une classe de référentiel distincte qui contient toute la logique d’accès à la base de données.

Lorsque vous créez la classe de référentiel, vous créez une interface qui représente toutes les méthodes utilisées par la classe de référentiel. Dans vos contrôleurs, vous écrivez votre code sur l’interface au lieu du référentiel. De cette façon, vous pouvez implémenter le référentiel à l’aide de différentes technologies d’accès aux données à l’avenir.

L’interface dans la liste 3 est nommée IMovieRepository et représente une méthode unique nommée ListAll ().

**Liste 3 – `Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

La classe de référentiel dans Listing 4 implémente l’interface IMovieRepository. Notez qu’il contient une méthode nommée ListAll () qui correspond à la méthode requise par l’interface IMovieRepository.

**Liste 4 – `Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Enfin, la classe MoviesController de la liste 5 utilise le modèle de référentiel. Il n’utilise plus les classes LINQ to SQL directement.

**Liste 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Notez que la classe MoviesController dans la liste 5 a deux constructeurs. Le premier constructeur, le constructeur sans paramètre, est appelé quand votre application est en cours d’exécution. Ce constructeur crée une instance de la classe MovieRepository et la passe au deuxième constructeur.

Le deuxième constructeur a un seul paramètre : un paramètre IMovieRepository. Ce constructeur assigne simplement la valeur du paramètre à un champ au niveau de la classe nommé \_référentiel.

La classe MoviesController tire parti d’un modèle de conception de logiciels appelé modèle d’injection de dépendances. En particulier, il utilise un événement appelé injection de dépendances de constructeur. Pour plus d’informations sur ce modèle, consultez l’article suivant de Martin Fowler :

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Notez que tout le code de la classe MoviesController (à l’exception du premier constructeur) interagit avec l’interface IMovieRepository au lieu de la classe MovieRepository réelle. Le code interagit avec une interface abstraite au lieu d’une implémentation concrète de l’interface.

Si vous souhaitez modifier la technologie d’accès aux données utilisée par l’application, il vous suffit d’implémenter l’interface IMovieRepository avec une classe qui utilise la technologie d’accès à la base de données de remplacement. Par exemple, vous pouvez créer une classe EntityFrameworkMovieRepository ou une classe SubSonicMovieRepository. Étant donné que la classe Controller est programmée par rapport à l’interface, vous pouvez passer une nouvelle implémentation de IMovieRepository à la classe Controller et la classe continuera à fonctionner.

En outre, si vous souhaitez tester la classe MoviesController, vous pouvez passer une classe de référentiel de films factices à MoviesController. Vous pouvez implémenter la classe IMovieRepository avec une classe qui n’accède pas réellement à la base de données, mais qui contient toutes les méthodes requises de l’interface IMovieRepository. De cette façon, vous pouvez tester l’unité de la classe MoviesController sans réellement accéder à une base de données réelle.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel était de montrer comment vous pouvez créer des classes de modèle MVC en tirant parti de Microsoft LINQ to SQL. Nous avons examiné deux stratégies d’affichage des données de base de données dans une application MVC ASP.NET. Tout d’abord, nous avons créé LINQ to SQL classes et utilisé les classes directement dans une action de contrôleur. L’utilisation de LINQ to SQL classes au sein d’un contrôleur vous permet d’afficher rapidement et facilement des données de base de données dans une application MVC.

Nous avons ensuite abordé un chemin légèrement plus difficile, mais bien plus vertueux, permettant d’afficher les données de la base de données. Nous avons tiré parti du modèle de référentiel et placé toute notre logique d’accès à la base de données dans une classe de référentiel distincte. Dans notre contrôleur, nous avons écrit l’ensemble de notre code sur une interface au lieu d’une classe concrète. L’avantage du modèle de référentiel est qu’il nous permet de modifier facilement les technologies d’accès aux bases de données à l’avenir. il nous permet de tester facilement nos classes de contrôleur.

> [!div class="step-by-step"]
> [Précédent](creating-model-classes-with-the-entity-framework-vb.md)
> [Suivant](displaying-a-table-of-database-data-vb.md)
