---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET Web Forms à l’aide de la Entity Framework 4,0 et de Visual Studio 2010...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: fd88b32ad2a65e5b4c7ead15f0d6dc5dc6e97e75
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630458"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms

par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET Web Forms à l’aide de la Entity Framework 4,0 et de Visual Studio 2010. L’exemple d’application est un site Web pour une université contoso fictive. Il comprend des fonctionnalités telles que l’admission des étudiants, la création des cours et les affectations des formateurs.
> 
> Ce didacticiel présente des exemples C#dans. L' [exemple téléchargeable](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) contient du code C# à la fois dans et Visual Basic.
> 
> ## <a name="database-first"></a>Database First
> 
> Il existe trois façons de travailler avec des données dans le Entity Framework : *Database First*, *Model First*et *Code First*. Ce didacticiel est destiné à Database First. Pour plus d’informations sur les différences entre ces flux de travail et des conseils sur la façon de choisir le meilleur pour votre scénario, consultez [Entity Framework des flux de travail de développement](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Cette série de didacticiels utilise le modèle ASP.NET Web Forms et suppose que vous savez utiliser ASP.NET Web Forms dans Visual Studio. Si ce n’est pas le cas, consultez [prise en main avec ASP.NET 4,5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Si vous préférez utiliser l’infrastructure MVC ASP.NET, consultez [prise en main avec le Entity Framework à l’aide de ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versions de logiciels
> 
> | **Indiqué dans le didacticiel** | **Fonctionne également avec** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express pour le Web. Ce didacticiel n’a pas été testé avec les versions ultérieures de Visual Studio. Il existe de nombreuses différences dans les sélections de menus, les boîtes de dialogue et les modèles. |
> | .NET 4 | .NET 4,5 offre une compatibilité descendante avec .NET 4, mais le didacticiel n’a pas été testé avec .NET 4,5. |
> | Entity Framework 4 | Ce didacticiel n’a pas été testé avec les versions ultérieures de Entity Framework. À partir de Entity Framework 5, EF utilise par défaut le `DbContext API` introduit avec EF 4,1. Le contrôle EntityDataSource a été conçu pour utiliser l’API `ObjectContext`. Pour plus d’informations sur l’utilisation du contrôle EntityDataSource avec l’API `DbContext`, consultez ce billet de [blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Questions
> 
> Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), sur le [forum Entity Framework et LINQ to Entities](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), ou sur [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Présentation

L’application que vous allez créer dans ces didacticiels est un simple site Web universitaire.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Les utilisateurs peuvent afficher et mettre à jour les informations relatives aux étudiants, aux cours et aux formateurs. Voici quelques-uns des écrans que vous allez créer.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Création de l’application Web

Pour démarrer le didacticiel, ouvrez Visual Studio, puis créez un projet d’application Web ASP.NET à l’aide du modèle d' **application web ASP.net** :

[![image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Ce modèle crée un projet d’application Web qui contient déjà une feuille de style et des pages maîtres :

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Ouvrez le fichier *site. Master* et remplacez « mon application ASP.NET » par « Contoso University ».

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Recherchez le contrôle *menu* nommé `NavigationMenu` et remplacez-le par le balisage suivant, qui ajoute des éléments de menu pour les pages que vous allez créer.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Ouvrez la page *default. aspx* et remplacez le contrôle `Content` nommé `BodyContent` par ce qui suit :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Vous disposez maintenant d’une page d’hébergement simple avec des liens vers les différentes pages que vous allez créer :

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Création de la base de données

Pour ces didacticiels, vous allez utiliser le concepteur de modèle de données Entity Framework pour créer automatiquement le modèle de données basé sur une base de données existante (souvent appelée approche basée sur la *première base de données* ). Une alternative non traitée dans cette série de didacticiels consiste à créer le modèle de données manuellement et à faire en sorte que le concepteur génère des scripts pour créer la base de données (approche *modèle-première* ).

Pour la méthode basée sur la base de données utilisée dans ce didacticiel, l’étape suivante consiste à ajouter une base de données au site. Le moyen le plus simple consiste à télécharger le projet qui accompagne ce didacticiel. Cliquez ensuite avec le bouton droit sur le dossier de données de l' *application\_* , sélectionnez **Ajouter un élément existant**, puis sélectionnez le fichier de base de données *School. mdf* à partir du projet téléchargé.

Une alternative consiste à suivre les instructions de [création de l’exemple de base de données School](https://msdn.microsoft.com/library/bb399731.aspx). Que vous téléchargiez la base de données ou que vous la créez, copiez le fichier *School. mdf* du dossier suivant vers le dossier de données de l’application *\_* de votre application :

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Cet emplacement du fichier *. mdf* suppose que vous utilisez SQL Server 2008 Express.)

Si vous créez la base de données à partir d’un script, procédez comme suit pour créer un schéma de base de données :

1. Dans **Explorateur de serveurs**, développez **connexions de données**, développez *School. mdf*, cliquez avec le bouton droit sur **schémas de base de données**, puis sélectionnez **Ajouter un nouveau diagramme**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Sélectionnez toutes les tables, puis cliquez sur **Ajouter**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server crée un schéma de base de données qui affiche les tables, les colonnes des tables et les relations entre les tables. Vous pouvez déplacer les tables pour les organiser comme vous le souhaitez.
3. Enregistrez le diagramme en tant que « SchoolDiagram » et fermez-le.

Si vous téléchargez le fichier *School. mdf* qui accompagne ce didacticiel, vous pouvez afficher le schéma de base de données en double-cliquant sur **SchoolDiagram** sous **schémas de base de données** dans **Explorateur de serveurs**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Le diagramme ressemble à ce qui suit (les tables peuvent se trouver dans des emplacements différents de ce qui est indiqué ici) :

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Création du modèle de données Entity Framework

Vous pouvez maintenant créer un modèle de données Entity Framework à partir de cette base de données. Vous pouvez créer le modèle de données dans le dossier racine de l’application, mais pour ce didacticiel, vous allez le placer dans un dossier nommé *dal* (pour la couche d’accès aux données).

Dans **Explorateur de solutions**, ajoutez un dossier de projet nommé *dal* (Assurez-vous qu’il se trouve sous le projet, et non sous la solution).

Cliquez avec le bouton droit sur le dossier *dal* , puis sélectionnez **Ajouter** et **nouvel élément**. Sous **modèles installés**, sélectionnez **données**, sélectionnez le modèle **ADO.NET Entity Data Model** , nommez-le *SchoolModel. edmx*, puis cliquez sur **Ajouter**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Cette action démarre l’Assistant Entity Data Model. Dans la première étape de l’Assistant, l’option **générer à partir de la base de données** est sélectionnée par défaut. Cliquez sur **Next**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

Dans l’étape **choisir votre connexion de données** , laissez les valeurs par défaut et cliquez sur **suivant**. La base de données School est sélectionnée par défaut et le paramètre de connexion est enregistré dans le fichier *Web. config* en tant que **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

Dans l’étape de l’Assistant **choisir vos objets de base de données** , sélectionnez toutes les tables à l’exception de `sysdiagrams` (qui a été créée pour le diagramme que vous avez généré précédemment), puis cliquez sur **Terminer**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Une fois le modèle créé, Visual Studio affiche une représentation graphique des objets de Entity Framework (entités) qui correspondent à vos tables de base de données. (Comme avec le schéma de base de données, l’emplacement des éléments individuels peut être différent de ce que vous voyez dans cette illustration. Vous pouvez faire glisser les éléments autour de pour qu’ils correspondent à l’illustration si vous le souhaitez.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Exploration du modèle de données Entity Framework

Vous pouvez voir que le diagramme d’entité ressemble beaucoup au schéma de base de données, à quelques différences près. L’une des différences est l’ajout de symboles à la fin de chaque association qui indiquent le type d’Association (les relations entre les tables sont appelées associations d’entités dans le modèle de données) :

- Une association un-à-zéro-ou-un est représentée par « 1 » et « 0.. 1 ».

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    Dans ce cas, une entité `Person` peut ou ne peut pas être associée à une entité `OfficeAssignment`. Une entité de `OfficeAssignment` doit être associée à une entité `Person`. En d’autres termes, un formateur peut ou non être affecté à un bureau, et n’importe quel bureau ne peut être attribué qu’à un seul formateur.
- Une association un-à-plusieurs est représentée par « 1 » et «\*».

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    Dans ce cas, une entité `Person` peut avoir ou non des entités `StudentGrade` associées. Une entité de `StudentGrade` doit être associée à une entité `Person`. les entités `StudentGrade` représentent en fait des cours inscrits dans cette base de données. Si un étudiant est inscrit dans un cours et qu’il n’y a pas encore de niveau, la propriété `Grade` est null. En d’autres termes, un étudiant ne peut pas encore être inscrit à un cours, peut être inscrit dans un cours ou peut être inscrit dans plusieurs cours. Chaque niveau d’un cours inscrit ne s’applique qu’à un seul étudiant.
- Une association plusieurs-à-plusieurs est représentée par «\*» et «\*».

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    Dans ce cas, une entité `Person` peut avoir ou non des entités `Course` associées, et l’inverse est également vrai : une entité `Course` peut avoir ou non des entités `Person` associées. En d’autres termes, un formateur peut enseigner plusieurs cours et un cours peut être enseigné par plusieurs instructeurs. (Dans cette base de données, cette relation s’applique uniquement aux formateurs ; elle ne lie pas les étudiants aux cours. Les étudiants sont liés aux cours par la table StudentGrades.)

Une autre différence entre le schéma de base de données et le modèle de données est la section des **Propriétés de navigation** supplémentaires pour chaque entité. Une propriété de navigation d’une entité référence des entités associées. Par exemple, la propriété `Courses` dans une entité `Person` contient une collection de toutes les entités `Course` associées à cette `Person` entité.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Toutefois, une autre différence entre la base de données et le modèle de données est l’absence de la table d’association `CourseInstructor` qui est utilisée dans la base de données pour lier les tables `Person` et `Course` dans une relation plusieurs-à-plusieurs. Les propriétés de navigation vous permettent d’accéder aux entités de `Course` connexes à partir de l’entité `Person` et des entités de `Person` associées de l’entité `Course`. il n’est donc pas nécessaire de représenter la table d’association dans le modèle de données.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Dans le cadre de ce didacticiel, supposons que la `FirstName` colonne de la table `Person` contient en fait le prénom et le deuxième prénom d’une personne. Vous souhaitez modifier le nom du champ pour refléter cela, mais il se peut que l’administrateur de base de données (DBA) ne souhaite pas modifier la base de données. Vous pouvez modifier le nom de la propriété `FirstName` dans le modèle de données, tout en laissant son équivalent de base de données inchangé.

Dans le concepteur, cliquez avec le bouton droit sur **FirstName** dans l’entité `Person`, puis sélectionnez **Renommer**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Tapez le nouveau nom « FirstMidName ». Cela modifie la façon dont vous faites référence à la colonne dans le code sans modifier la base de données.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

L’Explorateur de modèles fournit une autre manière d’afficher la structure de la base de données, la structure du modèle de données et le mappage entre eux. Pour le voir, cliquez avec le bouton droit sur une zone vide dans le concepteur d’entités, puis cliquez sur **Explorateur de modèles**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

Le volet **Explorateur de modèles** affiche une arborescence. (Le volet **Explorateur de modèles** peut être ancré avec le volet **Explorateur de solutions** .) Le nœud **SchoolModel** représente la structure du modèle de données et le nœud **SchoolModel. Store** représente la structure de la base de données.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Développez **SchoolModel. Store** pour afficher les tables, développez **tables/vues** pour afficher les tables, puis développez **cours** pour afficher les colonnes dans une table.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Développez **SchoolModel**, développez **types d’entités**, puis développez le nœud **course** pour voir les entités et les propriétés dans les entités.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

Dans le concepteur ou dans le volet **Explorateur de modèles** , vous pouvez voir comment le Entity Framework met en relation les objets des deux modèles. Cliquez avec le bouton droit sur l’entité `Person`, puis sélectionnez **mappage de table**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

La fenêtre **Détails de mappage** s’ouvre. Notez que cette fenêtre vous permet de voir que la colonne de base de données `FirstName` est mappée à `FirstMidName`, ce à quoi vous l’avez renommée dans le modèle de données.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Le Entity Framework utilise XML pour stocker des informations sur la base de données, le modèle de données et les mappages entre eux. Le fichier *SchoolModel. edmx* est en fait un fichier XML qui contient ces informations. Le concepteur affiche les informations dans un format graphique, mais vous pouvez également afficher le fichier au format XML en cliquant avec le bouton droit sur le fichier *. edmx* dans **Explorateur de solutions**, en cliquant sur **Ouvrir avec**, puis en sélectionnant l' **éditeur XML (texte)** . (Le concepteur de modèle de données et un éditeur XML ne sont que deux méthodes différentes pour ouvrir et utiliser le même fichier. vous ne pouvez donc pas ouvrir le concepteur et ouvrir le fichier dans un éditeur XML en même temps.)

Vous avez maintenant créé un site Web, une base de données et un modèle de données. Dans la procédure pas à pas suivante, vous commencerez à utiliser des données à l’aide du modèle de données et du contrôle de `EntityDataSource` ASP.NET.

> [!div class="step-by-step"]
> [Next](the-entity-framework-and-aspnet-getting-started-part-2.md)
