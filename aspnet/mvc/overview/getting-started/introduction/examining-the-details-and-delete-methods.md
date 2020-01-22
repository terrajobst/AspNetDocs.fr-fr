---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: Examen des méthodes Details et Delete | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 03/26/2015
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 4ec8d239377d37d7e27fa23c0b1caef7420046ae
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519009"
---
# <a name="examining-the-details-and-delete-methods"></a>Examen des méthodes Details et Delete

par [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

Dans cette partie du didacticiel, vous allez examiner les méthodes de `Details` et de `Delete` générées automatiquement.

## <a name="examining-the-details-and-delete-methods"></a>Examen des méthodes Details et Delete

Ouvrez le contrôleur `Movie` et examinez la méthode `Details`.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Le moteur de génération de modèles automatique MVC qui a créé cette méthode d’action ajoute un commentaire indiquant une requête HTTP qui appelle la méthode. Dans ce cas, il s’agit d’une demande `GET` avec trois segments d’URL, le contrôleur de `Movies`, la méthode `Details` et une valeur `ID`.

Code First facilite la recherche de données à l’aide de la méthode `Find`. Une fonctionnalité de sécurité importante intégrée à la méthode est que le code vérifie que la méthode `Find` a trouvé un film avant que le code ne tente d’en faire quelque chose. Par exemple, un pirate peut introduire des erreurs dans le site en modifiant l’URL créée par les liens de `http://localhost:xxxx/Movies/Details/1` à une valeur telle que `http://localhost:xxxx/Movies/Details/12345` (ou une autre valeur qui ne représente pas un film réel). Si vous n’avez pas vérifié un film null, un film null entraînerait une erreur de base de données.

Examinez les méthodes `Delete` et `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Notez que la méthode HTTP d' `Delete` ne supprime pas le film spécifié, mais renvoie une vue du film dans laquelle vous pouvez envoyer (`HttpPost`) la suppression. L’exécution d’une opération de suppression en réponse à une requête GET (ou encore l’exécution d’une opération de modification, d’une opération de création ou de toute autre opération qui modifie des données) génère une faille de sécurité. Pour plus d’informations à ce sujet, consultez entrée de blog de Stephen Walther [ASP.net Conseil MVC #46 — n’utilisez pas supprimer les liens, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

La méthode `HttpPost` qui supprime les données est nommée `DeleteConfirmed` pour donner à la méthode HTTP POST une signature ou un nom unique. Les signatures des deux méthodes sont illustrées ci-dessous :

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Le Common Language Runtime (CLR) nécessite des méthodes surchargées pour avoir une signature à paramètre unique (même nom de méthode, mais liste de paramètres différentes). Toutefois, ici, vous avez besoin de deux méthodes Delete, une pour obtenir et une pour la publication, qui ont toutes les deux la même signature de paramètre. (Elles doivent toutes les deux accepter un entier unique comme paramètre.)

Pour le trier, vous pouvez effectuer quelques opérations. La première consiste à attribuer des noms différents aux méthodes. C’est ce qu’a fait le mécanisme de génération de modèles automatique dans l’exemple précédent. Toutefois, elle présente un petit problème : ASP.NET mappe des segments d’une URL à des méthodes d’action par nom. Si vous renommez une méthode, il est probable que le routage ne pourra pas trouver cette méthode. La solution consiste à faire ce que vous voyez dans l’exemple, c’est-à-dire à ajouter l’attribut `ActionName("Delete")` à la méthode `DeleteConfirmed`. Cela permet d’effectuer le mappage pour le système de routage de sorte qu’une URL qui comprend */Delete/* pour une requête de publication trouve la méthode `DeleteConfirmed`.

Une autre méthode courante pour éviter un problème avec les méthodes qui ont des noms et des signatures identiques consiste à modifier artificiellement la signature de la méthode de publication pour inclure un paramètre inutilisé. Par exemple, certains développeurs ajoutent un type de paramètre `FormCollection` qui est passé à la méthode de publication, puis n’utilisent pas le paramètre :

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Récapitulatif

Vous disposez maintenant d’une application MVC ASP.NET complète qui stocke les données dans une base de données de base de données locale. Vous pouvez créer, lire, mettre à jour, supprimer et Rechercher des films.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez créé et testé une application Web, l’étape suivante consiste à la mettre à la disposition d’autres personnes à utiliser sur Internet. Pour ce faire, vous devez le déployer sur un fournisseur d’hébergement Web. Microsoft offre un hébergement Web gratuit pour un maximum de 10 sites Web dans un [compte d’évaluation Azure gratuit](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Je vous suggère de suivre le didacticiel suivant : [déploiement d’une application ASP.NET MVC sécurisée avec appartenance, OAuth et SQL Database à Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un excellent didacticiel est le niveau intermédiaire de Tom Dykstra [création d’un modèle de données Entity Framework pour une Application ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [StackOverflow](http://stackoverflow.com/help) et les [Forums ASP.NET MVC](https://forums.asp.net/1146.aspx) sont des emplacements formidables pour poser des questions. Suivez le didacticiel sur Twitter pour pouvoir [Télécharger des mises](https://twitter.com/RickAndMSFT) à jour dans Mes derniers didacticiels.

Nous vous invitons à nous faire part de vos commentaires.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter : [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter : [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Précédent](adding-validation.md)
