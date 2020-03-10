---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Afficher les éléments de données et les détails | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous montrera les bases de la création d’une application ASP.NET Web Forms avec ASP.NET 4,7 et Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 130c9ffd29df612dac5bb954830a2eb9b738aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641112"
---
# <a name="display-data-items-and-details"></a>Afficher les éléments de données et les détails

par [Erik Reitan](https://github.com/Erikre)

> Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms avec ASP.NET 4,7 et Microsoft Visual Studio 2017.

Dans ce didacticiel, vous allez apprendre à afficher les éléments de données et les détails de l’élément de données avec ASP.NET Web Forms et Entity Framework Code First. Ce didacticiel s’appuie sur le didacticiel « UI and navigation » précédent dans le cadre de la série de didacticiels pour Wingtip Toys Store. À l’issue de ce didacticiel, vous verrez des produits sur la page *ProductsList. aspx* et les détails d’un produit sur la page *ProductDetails. aspx* .

## <a name="youll-learn-how-to"></a>Vous allez apprendre à :

- Ajouter un contrôle de données pour afficher les produits de la base de données
- Connecter un contrôle de données aux données sélectionnées
- Ajouter un contrôle de données pour afficher les détails du produit à partir de la base de données
- Récupérer une valeur à partir de la chaîne de requête et utiliser cette valeur pour limiter les données récupérées de la base de données

### <a name="features-introduced-in-this-tutorial"></a>Fonctionnalités introduites dans ce didacticiel :

- Liaison de données
- Fournisseurs de valeurs

## <a name="add-a-data-control"></a>Ajouter un contrôle de données

Vous pouvez utiliser différentes options pour lier des données à un contrôle serveur. Les plus courants sont les suivants :

* Ajout d’un contrôle de source de données
* Ajout de code à la main
* Utilisation de la liaison de modèle

### <a name="use-a-data-source-control-to-bind-data"></a>Utiliser un contrôle de source de données pour lier des données

L’ajout d’un contrôle de source de données vous permet de lier le contrôle de source de données au contrôle qui affiche les données. Avec cette approche, vous pouvez, plutôt que par programmation, connecter des contrôles côté serveur à des sources de données.

### <a name="code-by-hand-to-bind-data"></a>Coder à la main pour lier des données

Le codage à la main implique :

1. Lecture d’une valeur
2. Vérification de la présence de null
3. Conversion en type approprié
4. Vérification réussie de la conversion
5. Utilisation de la valeur dans la requête 

Cette approche vous permet d’avoir un contrôle total sur votre logique d’accès aux données.

### <a name="use-model-binding-to-bind-data"></a>Utiliser la liaison de modèle pour lier des données

La liaison de modèle vous permet de lier des résultats avec beaucoup moins de code et vous donne la possibilité de réutiliser les fonctionnalités dans votre application. Il simplifie l’utilisation de la logique d’accès aux données axée sur le code tout en fournissant une infrastructure de liaison de données riche.

## <a name="display-products"></a>Afficher les produits

Dans ce didacticiel, vous allez utiliser la liaison de modèle pour lier des données. Pour configurer un contrôle de données afin d’utiliser la liaison de modèle pour sélectionner des données, vous affectez à la propriété `SelectMethod` du contrôle un nom de méthode dans le code de la page. Le contrôle de données appelle la méthode au moment approprié dans le cycle de vie de la page et lie automatiquement les données retournées. Il n’est pas nécessaire d’appeler explicitement la méthode `DataBind`.

1. Dans **Explorateur de solutions**, ouvrez *ProductList. aspx*.
2. Remplacez le balisage existant par le balisage suivant :   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Ce code utilise un contrôle **ListView** nommé `productList` pour afficher les produits.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Avec les modèles et les styles, vous définissez la manière dont le contrôle **ListView** affiche des données. Elle est utile pour les données de toute structure répétée. Bien que cet exemple de **ListView** affiche simplement des données de base de données, vous pouvez également, sans code, permettre aux utilisateurs de modifier, d’insérer et de supprimer des données, ainsi que de trier et de paginer des données.

En définissant la propriété `ItemType` dans le contrôle **ListView** , l’expression de liaison de données `Item` est disponible et le contrôle est fortement typé. Comme mentionné dans le didacticiel précédent, vous pouvez sélectionner les détails de l’objet d’élément avec IntelliSense, par exemple en spécifiant les `ProductName`:

![Afficher les éléments de données et les détails-IntelliSense](display_data_items_and_details/_static/image1.png)

Vous utilisez également la liaison de modèle pour spécifier une valeur de `SelectMethod`. Cette valeur (`GetProducts`) correspond à la méthode que vous ajouterez au code-behind pour afficher les produits à l’étape suivante.

### <a name="add-code-to-display-products"></a>Ajouter du code pour afficher les produits

Dans cette étape, vous allez ajouter du code pour remplir le contrôle **ListView** avec les données de produit de la base de données. Le code prend en charge l’indication de tous les produits et de chaque catégorie.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *ProductList. aspx* , puis sélectionnez **afficher le code**.
2. Remplacez le code existant dans le fichier *ProductList.aspx.cs* par ce qui suit :   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Ce code montre la méthode `GetProducts` que la propriété du contrôle **ListView** `ItemType` référence dans la page *ProductList. aspx* . Pour limiter les résultats à une catégorie de base de données spécifique, le code définit la valeur de la `categoryId` à partir de la valeur de la chaîne de requête transmise à la page *ProductList. aspx* lorsque la page *ProductList. aspx* est parcourue. La classe `QueryStringAttribute` de l’espace de noms `System.Web.ModelBinding` est utilisée pour récupérer la valeur de la variable de chaîne de requête `id`. Cela indique à la liaison de modèle d’essayer de lier une valeur de la chaîne de requête au paramètre `categoryId` au moment de l’exécution.

Lorsqu’une catégorie valide est transmise en tant que chaîne de requête à la page, les résultats de la requête sont limités aux produits de la base de données qui correspondent à la valeur `categoryId`. Par exemple, si l’URL de la page *ProductsList. aspx* est la suivante :

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

La page affiche uniquement les produits dont le `categoryId` est égal à `1`.

Tous les produits sont affichés si aucune chaîne de requête n’est incluse lorsque la page *ProductList. aspx* est appelée.

Les sources de valeurs pour ces méthodes sont appelées *fournisseurs* de valeurs (telles que *QueryString*) et les attributs de paramètres qui indiquent le fournisseur de valeur à utiliser sont appelés *attributs de fournisseur de valeur* (par exemple, `id`). ASP.NET comprend des fournisseurs de valeurs et des attributs correspondants pour toutes les sources typiques de l’entrée utilisateur dans une application Web Forms, telle que la chaîne de requête, les cookies, les valeurs de formulaire, les contrôles, l’état d’affichage, l’état de session et les propriétés de profil. Vous pouvez également écrire des fournisseurs de valeurs personnalisés.

### <a name="run-the-application"></a>Exécuter l'application

Exécutez l’application maintenant pour afficher tous les produits ou les produits d’une catégorie.

1. Appuyez sur **F5** dans Visual Studio pour exécuter l’application.  
   Le navigateur s’ouvre et affiche la page *default. aspx* .

2. Sélectionnez **Cars** dans le menu de navigation des catégories de produits.  
   La page *ProductList. aspx* affiche uniquement les produits de catégorie **Cars** . Plus loin dans ce didacticiel, vous allez afficher les détails du produit.  

    ![Afficher les éléments de données et les détails-voitures](display_data_items_and_details/_static/image2.png)

3. Sélectionnez **produits** dans le menu de navigation en haut.  
   Là encore, la page *ProductList. aspx* s’affiche, mais cette fois-ci, elle affiche la liste complète des produits.   

    ![Afficher les éléments de données et les détails-produits](display_data_items_and_details/_static/image3.png)

4. Fermez le navigateur et revenez à Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Ajouter un contrôle de données pour afficher les détails du produit

Ensuite, vous allez modifier le balisage de la page *ProductDetails. aspx* que vous avez ajoutée dans le didacticiel précédent pour afficher des informations spécifiques sur le produit.

1. Dans **Explorateur de solutions**, ouvrez *ProductDetails. aspx*.

2. Remplacez le balisage existant par le balisage suivant :

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    Ce code utilise un contrôle **FormView** pour afficher des détails spécifiques sur un produit. Ce balisage utilise des méthodes comme les méthodes utilisées pour afficher des données dans la page *ProductList. aspx* . Le contrôle **FormView** est utilisé pour afficher un seul enregistrement à la fois à partir d’une source de données. Quand vous utilisez le contrôle **FormView** , vous créez des modèles pour afficher et modifier des valeurs liées aux données. Ces modèles contiennent des contrôles, des expressions de liaison et une mise en forme qui définissent l’apparence et les fonctionnalités du formulaire.

La connexion du balisage précédent à la base de données nécessite du code supplémentaire.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *ProductDetails. aspx* , puis cliquez sur **afficher le code**.  
   Le fichier *ProductDetails.aspx.cs* est affiché.

2. Remplacez le code existant par le code suivant :   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Ce code recherche une valeur de chaîne de requête «`productID`». Si une valeur de chaîne de requête valide est trouvée, le produit correspondant est affiché. Si la chaîne de requête est introuvable ou si sa valeur n’est pas valide, aucun produit n’est affiché.

### <a name="run-the-application"></a>Exécuter l'application

Vous pouvez maintenant exécuter l’application pour voir un produit individuel affiché en fonction de l’ID de produit.

1. Appuyez sur **F5** dans Visual Studio pour exécuter l’application.  
   Le navigateur s’ouvre et affiche la page *default. aspx* .

2. Sélectionnez **bateaux** dans le menu de navigation de la catégorie.  
   La page *ProductList. aspx* s’affiche.

3. Sélectionnez **livre papier** dans la liste des produits.
   La page *ProductDetails. aspx* s’affiche.

    ![Afficher les éléments de données et les détails-produits](display_data_items_and_details/_static/image4.png)
    
4. Fermez le navigateur.

## <a name="additional-resources"></a>Ressources supplémentaires

[Récupération et affichage de données avec la liaison de modèle et les Web Forms](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez ajouté le balisage et le code pour afficher les produits et les détails du produit. Vous avez appris à propos des contrôles de données fortement typés, la liaison de modèle et les fournisseurs de valeurs. Dans le didacticiel suivant, vous allez ajouter un panier à l’exemple d’application Wingtip Toys. 

> [!div class="step-by-step"]
> [Précédent](ui_and_navigation.md)
> [Suivant](shopping-cart.md)
