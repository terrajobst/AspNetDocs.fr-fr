---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Présentation de ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Une version mise à jour si ce didacticiel est disponible ici à l’aide de Visual Studio 2013. Le nouveau didacticiel utilise ASP.NET MVC 5, qui offre de nombreuses améliorations par rapport à t...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 51709a9c6ddb39b8fcd1cd94cd08d530a595825a
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455540"
---
# <a name="intro-to-aspnet-mvc-4"></a>Introduction à ASP.NET MVC

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Une version mise à jour si ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) à l’aide de [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). Le nouveau didacticiel utilise ASP.NET MVC 5, qui offre de nombreuses améliorations par rapport à ce didacticiel.
>
> Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC 4 à l’aide de Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) ou de Visual Web developer 2010 Express Service Pack 1. Visual Studio 2012 est recommandé, vous n’avez pas besoin d’installer quoi que ce soit pour suivre le didacticiel. Si vous utilisez Visual Studio 2010, vous devez installer les composants ci-dessous. Vous pouvez les installer en cliquant sur les liens suivants :
>
> - [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Programme d’installation WPI pour ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
>
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez le [programme d’installation WPI pour ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) et les éléments suivants : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
>
> Un projet Visual Web Developer C# avec le code source est disponible pour accompagner cette rubrique. [Téléchargez la C# version](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
>
> Dans le didacticiel, vous exécutez l’application dans Visual Studio. Vous pouvez également rendre l’application disponible sur Internet en la déployant sur un fournisseur d’hébergement. Microsoft offre un hébergement Web gratuit pour un maximum de 10 sites Web dans un [compte d’essai gratuit de Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Pour plus d’informations sur le déploiement d’un projet Web Visual Studio sur un site Web Windows Azure, consultez [créer et déployer un site web ASP.net et SQL Database avec Visual Studio](https://docs.microsoft.com/dotnet/azure/). Ce didacticiel montre également comment utiliser Migrations Entity Framework Code First pour déployer votre base de données SQL Server sur Windows Azure SQL Database (anciennement SQL Azure).
>
> Ce didacticiel a été rédigé par Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ).

## <a name="what-youll-build"></a>Contenu

> [!NOTE]
> Une version mise à jour si ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) à l’aide de [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). Le nouveau didacticiel utilise ASP.NET MVC 5, qui offre de nombreuses améliorations par rapport à ce didacticiel.

Vous allez implémenter une simple application de liste de films qui prend en charge la création, la modification, la recherche et l’affichage des films à partir d’une base de données. Vous trouverez ci-dessous deux captures d’écran de l’application que vous allez générer. Il comprend une page qui affiche une liste de films d’une base de données :

![](intro-to-aspnet-mvc-4/_static/image1.png)

L’application vous permet également d’ajouter, de modifier et de supprimer des films, ainsi que d’afficher des détails sur ceux-ci. Tous les scénarios d’entrée de données incluent la validation pour garantir que les données stockées dans la base de données sont correctes.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Mise en route

Commencez par exécuter Visual Studio Express 2012 ou Visual Web Developer 2010 Express. La plupart des captures d’écran de cette série utilisent Visual Studio Express 2012, mais vous pouvez suivre ce didacticiel avec Visual Studio 2010/SP1, Visual Studio 2012, Visual Studio Express 2012 ou Visual Web Developer 2010 Express. Sélectionnez **nouveau projet** dans la page de **démarrage** .

Visual Studio est un environnement de développement intégré ou IDE. Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications. Dans Visual Studio, il y a une barre d’outils en haut de la vue qui présente les différentes options disponibles. Il y a également un menu qui offre une autre façon d’effectuer des tâches dans l’IDE. (Par exemple, au lieu de sélectionner **nouveau projet** dans la page de **démarrage** , vous pouvez utiliser le menu et sélectionner **fichier** &gt; **nouveau projet**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Création de votre première application

Vous pouvez créer des applications à l’aide de C# Visual Basic ou d’un visuel comme langage de programmation. Sélectionnez visuel C# sur la gauche, puis sélectionnez **application Web ASP.NET MVC 4**. Nommez votre projet &quot;MvcMovie&quot; puis cliquez sur **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez **application Internet**. Laissez **Razor** comme moteur d’affichage par défaut.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Cliquez sur **OK**. Visual Studio utilisait un modèle par défaut pour le projet MVC ASP.NET que vous venez de créer, ce qui vous permet de disposer d’une application fonctionnelle sans rien faire ! Il s’agit d’un &quot;simple Hello World !&quot; projet et c’est un bon point de départ pour démarrer votre application.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Dans le menu **Déboguer**, sélectionnez **Démarrer le débogage**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Notez que le raccourci clavier permettant de démarrer le débogage est F5.

F5 provoque le démarrage de Visual Studio IIS Express et l’exécution de votre application Web. Visual Studio lance ensuite un navigateur et ouvre la page d’hébergement de l’application. Notez que la barre d’adresses du navigateur indique `localhost` et non `example.com`. En effet, `localhost` pointe toujours vers votre propre ordinateur local, ce qui, dans ce cas, exécute l’application que vous venez de créer. Lorsque Visual Studio exécute un projet Web, un port aléatoire est utilisé pour le serveur Web. Dans l’image ci-dessous, le numéro de port est 41788. Lorsque vous exécutez l’application, vous verrez probablement un autre numéro de port.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Par défaut, ce modèle par défaut vous donne des pages de démarrage, de contact et à propos de. Il prend également en charge l’inscription et la connexion, ainsi que des liens vers Facebook et Twitter. L’étape suivante consiste à modifier le fonctionnement de cette application et à en savoir plus sur ASP.NET MVC. Fermez votre navigateur et modifiez du code.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
