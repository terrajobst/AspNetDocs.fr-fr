---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Partie 4 : liste des produits | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 4 couvre la liste des produits avec le contrôle GridView contr...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7af1b8afa2ecc8df9846f2edd2091b26b93a811c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566982"
---
# <a name="part-4-listing-products"></a>Partie 4 : liste des produits

par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.
> 
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 4 couvre la liste des produits avec le contrôle GridView.

## <a id="_Toc260221670"></a>Liste des produits avec le contrôle GridView

Commençons à implémenter notre page ProductsList. aspx en cliquant avec le bouton droit sur notre solution et en sélectionnant « Ajouter » et « nouvel élément ».

![](tailspin-spyworks-part-4/_static/image1.jpg)

Choisissez « Web Form using Master page » et entrez le nom de la page ProductsList. aspx».

Cliquez sur « Ajouter ».

![](tailspin-spyworks-part-4/_static/image2.jpg)

Ensuite, choisissez le dossier « styles » dans lequel nous avons placé la page site. Master et sélectionnez-la dans la fenêtre « contenu du dossier ».

![](tailspin-spyworks-part-4/_static/image3.jpg)

Cliquez sur OK pour créer la page.

Notre base de données est remplie avec les données du produit, comme indiqué ci-dessous.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Une fois la page créée, nous allons utiliser une source de données d’entité pour accéder aux données du produit, mais dans cette instance, nous devons sélectionner les entités Product et nous devons restreindre les éléments qui sont renvoyés uniquement à ceux de la catégorie sélectionnée.

Pour ce faire, nous indiquons à EntityDataSource de générer automatiquement la clause WHERE et nous allons spécifier WhereParameter.

Vous vous souviendrez que lorsque nous avons créé les éléments de menu dans notre « menu de catégorie de produits », nous avons créé le lien de manière dynamique en ajoutant CategoryID à la chaîne de chaîne de chaque lien. Nous indiquons à la source de données d’entité de dériver le paramètre WHERE de ce paramètre QueryString.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Ensuite, nous allons configurer le contrôle ListView pour afficher une liste de produits. Pour créer une expérience d’achat optimale, nous allons compacter plusieurs fonctionnalités concises dans chaque produit affiché dans notre ListVew.

- Le nom du produit est un lien vers l’affichage détaillé du produit.
- Le prix du produit s’affiche.
- Une image du produit s’affiche et l’image est sélectionnée de manière dynamique dans un répertoire d’images de catalogue dans notre application.
- Nous allons inclure un lien pour ajouter immédiatement le produit spécifique au panier d’achat.

Voici le balisage de notre instance de contrôle ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Nous créons de manière dynamique plusieurs liens pour chaque produit affiché.

En outre, avant de tester une nouvelle page, nous devons créer la structure de répertoires pour les images du catalogue de produits comme suit.

![](tailspin-spyworks-part-4/_static/image1.png)

Une fois nos images de produit accessibles, nous pouvons tester notre page de liste de produits.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Sur la page d’hébergement du site, cliquez sur l’un des liens de la liste des catégories.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Nous devons à présent implémenter la page ProductDetails. aspx et la fonctionnalité AddToCart.

Utilisez fichier-&gt;nouveau pour créer un nom de page ProductDetails. aspx à l’aide de la page maître de site comme nous l’avons fait précédemment.

Nous utiliserons à nouveau un contrôle EntityDataSource pour accéder à l’enregistrement de produit spécifique dans la base de données et nous utiliserons un contrôle FormView ASP.NET pour afficher les données du produit comme suit.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Ne vous inquiétez pas si la mise en forme semble un peu amusante pour vous. Le balisage ci-dessus laisse de l’espace dans la disposition d’affichage pour quelques fonctionnalités que nous allons implémenter ultérieurement.

Le panier représente la logique la plus complexe de notre application. Pour commencer, utilisez fichier-&gt;nouveau pour créer une page nommée MyShoppingCart. aspx.

Notez que nous n’avons pas choisi le nom ShoppingCart. aspx.

Notre base de données contient une table nommée « ShoppingCart ». Lorsque nous avons généré une Entity Data Model une classe a été créée pour chaque table de la base de données. Par conséquent, le Entity Data Model généré une classe d’entité nommée « ShoppingCart ». Nous pouvons modifier le modèle afin de pouvoir utiliser ce nom pour notre implémentation de panier d’achat ou l’étendre pour nos besoins, mais nous allons choisir de simplement sélectionner un nom qui évitera le conflit.

Il est également intéressant de noter que nous allons créer un panier d’achat simple et incorporer la logique du panier d’achat avec l’affichage du panier d’achat. Nous pouvons également choisir d’implémenter notre panier d’achat dans une couche métier complètement distincte.

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-3.md)
> [Suivant](tailspin-spyworks-part-5.md)
