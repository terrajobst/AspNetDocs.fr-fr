---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Créer la couche d’accès aux données | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 0fcf050474a57be9ed53ec0783a6d6b7dde2bf4c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544925"
---
# <a name="create-the-data-access-layer"></a>Créer la couche d’accès aux données

par [Erik Reitan](https://github.com/Erikre)

[Télécharger l’exemple de projet WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Télécharger le livre électronique (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour le Web. Un [projet Visual Studio 2013 avec C# le code source](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.

Ce didacticiel explique comment créer, accéder et examiner les données d’une base de données à l’aide de ASP.NET Web Forms et Entity Framework Code First. Ce didacticiel s’appuie sur le didacticiel précédent « créer le projet » et fait partie de la série de didacticiels sur Wingtip Toys Store. Lorsque vous aurez terminé ce didacticiel, vous aurez créé un groupe de classes d’accès aux données qui se trouvent dans le dossier *modèles* du projet.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment créer les modèles de données.
- Comment initialiser et amorcer la base de données.
- Comment mettre à jour et configurer l’application pour prendre en charge la base de données.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Voici les fonctionnalités introduites dans le didacticiel :

- Entity Framework Code First
- LocalDB
- Annotations de données

## <a name="creating-the-data-models"></a>Création des modèles de données

[Entity Framework](https://msdn.microsoft.com/data/aa937723) est un Framework ORM (Object-Relational Mapping). Elle vous permet d’utiliser des données relationnelles en tant qu’objets, en éliminant la majeure partie du code d’accès aux données que vous auriez normalement besoin d’écrire. À l’aide de Entity Framework, vous pouvez émettre des requêtes à l’aide de [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), puis récupérer et manipuler des données en tant qu’objets fortement typés. LINQ fournit des modèles pour l’interrogation et la mise à jour des données. L’utilisation de Entity Framework vous permet de vous concentrer sur la création du reste de votre application, au lieu de vous concentrer sur les notions de base de l’accès aux données. Plus loin dans cette série de didacticiels, nous allons vous montrer comment utiliser les données pour remplir les requêtes de navigation et de produit.

Entity Framework prend en charge un paradigme de développement appelé *Code First*. Code First vous permet de définir vos modèles de données à l’aide de classes. Une classe est une construction qui vous permet de créer vos propres types personnalisés en regroupant des variables d’autres types, méthodes et événements. Vous pouvez mapper des classes à une base de données existante ou les utiliser pour générer une base de données. Dans ce didacticiel, vous allez créer les modèles de données en écrivant des classes de modèle de données. Ensuite, vous laisserez Entity Framework créer la base de données à la volée à partir de ces nouvelles classes.

Vous allez commencer par créer les classes d’entité qui définissent les modèles de données pour l’application Web Forms. Vous allez ensuite créer une classe de contexte qui gère les classes d’entité et fournit l’accès aux données à la base de données. Vous allez également créer une classe d’initialiseur à utiliser pour remplir la base de données.

### <a name="entity-framework-and-references"></a>Entity Framework et références

Par défaut, Entity Framework est inclus lorsque vous créez une **application Web ASP.net** à l’aide du modèle de **Web Forms** . Entity Framework peuvent être installés, désinstallés et mis à jour en tant que package NuGet.

Ce package NuGet comprend les assemblys de **Runtime** suivants dans votre projet :

- EntityFramework. dll : tout le code d’exécution courant utilisé par Entity Framework
- EntityFramework. SqlServer. dll : fournisseur Microsoft SQL Server pour Entity Framework

### <a name="entity-classes"></a>Classes d’entité

Les classes que vous créez pour définir le schéma des données sont appelées classes d’entité. Si vous ne connaissez pas la conception de base de données, considérez les classes d’entité comme des définitions de table d’une base de données. Chaque propriété de la classe spécifie une colonne dans la table de la base de données. Ces classes fournissent une interface objet-relationnelle légère entre le code orienté objet et la structure de table relationnelle de la base de données.

Dans ce didacticiel, vous allez commencer par ajouter des classes d’entité simples représentant les schémas des produits et des catégories. La classe Products contient des définitions pour chaque produit. Le nom de chacun des membres de la classe Product sera `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`et `Category`. La classe Category contient des définitions pour chaque catégorie à laquelle un produit peut appartenir, tel qu’une voiture, un bateau ou un plan. Le nom de chacun des membres de la classe Category sera `CategoryID`, `CategoryName`, `Description`et `Products`. Chaque produit appartient à l’une des catégories. Ces classes d’entité seront ajoutées au dossier *modèles* existants du projet.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *modèles* , puis sélectionnez **Ajouter** -&gt; **nouvel élément**. 

    ![Créer le menu couche d’accès aux données-nouvel élément](create_the_data_access_layer/_static/image1.png)

   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sous **Visual C#**  dans le volet **installé** sur la gauche, sélectionnez **code**. 

    ![Créer le menu couche d’accès aux données-nouvel élément](create_the_data_access_layer/_static/image2.png)
3. Dans le volet central, sélectionnez **classe** , puis nommez cette nouvelle classe *Product.cs*.
4. Cliquez sur **Ajouter**.  
   Le nouveau fichier de classe s’affiche dans l’éditeur.
5. Remplacez le code par défaut par le code suivant :   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Créez une autre classe en répétant les étapes 1 à 4. Toutefois, nommez la nouvelle classe *Category.cs* et remplacez le code par défaut par le code suivant :  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Comme mentionné précédemment, la classe `Category` représente le type de produit que l’application est conçu pour vendre (par exemple <a id="a"></a> ,&quot;Cars&quot;, &quot;canots&quot;, &quot;fusées&quot;, etc.), et la classe `Product` représente les produits individuels (Toys) dans la base de données. Chaque instance d’un objet `Product` correspond à une ligne dans une table de base de données relationnelle, et chaque propriété de la classe Product est mappée à une colonne de la table de base de données relationnelle. Plus loin dans ce didacticiel, vous allez examiner les données du produit contenues dans la base de données.

### <a name="data-annotations"></a>Annotations de données

Vous avez peut-être remarqué que certains membres des classes ont des attributs qui spécifient des détails sur le membre, par exemple `[ScaffoldColumn(false)]`. Il s’agit d' *Annotations de données*. Les attributs d’annotation de données peuvent décrire comment valider les entrées d’utilisateur pour ce membre, pour spécifier la mise en forme pour celui-ci et pour spécifier la manière dont il est modélisé lors de la création de la base de données.

### <a name="context-class"></a>Context, classe

Pour commencer à utiliser les classes pour l’accès aux données, vous devez définir une classe de contexte. Comme mentionné précédemment, la classe de contexte gère les classes d’entité (telles que la classe `Product` et la classe `Category`) et fournit l’accès aux données à la base de données.

Cette procédure ajoute une nouvelle C# classe de contexte au dossier *Models* .

1. Cliquez avec le bouton droit sur le dossier *modèles* , puis sélectionnez **Ajouter** -&gt; **nouvel élément**.   
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Dans le volet central, sélectionnez **classe** , nommez-le *ProductContext.cs* , puis cliquez sur **Ajouter**.
3. Remplacez le code par défaut contenu dans la classe par le code suivant :   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Ce code ajoute l’espace de noms `System.Data.Entity` afin que vous ayez accès à toutes les fonctionnalités de base de Entity Framework, ce qui inclut la capacité d’interroger, d’insérer, de mettre à jour et de supprimer des données en utilisant des objets fortement typés.

La classe `ProductContext` représente Entity Framework contexte de base de données de produit, qui gère l’extraction, le stockage et la mise à jour des instances de classe `Product` dans la base de données. La classe `ProductContext` dérive de la classe de base `DbContext` fournie par Entity Framework.

### <a name="initializer-class"></a>Classe d’initialiseur

Vous devrez exécuter une logique personnalisée pour initialiser la base de données lors de la première utilisation du contexte. Cela permet d’ajouter des données de départ à la base de données afin que vous puissiez afficher immédiatement les produits et les catégories.

Cette procédure ajoute une nouvelle C# classe d’initialiseur au dossier *Models* .

1. Créez un autre `Class` dans le dossier *Models* et nommez-le *ProductDatabaseInitializer.cs*.
2. Remplacez le code par défaut contenu dans la classe par le code suivant :   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Comme vous pouvez le voir dans le code ci-dessus, lorsque la base de données est créée et initialisée, la propriété `Seed` est remplacée et définie. Lorsque la propriété `Seed` est définie, les valeurs des catégories et produits sont utilisées pour remplir la base de données. Si vous tentez de mettre à jour les données de départ en modifiant le code ci-dessus après la création de la base de données, vous ne verrez aucune mise à jour lorsque vous exécuterez l’application Web. La raison est que le code ci-dessus utilise une implémentation de la classe `DropCreateDatabaseIfModelChanges` pour reconnaître si le modèle (schéma) a changé avant de réinitialiser les données de départ. Si aucune modification n’est apportée au `Category` et `Product` classes d’entité, la base de données ne sera pas réinitialisée avec les données de départ.

> [!NOTE] 
> 
> Si vous souhaitez que la base de données soit recréée chaque fois que vous avez exécuté l’application, vous pouvez utiliser la classe `DropCreateDatabaseAlways` à la place de la classe `DropCreateDatabaseIfModelChanges`. Toutefois, pour cette série de didacticiels, utilisez la classe `DropCreateDatabaseIfModelChanges`.

À ce stade de ce didacticiel, vous aurez un dossier *Models* avec quatre nouvelles classes et une classe par défaut :

![Créer le dossier des modèles de couche d’accès aux données](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Configuration de l’application pour utiliser le modèle de données

Maintenant que vous avez créé les classes qui représentent les données, vous devez configurer l’application pour qu’elle utilise les classes. Dans le fichier *global. asax* , vous ajoutez du code qui initialise le modèle. Dans le fichier *Web. config* , vous ajoutez des informations qui indiquent à l’application la base de données que vous allez utiliser pour stocker les données représentées par les nouvelles classes de données. Le fichier *global. asax* peut être utilisé pour gérer des événements d’application ou des méthodes. Le fichier *Web. config* vous permet de contrôler la configuration de votre application Web ASP.net.

#### <a name="updating-the-globalasax-file"></a>Mise à jour du fichier global. asax

Pour initialiser les modèles de données au démarrage de l’application, vous devez mettre à jour le gestionnaire de `Application_Start` dans le fichier *global.asax.cs* .

> [!NOTE] 
> 
> Dans Explorateur de solutions, vous pouvez sélectionner le fichier *global. asax* ou le fichier *global.asax.cs* pour modifier le fichier *global.asax.cs* .

1. Ajoutez le code suivant mis en surbrillance en jaune à la méthode `Application_Start` dans le fichier *global.asax.cs* .   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Votre navigateur doit prendre en charge HTML5 pour afficher le code mis en surbrillance en jaune lors de l’affichage de cette série de didacticiels dans un navigateur.

Comme indiqué dans le code ci-dessus, lorsque l’application démarre, l’application spécifie l’initialiseur qui s’exécutera pendant la première opération d’accès aux données. Les deux espaces de noms supplémentaires sont requis pour accéder à l’objet `Database` et à l’objet `ProductDatabaseInitializer`.

 Modification du fichier Web. config 

Bien que Entity Framework Code First génère automatiquement une base de données dans un emplacement par défaut lorsque la base de données est remplie avec des données de départ, l’ajout de vos propres informations de connexion à votre application vous permet de contrôler l’emplacement de la base de données. Vous spécifiez cette connexion de base de données à l’aide d’une chaîne de connexion dans le fichier *Web. config* de l’application à la racine du projet. En ajoutant une nouvelle chaîne de connexion, vous pouvez diriger l’emplacement de la base de données (*wingtiptoys. mdf*) à générer dans le répertoire de données de l’application (données de l’application *\_* ), plutôt que dans son emplacement par défaut. Cette modification vous permettra de rechercher et d’inspecter le fichier de base de données plus loin dans ce didacticiel.

1. Dans **Explorateur de solutions**, recherchez et ouvrez le fichier *Web. config* .
2. Ajoutez la chaîne de connexion mise en surbrillance en jaune à la section `<connectionStrings>` du fichier *Web. config* , comme suit :  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Lorsque l’application est exécutée pour la première fois, elle génère la base de données à l’emplacement spécifié par la chaîne de connexion. Mais avant d’exécuter l’application, créons-la d’abord.

## <a name="building-the-application"></a>Génération de l'application

Pour vous assurer que toutes les classes et modifications apportées à votre application Web fonctionnent, vous devez générer l’application.

1. Dans le menu **Déboguer** , sélectionnez **générer WingtipToys**.  
 La fenêtre **sortie** s’affiche, et si tout s’est bien passé, un message indiquant que l’opération *a réussi* s’affiche.  

    ![Créer les fenêtres de sortie de couche d’accès aux données](create_the_data_access_layer/_static/image4.png)

Si vous rencontrez une erreur, revérifiez les étapes ci-dessus. Les informations de la fenêtre **sortie** indiquent quel fichier présente un problème et où une modification est nécessaire dans le fichier. Ces informations vous permettront de déterminer quelle partie des étapes ci-dessus doit être examinée et corrigée dans votre projet.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel de la série, vous avez créé le modèle de données, ainsi que, vous avez ajouté le code qui sera utilisé pour initialiser et amorcer la base de données. Vous avez également configuré l’application pour utiliser les modèles de données lors de l’exécution de l’application.

Dans le didacticiel suivant, vous allez mettre à jour l’interface utilisateur, ajouter la navigation et récupérer des données de la base de données. Cela entraînera la création automatique de la base de données en fonction des classes d’entité que vous avez créées dans ce didacticiel.

## <a name="additional-resources"></a>Ressources supplémentaires

[Vue d’ensemble de Entity Framework](https://msdn.microsoft.com/library/bb399567.aspx)   
[Guide du débutant en ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[Développement Code First avec Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (vidéo)   
[API Fluent des relations Code First](https://msdn.microsoft.com/data/hh134698)   
[Annotations de données Code First](https://msdn.microsoft.com/data/gg193958)  
[Améliorations de la productivité pour la Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Précédent](create-the-project.md)
> [Suivant](ui_and_navigation.md)
