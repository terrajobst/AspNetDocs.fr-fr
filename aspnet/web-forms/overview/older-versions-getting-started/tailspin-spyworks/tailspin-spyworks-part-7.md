---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Partie 7 : ajout de fonctionnalités | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 7 ajoute des fonctionnalités supplémentaires, telles que le compte Revie...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: ffd2b862c727db9572c272b7b21bcc33c822fffa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641994"
---
# <a name="part-7-adding-features"></a>Partie 7 : ajout de fonctionnalités

par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.
> 
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 7 ajoute des fonctionnalités supplémentaires, telles que la révision de compte, les révisions de produits et les contrôles utilisateur « éléments populaires » et « également achetés ».

## <a id="_Toc260221673"></a>Ajout de fonctionnalités

Bien que les utilisateurs puissent parcourir notre catalogue, placer des articles dans leur panier et terminer le processus de validation, il existe un certain nombre de fonctionnalités de prise en charge que nous inclurons pour améliorer notre site.

1. Revue de compte (répertorier les commandes passées et afficher les détails.)
2. Ajoutez du contenu spécifique au contexte sur la page de premier plan.
3. Ajoutez une fonctionnalité pour permettre aux utilisateurs de passer en revue les produits du catalogue.
4. Créez un contrôle utilisateur pour afficher les éléments populaires et placez ce contrôle sur la première page.
5. Créez un contrôle utilisateur « également acheté » et ajoutez-le à la page Détails du produit.
6. Ajoutez une page de contact.
7. Ajoutez une page à propos de.
8. Erreur globale

## <a id="_Toc260221674"></a>Revue de compte

Dans le dossier « Account », créez deux pages. aspx, l’une nommée OrderList. aspx et l’autre nommée OrderDetails. aspx.

OrderList. aspx tirera parti des contrôles GridView et EntityDataSource comme nous l’avons déjà fait.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

La fonction EntityDataSource sélectionne les enregistrements de la table Orders filtrés sur le nom d’utilisateur (voir WhereParameter) que nous avons défini dans une variable de session lorsque le journal de l’utilisateur est dans.

Notez également ces paramètres dans le HyperlinkField du contrôle GridView :

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Celles-ci spécifient le lien vers la vue des détails de la commande pour chaque produit en spécifiant le champ OrderID en tant que paramètre QueryString dans la page OrderDetails. aspx.

## <a id="_Toc260221675"></a>OrderDetails. aspx

Nous allons utiliser un contrôle EntityDataSource pour accéder aux commandes et à un FormView pour afficher les données de commande et un autre EntityDataSource avec un GridView pour afficher tous les éléments de ligne de la commande.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

Dans le fichier code-behind (OrderDetails.aspx.cs), nous avons deux petits bits de maintenance.

Tout d’abord, assurez-vous que OrderDetails obtient toujours un OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Nous avons également besoin de calculer et d’afficher le total des commandes à partir des éléments de ligne.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>Page d’hébergement

Nous allons ajouter du contenu statique à la page default. aspx.

Je vais tout d’abord créer un dossier « Content » (contenu) et, dans celui-ci, un dossier images (et je vais inclure une image à utiliser sur la page d’hébergement).

Dans l’espace réservé inférieur de la page default. aspx, ajoutez le balisage suivant.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>Révisions de produits

Tout d’abord, nous allons ajouter un bouton avec un lien vers un formulaire que nous pouvons utiliser pour passer en revue un produit.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Notez que nous transmettons le ProductID dans la chaîne de requête

Nous allons maintenant ajouter une page nommée ReviewAdd. aspx

Cette page utilise ASP.NET AJAX Control Toolkit. Si vous ne l’avez pas encore fait, vous pouvez le télécharger à partir de [devexpress](http://devexpress.com/act) . vous y trouverez des conseils sur la configuration de la boîte à outils pour une utilisation avec Visual Studio [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

En mode création, faites glisser les contrôles et les validateurs de la boîte à outils et créez un formulaire comme celui ci-dessous.

![](tailspin-spyworks-part-7/_static/image2.jpg)

La balise doit ressembler à ceci.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Maintenant que nous pouvons entrer des révisions, permet d’afficher ces révisions sur la page du produit.

Ajoutez cette balise à la page ProductDetails. aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

L’exécution de notre application maintenant et la navigation vers un produit affichent les informations sur le produit, y compris les évaluations des clients.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>Contrôle des éléments populaires (création de contrôles utilisateur)

Afin d’augmenter les ventes sur votre site Web, nous allons ajouter quelques fonctionnalités à des produits populaires ou connexes.

La première de ces fonctionnalités sera une liste des produits les plus populaires dans notre catalogue de produits.

Nous allons créer un « contrôle utilisateur » pour afficher les éléments les plus vendus sur la page d’hébergement de notre application. Comme il s’agit d’un contrôle, nous pouvons l’utiliser sur n’importe quelle page en faisant simplement glisser-déplacer le contrôle dans le concepteur de Visual Studio vers une page de votre choix.

Dans l’Explorateur de solutions de Visual Studio, cliquez avec le bouton droit sur le nom de la solution et créez un nouveau répertoire nommé « contrôles ». Bien qu’il ne soit pas nécessaire de le faire, nous vous aidons à garder notre projet organisé en créant tous nos contrôles utilisateur dans le répertoire « contrôles ».

Cliquez avec le bouton droit sur le dossier contrôles, puis choisissez « nouvel élément » :

![](tailspin-spyworks-part-7/_static/image4.jpg)

Spécifiez un nom pour notre contrôle de « PopularItems ». Notez que l’extension de fichier pour les contrôles utilisateur est. ascx not. aspx.

Le contrôle utilisateur des éléments populaires est défini comme suit.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Ici, nous utilisons une méthode que nous n’avons pas encore utilisée dans cette application. Nous utilisons le contrôle Repeater et, au lieu d’utiliser un contrôle de source de données, nous créons la liaison entre le contrôle Repeater et les résultats d’une requête LINQ to Entities.

Dans le code-behind de notre contrôle, procédez comme suit.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Notez également cette ligne importante en haut du balisage de votre contrôle.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Étant donné que les éléments les plus populaires ne changent pas une minute à la minute, nous pouvons ajouter une directive Aching pour améliorer les performances de notre application. Cette directive entraîne l’exécution du code de contrôle uniquement lorsque la sortie mise en cache du contrôle expire. Dans le cas contraire, la version mise en cache de la sortie du contrôle sera utilisée.

Nous devons maintenant inclure notre nouveau contrôle dans notre page default. aspx.

Utilisez l’opération glisser-déplacer pour placer une instance du contrôle dans la colonne ouverte de notre formulaire par défaut.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Maintenant, lorsque nous exécutons notre application, la page d’hébergement affiche les éléments les plus populaires.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>Contrôle « également acheté » (contrôles utilisateur avec paramètres)

Le deuxième contrôle utilisateur que nous allons créer prendrait des ventes au niveau supérieur en ajoutant une spécificité du contexte.

La logique de calcul des éléments les plus importants « également achetés » n’est pas triviale.

Notre contrôle « également acheté » sélectionne les enregistrements OrderDetails (précédemment achetés) pour le ProductID actuellement sélectionné et récupère le OrderIDs pour chaque commande unique trouvée.

Ensuite, nous allons sélectionner al les produits de toutes ces commandes et additionner les quantités achetées. Nous allons trier les produits par la somme de la quantité et afficher les cinq premiers éléments.

Étant donné la complexité de cette logique, nous allons implémenter cet algorithme en tant que procédure stockée.

Le code T-SQL de la procédure stockée est le suivant.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Notez que cette procédure stockée (SelectPurchasedWithProducts) existait dans la base de données lorsque nous l’avons incluse dans notre application et que nous avons généré le Entity Data Model nous avons spécifié cela, en plus des tables et des vues dont nous avons besoin, le Entity Data Model doit inclure cette procédure stockée.

Pour accéder à la procédure stockée à partir de la Entity Data Model nous avons besoin d’importer la fonction.

Double-cliquez sur l’Entity Data Model dans l’Explorateur de solutions pour l’ouvrir dans le concepteur et ouvrez l’Explorateur de modèles, cliquez avec le bouton droit dans le concepteur et sélectionnez « Ajouter une importation de fonction ».

![](tailspin-spyworks-part-7/_static/image1.png)

Cela ouvre cette boîte de dialogue.

![](tailspin-spyworks-part-7/_static/image2.png)

Renseignez les champs comme indiqué ci-dessus, en sélectionnant « SelectPurchasedWithProducts » et en utilisant le nom de la procédure pour le nom de notre fonction importée.

Cliquez sur « OK ».

Cela fait, nous pouvons simplement programmer par rapport à la procédure stockée, comme tout autre élément dans le modèle.

Ainsi, dans notre dossier « contrôles », créez un nouveau contrôle utilisateur nommé AlsoPurchased. ascx.

Le balisage de ce contrôle sera très familier au contrôle PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

La différence notable est que ne met pas en cache la sortie, car le rendu de l’élément est différent selon le produit.

ProductId sera une « propriété » pour le contrôle.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

Dans le gestionnaire d’événements PreRender du contrôle, nous seau à effectuer trois opérations.

1. Assurez-vous que le ProductID est défini.
2. Vérifiez si des produits ont été achetés avec le produit actuel.
3. Sortie de certains éléments, comme déterminé dans #2.

Notez combien il est facile d’appeler la procédure stockée par le biais du modèle.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Après avoir déterminé qu’il y a « également acheté », nous pouvons simplement lier le répéteur aux résultats retournés par la requête.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

S’il n’y avait pas d’éléments « également achetés », nous affichons simplement d’autres éléments populaires à partir de notre catalogue.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Pour afficher les éléments « également achetés », ouvrez la page ProductDetails. aspx, puis faites glisser le contrôle AlsoPurchased à partir de l’Explorateur de solutions afin qu’il apparaisse à cette position dans le balisage.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Vous créerez ainsi une référence au contrôle en haut de la page ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Étant donné que le contrôle utilisateur AlsoPurchased requiert un numéro ProductId, nous définissons la propriété ProductID de notre contrôle à l’aide d’une instruction eval sur l’élément de modèle de données actuel de la page.

![](tailspin-spyworks-part-7/_static/image3.png)

Quand nous créons et exécutons maintenant et que vous accédez à un produit, nous voyons les éléments « également achetés ».

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-6.md)
> [Suivant](tailspin-spyworks-part-8.md)
