---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Création de classes de modèle avec la Entity Framework (VB) | Microsoft Docs
author: microsoft
description: Dans ce didacticiel, vous allez apprendre à utiliser ASP.NET MVC avec Microsoft Entity Framework. Vous allez apprendre à utiliser l’Assistant entité pour créer une entité ADO.NET da...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: f6c896c6f5f6d898ac6f99d5998fb29cb73bcb10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543329"
---
# <a name="creating-model-classes-with-the-entity-framework-vb"></a>Création de classes de modèle avec Entity Framework (VB)

par [Microsoft](https://github.com/microsoft)

> Dans ce didacticiel, vous allez apprendre à utiliser ASP.NET MVC avec Microsoft Entity Framework. Vous allez apprendre à utiliser l’Assistant entité pour créer une Entity Data Model ADO.NET. Au cours de ce didacticiel, nous créons une application Web qui illustre comment sélectionner, insérer, mettre à jour et supprimer des données de base de données à l’aide de l’Entity Framework.

L’objectif de ce didacticiel est d’expliquer comment vous pouvez créer des classes d’accès aux données à l’aide de l’Entity Framework Microsoft lors de la création d’une application MVC ASP.NET. Ce didacticiel ne suppose aucune connaissance préalable de la Entity Framework Microsoft. À la fin de ce didacticiel, vous comprendrez comment utiliser l’Entity Framework pour sélectionner, insérer, mettre à jour et supprimer des enregistrements de base de données.

Le Entity Framework Microsoft est un outil de mappage relationnel d’objets (O/RM) qui vous permet de générer automatiquement une couche d’accès aux données à partir d’une base de données. Le Entity Framework vous permet d’éviter le travail fastidieux de création de vos classes d’accès aux données à la main.

> [!NOTE] 
> 
> Il n’existe pas de connexion essentielle entre ASP.NET MVC et le Entity Framework Microsoft. Il existe plusieurs alternatives au Entity Framework que vous pouvez utiliser avec ASP.NET MVC. Par exemple, vous pouvez créer vos classes de modèle MVC à l’aide d’autres outils O/RM tels que Microsoft LINQ to SQL, NHibernate ou Subsonic.

Pour illustrer la façon dont vous pouvez utiliser Microsoft Entity Framework avec ASP.NET MVC, nous allons créer un exemple d’application simple. Nous allons créer une application de base de données de films qui vous permet d’afficher et de modifier les enregistrements de base de données de films.

Ce didacticiel part du principe que vous disposez de Visual Studio 2008 ou Visual Web Developer 2008 avec Service Pack 1. Vous avez besoin du Service Pack 1 pour pouvoir utiliser le Entity Framework. Vous pouvez télécharger Visual Studio 2008 Service Pack 1 ou Visual Web Developer avec Service Pack 1 à partir de l’adresse suivante :

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)

## <a name="creating-the-movie-sample-database"></a>Création de l’exemple de base de données Movie

L’application de base de données Movie utilise une table de base de données nommée movies qui contient les colonnes suivantes :

| Nom de la colonne | Type de données | Autoriser les valeurs null ? | Est une clé primaire ? |
| --- | --- | --- | --- |
| Id | int | False | True |
| Titre | nvarchar(100) | False | False |
| Directeur | nvarchar(100) | False | False |

Vous pouvez ajouter cette table à un projet MVC ASP.NET en procédant comme suit :

1. Cliquez avec le bouton droit sur le dossier de données de l’application\_dans la fenêtre Explorateur de solutions et sélectionnez l’option de menu **Ajouter, nouvel élément.**
2. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **SQL Server base de données**, nommez la base de données MoviesDB. mdf, puis cliquez sur le bouton **Ajouter** .
3. Double-cliquez sur le fichier MoviesDB. mdf pour ouvrir la fenêtre Explorateur de serveurs/explorateur de base de données.
4. Développez la connexion à la base de données MoviesDB. mdf, cliquez avec le bouton droit sur le dossier tables, puis sélectionnez l’option de menu **Ajouter une nouvelle table**.
5. Dans la Concepteur de tables, ajoutez les colonnes ID, title et Director.
6. Cliquez sur le bouton **Enregistrer** (l’icône de la disquette) pour enregistrer la nouvelle table avec le nom films.

Après avoir créé la table de base de données movies, vous devez ajouter des exemples de données à la table. Cliquez avec le bouton droit sur le tableau films et sélectionnez l’option de menu **afficher les données**de la table. Vous pouvez entrer des données factices dans la grille qui s’affiche.

## <a name="creating-the-adonet-entity-data-model"></a>Création du Entity Data Model ADO.NET

Pour utiliser le Entity Framework, vous devez créer une Entity Data Model. Vous pouvez tirer parti de l' *assistant Entity Data Model* de Visual Studio pour générer automatiquement un Entity Data Model à partir d’une base de données.

Procédez comme suit :

1. Cliquez avec le bouton droit sur le dossier modèles dans la fenêtre Explorateur de solutions et sélectionnez l’option de menu **Ajouter, nouvel élément**.
2. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez la catégorie de données (voir la figure 1).
3. Sélectionnez le modèle **ADO.NET Entity Data Model** , attribuez au Entity Data Model le nom MoviesDBModel. edmx, puis cliquez sur le bouton **Ajouter** . Cliquez sur le bouton **Ajouter** pour lancer l’Assistant modèle de données.
4. Dans l’étape **choisir le contenu du modèle** , choisissez l’option **générer à partir d’une base de données** , puis cliquez sur le bouton **suivant** (voir figure 2).
5. Dans l’étape **choisir votre connexion de données** , sélectionnez la connexion à la base de données MoviesDB. mdf, entrez le nom des paramètres de connexion des entités MoviesDBEntities, puis cliquez sur le bouton **suivant** (voir figure 3).
6. Dans l’étape **choisir vos objets de base de données** , sélectionnez la table de base de données de films, puis cliquez sur le bouton **Terminer** (voir figure 4).

Une fois ces étapes terminées, le ADO.NET Entity Data Model Designer (Entity Designer) s’ouvre.

**Figure 1 : création d’un Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Figure 2 – étape choisir le contenu du modèle**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Figure 3 – choisir votre connexion de données**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Figure 4 – choisir vos objets de base de données**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>Modification de la Entity Data Model ADO.NET

Après avoir créé une Entity Data Model, vous pouvez modifier le modèle en tirant parti de la Entity Designer (voir figure 5). Vous pouvez ouvrir le Entity Designer à tout moment en double-cliquant sur le fichier MoviesDBModel. edmx contenu dans le dossier modèles dans la fenêtre de Explorateur de solutions.

**Figure 5 : ADO.NET Entity Data Model Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Par exemple, vous pouvez utiliser la Entity Designer pour modifier les noms des classes générées par l’Assistant données de modèle d’entité. L’Assistant a créé une nouvelle classe d’accès aux données nommée movies. En d’autres termes, l’Assistant a donné à la classe le nom identique à celui de la table de base de données. Étant donné que nous allons utiliser cette classe pour représenter une instance de film particulière, nous devons renommer la classe films en film.

Si vous souhaitez renommer une classe d’entité, vous pouvez double-cliquer sur le nom de la classe dans la Entity Designer et entrer un nouveau nom (voir figure 6). Vous pouvez également modifier le nom d’une entité dans le Fenêtre Propriétés après avoir sélectionné une entité dans le Entity Designer.

**Figure 6 – modification d’un nom d’entité**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

N’oubliez pas d’enregistrer votre Entity Data Model après avoir apporté une modification en cliquant sur le bouton Enregistrer (l’icône de la disquette). En arrière-plan, le Entity Designer génère un ensemble de Visual Basic classes .NET. Vous pouvez afficher ces classes en ouvrant le fichier MoviesDBModel. Designer. vb à partir de la fenêtre Explorateur de solutions.

Ne modifiez pas le code dans le fichier designer. vb, car vos modifications seront remplacées la prochaine fois que vous utiliserez le Entity Designer. Si vous souhaitez étendre les fonctionnalités des classes d’entité définies dans le fichier designer. vb, vous pouvez créer des *classes partielles* dans des fichiers distincts.

#### <a name="selecting-database-records-with-the-entity-framework"></a>Sélection des enregistrements de base de données avec le Entity Framework

Commençons à créer notre application de base de données de films en créant une page qui affiche une liste d’enregistrements de films. Le contrôleur d’hébergement de la liste 1 expose une action nommée index (). L’action index () retourne tous les enregistrements de films à partir de la table de base de données de films en tirant parti de la Entity Framework.

**Liste 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Notez que le contrôleur de la liste 1 comprend un constructeur. Le constructeur initialise un champ au niveau de la classe nommé \_DB. Le champ \_DB représente les entités de base de données générées par le Entity Framework Microsoft. Le champ \_DB est une instance de la classe MoviesDBEntities qui a été générée par l’Entity Designer.

Le champ \_DB est utilisé dans l’action index () pour récupérer les enregistrements de la table de base de données movies. Expression \_DB. MovieSet représente tous les enregistrements de la table de base de données de films. La méthode ToList () est utilisée pour convertir l’ensemble de films en une collection générique d’objets Movie : List (of Movie).

Les enregistrements de film sont extraits avec l’aide de LINQ to Entities. L’action index () dans la liste 1 utilise la *syntaxe de méthode* LINQ pour récupérer le jeu d’enregistrements de base de données. Si vous préférez, vous pouvez utiliser la *syntaxe de requête* LINQ à la place. Les deux instructions suivantes font la même chose :

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

Utilisez la syntaxe LINQ (syntaxe de méthode ou syntaxe de requête) la plus intuitive. Il n’existe aucune différence de performances entre les deux approches : la seule différence est le style.

La vue dans la liste 2 est utilisée pour afficher les enregistrements de film.

**Liste 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

La vue dans la liste 2 contient une boucle **for each** qui itère au sein de chaque enregistrement de film et affiche les valeurs des propriétés Title et Director de l’enregistrement de film. Notez qu’un lien modifier et supprimer est affiché en regard de chaque enregistrement. En outre, un lien ajouter un film s’affiche au bas de la vue (voir la figure 7).

**Figure 7 : vue index**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

La vue index est une *vue typée*. La vue index comporte une directive &lt;% @ page%&gt; qui inclut un attribut Inherits. L’attribut Inherits effectue un cast de la propriété ViewData. Model en une collection de listes génériques fortement typée d’objets Movie, liste (de films).

## <a name="inserting-database-records-with-the-entity-framework"></a>Insertion d’enregistrements de base de données avec le Entity Framework

Vous pouvez utiliser la Entity Framework pour faciliter l’insertion de nouveaux enregistrements dans une table de base de données. La liste 3 contient deux nouvelles actions ajoutées à la classe de contrôleur d’hébergement que vous pouvez utiliser pour insérer de nouveaux enregistrements dans la table de base de données de films.

**Liste 3 – Controllers\HomeController.vb (ajouter des méthodes)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

La première action Add () retourne simplement une vue. La vue contient un formulaire pour l’ajout d’un nouvel enregistrement de base de données de films (voir figure 8). Lorsque vous envoyez le formulaire, la deuxième action Add () est appelée.

Notez que la deuxième action Add () est décorée avec l’attribut AcceptVerbs. Cette action ne peut être appelée que lors de l’exécution d’une opération HTTP après. En d’autres termes, cette action ne peut être appelée que lors de la publication d’un formulaire HTML.

La deuxième action Add () crée une nouvelle instance de la classe Entity Framework Movie avec l’aide de la méthode ASP.NET MVC TryUpdateModel (). La méthode TryUpdateModel () prend les champs du FormCollection transmis à la méthode Add () et assigne les valeurs de ces champs de formulaire HTML à la classe Movie.

Lorsque vous utilisez Entity Framework, vous devez fournir une « liste verte » des propriétés lorsque vous utilisez les méthodes TryUpdateModel ou UpdateModel pour mettre à jour les propriétés d’une classe d’entité.

Ensuite, l’action Add () effectue une validation de formulaire simple. L’action vérifie que les propriétés Title et Director ont des valeurs. En cas d’erreur de validation, un message d’erreur de validation est ajouté à ModelState.

S’il n’y a pas d’erreurs de validation, un nouvel enregistrement de film est ajouté à la table de base de données de films avec l’aide de l’Entity Framework. Le nouvel enregistrement est ajouté à la base de données avec les deux lignes de code suivantes :

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

La première ligne de code ajoute la nouvelle entité Movie à l’ensemble des films suivis par le Entity Framework. La deuxième ligne de code enregistre toutes les modifications apportées aux films faisant l’objet d’un suivi dans la base de données sous-jacente.

**Figure 8 – Ajout d’une vue**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Mise à jour des enregistrements de base de données avec le Entity Framework

Vous pouvez suivre presque la même approche pour modifier un enregistrement de base de données avec le Entity Framework que l’approche que nous venons de suivre pour insérer un nouvel enregistrement de base de données. La liste 4 contient deux nouvelles actions de contrôleur nommées modifier (). La première action Edit () retourne un formulaire HTML pour la modification d’un enregistrement de film. La deuxième action de modification () tente de mettre à jour la base de données.

**Liste 4 – Controllers\HomeController.vb (méthodes de modification)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

La deuxième action Edit () commence par récupérer l’enregistrement Movie de la base de données qui correspond à l’ID du film en cours de modification. L’instruction LINQ to Entities suivante récupère le premier enregistrement de base de données qui correspond à un ID particulier :

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

Ensuite, la méthode TryUpdateModel () est utilisée pour assigner les valeurs des champs de formulaire HTML aux propriétés de l’entité Movie. Notez qu’une liste verte est fournie pour spécifier les propriétés exactes à mettre à jour.

Ensuite, une validation simple est effectuée pour vérifier que les propriétés titre du film et directeur ont des valeurs. Si une valeur est manquante pour une propriété, un message d’erreur de validation est ajouté à ModelState et ModelState. IsValid retourne la valeur false.

Enfin, s’il n’y a aucune erreur de validation, la table de base de données de films sous-jacente est mise à jour avec toutes les modifications en appelant la méthode SaveChanges ().

Lorsque vous modifiez des enregistrements de base de données, vous devez transmettre l’ID de l’enregistrement en cours de modification à l’action du contrôleur qui effectue la mise à jour de la base de données. Dans le cas contraire, l’action du contrôleur ne saura pas quel enregistrement mettre à jour dans la base de données sous-jacente. La vue Edit, contenue dans la liste 5, comprend un champ de formulaire masqué qui représente l’ID de l’enregistrement de base de données en cours de modification.

**Liste 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Suppression des enregistrements de base de données avec le Entity Framework

La dernière opération de base de données, que nous devons aborder dans ce didacticiel, consiste à supprimer les enregistrements de base de données. Vous pouvez utiliser l’action contrôleur dans la liste 6 pour supprimer un enregistrement de base de données spécifique.

**Liste 6--\Controllers\HomeController.vb (action de suppression)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

L’action DELETE () récupère d’abord l’entité Movie qui correspond à l’ID passé à l’action. Ensuite, le film est supprimé de la base de données en appelant la méthode SupprimerObjet () suivie de la méthode SaveChanges (). Enfin, l’utilisateur est redirigé vers la vue index.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel était de montrer comment vous pouvez créer des applications Web basées sur les bases de données en tirant parti de ASP.NET MVC et de l’Entity Framework Microsoft. Vous avez appris à créer une application qui vous permet de sélectionner, d’insérer, de mettre à jour et de supprimer des enregistrements de base de données.

Tout d’abord, nous avons abordé la manière dont vous pouvez utiliser l’Assistant Entity Data Model pour générer une Entity Data Model à partir de Visual Studio. Ensuite, vous allez apprendre à utiliser LINQ to Entities pour récupérer un ensemble d’enregistrements de base de données à partir d’une table de base de données. Enfin, nous avons utilisé le Entity Framework pour insérer, mettre à jour et supprimer des enregistrements de base de données.

> [!div class="step-by-step"]
> [Précédent](validation-with-the-data-annotation-validators-cs.md)
> [Suivant](creating-model-classes-with-linq-to-sql-vb.md)
