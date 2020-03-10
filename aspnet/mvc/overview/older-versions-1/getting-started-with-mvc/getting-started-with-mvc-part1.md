---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introduction à ASP.NET MVC | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Créer une application Web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: f8f0014776ba1313119e8c39c63a216b0fc864e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581584"
---
# <a name="intro-to-aspnet-mvc"></a>Introduction à ASP.NET MVC

par [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Une version mise à jour si ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) à l’aide de [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). Le nouveau didacticiel utilise ASP.NET MVC 5, qui offre de nombreuses améliorations par rapport à ce didacticiel.
>
>
> Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données. Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.

Nous allons créer notre première application Web MVC ASP.NET à l’aide de [Visual Web developer 2010 Express](https://www.microsoft.com/express/Web/). Nous allons créer une petite application de liste de films qui nous permettra de créer et de répertorier des films.

## <a name="what-youll-build"></a>Contenu

Voici deux captures d’écran de l’application que vous allez générer. Vous disposerez d’un simple tableau de films avec différentes colonnes.

[Liste des films ![-Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

Et vous disposerez d’un formulaire créer pour pouvoir ajouter des films à la liste.

[![créer un film-Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Compétences

Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Visual Studio. Vous apprendrez ce qui suit :

- Comment créer un nouveau projet MVC ASP.NET
- Comment créer une base de données avec SQL Server
- Comment créer des contrôleurs et des vues MVC ASP.NET
- Comment récupérer et afficher des données
- Comment modifier des données et activer la validation des données
- Comment mettre à jour le schéma de base de données

## <a name="get-started"></a>Pour commencer

Commencez par exécuter Visual Web Developer 2010 Express (je l’appellerai « VWD existant » à partir de maintenant) et sélectionnez Nouveau projet dans l’écran d’accueil.

Visual Web Developer est un environnement de développement intégré (IDE). Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications. Une barre d’outils située en haut montre les différentes options disponibles, ainsi que le menu que vous pouvez également utiliser pour sélectionner fichier | Nouveau projet.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Création de votre première application

Vous pouvez créer des applications à l’aide C#de Visual Basic ou Visual. Pour le moment, sélectionnez C# visuel à gauche, puis choisissez « Application Web ASP.NET MVC 2 ». Nommez le projet « films », puis cliquez sur OK.

[![un nouveau projet](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Dans la partie droite se trouve le Explorateur de solutions qui montre tous les fichiers et dossiers de votre application. La grande fenêtre au milieu est l’endroit où vous modifiez votre code et passez la majeure partie de votre temps. Visual Studio utilisait un modèle par défaut pour le projet MVC ASP.NET que vous venez de créer, ce qui vous permet de disposer d’une application fonctionnelle sans rien faire ! Il s’agit d’un simple «Hello World ! et c’est un bon point de départ pour notre application.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Sélectionnez le bouton « lecture » dans la barre d’outils.

![Démarrer le débogage](getting-started-with-mvc-part1/_static/image11.png)

Il s’agit d’une flèche verte pointant vers la droite qui compilera votre programme et démarrera votre application dans un navigateur Web.

*Remarque : vous pouvez appuyer plutôt sur la touche F5 de votre clavier, ou sélectionner déboguer-&gt;démarrer le débogage à partir du menu Déboguer.*

Visual Web Developer démarrera donc un serveur Web de développement et exécutera l’application Web (aucune étape de configuration ou manuelle n’est requise pour activer cette opération). Il lance ensuite un navigateur et le configure pour parcourir la page d’hébergement de l’application. Notez ci-dessous que la barre d’adresses du navigateur indique « localhost », et non pas une telle example.com. C’est parce que localhost pointe toujours vers votre propre ordinateur local, ce qui, dans ce cas, exécute l’application que nous venons de créer.

[Page d’espace ![](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Ce modèle par défaut vous donne deux pages à visiter et une page de connexion de base. Nous allons modifier le fonctionnement de cette application et apprendre un peu sur ASP.NET MVC dans le processus. Fermez votre navigateur et modifiez du code.

> [!div class="step-by-step"]
> [Next](getting-started-with-mvc-part2.md)
