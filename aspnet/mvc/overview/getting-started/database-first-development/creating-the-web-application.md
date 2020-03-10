---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Didacticiel : créer l’application Web et les modèles de données pour EF Database First avec ASP.NET MVC'
description: Ce didacticiel se concentre sur la création de l’application Web et la génération des modèles de données basés sur vos tables de base de données.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616269"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>Didacticiel : créer l’application Web et les modèles de données pour EF Database First avec ASP.NET MVC

 À l’aide de la génération de modèles automatique MVC, Entity Framework et ASP.NET, vous pouvez créer une application Web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer automatiquement du code qui permet aux utilisateurs d’afficher, de modifier, de créer et de supprimer des données qui se trouvent dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.

Ce didacticiel se concentre sur la création de l’application Web et la génération des modèles de données basés sur vos tables de base de données.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créer une application web ASP.NET
> * Générer les modèles

## <a name="prerequisites"></a>Conditions préalables requises

* [Prise en main de Entity Framework 6 Database First à l’aide de MVC 5](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>Créer une application web ASP.NET

Dans une nouvelle solution ou dans la même solution que le projet de base de données, créez un nouveau projet dans Visual Studio et sélectionnez le modèle **application Web ASP.net** . Nommez le projet **ContosoSite**.

![créer un projet](creating-the-web-application/_static/image1.png)

Cliquez sur **OK**.

Dans la fenêtre nouveau projet ASP.NET, sélectionnez le modèle **MVC** . Vous pouvez désactiver l' **ordinateur hôte dans l’option Cloud** pour le moment, car vous déploierez l’application dans le Cloud ultérieurement. Cliquez sur **OK** pour créer l’application.

Le projet est créé avec les fichiers et dossiers par défaut.

Dans ce didacticiel, vous allez utiliser Entity Framework 6. Vous pouvez double-vérifier la version de Entity Framework dans votre projet via la fenêtre gérer les packages NuGet. Si nécessaire, mettez à jour votre version de Entity Framework.

![afficher la version](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Générer les modèles

Vous allez maintenant créer Entity Framework des modèles à partir des tables de base de données. Ces modèles sont des classes que vous allez utiliser pour travailler avec les données. Chaque modèle reflète une table dans la base de données et contient des propriétés qui correspondent aux colonnes de la table.

Cliquez avec le bouton droit sur le dossier **modèles** , puis sélectionnez **Ajouter** et **nouvel élément**.

Dans la fenêtre Ajouter un nouvel élément, sélectionnez **données** dans le volet gauche et **ADO.NET Entity Data Model** dans les options du volet central. Nommez le nouveau fichier de modèle **ContosoModel**.

Cliquez sur **Ajouter**.

Dans l’Assistant Entity Data Model, sélectionnez le **Concepteur EF à partir de la base de données**.

Cliquez sur **Next**.

Si vous avez défini des connexions de base de données dans votre environnement de développement, vous pouvez voir une de ces connexions présélectionnée. Toutefois, vous souhaitez créer une nouvelle connexion à la base de données que vous avez créée dans la première partie de ce didacticiel. Cliquez sur le bouton **nouvelle connexion** .

Dans la Fenêtre Propriétés de connexion, indiquez le nom du serveur local sur lequel votre base de données a été créée (dans le cas présent, **\ProjectsV13**). Après avoir fourni le nom du serveur, sélectionnez ContosoUniversityData dans les bases de données disponibles.

![définir les propriétés de connexion](creating-the-web-application/_static/image8.png)

Cliquez sur **OK**.

Les propriétés de connexion correctes sont désormais affichées. Vous pouvez utiliser le nom par défaut pour la connexion dans le fichier Web. config.

Cliquez sur **Next**.

Sélectionnez la dernière version de Entity Framework.

Cliquez sur **Next**.

Sélectionnez **tables** pour générer des modèles pour les trois tables.

Cliquez sur **Finish**.

Si vous recevez un avertissement de sécurité, sélectionnez **OK** pour continuer à exécuter le modèle.

Les modèles sont générés à partir des tables de base de données, et un diagramme s’affiche pour afficher les propriétés et les relations entre les tables.

![diagramme du modèle](creating-the-web-application/_static/image11.png)

Le dossier Models comprend désormais de nombreux nouveaux fichiers associés aux modèles qui ont été générés à partir de la base de données.

Le fichier **ContosoModel.Context.cs** contient une classe qui dérive de la classe **DbContext** et fournit une propriété pour chaque classe de modèle qui correspond à une table de base de données. Les fichiers **course.cs**, **Enrollment.cs**et **Student.cs** contiennent les classes de modèle qui représentent les tables de bases de données. Vous allez utiliser la classe de contexte et les classes de modèle lors de l’utilisation de la génération de modèles automatique.

Avant de poursuivre ce didacticiel, générez le projet. Dans la section suivante, vous allez générer du code basé sur les modèles de données, mais cette section ne fonctionnera pas si le projet n’a pas été généré.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Création d’une application Web ASP.NET
> * Modèles générés

Passez au didacticiel suivant pour apprendre à créer du code basé sur les modèles de données.
> [!div class="nextstepaction"]
> [Génération de vues](generating-views.md)
