---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Panier d’achat | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: d3b619ebd9448d30857ffbaf17fd245b1d54a662
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519295"
---
# <a name="shopping-cart"></a>Panier d'achat

par [Erik Reitan](https://github.com/Erikre)

[Télécharger l’exemple de projet WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Télécharger le livre électronique (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour le Web. Un [projet Visual Studio 2013 avec C# le code source](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.

Ce didacticiel décrit la logique métier requise pour ajouter un panier d’achat à l’exemple d’application ASP.NET Web Forms de Wingtip Toys. Ce didacticiel s’appuie sur le didacticiel précédent « afficher les éléments de données et les détails » et fait partie de la série de didacticiels sur Wingtip Toys Store. Lorsque vous aurez terminé ce didacticiel, les utilisateurs de votre exemple d’application pourront ajouter, supprimer et modifier les produits dans leur panier d’achat.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

1. Comment créer un panier d’achat pour l’application Web.
2. Comment permettre aux utilisateurs d’ajouter des éléments au panier d’achat.
3. Comment ajouter un contrôle [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) pour afficher les détails du panier d’achat.
4. Comment calculer et afficher le total de la commande.
5. Comment supprimer et mettre à jour des éléments dans le panier d’achat.
6. Comment inclure un compteur de panier d’achat.

## <a name="code-features-in-this-tutorial"></a>Fonctionnalités de code dans ce didacticiel :

1. Entity Framework Code First
2. Annotations de données
3. Contrôles de données fortement typés
4. Liaison de données

## <a name="creating-a-shopping-cart"></a>Création d’un panier d’achat

Plus haut dans cette série de didacticiels, vous avez ajouté des pages et du code pour afficher les données de produit d’une base de données. Dans ce didacticiel, vous allez créer un panier d’achat pour gérer les produits que les utilisateurs souhaitent acheter. Les utilisateurs peuvent parcourir et ajouter des éléments au panier d’achat même s’ils ne sont pas inscrits ou connectés. Pour gérer l’accès au panier d’achat, vous affectez aux utilisateurs un `ID` unique à l’aide d’un identificateur global unique (GUID) lorsque l’utilisateur accède pour la première fois au panier d’achat. Vous allez stocker ce `ID` à l’aide de l’état de session ASP.NET.

> [!NOTE] 
> 
> L’état de session ASP.NET est un emplacement pratique pour stocker des informations spécifiques à l’utilisateur, qui expirent une fois que l’utilisateur a quitté le site. Bien qu’une mauvaise utilisation de l’état de session puisse avoir un impact sur les performances sur les sites plus importants, l’utilisation légère de l’état de session fonctionne bien à des fins de démonstration. L’exemple de projet Wingtip Toys montre comment utiliser l’état de session sans fournisseur externe, où l’état de session est stocké in-process sur le serveur Web qui héberge le site. Pour les sites plus importants qui fournissent plusieurs instances d’une application ou pour les sites qui exécutent plusieurs instances d’une application sur des serveurs différents, envisagez d’utiliser **Windows Azure cache service**. Cette Cache Service fournit un service de mise en cache distribué qui est externe au site Web et résout le problème lié à l’utilisation de l’état de session in-process. Pour plus d’informations, consultez [utilisation de l’état de Session ASP.net avec les sites Web Windows Azure](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).

### <a name="add-cartitem-as-a-model-class"></a>Ajouter CartItem en tant que classe de modèle

Plus haut dans cette série de didacticiels, vous avez défini le schéma pour les données de catégorie et de produit en créant les classes `Category` et `Product` dans le dossier *modèles* . À présent, ajoutez une nouvelle classe pour définir le schéma du panier d’achat. Plus loin dans ce didacticiel, vous allez ajouter une classe pour gérer l’accès aux données à la table `CartItem`. Cette classe fournit la logique métier pour ajouter, supprimer et mettre à jour des éléments dans le panier d’achat.

1. Cliquez avec le bouton droit sur le dossier *modèles* , puis sélectionnez **Ajouter** -&gt; **nouvel élément**. 

    ![Panier d’achat-nouvel élément](shopping-cart/_static/image1.png)
2. La boîte de dialogue **Ajouter un nouvel élément** s’affiche. Sélectionnez **code**, puis **classe**. 

    ![Panier d’achat-boîte de dialogue Ajouter un nouvel élément](shopping-cart/_static/image2.png)
3. Nommez cette nouvelle classe *CartItem.cs*.
4. Cliquez sur **Ajouter**.  
   Le nouveau fichier de classe s’affiche dans l’éditeur.
5. Remplacez le code par défaut par le code suivant :   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

La classe `CartItem` contient le schéma qui définit chaque produit qu’un utilisateur ajoute au panier d’achat. Cette classe est similaire aux autres classes de schéma que vous avez créées précédemment dans cette série de didacticiels. Par Convention, Entity Framework Code First s’attend à ce que la clé primaire de la table `CartItem` soit `CartItemId` ou `ID`. Toutefois, le code remplace le comportement par défaut à l’aide de l’attribut de `[Key]` d’annotation de données. L’attribut `Key` de la propriété ItemId spécifie que la propriété `ItemID` est la clé primaire.

La propriété `CartId` spécifie le `ID` de l’utilisateur associé à l’élément à acheter. Vous allez ajouter du code pour créer cet utilisateur `ID` lorsque l’utilisateur accède au panier d’achat. Cette `ID` est également stockée en tant que variable de session ASP.NET.

### <a name="update-the-product-context"></a>Mettre à jour le contexte du produit

Outre l’ajout de la classe `CartItem`, vous devrez mettre à jour la classe de contexte de base de données qui gère les classes d’entité et qui fournit l’accès aux données à la base de données. Pour ce faire, vous allez ajouter la classe de modèle de `CartItem` nouvellement créée à la classe `ProductContext`.

1. Dans **Explorateur de solutions**, recherchez et ouvrez le fichier *ProductContext.cs* dans le dossier *modèles* .
2. Ajoutez le code en surbrillance au fichier *ProductContext.cs* comme suit :  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Comme mentionné précédemment dans cette série de didacticiels, le code du fichier *ProductContext.cs* ajoute l’espace de noms `System.Data.Entity` afin que vous ayez accès à toutes les fonctionnalités principales du Entity Framework. Cette fonctionnalité offre la possibilité d’interroger, d’insérer, de mettre à jour et de supprimer des données en utilisant des objets fortement typés. La classe `ProductContext` ajoute l’accès à la classe de modèle `CartItem` récemment ajoutée.

### <a name="managing-the-shopping-cart-business-logic"></a>Gestion de la logique métier du panier d’achat

Ensuite, vous allez créer la classe `ShoppingCart` dans un nouveau dossier *logique* . La classe `ShoppingCart` gère l’accès aux données de la table `CartItem`. La classe inclura également la logique métier pour ajouter, supprimer et mettre à jour des éléments dans le panier d’achat.

La logique du panier d’achat que vous allez ajouter contiendra les fonctionnalités permettant de gérer les actions suivantes :

1. Ajout d’éléments au panier d’achat
2. Suppression d’éléments du panier d’achat
3. Obtention de l’ID du panier d’achat
4. Récupération d’éléments du panier d’achat
5. Total de la quantité de tous les éléments du panier d’achat
6. Mise à jour des données du panier d’achat

Une page de panier d’achat (*ShoppingCart. aspx*) et la classe de panier d’achat seront utilisées ensemble pour accéder aux données du panier d’achat. La page du panier d’achat affiche tous les éléments que l’utilisateur ajoute au panier d’achat. Outre la page et la page du panier d’achat, vous allez créer une page (*AddToCart. aspx*) pour ajouter des produits au panier d’achat. Vous allez également ajouter du code à la page *ProductList. aspx* et à la page *ProductDetails. aspx* qui fournira un lien vers la page *AddToCart. aspx* , afin que l’utilisateur puisse ajouter des produits au panier d’achat.

Le diagramme suivant illustre le processus de base qui se produit lorsque l’utilisateur ajoute un produit au panier d’achat.

![Panier d’achat-ajout au panier d’achat](shopping-cart/_static/image3.png)

Quand l’utilisateur clique sur le lien **Ajouter au panier** sur la page *ProductList. aspx* ou sur la page *ProductDetails. aspx* , l’application accède à la page *AddToCart. aspx* , puis automatiquement à la page *ShoppingCart. aspx* . La page *AddToCart. aspx* ajoute le produit Select au panier d’achat en appelant une méthode dans la classe ShoppingCart. La page *ShoppingCart. aspx* affiche les produits qui ont été ajoutés au panier d’achat.

#### <a name="creating-the-shopping-cart-class"></a>Création de la classe de panier d’achat

La classe `ShoppingCart` sera ajoutée à un dossier distinct dans l’application afin qu’il y ait une distinction claire entre le modèle (dossier Models), les pages (dossier racine) et la logique (dossier logique).

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **WingtipToys**, puis sélectionnez **Ajouter**-&gt;**nouveau dossier**. Nommez la nouvelle *logique*de dossier.
2. Cliquez avec le bouton droit sur le dossier *logique* , puis sélectionnez **Ajouter** -&gt; **nouvel élément**.
3. Ajoutez un nouveau fichier de classe nommé *ShoppingCartActions.cs*.
4. Remplacez le code par défaut par le code suivant :   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

La méthode `AddToCart` permet à des produits individuels d’être inclus dans le panier d’achat en fonction du produit `ID`. Le produit est ajouté au panier, ou si le panier contient déjà un élément pour ce produit, la quantité est incrémentée.

La méthode `GetCartId` retourne le `ID` de panier de l’utilisateur. Le `ID` du panier est utilisé pour suivre les éléments dont dispose un utilisateur dans son panier. Si l’utilisateur n’a pas de `ID`de panier existant, un nouveau panier `ID` est créé pour eux. Si l’utilisateur est connecté en tant qu’utilisateur inscrit, le `ID` du panier est défini sur son nom d’utilisateur. Toutefois, si l’utilisateur n’est pas connecté, le `ID` du panier est défini sur une valeur unique (un GUID). Un GUID garantit qu’un seul panier est créé pour chaque utilisateur, en fonction de la session.

La méthode `GetCartItems` retourne une liste d’éléments de panier d’achat pour l’utilisateur. Plus loin dans ce didacticiel, vous verrez que la liaison de modèle est utilisée pour afficher les éléments du panier dans le panier à l’aide de la méthode `GetCartItems`.

### <a name="creating-the-add-to-cart-functionality"></a>Création de la fonctionnalité d’ajout de panier

Comme mentionné précédemment, vous allez créer une page de traitement nommée *AddToCart. aspx* qui sera utilisée pour ajouter de nouveaux produits au panier d’achat de l’utilisateur. Cette page appellera la méthode `AddToCart` dans la classe `ShoppingCart` que vous venez de créer. La page *AddToCart. aspx* s’attend à ce qu’un `ID` de produit soit passé à celle-ci. Ce produit `ID` sera utilisé lors de l’appel de la méthode `AddToCart` dans la classe `ShoppingCart`.

> [!NOTE] 
> 
> Vous allez modifier le code-behind (*AddToCart.aspx.cs*) de cette page, et non l’interface utilisateur de la page (*AddToCart. aspx*).

#### <a name="to-create-the-add-to-cart-functionality"></a>Pour créer la fonctionnalité d’ajout de panier :

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **WingtipToys**, cliquez sur **Ajouter** -&gt; **nouvel élément**.  
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Ajoutez une nouvelle page standard (Web Form) à l’application nommée *AddToCart. aspx*. 

    ![Panier d’achat-ajouter un formulaire Web](shopping-cart/_static/image4.png)
3. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la page *AddToCart. aspx* , puis cliquez sur **afficher le code**. Le fichier code-behind *AddToCart.aspx.cs* s’ouvre dans l’éditeur.
4. Remplacez le code existant dans le code-behind *AddToCart.aspx.cs* par le code suivant :   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Lorsque la page *AddToCart. aspx* est chargée, le `ID` de produit est récupéré à partir de la chaîne de requête. Ensuite, une instance de la classe de panier d’achat est créée et utilisée pour appeler la méthode `AddToCart` que vous avez ajoutée précédemment dans ce didacticiel. La méthode `AddToCart`, contenue dans le fichier *ShoppingCartActions.cs* , comprend la logique permettant d’ajouter le produit sélectionné au panier d’achat ou d’incrémenter la quantité de produits du produit sélectionné. Si le produit n’a pas été ajouté au panier d’achat, le produit est ajouté à la table `CartItem` de la base de données. Si le produit a déjà été ajouté au panier d’achat et que l’utilisateur ajoute un élément supplémentaire du même produit, la quantité de produit est incrémentée dans la table `CartItem`. Enfin, la page redirige vers la page *ShoppingCart. aspx* que vous ajouterez à l’étape suivante, où l’utilisateur verra une liste actualisée d’éléments dans le panier.

Comme mentionné précédemment, un utilisateur `ID` est utilisé pour identifier les produits associés à un utilisateur spécifique. Cette `ID` est ajoutée à une ligne de la table `CartItem` chaque fois que l’utilisateur ajoute un produit au panier d’achat.

### <a name="creating-the-shopping-cart-ui"></a>Création de l’interface utilisateur du panier d’achat

La page *ShoppingCart. aspx* affiche les produits que l’utilisateur a ajoutés à son panier d’achat. Il permet également d’ajouter, de supprimer et de mettre à jour des éléments dans le panier d’achat.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **WingtipToys**, cliquez sur **Ajouter** -&gt; **nouvel élément**.  
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Ajoutez une nouvelle page (formulaire Web) qui comprend une page maître en sélectionnant **Web Form à l’aide de la page maître**. Nommez la nouvelle page *ShoppingCart. aspx*.
3. Sélectionnez **site. Master** pour attacher la page maître à la page *. aspx* nouvellement créée.
4. Dans la page *ShoppingCart. aspx* , remplacez le balisage existant par le balisage suivant :   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

La page *ShoppingCart. aspx* comprend un contrôle **GridView** nommé `CartList`. Ce contrôle utilise la liaison de modèle pour lier les données du panier d’achat de la base de données au contrôle **GridView** . Lorsque vous définissez la propriété `ItemType` du contrôle **GridView** , l’expression de liaison de données `Item` est disponible dans le balisage du contrôle et le contrôle est fortement typé. Comme mentionné plus haut dans cette série de didacticiels, vous pouvez sélectionner les détails de l’objet `Item` à l’aide d’IntelliSense. Pour configurer un contrôle de données afin d’utiliser la liaison de modèle pour sélectionner des données, vous devez définir la propriété `SelectMethod` du contrôle. Dans le balisage ci-dessus, vous définissez la `SelectMethod` pour utiliser la méthode GetShoppingCartItems qui retourne une liste d’objets `CartItem`. Le contrôle de données **GridView** appelle la méthode au moment approprié dans le cycle de vie de la page et lie automatiquement les données retournées. La méthode `GetShoppingCartItems` doit toujours être ajoutée.

#### <a name="retrieving-the-shopping-cart-items"></a>Récupération des éléments du panier d’achat

Ensuite, vous ajoutez du code au code-behind *ShoppingCart.aspx.cs* pour récupérer et remplir l’interface utilisateur du panier d’achat.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la page *ShoppingCart. aspx* , puis cliquez sur **afficher le code**. Le fichier code-behind *ShoppingCart.aspx.cs* s’ouvre dans l’éditeur.
2. Remplacez le code existant par le code ci-dessous :  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Comme indiqué ci-dessus, le contrôle de données `GridView` appelle la méthode `GetShoppingCartItems` au moment approprié dans le cycle de vie de la page et lie automatiquement les données retournées. La méthode `GetShoppingCartItems` crée une instance de l’objet `ShoppingCartActions`. Ensuite, le code utilise cette instance pour retourner les éléments du panier en appelant la méthode `GetCartItems`.

### <a name="adding-products-to-the-shopping-cart"></a>Ajout de produits au panier d’achat

Lorsque la page *ProductList. aspx* ou *ProductDetails. aspx* s’affiche, l’utilisateur peut ajouter le produit au panier d’achat à l’aide d’un lien. Lorsqu’il clique sur le lien, l’application accède à la page de traitement nommée *AddToCart. aspx*. La page *AddToCart. aspx* appellera la méthode `AddToCart` dans la classe `ShoppingCart` que vous avez ajoutée précédemment dans ce didacticiel.

À présent, vous allez ajouter un lien **Ajouter au panier** à la page *ProductList. aspx* et à la page *ProductDetails. aspx* . Ce lien inclut le produit `ID` qui est récupéré à partir de la base de données.

1. Dans **Explorateur de solutions**, recherchez et ouvrez la page *ProductList. aspx*.
2. Ajoutez la balise mise en surbrillance en jaune à la page *ProductList. aspx* afin que la page entière apparaisse comme suit :  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Test du panier d’achat

Exécutez l’application pour voir comment vous ajoutez des produits au panier d’achat.

1. Appuyez sur **F5** pour exécuter l’application.  
 Une fois que le projet a recréé la base de données, le navigateur s’ouvre et affiche la page *default. aspx* .
2. Sélectionnez **Cars** dans le menu de navigation de la catégorie.  
 La page *ProductList. aspx* s’affiche uniquement avec les produits inclus dans la catégorie « cars ». 

    ![Panier d’achat-voitures](shopping-cart/_static/image5.png)
3. Cliquez sur le lien **Ajouter au panier** en regard du premier produit listé (la voiture convertible).   
 La page *ShoppingCart. aspx* s’affiche, affichant la sélection dans votre panier. 

    ![Panier d’achat-panier](shopping-cart/_static/image6.png)
4. Affichez des produits supplémentaires en sélectionnant **plans** dans le menu de navigation catégorie.
5. Cliquez sur le lien **Ajouter au panier** en regard du premier produit listé.  
 La page *ShoppingCart. aspx* s’affiche avec l’élément supplémentaire.
6. Fermez le navigateur.

### <a name="calculating-and-displaying-the-order-total"></a>Calcul et affichage du total de la commande

Outre l’ajout de produits au panier d’achat, vous allez ajouter une méthode de `GetTotal` à la classe `ShoppingCart` et afficher le montant total de la commande dans la page du panier d’achat.

1. Dans **Explorateur de solutions**, ouvrez le fichier *ShoppingCartActions.cs* dans le dossier *logique* .
2. Ajoutez la méthode `GetTotal` suivante mise en surbrillance en jaune à la classe `ShoppingCart`, afin que la classe apparaisse comme suit :   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Tout d’abord, la méthode `GetTotal` obtient l’ID du panier d’achat de l’utilisateur. La méthode obtient ensuite le total du panier en multipliant le prix du produit par la quantité de produit pour chaque produit figurant dans le panier.

> [!NOTE] 
> 
> Le code ci-dessus utilise le type Nullable «`int?`». Les types Nullable peuvent représenter toutes les valeurs d’un type sous-jacent, et également en tant que valeur null. Pour plus d’informations, consultez [utilisation de types Nullable](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).

### <a name="modify-the-shopping-cart-display"></a>Modifier l’affichage du panier d’achat

Ensuite, vous allez modifier le code de la page *ShoppingCart. aspx* pour appeler la méthode `GetTotal` et afficher ce total sur la page *ShoppingCart. aspx* lors du chargement de la page.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la page *ShoppingCart. aspx* , puis sélectionnez **afficher le code**.
2. Dans le fichier *ShoppingCart.aspx.cs* , mettez à jour le gestionnaire de `Page_Load` en ajoutant le code suivant mis en surbrillance en jaune :   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Lorsque la page *ShoppingCart. aspx* est chargée, elle charge l’objet de panier d’achat, puis récupère le total du panier en appelant la méthode `GetTotal` de la classe `ShoppingCart`. Si le panier d’achat est vide, un message s’affiche à cet effet.

### <a name="testing-the-shopping-cart-total"></a>Test du total du panier d’achat

Exécutez l’application maintenant pour voir comment vous pouvez non seulement ajouter un produit au panier d’achat, mais vous pouvez voir le total du panier d’achat.

1. Appuyez sur **F5** pour exécuter l’application.  
 Le navigateur s’ouvre et affiche la page *Default.aspx* .
2. Sélectionnez **Cars** dans le menu de navigation de la catégorie.
3. Cliquez sur le lien **Ajouter au panier** en regard du premier produit.   
 La page *ShoppingCart. aspx* s’affiche avec le total de commande. 

    ![Panier d’achat-total du panier](shopping-cart/_static/image7.png)
4. Ajoutez d’autres produits (par exemple, un plan) au panier.
5. La page *ShoppingCart. aspx* s’affiche avec un total mis à jour pour tous les produits que vous avez ajoutés. 

    ![Panier d’achat-plusieurs produits](shopping-cart/_static/image8.png)
6. Arrêtez l’application en cours d’exécution en fermant la fenêtre du navigateur.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Ajout de boutons de mise à jour et d’extraction au panier d’achat

Pour permettre aux utilisateurs de modifier le panier d’achat, vous devez ajouter un bouton **mettre à jour** et un bouton d' **extraction** à la page du panier d’achat. Le bouton d' **extraction** n’est pas utilisé plus loin dans cette série de didacticiels.

1. Dans **Explorateur de solutions**, ouvrez la page *ShoppingCart. aspx* à la racine du projet d’application Web.
2. Pour ajouter le bouton **mettre à jour** et le bouton **Checkout** à la page *ShoppingCart. aspx* , ajoutez le balisage mis en surbrillance en jaune à la balise existante, comme illustré dans le code suivant :   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Quand l’utilisateur clique sur le bouton **mettre à jour** , le gestionnaire d’événements `UpdateBtn_Click` est appelé. Ce gestionnaire d’événements appellera le code que vous ajouterez à l’étape suivante.

Ensuite, vous pouvez mettre à jour le code contenu dans le fichier *ShoppingCart.aspx.cs* pour parcourir les éléments du panier et appeler les méthodes `RemoveItem` et `UpdateItem`.

1. Dans **Explorateur de solutions**, ouvrez le fichier *ShoppingCart.aspx.cs* à la racine du projet d’application Web.
2. Ajoutez les sections de code suivantes mises en surbrillance en jaune au fichier *ShoppingCart.aspx.cs* :   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Quand l’utilisateur clique sur le bouton **mettre à jour** sur la page *ShoppingCart. aspx* , la méthode UpdateCartItems est appelée. La méthode UpdateCartItems obtient les valeurs mises à jour pour chaque élément dans le panier d’achat. Ensuite, la méthode UpdateCartItems appelle la méthode `UpdateShoppingCartDatabase` (ajoutée et expliquée à l’étape suivante) pour ajouter ou supprimer des éléments dans le panier d’achat. Une fois que la base de données a été mise à jour pour refléter les mises à jour du panier d’achat, le contrôle **GridView** est mis à jour sur la page du panier d’achat en appelant la méthode `DataBind` pour le **GridView**. En outre, le montant total de la commande sur la page du panier d’achat est mis à jour pour refléter la liste mise à jour des éléments.

### <a name="updating-and-removing-shopping-cart-items"></a>Mise à jour et suppression des éléments du panier d’achat

Sur la page *ShoppingCart. aspx* , vous pouvez voir que des contrôles ont été ajoutés pour mettre à jour la quantité d’un élément et supprimer un élément. À présent, ajoutez le code qui permet de faire fonctionner ces contrôles.

1. Dans **Explorateur de solutions**, ouvrez le fichier *ShoppingCartActions.cs* dans le dossier *logique* .
2. Ajoutez le code suivant mis en surbrillance en jaune au fichier de classe *ShoppingCartActions.cs* :   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

La méthode `UpdateShoppingCartDatabase`, appelée à partir de la méthode `UpdateCartItems` sur la page *ShoppingCart.aspx.cs* , contient la logique pour mettre à jour ou supprimer des éléments du panier d’achat. La méthode `UpdateShoppingCartDatabase` itère au sein de toutes les lignes dans la liste de panier d’achat. Si un élément de panier d’achat a été marqué pour être supprimé ou si la quantité est inférieure à un, la méthode `RemoveItem` est appelée. Dans le cas contraire, les mises à jour sont vérifiées lors de l’appel de la méthode `UpdateItem`. Une fois que l’élément de panier d’achat a été supprimé ou mis à jour, les modifications apportées à la base de données sont enregistrées.

La structure `ShoppingCartUpdates` est utilisée pour contenir tous les éléments du panier d’achat. La méthode `UpdateShoppingCartDatabase` utilise la structure `ShoppingCartUpdates` pour déterminer si l’un des éléments doit être mis à jour ou supprimé.

Dans le didacticiel suivant, vous allez utiliser la méthode `EmptyCart` pour effacer le panier d’achat après avoir acheté des produits. Mais pour le moment, vous allez utiliser la méthode `GetCount` que vous venez d’ajouter au fichier *ShoppingCartActions.cs* pour déterminer le nombre d’éléments dans le panier d’achat.

### <a name="adding-a-shopping-cart-counter"></a>Ajout d’un compteur de panier d’achat

Pour permettre à l’utilisateur d’afficher le nombre total d’éléments dans le panier d’achat, vous allez ajouter un compteur à la page *site. Master* . Ce compteur fera également office de lien vers le panier d’achat.

1. Dans **Explorateur de solutions**, ouvrez la page *site. Master* .
2. Modifiez le balisage en ajoutant le lien du compteur du panier d’achat, comme indiqué en jaune à la section de navigation, afin qu’il apparaisse comme suit :  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Ensuite, mettez à jour le code-behind du fichier *site.Master.cs* en ajoutant le code mis en surbrillance en jaune comme suit :  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Avant que la page ne soit rendue en HTML, l’événement `Page_PreRender` est déclenché. Dans le gestionnaire de `Page_PreRender`, le nombre total du panier d’achat est déterminé par l’appel de la méthode `GetCount`. La valeur retournée est ajoutée à la `cartCount` étendue incluse dans le balisage de la page *site. Master* . Les balises `<span>` permettent de restituer correctement les éléments internes. Quand une page du site s’affiche, le total du panier d’achat s’affiche. L’utilisateur peut également cliquer sur le total du panier d’achat pour afficher le panier d’achat.

## <a name="testing-the-completed-shopping-cart"></a>Test du panier d’achat terminé

Vous pouvez exécuter l’application maintenant pour voir comment vous pouvez ajouter, supprimer et mettre à jour des éléments dans le panier d’achat. Le total du panier d’achat reflète le coût total de tous les articles dans le panier d’achat.

1. Appuyez sur **F5** pour exécuter l’application.  
 Le navigateur s’ouvre et affiche la page *default. aspx* .
2. Sélectionnez **Cars** dans le menu de navigation de la catégorie.
3. Cliquez sur le lien **Ajouter au panier** en regard du premier produit.   
 La page *ShoppingCart. aspx* s’affiche avec le total de commande.
4. Sélectionnez **plans** dans le menu de navigation de la catégorie.
5. Cliquez sur le lien **Ajouter au panier** en regard du premier produit.
6. Définissez la quantité du premier élément du panier d’achat sur 3 et cochez la case **Supprimer l’élément** du deuxième élément.<a id="a"></a>
7. Cliquez sur le bouton **mettre à jour** pour mettre à jour la page du panier d’achat et afficher le nouveau total de commande. 

    ![Panier d’achat-mise à jour du panier](shopping-cart/_static/image9.png)

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez créé un panier d’achat pour l’exemple d’application Wingtip Toys Web Forms. Au cours de ce didacticiel, vous avez utilisé Entity Framework Code First, des annotations de données, des contrôles de données fortement typés et la liaison de modèle.

Le panier d’achat prend en charge l’ajout, la suppression et la mise à jour des éléments que l’utilisateur a sélectionnés pour l’achat. En plus d’implémenter la fonctionnalité de panier d’achat, vous avez appris à afficher les éléments du panier d’achat dans un contrôle **GridView** et à calculer le total de la commande.

Pour comprendre comment les fonctionnalités décrites fonctionnent dans une application métier réelle, vous pouvez consulter l’exemple de panier d’achat Open source basé sur [nopCommerce](https://github.com/nopSolutions/nopCommerce) -ASP.net. À l’origine, elle a été conçue sur Web Forms et au cours des années où elle a été déplacée vers MVC et désormais vers ASP.NET Core.

## <a name="addition-information"></a>Informations supplémentaires

[Vue d'ensemble de l'état de session ASP.NET](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Précédent](display_data_items_and_details.md)
> [Suivant](checkout-and-payment-with-paypal.md)
