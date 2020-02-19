---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Introduction à ASP.NET MVC 3 (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 24f7de303ef7f5a457bd509ecc6bd0e3be7e3d9d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456315"
---
# <a name="intro-to-aspnet-mvc-3-vb"></a>Introduction à ASP.NET MVC 3 (VB)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :
> 
> - [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez C#, passez à la [ C# version](../cs/intro-to-aspnet-mvc-3.md) de ce didacticiel.

Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :

- [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)

Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Un projet Visual Web Developer avec du code source VB est disponible pour accompagner cette rubrique. [Téléchargez la version VB ici](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Si vous préférez CSharp, passez à la [version CSharp](../cs/intro-to-aspnet-mvc-3.md) de ce didacticiel.

## <a name="what-youll-build"></a>Contenu

Vous allez implémenter une simple application de liste de films qui prend en charge la création, la modification et l’affichage de films à partir d’une base de données. Vous trouverez ci-dessous deux captures d’écran de l’application que vous allez générer. Il comprend une page qui affiche une liste de films d’une base de données :

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

L’application vous permet également d’ajouter, de modifier et de supprimer des films, ainsi que d’afficher des détails sur ceux-ci. Tous les scénarios d’entrée de données incluent la validation pour garantir que les données stockées dans la base de données sont correctes.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Compétences

Vous apprendrez les compétences suivantes :

- Comment créer un nouveau projet MVC ASP.NET
- Comment créer une base de données à l’aide d’Entity Framework code-First
- Comment créer des contrôleurs et des vues MVC ASP.NET
- Comment récupérer et afficher des données
- Comment modifier des données et activer la validation des données

## <a name="getting-started"></a>Mise en route

Commencez par exécuter Visual Web Developer 2010 Express (« VWD existant » pour le raccourci), puis sélectionnez **nouveau projet** dans la page de **démarrage** .

Visual Web Developer est un environnement de développement intégré ou IDE. Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications. Dans Visual Web Developer, il existe une barre d’outils en haut de la vue qui présente les différentes options disponibles. Il y a également un menu qui offre une autre façon d’effectuer des tâches dans l’IDE. (Par exemple, au lieu de sélectionner **nouveau projet** dans la page de **démarrage** , vous pouvez utiliser le menu et sélectionner **fichier** &gt; **nouveau projet**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Création de votre première application

Vous pouvez créer des applications à l’aide de votre choix, C# Visual Basic ou visuel, en tant que langage de programmation. Pour ce didacticiel, sélectionnez Visual Basic sur la gauche, puis sélectionnez **application Web ASP.NET MVC 3**. Nommez votre projet « MvcMovie », puis cliquez sur **OK**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

Dans la boîte de dialogue **nouveau projet ASP.NET MVC 3** , sélectionnez **application Internet**. Laissez **Razor** comme moteur d’affichage par défaut.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Cliquez sur **OK**. Visual Web Developer a utilisé un modèle par défaut pour le projet MVC ASP.NET que vous venez de créer. vous disposez donc d’une application fonctionnelle sans rien faire ! Il s’agit d’un simple « Hello World ! » et c’est un bon point de départ pour démarrer votre application.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Dans le menu **Déboguer**, sélectionnez **Démarrer le débogage**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Notez que le raccourci clavier permettant de démarrer le débogage est F5.

F5 force Visual Web Developer à démarrer un serveur Web de développement et à exécuter votre application Web. VWD existant lance ensuite un navigateur et ouvre la page d’hébergement de l’application. Notez que la barre d’adresses du navigateur indique `localhost` et non `example.com`. En effet, `localhost` pointe toujours vers votre propre ordinateur local, ce qui, dans ce cas, exécute l’application que vous venez de créer. Quand VWD existant exécute un projet Web, un port aléatoire est utilisé pour le projet. Dans l’image ci-dessous, le numéro de port aléatoire est 43246. Votre projet utilisera probablement un numéro de port différent.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Ce modèle par défaut vous donne deux pages à visiter et une page de connexion de base. Nous allons modifier le fonctionnement de cette application et apprendre un peu sur ASP.NET MVC dans le processus. Fermez votre navigateur et modifiez du code.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
