---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Ajout d’un modèle (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e69d59aed4d74f08f1c653c4965b128c4dbe20ff
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457243"
---
# <a name="adding-a-model-vb"></a>Ajout d’un modèle (VB)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :
> 
> - [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez C#, passez à la [ C# version](../cs/adding-a-model.md) de ce didacticiel.

## <a name="adding-a-model"></a>Ajout d’un modèle

Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données. Ces classes sont la partie « modèle » de l’application ASP.NET MVC.

Vous utiliserez une technologie d’accès aux données .NET Framework appelée Entity Framework pour définir et utiliser ces classes de modèle. Le Entity Framework (souvent appelé EF) prend en charge un paradigme de développement appelé *Code First*. Code First vous permet de créer des objets de modèle en écrivant des classes simples. (Elles sont également appelées « classes POCO » à partir d’objets CLR « Plain-Old ».) Vous pouvez ensuite créer la base de données à la volée à partir de vos classes, ce qui permet un flux de travail de développement rapide et rapide.

## <a name="adding-model-classes"></a>Ajout de classes de modèle

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *modèles* , sélectionnez **Ajouter**, puis **classe**.

![](adding-a-model/_static/image1.png)

Nommez la classe « Movie ».

Ajoutez les cinq propriétés suivantes à la classe `Movie` :

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Nous allons utiliser la classe `Movie` pour représenter des films dans une base de données. Chaque instance d’un objet `Movie` correspond à une ligne dans une table de base de données, et chaque propriété de la classe `Movie` sera mappée à une colonne de la table.

Dans le même fichier, ajoutez la classe `MovieDBContext` suivante :

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

La classe `MovieDBContext` représente le contexte de la base de données de films Entity Framework qui gère l’extraction, le stockage et la mise à jour des instances de classe `Movie` dans une base de données. Le `MovieDBContext` dérive de la classe de base `DbContext` fournie par le Entity Framework. Pour plus d’informations sur les `DbContext` et les `DbSet`, consultez [améliorations de la productivité pour le Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Pour pouvoir référencer `DbContext` et `DbSet`, vous devez ajouter l’instruction `imports` suivante en haut du fichier :

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

Le fichier *Movie. vb* complet est illustré ci-dessous.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Création d’une chaîne de connexion et utilisation de SQL Server Compact

La classe `MovieDBContext` que vous avez créée gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de base de données. L’une des questions que vous pouvez poser, cependant, est la façon de spécifier la base de données à laquelle elle doit se connecter. Pour ce faire, vous devez ajouter des informations de connexion dans le fichier *Web. config* de l’application.

Ouvrez le fichier *Web. config* racine de l’application. (Pas le fichier *Web. config* dans le dossier *views* .) L’image ci-dessous affiche les deux fichiers *Web. config* ; Ouvrez le fichier *Web. config* entouré d’un cercle rouge.

![](adding-a-model/_static/image2.png)

Ajoutez la chaîne de connexion suivante à l’élément `<connectionStrings>` dans le fichier *Web. config* .

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

L’exemple suivant montre une partie du fichier *Web. config* avec la nouvelle chaîne de connexion ajoutée :

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Cette petite quantité de code et XML est tout ce dont vous avez besoin pour écrire, afin de représenter et de stocker les données de film dans une base de données.

Ensuite, vous allez générer une nouvelle classe de `MoviesController` que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer des listes de films.

> [!div class="step-by-step"]
> [Précédent](adding-a-view.md)
> [Suivant](accessing-your-models-data-from-a-controller.md)
