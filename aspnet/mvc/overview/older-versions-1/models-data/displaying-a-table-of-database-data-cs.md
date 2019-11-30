---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Affichage d’une table de données deC#base de données () | Microsoft Docs
author: microsoft
description: Dans ce didacticiel, j’illustre deux méthodes d’affichage d’un ensemble d’enregistrements de base de données. J’affiche deux méthodes de mise en forme d’un ensemble d’enregistrements de base de données dans un fichier HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 3c908d030076fc8400190ef3cf1672632ac1ed6b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74589466"
---
# <a name="displaying-a-table-of-database-data-c"></a>Affichage d’une table de données de la base de données (C#)

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> Dans ce didacticiel, j’illustre deux méthodes d’affichage d’un ensemble d’enregistrements de base de données. J’affiche deux méthodes de mise en forme d’un ensemble d’enregistrements de base de données dans une table HTML. Tout d’abord, je vais vous montrer comment vous pouvez mettre en forme les enregistrements de base de données directement dans une vue. Ensuite, je vous montre comment tirer parti des partiels lors de la mise en forme des enregistrements de base de données.

L’objectif de ce didacticiel est d’expliquer comment vous pouvez afficher une table HTML de données de base de données dans une application MVC ASP.NET. Tout d’abord, vous allez apprendre à utiliser les outils de génération de modèles automatique inclus dans Visual Studio pour générer une vue qui affiche automatiquement un ensemble d’enregistrements. Ensuite, vous allez apprendre à utiliser une partie partielle comme modèle lors de la mise en forme d’enregistrements de base de données.

## <a name="create-the-model-classes"></a>Créer les classes de modèle

Nous allons afficher l’ensemble des enregistrements de la table de base de données de films. La table de base de données movies contient les colonnes suivantes :

<a id="0.3_table01"></a>

| **Nom de la colonne** | **Type de données** | **Autoriser les valeurs null** |
| --- | --- | --- |
| ID | int | False |
| Titre | Nvarchar (200) | False |
| MetaDirectory | NVarchar (50) | False |
| DateReleased | DateTime | False |

Pour représenter le tableau films dans notre application ASP.NET MVC, nous devons créer une classe de modèle. Dans ce didacticiel, nous utilisons le Entity Framework Microsoft pour créer nos classes de modèle.

> [!NOTE] 
> 
> Dans ce didacticiel, nous utilisons le Entity Framework Microsoft. Toutefois, il est important de comprendre que vous pouvez utiliser une variété de technologies différentes pour interagir avec une base de données à partir d’une application ASP.NET MVC, y compris LINQ to SQL, NHibernate ou ADO.NET.

Pour lancer l’Assistant Entity Data Model, procédez comme suit :

1. Cliquez avec le bouton droit sur le dossier modèles dans la fenêtre Explorateur de solutions et sélectionnez l’option de menu **Ajouter, nouvel élément**.
2. Sélectionnez la catégorie de **données** et sélectionnez le modèle de **Entity Data Model ADO.net** .
3. Donnez au modèle de données le nom *MoviesDBModel. edmx* , puis cliquez sur le bouton **Ajouter** .

Une fois que vous avez cliqué sur le bouton Ajouter, l’Assistant Entity Data Model s’affiche (voir figure 1). Pour terminer l’Assistant, procédez comme suit :

1. Dans l’étape **choisir le contenu du modèle** , sélectionnez l’option **générer à partir de la base de données** .
2. Dans l’étape **choisir votre connexion de données** , utilisez la connexion de données *MoviesDB. mdf* et le nom *MoviesDBEntities* pour les paramètres de connexion. Cliquez sur le bouton **suivant** .
3. Dans l’étape **choisir vos objets de base de données** , développez le nœud tables, puis sélectionnez le tableau films. Entrez les *modèles* d’espace de noms, puis cliquez sur le bouton **Terminer** .

[![de la création de classes LINQ to SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**Figure 01**: création de classes LINQ to SQL ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image2.png))

Une fois que vous avez terminé l’Assistant Entity Data Model, le concepteur de Entity Data Model s’ouvre. Le concepteur doit afficher l’entité Movies (voir figure 2).

[![le concepteur de Entity Data Model](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**Figure 02**: concepteur de Entity Data Model ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image4.png))

Nous devons apporter une modification avant de continuer. L’Assistant données d’entité génère une classe de modèle nommée *movies* qui représente la table de base de données de films. Étant donné que nous allons utiliser la classe movies pour représenter un film particulier, nous devons modifier le nom de la classe pour qu’elle soit *Movie* au lieu de *films* (singulier plutôt que pluriel).

Double-cliquez sur le nom de la classe sur l’aire du concepteur et remplacez le nom de la classe films par film. Après avoir apporté cette modification, cliquez sur le bouton **Enregistrer** (l’icône de la disquette) pour générer la classe Movie.

## <a name="create-the-movies-controller"></a>Créer le contrôleur movies

Maintenant que nous disposons d’un moyen de représenter nos enregistrements de base de données, nous pouvons créer un contrôleur qui retourne la collection de films. Dans la fenêtre de Explorateur de solutions de Visual Studio, cliquez avec le bouton droit sur le dossier Controllers et sélectionnez l’option de menu **Ajouter, contrôleur** (voir figure 3).

[![le menu Ajouter un contrôleur](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**Figure 03**: menu Ajouter un contrôleur ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image6.png))

Quand la boîte de dialogue **Ajouter un contrôleur** s’affiche, entrez le nom du contrôleur MovieController (voir figure 4). Cliquez sur le bouton **Ajouter** pour ajouter le nouveau contrôleur.

[![la boîte de dialogue Ajouter un contrôleur](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**Figure 04**: boîte de dialogue Ajouter un contrôleur ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image8.png))

Nous devons modifier l’action d’index () exposée par le contrôleur de film afin qu’il retourne l’ensemble des enregistrements de la base de données. Modifiez le contrôleur afin qu’il ressemble au contrôleur de la liste 1.

**Liste 1 – Controllers\MovieController.cs**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

Dans la liste 1, la classe MoviesDBEntities est utilisée pour représenter la base de données MoviesDB. Pour utiliser cette classe, vous devez importer l’espace de noms MvcApplication1. Models comme suit :

utilisation de MvcApplication1. Models ;

Entités d’expression *. MovieSet. ToList ()* retourne le jeu de tous les films de la table de base de données de films.

## <a name="create-the-view"></a>Créer la vue

Le moyen le plus simple d’afficher un ensemble d’enregistrements de base de données dans un tableau HTML est de tirer parti de la génération de modèles automatique fournie par Visual Studio.

Générez votre application en sélectionnant l’option de menu **Générer, générer la solution**. Vous devez générer votre application avant d’ouvrir la boîte de dialogue **Ajouter une vue** ou vos classes de données n’apparaissent pas dans la boîte de dialogue.

Cliquez avec le bouton droit sur l’action index () et sélectionnez l’option de menu **Ajouter une vue** (voir figure 5).

[![ajout d’une vue](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**Figure 05**: ajout d’une vue ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image10.png))

Dans la boîte de dialogue **Ajouter une vue** , activez la case à cocher **créer une vue fortement typée**. Sélectionnez la classe Movie comme **classe de données d’affichage**. Sélectionnez *liste* comme contenu de la **vue** (voir figure 6). La sélection de ces options génère un affichage fortement typé qui affiche une liste de films.

[![la boîte de dialogue Ajouter une vue](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**Figure 06**: boîte de dialogue Ajouter une vue ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image12.png))

Une fois que vous avez cliqué sur le bouton **Ajouter** , la vue dans la liste 2 est générée automatiquement. Cette vue contient le code requis pour itérer au sein de la collection de films et afficher chacune des propriétés d’un film.

**Liste 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

Vous pouvez exécuter l’application en sélectionnant l’option de menu **Déboguer, démarrer le débogage** (ou en appuyant sur la touche F5). L’exécution de l’application lance Internet Explorer. Si vous accédez à l’URL/Movie, vous verrez la page de la figure 7.

[![un tableau de films](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**Figure 07**: un tableau de films ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-cs/_static/image14.png))

Si vous n’aimez rien sur l’apparence de la grille des enregistrements de base de données de la figure 7, vous pouvez simplement modifier la vue index. Par exemple, vous pouvez remplacer l’en-tête *DateReleased* par *Date de publication* en modifiant la vue index.

## <a name="create-a-template-with-a-partial"></a>Créer un modèle avec une partie

Quand une vue est trop compliquée, il est judicieux de commencer à décomposer la vue en partiels. L’utilisation de partiels rend vos vues plus faciles à comprendre et à gérer. Nous allons créer une partie partielle que nous pouvons utiliser comme modèle pour mettre en forme chacun des enregistrements de la base de données de films.

Pour créer le partiel, procédez comme suit :

1. Cliquez avec le bouton droit sur le dossier Views\Movie et sélectionnez l’option de menu **Ajouter une vue**.
2. Cochez la case *créer une vue partielle (. ascx)* .
3. Nommez le *MovieTemplate*partiel.
4. Cochez la case **créer une vue fortement typée**.
5. Sélectionnez Movie comme *classe de données d’affichage*.
6. Sélectionnez vide comme contenu de la *vue*.
7. Cliquez sur le bouton **Ajouter** pour ajouter le partiel à votre projet.

Une fois ces étapes terminées, modifiez le MovieTemplate partiel pour qu’il ressemble à Listing 3.

**Liste 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

Le partiel de la liste 3 contient un modèle pour une seule ligne d’enregistrements.

La vue d’index modifiée de la liste 4 utilise le MovieTemplate partiel.

**Liste 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

La vue de la liste 4 contient une boucle foreach qui itère au sein de tous les films. Pour chaque film, le MovieTemplate partiel est utilisé pour mettre en forme le film. Le MovieTemplate est rendu en appelant la méthode d’assistance RenderPartial ().

La vue d’index modifiée affiche la même table HTML des enregistrements de base de données. Toutefois, la vue a été considérablement simplifiée.

La méthode RenderPartial () est différente de la plupart des autres méthodes d’assistance, car elle ne retourne pas de chaîne. Par conséquent, vous devez appeler la méthode RenderPartial () à l’aide de &lt;% html. RenderPartial (); %&gt; au lieu de &lt;% = html. RenderPartial (); %&gt;.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel était d’expliquer comment vous pouvez afficher un ensemble d’enregistrements de base de données dans une table HTML. Tout d’abord, vous avez appris à retourner un jeu d’enregistrements de base de données à partir d’une action de contrôleur en tirant parti de la Entity Framework Microsoft. Ensuite, vous avez appris à utiliser la génération de modèles automatique de Visual Studio pour générer une vue qui affiche automatiquement une collection d’éléments. Enfin, vous avez appris à simplifier la vue en tirant parti d’un partiel. Vous avez appris à utiliser une partie partielle comme modèle pour pouvoir mettre en forme chaque enregistrement de base de données.

> [!div class="step-by-step"]
> [Précédent](creating-model-classes-with-linq-to-sql-cs.md)
> [Suivant](performing-simple-validation-cs.md)
