---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Partie 5 : logique métier | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 5 ajoute une certaine logique métier.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: c60eece9223c47304786d7b0d0ca4b11ac0572e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630304"
---
# <a name="part-5-business-logic"></a>Partie 5 : logique métier

par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.
> 
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 5 ajoute une certaine logique métier.

## <a id="_Toc260221671"></a>Ajout d’une logique métier

Nous souhaitons que notre expérience d’achat soit disponible chaque fois que quelqu’un visite notre site Web. Les visiteurs seront en mesure de parcourir et d’ajouter des éléments au panier d’achat même s’ils ne sont pas enregistrés ou connectés. Lorsqu’ils sont prêts à vérifier, ils peuvent s’authentifier et, s’ils ne sont pas encore membres, ils peuvent créer un compte.

Cela signifie que nous aurons besoin d’implémenter la logique pour convertir le panier d’achat d’un État anonyme en état « utilisateur inscrit ».

Créons un répertoire nommé « classes », puis cliquons avec le bouton droit sur le dossier et créons un nouveau fichier de « classe » nommé MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Comme nous l’avons vu précédemment, nous allons étendre la classe qui implémente la page MyShoppingCart. aspx et nous allons utiliser. Construction « partielle de classe » puissante de .net.

L’appel généré pour notre fichier MyShoppingCart.aspx.cf ressemble à ce qui suit.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Notez l’utilisation du mot clé « Partial ».

Le fichier de classe que nous venons de générer ressemble à ce qui suit.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Nous fusionnerons nos implémentations en ajoutant également le mot clé Partial à ce fichier.

Notre nouveau fichier de classe ressemble maintenant à ce qui suit.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

La première méthode que nous ajouterons à notre classe est la méthode « AddItem ». Il s’agit de la méthode qui sera finalement appelée lorsque l’utilisateur cliquera sur les liens « ajouter à l’art » dans les pages liste des produits et Détails du produit.

Ajoutez ce qui suit aux instructions using en haut de la page.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

Et ajoutez cette méthode à la classe MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Nous utilisons LINQ to Entities pour voir si l’élément est déjà dans le panier. Dans ce cas, nous mettons à jour la quantité commandée de l’article. dans le cas contraire, nous créons une nouvelle entrée pour l’élément sélectionné.

Pour appeler cette méthode, nous allons implémenter une page AddToCart. aspx qui non seulement classe cette méthode, mais qui affiche ensuite le panier en cours a = Cart après l’ajout de l’élément.

Cliquez avec le bouton droit sur le nom de la solution dans l’Explorateur de solutions et ajoutez une nouvelle page nommée AddToCart. aspx, comme nous l’avons fait précédemment.

Bien que nous puissions utiliser cette page pour afficher des résultats intermédiaires comme des problèmes de stock faible, etc., dans notre implémentation, la page ne s’affiche pas réellement, mais elle appelle plutôt la logique « ajouter » et la redirection.

Pour ce faire, nous allons ajouter le code suivant à la page\_événement Load.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Notez que nous récupérons le produit à ajouter au panier d’achat à partir d’un paramètre QueryString et que j’appelle la méthode AddItem de notre classe.

En supposant qu’aucune erreur ne se produise, le contrôle est passé à la page SHoppingCart. aspx que nous allons implémenter entièrement. S’il doit y avoir une erreur, nous levez une exception.

Actuellement, nous n’avons pas encore implémenté de gestionnaire d’erreurs global. par conséquent, cette exception n’est pas gérée par notre application, mais nous y remédierons prochainement.

Notez également l’utilisation de l’instruction Debug. Fail () (disponible par le biais de `using System.Diagnostics;)`

Si l’application s’exécute dans le débogueur, cette méthode affiche une boîte de dialogue détaillée avec les informations relatives à l’état des applications, ainsi que le message d’erreur que nous spécifions.

En cas d’exécution en production, l’instruction Debug. Fail () est ignorée.

Vous noterez dans le code ci-dessus un appel à une méthode dans nos noms de classe de panier d’achat « GetShoppingCartId ».

Ajoutez le code pour implémenter la méthode comme suit.

Notez que nous avons également ajouté les boutons de mise à jour et d’extraction et une étiquette dans laquelle vous pouvez afficher le « total » du panier.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Nous pouvons maintenant ajouter des articles à notre panier, mais nous n’avons pas implémenté la logique permettant d’afficher le panier après l’ajout d’un produit.

Ainsi, dans la page MyShoppingCart. aspx, nous allons ajouter un contrôle EntityDataSource et un contrôle GridVire comme suit.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Appelez le formulaire dans le concepteur afin de pouvoir double-cliquer sur le bouton mettre à jour le panier et générer le gestionnaire d’événements Click qui est spécifié dans la déclaration dans le balisage.

Nous allons implémenter les détails ultérieurement, mais cela nous permettra de générer et d’exécuter notre application sans erreurs.

Lorsque vous exécutez l’application et ajoutez un élément au panier d’achat, vous le verrez.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Notez que nous avons utilisé l’affichage de grille « par défaut » en implémentant trois colonnes personnalisées.

Le premier est un champ modifiable « lié » pour la quantité :

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

La suivante est une colonne « calculée » qui affiche le total de la ligne (le coût de l’élément multiplié par la quantité à commander) :

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Enfin, nous avons une colonne personnalisée qui contient un contrôle CheckBox que l’utilisateur utilisera pour indiquer que l’élément doit être supprimé du graphique d’achat.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Comme vous pouvez le voir, la ligne de total de commande est vide. nous allons donc ajouter une logique pour calculer le total de la commande.

Nous allons tout d’abord implémenter une méthode « GetTotal » pour notre classe MyShoppingCart.

Dans le fichier MyShoppingCart.cs, ajoutez le code suivant.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Ensuite, dans la page\_gestionnaire d’événements Load, nous pouvons appeler notre méthode GetTotal. En même temps, nous allons ajouter un test pour voir si le panier d’achat est vide et ajuster l’affichage en conséquence, le cas échéant.

Maintenant, si le panier est vide, nous obtenons ce qui suit :

![](tailspin-spyworks-part-5/_static/image4.jpg)

Si ce n’est pas le cas, nous voyons notre total.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Toutefois, cette page n’est pas encore terminée.

Nous aurons besoin d’une logique supplémentaire pour recalculer le panier d’achat en supprimant les éléments marqués pour suppression et en déterminant les nouvelles valeurs de quantité, car certaines peuvent avoir été modifiées dans la grille par l’utilisateur.

Permet d’ajouter une méthode « RemoveItem » à notre classe de panier d’achat dans MyShoppingCart.cs pour gérer le cas où un utilisateur marque un élément en vue de sa suppression.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

À présent, nous allons utiliser une méthode pour gérer le cas où un utilisateur modifie simplement la qualité à trier dans le GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Avec les fonctionnalités de base de suppression et de mise à jour en place, nous pouvons implémenter la logique qui met réellement à jour le panier d’achat dans la base de données. (Dans MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Vous noterez que cette méthode attend deux paramètres. L’un est l’ID du panier d’achat et l’autre est un tableau d’objets de type défini par l’utilisateur.

Par conséquent, pour réduire la dépendance de notre logique sur les caractéristiques de l’interface utilisateur, nous avons défini une structure de données que nous pouvons utiliser pour transmettre les éléments du panier d’achat à notre code sans que notre méthode doive accéder directement au contrôle GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

Dans notre fichier MyShoppingCart.aspx.cs, nous pouvons utiliser cette structure dans notre gestionnaire d’événements de clic du bouton mettre à jour comme suit. Notez qu’en plus de la mise à jour du panier, nous mettrons également à jour le total du panier.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Notez, avec un intérêt particulier, cette ligne de code :

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues () est une fonction d’assistance spéciale que nous allons implémenter dans MyShoppingCart.aspx.cs comme suit.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Cela offre un moyen propre pour accéder aux valeurs des éléments liés dans notre contrôle GridView. Étant donné que notre contrôle de case à cocher « Supprimer l’élément » n’est pas lié, nous y accéderons via la méthode FindControl ().

À ce niveau du développement de votre projet, nous sommes prêts à implémenter le processus d’extraction.

Avant cela, nous allons utiliser Visual Studio pour générer la base de données d’appartenance et ajouter un utilisateur au référentiel d’appartenance.

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-4.md)
> [Suivant](tailspin-spyworks-part-6.md)
