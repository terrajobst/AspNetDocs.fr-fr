---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Partie 8 : panier d’achat avec mises à jour d’Ajax | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 8 couvre le panier d’achat avec les mises à jour Ajax.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 89897ad41b217764cbd17317d4bf5d6a5c5d488f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539255"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a>Partie 8 : panier d’achat avec mises à jour d’Ajax

par [Jon Galloway](https://github.com/jongalloway)

> Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.  
>   
> Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.  
>   
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 8 couvre le panier d’achat avec les mises à jour Ajax.

Nous autoriserons les utilisateurs à placer des albums dans leur panier sans les inscrire, mais ils devront s’inscrire en tant qu’invités pour finaliser l’extraction. Le processus d’achat et de validation sera divisé en deux contrôleurs : un contrôleur ShoppingCart qui permet d’ajouter des éléments de manière anonyme à un panier et un contrôleur d’extraction qui gère le processus d’extraction. Nous allons commencer par le panier dans cette section, puis créer le processus d’extraction dans la section suivante.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Ajout des classes de modèle cart, Order et OrderDetail

Nos processus de panier d’achat et de validation feront appel à certaines nouvelles classes. Cliquez avec le bouton droit sur le dossier Models et ajoutez une classe de panier (Cart.cs) avec le code suivant.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Cette classe est assez similaire à celle que nous avons utilisée jusqu’à présent, à l’exception de l’attribut [Key] pour la propriété RecordId. Nos éléments de panier auront un identificateur de chaîne nommé CartID pour autoriser les achats anonymes, mais la table contient une clé primaire entière nommée RecordId. Par Convention, Entity Framework code First s’attend à ce que la clé primaire d’une table nommée Cart soit CartId ou ID, mais nous pouvons facilement la remplacer par des annotations ou du code, si nous le souhaitons. Il s’agit d’un exemple de la façon dont nous pouvons utiliser les conventions simples dans Entity Framework code-tout d’abord lorsqu’ils nous intéressent, mais nous n’y sommes pas contraints.

Ensuite, ajoutez une classe Order (Order.cs) avec le code suivant.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Cette classe effectue le suivi des informations de synthèse et de livraison pour une commande. **Elle n’est pas encore compilée**, car elle possède une propriété de navigation OrderDetails qui dépend d’une classe que nous n’avons pas encore créée. Nous allons maintenant résoudre le problème en ajoutant une classe nommée OrderDetail.cs, en ajoutant le code suivant.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Nous allons effectuer une dernière mise à jour de notre classe MusicStoreEntities pour inclure des DbSets qui exposent ces nouvelles classes de modèle, y compris un DbSet&lt;Artist&gt;. La classe MusicStoreEntities mise à jour apparaît comme indiqué ci-dessous.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Gestion de la logique métier du panier d’achat

Ensuite, nous allons créer la classe ShoppingCart dans le dossier Models. Le modèle ShoppingCart gère l’accès aux données de la table Cart. En outre, il gère la logique métier pour l’ajout et la suppression d’articles dans le panier.

Étant donné que nous ne souhaitons pas obliger les utilisateurs à s’inscrire à un compte simplement pour ajouter des éléments à leur panier d’achat, nous attribuons aux utilisateurs un identificateur unique temporaire (à l’aide d’un GUID ou d’un identificateur global unique) lorsqu’ils accèdent au panier d’achat. Nous allons stocker cet ID à l’aide de la classe de session ASP.NET.

*Remarque : la session ASP.NET est un emplacement pratique pour stocker des informations spécifiques à l’utilisateur, qui expireront une fois le site quitté. Bien qu’une mauvaise utilisation de l’état de session puisse avoir un impact sur les performances sur les sites plus importants, notre utilisation légère fonctionnera bien à des fins de démonstration.*

La classe ShoppingCart expose les méthodes suivantes :

**AddToCart** prend un album comme paramètre et l’ajoute au panier de l’utilisateur. Étant donné que la table Cart effectue le suivi de la quantité pour chaque album, elle inclut une logique pour créer une nouvelle ligne si nécessaire ou simplement incrémenter la quantité si l’utilisateur a déjà commandé une copie de l’album.

**RemoveFromCart** prend un ID d’album et le supprime du panier de l’utilisateur. Si l’utilisateur n’avait qu’une seule copie de l’album dans son panier, la ligne est supprimée.

**EmptyCart** supprime tous les éléments du panier d’achat d’un utilisateur.

**GetCartItems** récupère une liste de CartItems pour l’affichage ou le traitement.

**GetCount** récupère un nombre total d’albums dont dispose un utilisateur dans son panier.

**GetTotal** calcule le coût total de tous les articles dans le panier.

**CreateOrder** convertit le panier d’achat en commande au cours de la phase de validation.

**GetCart** est une méthode statique qui permet à nos contrôleurs d’obtenir un objet de panier. Elle utilise la méthode **GetCartId** pour gérer la lecture du CartId à partir de la session de l’utilisateur. La méthode GetCartId nécessite la méthode HttpContextBase pour pouvoir lire le CartId de l’utilisateur à partir de la session de l’utilisateur.

Voici la **classe ShoppingCart**complète :

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Notre contrôleur de panier d’achat doit communiquer des informations complexes à ses vues qui ne sont pas correctement mappées à nos objets de modèle. Nous ne voulons pas modifier nos modèles pour les adapter à nos vues ; Les classes de modèle doivent représenter notre domaine, et non l’interface utilisateur. Une solution consisterait à transmettre les informations à nos vues à l’aide de la classe ViewBag, comme nous l’avons fait avec les informations de la liste déroulante du gestionnaire de magasin, mais le fait de passer beaucoup d’informations via ViewBag devient difficile à gérer.

Une solution consiste à utiliser le modèle *ViewModel* . Lors de l’utilisation de ce modèle, nous créons des classes fortement typées qui sont optimisées pour nos scénarios d’affichage spécifiques, et qui exposent des propriétés pour les valeurs/contenus dynamiques requis par nos modèles de vue. Nos classes de contrôleur peuvent ensuite remplir et transmettre ces classes optimisées en vue à notre modèle de vue à utiliser. Cela permet la sécurité des types, la vérification au moment de la compilation et l’IntelliSense de l’éditeur dans les modèles de vue.

Nous allons créer deux modèles de vue à utiliser dans notre contrôleur de panier d’achat : le ShoppingCartViewModel contiendra le contenu du panier d’achat de l’utilisateur, et le ShoppingCartRemoveViewModel sera utilisé pour afficher les informations de confirmation lorsqu’un utilisateur supprime un événement à partir de leur panier.

Nous allons créer un nouveau dossier ViewModels à la racine de notre projet pour que les choses restent organisées. Cliquez avec le bouton droit sur le projet, puis sélectionnez Ajouter/nouveau dossier.

![](mvc-music-store-part-8/_static/image1.jpg)

Nommez le dossier ViewModels.

![](mvc-music-store-part-8/_static/image1.png)

Ensuite, ajoutez la classe ShoppingCartViewModel dans le dossier ViewModels. Il a deux propriétés : une liste d’éléments de panier et une valeur décimale pour stocker le prix total de tous les articles dans le panier.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Ajoutez maintenant ShoppingCartRemoveViewModel au dossier ViewModels, avec les quatre propriétés suivantes.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Le contrôleur de panier d’achat

Le contrôleur de panier d’achat a trois objectifs principaux : l’ajout d’éléments à un panier, la suppression d’éléments du panier et l’affichage des éléments dans le panier. Elle utilisera les trois classes que nous venons de créer : ShoppingCartViewModel, ShoppingCartRemoveViewModel et ShoppingCart. Comme dans StoreController et StoreManagerController, nous allons ajouter un champ pour contenir une instance de MusicStoreEntities.

Ajoutez un nouveau contrôleur de panier d’achat au projet à l’aide du modèle de contrôleur vide.

![](mvc-music-store-part-8/_static/image2.png)

Voici le contrôleur ShoppingCart complet. Les actions d’index et d’ajout de contrôleur doivent paraître très familières. Les actions du contrôleur Remove et CartSummary gèrent deux cas spéciaux, que nous aborderons dans la section suivante.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Mises à jour Ajax avec jQuery

Nous allons ensuite créer une page d’index de panier d’achat fortement typée en ShoppingCartViewModel et utilisant le modèle de vue liste en utilisant la même méthode que précédemment.

![](mvc-music-store-part-8/_static/image3.png)

Toutefois, au lieu d’utiliser un fichier html. ActionLink pour supprimer des éléments du panier, nous allons utiliser jQuery pour « associer » l’événement Click pour tous les liens de cette vue qui ont la classe HTML RemoveLink. Au lieu de publier le formulaire, ce gestionnaire d’événements Click effectue simplement un rappel AJAX à notre action de contrôleur RemoveFromCart. RemoveFromCart retourne un résultat sérialisé JSON, que notre rappel jQuery analyse ensuite et exécute quatre mises à jour rapides sur la page à l’aide de jQuery :

- 1. Supprime l’album supprimé de la liste
- 2. Met à jour le nombre de caddies dans l’en-tête
- 3. Affiche un message de mise à jour à l’utilisateur
- 4. Met à jour le prix total du panier

Étant donné que le scénario de suppression est géré par un rappel Ajax au sein de la vue index, nous n’avons pas besoin d’une vue supplémentaire pour l’action RemoveFromCart. Voici le code complet de la vue/ShoppingCart/Index :

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Pour le tester, nous devons être en mesure d’ajouter des éléments à notre panier. Nous allons mettre à jour notre vue des **Détails du magasin** pour inclure un bouton « Ajouter au panier ». Pendant que nous sommes là, nous pouvons inclure certaines informations supplémentaires sur l’album, que nous avons ajoutées depuis la dernière mise à jour de cette vue : genre, artiste, prix et pochette de l’album. Le code de vue des détails du magasin mis à jour s’affiche comme indiqué ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

À présent, nous pouvons cliquer sur le Store et tester l’ajout et la suppression d’albums vers et depuis notre panier. Exécutez l’application et accédez à l’index Store.

![](mvc-music-store-part-8/_static/image4.png)

Ensuite, cliquez sur un genre pour afficher une liste d’albums.

![](mvc-music-store-part-8/_static/image5.png)

En cliquant sur le titre d’un album, vous affichez la vue des détails de l’album mis à jour, y compris le bouton « Ajouter au panier ».

![](mvc-music-store-part-8/_static/image6.png)

Si vous cliquez sur le bouton « Ajouter au panier », notre vue d’index de panier d’achat s’affiche avec la liste Résumé du panier d’achat.

![](mvc-music-store-part-8/_static/image7.png)

Après avoir chargé votre panier, vous pouvez cliquer sur le lien supprimer du panier pour afficher la mise à jour Ajax de votre panier d’achat.

![](mvc-music-store-part-8/_static/image8.png)

Nous avons créé un panier d’achat qui permet aux utilisateurs non inscrits d’ajouter des éléments à leur panier. Dans la section suivante, nous allons les autoriser à s’inscrire et à terminer le processus d’extraction.

> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-7.md)
> [Suivant](mvc-music-store-part-9.md)
