---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Ajout d’un modèle | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : une version mise à jour de ce didacticiel est disponible ici et utilise ASP.NET MVC 5 et Visual Studio 2013. C’est plus sécurisé, bien plus simple à suivre et à faire une démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a9851d93dde495814f67fa0c807d3534f5f0d8ef
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540305"
---
# <a name="adding-a-model"></a>Ajout d’un modèle

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) et utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sûr, bien plus simple à suivre et qui illustre davantage de fonctionnalités.

Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données. Ces classes seront le modèle de &quot;&quot; partie de l’application ASP.NET MVC.

Vous utiliserez une technologie d’accès aux données .NET Framework appelée [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) pour définir et utiliser ces classes de modèle. Le Entity Framework (souvent appelé EF) prend en charge un paradigme de développement appelé *Code First*. Code First vous permet de créer des objets de modèle en écrivant des classes simples. (Elles sont également appelées classes POCO à partir de &quot;objets CLR ordinaires.&quot;) Vous pouvez ensuite créer la base de données à la volée à partir de vos classes, ce qui permet un flux de travail de développement rapide et rapide.

## <a name="adding-model-classes"></a>Ajout de classes de modèle

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *modèles* , sélectionnez **Ajouter**, puis **classe**.

![](adding-a-model/_static/image1.png)

Entrez le nom de la *classe* &quot;&quot;Movie.

Ajoutez les cinq propriétés suivantes à la classe `Movie` :

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Nous allons utiliser la classe `Movie` pour représenter des films dans une base de données. Chaque instance d’un objet `Movie` correspond à une ligne dans une table de base de données, et chaque propriété de la classe `Movie` sera mappée à une colonne de la table.

Dans le même fichier, ajoutez la classe `MovieDBContext` suivante :

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

La classe `MovieDBContext` représente le contexte de la base de données de films Entity Framework qui gère l’extraction, le stockage et la mise à jour des instances de classe `Movie` dans une base de données. Le `MovieDBContext` dérive de la classe de base `DbContext` fournie par le Entity Framework.

Pour pouvoir référencer `DbContext` et `DbSet`, vous devez ajouter l’instruction `using` suivante en haut du fichier :

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Le fichier *Movie.cs* complet est illustré ci-dessous. (Plusieurs instructions using qui ne sont pas nécessaires ont été supprimées.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Création d’une chaîne de connexion et utilisation de SQL Server LocalDB

La classe `MovieDBContext` que vous avez créée gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de base de données. L’une des questions que vous pouvez poser, cependant, est la façon de spécifier la base de données à laquelle elle doit se connecter. Pour ce faire, vous devez ajouter des informations de connexion dans le fichier *Web. config* de l’application.

Ouvrez le fichier *Web. config* racine de l’application. (Pas le fichier *Web. config* dans le dossier *views* .) Ouvrez le fichier *Web. config* présenté en rouge.

![](adding-a-model/_static/image2.png)

Ajoutez la chaîne de connexion suivante à l’élément `<connectionStrings>` dans le fichier *Web. config* .

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

L’exemple suivant montre une partie du fichier *Web. config* avec la nouvelle chaîne de connexion ajoutée :

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Cette petite quantité de code et XML est tout ce dont vous avez besoin pour écrire, afin de représenter et de stocker les données de film dans une base de données.

Ensuite, vous allez générer une nouvelle classe de `MoviesController` que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer des listes de films.

> [!div class="step-by-step"]
> [Précédent](adding-a-view.md)
> [Suivant](accessing-your-models-data-from-a-controller.md)
