---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: Créer une application de base de données de films en 15 minutes avec ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther génère une application MVC ASP.NET basée sur une base de données complète, du début à la fin. Ce didacticiel est une introduction intéressante pour les personnes qui découvrent de nouveaux t...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ce8161d29a8ab4005e2b20462b08c9e10ee815a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541887"
---
# <a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>Créer une application de base de données de films en 15 minutes avec ASP.NET MVC (VB)

par [Stephen Walther](https://github.com/StephenWalther)

[Télécharger le code](https://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Stephen Walther génère une application MVC ASP.NET basée sur une base de données complète, du début à la fin. Ce didacticiel est une introduction intéressante pour les personnes qui débutent dans l’infrastructure MVC ASP.NET et qui souhaitent avoir une idée du processus de création d’une application ASP.NET MVC.

L’objectif de ce didacticiel est de vous faire une idée de « mon utilité » pour créer une application ASP.NET MVC. Dans ce didacticiel, j’ai créé une application ASP.NET MVC entière du début à la fin. Je vous montre comment créer une application simple orientée base de données qui illustre comment vous pouvez répertorier, créer et modifier des enregistrements de base de données.

Pour simplifier le processus de création de notre application, nous allons tirer parti des fonctionnalités de génération de modèles automatique de Visual Studio 2008. Nous allons laisser Visual Studio générer le code et le contenu initiaux de nos contrôleurs, modèles et vues.

Si vous avez travaillé avec Active Server pages ou ASP.NET, vous devriez trouver ASP.NET MVC très familier. Les vues MVC ASP.NET sont très similaires aux pages d’une application Active Server pages. Et, tout comme une application ASP.NET Web Forms traditionnelle, ASP.NET MVC vous offre un accès complet à l’ensemble complet des langages et des classes fournis par le .NET Framework.

Mon espoir est que ce didacticiel vous donnera une idée de la façon dont l’expérience de la création d’une application ASP.NET MVC est semblable et différente de l’expérience de création d’une application Active Server pages ou ASP.NET Web Forms.

## <a name="overview-of-the-movie-database-application"></a>Vue d’ensemble de l’application de base de données de films

Étant donné que notre objectif est de simplifier les choses, nous allons créer une application de base de données de films très simple. Notre simple application de base de données Movie nous permettra de faire trois choses :

1. Répertorier un ensemble d’enregistrements de base de données de films
2. Créer un enregistrement de base de données de films
3. Modifier un enregistrement de base de données Movie existant

Là encore, étant donné que nous souhaitons simplifier les choses, nous allons tirer parti du nombre minimal de fonctionnalités de l’infrastructure MVC ASP.NET nécessaire pour créer notre application. Par exemple, nous n’allons pas tirer parti du développement piloté par les tests.

Pour créer notre application, vous devez effectuer les étapes suivantes :

1. Créer le projet d’application Web MVC ASP.NET
2. Créer la base de données
3. Créer le modèle de base de données
4. Créer le contrôleur MVC ASP.NET
5. Créer les vues MVC ASP.NET

## <a name="preliminaries"></a>Étapes préalables

Pour générer une application ASP.NET MVC, vous avez besoin de Visual Studio 2008 ou de Visual Web Developer 2008 Express. Vous devez également télécharger l’infrastructure MVC ASP.NET.

Si vous ne possédez pas Visual Studio 2008, vous pouvez télécharger une version d’évaluation de 90 jour de Visual Studio 2008 à partir de ce site Web :

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Vous pouvez également créer des applications ASP.NET MVC avec Visual Web Developer Express 2008. Si vous décidez d’utiliser Visual Web Developer Express, le Service Pack 1 doit être installé. Vous pouvez télécharger Visual Web Developer 2008 Express avec Service Pack 1 à partir de ce site Web :

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;d isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Après l’installation de Visual Studio 2008 ou de Visual Web Developer 2008, vous devez installer l’infrastructure MVC ASP.NET. Vous pouvez télécharger l’infrastructure MVC ASP.NET à partir du site Web suivant :

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Au lieu de télécharger le Framework ASP.NET et l’infrastructure MVC ASP.NET individuellement, vous pouvez tirer parti de la Web Platform Installer. Le Web Platform Installer est une application qui vous permet de gérer facilement les applications installées sur votre ordinateur :
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)

## <a name="creating-an-aspnet-mvc-web-application-project"></a>Création d’un projet d’application Web MVC ASP.NET

Commençons par créer un nouveau projet d’application Web ASP.NET MVC dans Visual Studio 2008. Sélectionnez l’option de menu **fichier, nouveau projet** . la boîte de dialogue Nouveau projet s’affiche dans la figure 1. Sélectionnez Visual Basic comme langage de programmation et sélectionnez le modèle de projet d’application Web MVC ASP.NET. Donnez au projet le nom MovieApp et cliquez sur le bouton OK.

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**Figure 01**: boîte de dialogue Nouveau projet ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))

Veillez à sélectionner .NET Framework 3,5 dans la liste déroulante en haut de la boîte de dialogue Nouveau projet ou le modèle de projet d’application Web ASP.NET MVC n’apparaîtra pas.

Chaque fois que vous créez un projet d’application Web MVC, Visual Studio vous invite à créer un projet de test unitaire distinct. La boîte de dialogue de la figure 2 s’affiche. Étant donné que nous ne créerons pas de tests dans ce didacticiel en raison de contraintes de temps (et, oui, nous devrions ressentir un peu de choses à l’aise), sélectionnez l’option **non** , puis cliquez sur le bouton **OK** .

> [!NOTE] 
> 
> Visual Web Developer ne prend pas en charge les projets de test.

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**Figure 02**: boîte de dialogue créer un projet de test unitaire ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))

Une application ASP.NET MVC possède un ensemble de dossiers standard : un dossier de modèles, de vues et de contrôleurs. Vous pouvez voir cet ensemble standard de dossiers dans la fenêtre Explorateur de solutions. Nous devons ajouter des fichiers à chacun des dossiers de modèles, de vues et de contrôleurs afin de créer notre application de base de données de films.

Lorsque vous créez une application MVC avec Visual Studio, vous recevez un exemple d’application. Étant donné que nous voulons commencer à partir de zéro, nous devons supprimer le contenu de cet exemple d’application. Vous devez supprimer le fichier suivant et le dossier suivant :

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>Création de la base de données

Nous devons créer une base de données qui contiendra les enregistrements de la base de données Movie. Heureusement, Visual Studio comprend une base de données gratuite nommée SQL Server Express. Pour créer la base de données, procédez comme suit :

1. Cliquez avec le bouton droit sur le dossier de données de l’application\_dans la fenêtre Explorateur de solutions et sélectionnez l’option de menu **Ajouter, nouvel élément**.
2. Sélectionnez la catégorie de **données** et sélectionnez le modèle **de base de données SQL Server** (voir figure 3).
3. Nommez votre nouvelle base de données *MoviesDB. mdf* , puis cliquez sur le bouton **Ajouter** .

Après avoir créé votre base de données, vous pouvez vous connecter à la base de données en double-cliquant sur le fichier MoviesDB. mdf situé dans le dossier de données de l’application\_. Double-cliquez sur le fichier MoviesDB. mdf pour ouvrir la fenêtre Explorateur de serveurs.

> [!NOTE] 
> 
> La fenêtre Explorateur de serveurs s’intitule la fenêtre explorateur de base de données dans le cas de Visual Web Developer.

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**Figure 03**: création d’une base de données Microsoft SQL Server ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))

Ensuite, nous devons créer une nouvelle table de base de données. Dans la fenêtre Explorateur de serveurs, cliquez avec le bouton droit sur le dossier tables et sélectionnez l’option de menu **Ajouter une nouvelle table**. La sélection de cette option de menu ouvre le concepteur de tables de base de données. Créez les colonnes de base de données suivantes :

<a id="0.2_table01"></a>

| **Nom de la colonne** | **Type de données** | **Null autorisé** |
| --- | --- | --- |
| Id | Int | False |
| Titre | Nvarchar(100) | False |
| Directeur | Nvarchar(100) | False |
| DateReleased | DateTime | False |

La première colonne, la colonne ID, possède deux propriétés spéciales. Tout d’abord, vous devez marquer la colonne ID en tant que colonne de clé primaire. Après avoir sélectionné la colonne ID, cliquez sur le bouton **définir la clé primaire** (il s’agit de l’icône qui ressemble à une clé). Deuxièmement, vous devez marquer la colonne ID en tant que colonne d’identité. Dans la Fenêtre Propriétés de colonne, faites défiler jusqu’à la section Spécification d’identité et développez-la. Remplacez la valeur de la propriété **is Identity** par la valeur **Yes**. Lorsque vous avez terminé, le tableau doit ressembler à la figure 4.

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**Figure 04**: table de base de données Movies ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))

La dernière étape consiste à enregistrer la nouvelle table. Cliquez sur le bouton Enregistrer (l’icône de la disquette) et donnez au nouveau tableau le nom films.

Une fois que vous avez terminé la création de la table, ajoutez des enregistrements de films à la table. Cliquez avec le bouton droit sur le tableau films dans la fenêtre Explorateur de serveurs et sélectionnez l’option de menu **afficher les données**de la table. Entrez la liste de vos films préférés (voir figure 5).

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**Figure 05**: saisie d’enregistrements de films ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))

## <a name="creating-the-model"></a>Création du modèle

Nous devons ensuite créer un ensemble de classes pour représenter notre base de données. Nous devons créer un modèle de base de données. Nous allons tirer parti de la Entity Framework Microsoft pour générer automatiquement les classes pour notre modèle de base de données.

> [!NOTE] 
> 
> L’infrastructure MVC ASP.NET n’est pas liée à l’Entity Framework Microsoft. Vous pouvez créer vos classes de modèle de base de données en tirant parti d’un large éventail d’outils de mappage relationnel objet (ou/M), notamment LINQ to SQL, SubSonic et NHibernate.

Pour lancer l’Assistant Entity Data Model, procédez comme suit :

1. Cliquez avec le bouton droit sur le dossier modèles dans la fenêtre Explorateur de solutions et sélectionnez l’option de menu **Ajouter, nouvel élément**.
2. Sélectionnez la catégorie de **données** et sélectionnez le modèle de **Entity Data Model ADO.net** .
3. Donnez au modèle de données le nom *MoviesDBModel. edmx* , puis cliquez sur le bouton **Ajouter** .

Une fois que vous avez cliqué sur le bouton Ajouter, l’Assistant Entity Data Model s’affiche (voir figure 6). Pour terminer l’Assistant, procédez comme suit :

1. Dans l’étape **choisir le contenu du modèle** , sélectionnez l’option **générer à partir de la base de données** .
2. Dans l’étape **choisir votre connexion de données** , utilisez la connexion de données *MoviesDB. mdf* et le nom *MoviesDBEntities* pour les paramètres de connexion. Cliquez sur le bouton **Suivant**.
3. Dans l’étape **choisir vos objets de base de données** , développez le nœud tables, puis sélectionnez le tableau films. Entrez l’espace de noms *MovieApp. Models* , puis cliquez sur le bouton **Terminer** .

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**Figure 06**: génération d’un modèle de base de données à l’aide de l’Assistant Entity Data Model ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))

Une fois que vous avez terminé l’Assistant Entity Data Model, le concepteur de Entity Data Model s’ouvre. Le concepteur doit afficher la table de base de données Movies (voir la figure 7).

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**Figure 07**: concepteur de Entity Data Model ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))

Nous devons apporter une modification avant de continuer. L’Assistant données d’entité génère une classe de modèle nommée movies qui représente la table de base de données de films. Étant donné que nous allons utiliser la classe movies pour représenter un film particulier, nous devons modifier le nom de la classe pour qu’elle soit *Movie* au lieu de *films* (singulier plutôt que pluriel).

Double-cliquez sur le nom de la classe sur l’aire du concepteur et remplacez le nom de la classe films par film. Après avoir apporté cette modification, cliquez sur le bouton **Enregistrer** (l’icône de la disquette) pour générer la classe Movie.

## <a name="creating-the-aspnet-mvc-controller"></a>Création du contrôleur MVC ASP.NET

L’étape suivante consiste à créer le contrôleur MVC ASP.NET. Un contrôleur est chargé de contrôler la manière dont un utilisateur interagit avec une application MVC ASP.NET.

Procédez comme suit :

1. Dans la fenêtre Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers et sélectionnez l’option de menu **Ajouter, contrôleur**.
2. Dans la boîte de dialogue Ajouter un contrôleur, entrez le nom *HomeController* et cochez la case **Ajouter des méthodes d’action pour les scénarios créer, mettre à jour et détails** (voir figure 8).
3. Cliquez sur le bouton **Ajouter** pour ajouter le nouveau contrôleur à votre projet.

Une fois ces étapes terminées, le contrôleur de la liste 1 est créé. Notez qu’elle contient des méthodes appelées index, Details, Create et Edit. Dans les sections suivantes, nous allons ajouter le code nécessaire pour que ces méthodes fonctionnent.

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**Figure 08**: ajout d’un nouveau contrôleur ASP.NET MVC ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))

**Liste 1 – Controllers\HomeController.vb**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>Liste des enregistrements de base de données

La méthode index () du contrôleur d’hébergement est la méthode par défaut pour une application MVC ASP.NET. Quand vous exécutez une application ASP.NET MVC, la méthode index () est la première méthode de contrôleur appelée.

Nous allons utiliser la méthode index () pour afficher la liste des enregistrements de la table de base de données de films. Nous allons tirer parti des classes de modèle de base de données que nous avons créées précédemment pour récupérer les enregistrements de base de données de films avec la méthode index ().

J’ai modifié la classe HomeController dans Listing 2 afin qu’elle contienne un nouveau champ privé nommé \_DB. La classe MoviesDBEntities représente notre modèle de base de données et nous utiliserons cette classe pour communiquer avec notre base de données.

J’ai également modifié la méthode index () dans la liste 2. La méthode index () utilise la classe MoviesDBEntities pour récupérer tous les enregistrements Movie de la table de base de données movies. Expression *\_DB. MovieSet. ToList ()* renvoie la liste de tous les enregistrements de film de la table de base de données de films.

La liste des films est transmise à la vue. Tout ce qui est passé à la méthode View () est passé à la vue en tant que données de vue.

**Liste 2 – contrôleurs/HomeController. vb (méthode d’index modifiée)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

La méthode index () renvoie une vue nommée index. Nous devons créer cette vue pour afficher la liste des enregistrements de la base de données de films. Procédez comme suit :

Vous devez générer votre projet (sélectionnez l’option de menu **Générer, générer la solution**) avant d’ouvrir la boîte de dialogue **Ajouter une vue** , ou aucune classe ne s’affichera dans la liste déroulante classe de **données d’affichage** .

1. Cliquez avec le bouton droit sur la méthode index () dans l’éditeur de code, puis sélectionnez l’option de menu **Ajouter une vue** (voir figure 9).
2. Dans la boîte de dialogue Ajouter une vue, vérifiez que la case à cocher **créer une vue fortement typée** est activée.
3. Dans la liste déroulante **afficher le contenu** , sélectionnez la *liste*valeur.
4. Dans la liste déroulante **afficher la classe de données** , sélectionnez la valeur *MovieApp. Movie*.
5. Cliquez sur le bouton Ajouter pour créer le nouvel affichage (voir figure 10).

Une fois ces étapes terminées, une nouvelle vue nommée index. aspx est ajoutée au dossier Views\Home. Le contenu de la vue index est inclus dans le Listing 3.

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**Figure 09**: ajout d’une vue à partir d’une action de contrôleur ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**Figure 10**: création d’un nouvel affichage avec la boîte de dialogue Ajouter une vue ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

La vue index affiche tous les enregistrements de film de la table de base de données movies dans une table HTML. La vue contient une boucle for each qui itère au sein de chaque film représenté par la propriété ViewData. Model. Si vous exécutez votre application en appuyant sur la touche F5, vous verrez la page Web dans la figure 11.

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**Figure 11**: vue d’index ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))

## <a name="creating-new-database-records"></a>Création d’enregistrements de base de données

La vue d’index que nous avons créée dans la section précédente comprend un lien permettant de créer des enregistrements de base de données. Commençons par implémenter la logique et créer l’affichage nécessaire à la création de nouveaux enregistrements de base de données de films.

Le contrôleur d’hébergement contient deux méthodes nommées Create (). La première méthode Create () n’a aucun paramètre. Cette surcharge de la méthode Create () permet d’afficher le formulaire HTML pour la création d’un nouvel enregistrement de base de données de film.

La deuxième méthode Create () a un paramètre FormCollection. Cette surcharge de la méthode Create () est appelée lorsque le formulaire HTML pour la création d’un nouveau film est publié sur le serveur. Notez que cette deuxième méthode Create () a un attribut AcceptVerbs qui empêche la méthode d’être appelée, à moins qu’une opération HTTP postale ne soit effectuée.

Cette seconde méthode Create () a été modifiée dans la classe HomeController mise à jour de la liste 4. La nouvelle version de la méthode Create () accepte un paramètre Movie et contient la logique d’insertion d’un nouveau film dans la table de base de données movies.

> [!NOTE] 
> 
> Notez l’attribut de liaison. Étant donné que nous ne souhaitons pas mettre à jour la propriété Movie ID à partir du formulaire HTML, nous devons explicitement exclure cette propriété.

**Liste 4 – Controllers\HomeController.vb (méthode de création modifiée)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Visual Studio facilite la création du formulaire pour la création d’un nouvel enregistrement de base de données de films (voir la figure 12). Procédez comme suit :

1. Cliquez avec le bouton droit sur la méthode Create () dans l’éditeur de code, puis sélectionnez l’option de menu **Ajouter une vue**.
2. Vérifiez que la case à cocher **créer une vue fortement typée** est activée.
3. Dans la liste déroulante **afficher le contenu** , sélectionnez la valeur *créer*.
4. Dans la liste déroulante **afficher la classe de données** , sélectionnez la valeur *MovieApp. Movie*.
5. Cliquez sur le bouton **Ajouter** pour créer le nouvel affichage.

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**Figure 12**: ajout de la vue Create ([Cliquer pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))

Visual Studio génère automatiquement la vue dans la liste 5. Cette vue contient un formulaire HTML qui inclut des champs qui correspondent à chacune des propriétés de la classe Movie.

**Liste 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> Le formulaire HTML généré par la boîte de dialogue Ajouter une vue génère un champ de formulaire d’ID. Étant donné que la colonne ID est une colonne d’identité, nous n’avons pas besoin de ce champ de formulaire et vous pouvez le supprimer en toute sécurité.

Après avoir ajouté la vue Create, vous pouvez ajouter de nouveaux enregistrements de films à la base de données. Exécutez votre application en appuyant sur la touche F5, puis cliquez sur le lien créer pour afficher le formulaire dans la figure 13. Si vous terminez et envoyez le formulaire, un nouvel enregistrement de la base de données de films est créé.

Notez que vous recevez automatiquement la validation du formulaire. Si vous négligez d’entrer une date de publication pour un film ou si vous entrez une date de publication non valide, le formulaire est réaffiché et le champ date de publication est mis en surbrillance.

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**Figure 13**: création d’un nouvel enregistrement de base de données de film ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))

## <a name="editing-existing-database-records"></a>Modification d’enregistrements de base de données existants

Dans les sections précédentes, nous avons abordé la manière dont vous pouvez répertorier et créer des enregistrements de base de données. Dans cette dernière section, nous expliquons comment vous pouvez modifier les enregistrements de base de données existants.

Tout d’abord, nous devons générer le formulaire de modification. Cette étape est simple, car Visual Studio génère automatiquement le formulaire de modification pour nous. Ouvrez la classe HomeController. vb dans l’éditeur de code Visual Studio et procédez comme suit :

1. Cliquez avec le bouton droit sur la méthode Edit () dans l’éditeur de code, puis sélectionnez l’option de menu **Ajouter une vue** (voir figure 14).
2. Cochez la case **créer une vue fortement typée**.
3. Dans la liste déroulante **afficher le contenu** , sélectionnez la valeur *modifier*.
4. Dans la liste déroulante **afficher la classe de données** , sélectionnez la valeur *MovieApp. Movie*.
5. Cliquez sur le bouton **Ajouter** pour créer le nouvel affichage.

L’exécution de ces étapes ajoute une nouvelle vue nommée Edit. aspx au dossier Views\Home. Cette vue contient un formulaire HTML pour la modification d’un enregistrement de film.

[![la boîte de dialogue Nouveau projet](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**Figure 14**: ajout de la vue Edit ([cliquez pour afficher l’image en taille réelle](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))

> [!NOTE] 
> 
> La vue Edit contient un champ de formulaire HTML qui correspond à la propriété Movie ID. Étant donné que vous ne voulez pas que les utilisateurs modifient la valeur de la propriété ID, vous devez supprimer ce champ de formulaire.

Enfin, nous devons modifier le contrôleur de démarrage afin qu’il prenne en charge la modification d’un enregistrement de base de données. La classe HomeController mise à jour est contenue dans la liste 6.

**Liste 6 – Controllers\HomeController.vb (méthodes de modification)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

Dans la liste 6, j’ai ajouté une logique supplémentaire aux deux surcharges de la méthode Edit (). La première méthode Edit () retourne l’enregistrement de la base de données Movie qui correspond au paramètre ID transmis à la méthode. La deuxième surcharge effectue les mises à jour d’un enregistrement de film dans la base de données.

Notez que vous devez récupérer le film d’origine, puis appeler ApplyPropertyChanges () pour mettre à jour le film existant dans la base de données.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel était de vous donner une idée de l’expérience de la création d’une application ASP.NET MVC. J’espère que vous avez découvert que la création d’une application Web ASP.NET MVC est très similaire à l’expérience de la création d’une application Active Server pages ou ASP.NET.

Dans ce didacticiel, nous avons examiné uniquement les fonctionnalités les plus basiques de l’infrastructure MVC ASP.NET. Dans les prochains didacticiels, nous explorons plus en détail des sujets tels que les contrôleurs, les actions de contrôleur, les affichages, les données d’affichage et les applications auxiliaires HTML.

> [!div class="step-by-step"]
> [Précédent](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
