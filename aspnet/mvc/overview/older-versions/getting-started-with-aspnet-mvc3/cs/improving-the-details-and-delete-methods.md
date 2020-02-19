---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Amélioration des méthodes Details et DeleteC#() | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 54d7be8fe1bff604ae9c9e9914d7c6426ab85c1c
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457503"
---
# <a name="improving-the-details-and-delete-methods-c"></a>Examen des méthodes Details et Delete (C#)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../../getting-started/introduction/getting-started.md) et utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sûr, bien plus simple à suivre et qui illustre davantage de fonctionnalités.
> 
> 
> Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :
> 
> - [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet Visual Web Developer C# avec le code source est disponible pour accompagner cette rubrique. [Téléchargez la C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez Visual Basic, passez à la [version Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de ce didacticiel.

Dans cette partie du didacticiel, vous allez apporter des améliorations aux méthodes `Details` et `Delete` générées automatiquement. Ces modifications ne sont pas requises, mais avec seulement quelques petits morceaux de code, vous pouvez facilement améliorer l’application.

## <a name="improving-the-details-and-delete-methods"></a>Amélioration des méthodes Details et Delete

Lorsque vous avez généré le contrôleur de `Movie`, ASP.NET MVC a généré du code qui fonctionnait bien, mais cela peut être rendu plus robuste avec seulement quelques petites modifications.

Ouvrez le contrôleur `Movie` et modifiez la méthode `Details` en retournant `HttpNotFound` lorsqu’un film est introuvable. Vous devez également modifier la méthode `Details` pour définir une valeur par défaut pour l’ID qui lui est passé. (Vous avez apporté des modifications similaires à la méthode `Edit` dans la [partie 6](examining-the-edit-methods-and-edit-view.md) de ce didacticiel). Toutefois, vous devez modifier le type de retour de la méthode `Details` de `ViewResult` à `ActionResult`, car la méthode `HttpNotFound` ne retourne pas d’objet `ViewResult`. L’exemple suivant illustre la méthode `Details` modifiée.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Code First facilite la recherche de données à l’aide de la méthode `Find`. Une fonctionnalité de sécurité importante que nous avons créée dans la méthode est que le code vérifie que la méthode `Find` a trouvé un film avant que le code ne tente d’en faire quelque chose. Par exemple, un pirate peut introduire des erreurs dans le site en modifiant l’URL créée par les liens de `http://localhost:xxxx/Movies/Details/1` à une valeur telle que `http://localhost:xxxx/Movies/Details/12345` (ou une autre valeur qui ne représente pas un film réel). Si vous n’activez pas la case à cocher pour un film null, une erreur de base de données peut se produire.

De même, modifiez les méthodes `Delete` et `DeleteConfirmed` pour spécifier une valeur par défaut pour le paramètre ID et pour retourner `HttpNotFound` lorsqu’un film est introuvable. Les méthodes de `Delete` mises à jour dans le contrôleur de `Movie` sont présentées ci-dessous.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Notez que la méthode `Delete` ne supprime pas les données. L’exécution d’une opération de suppression en réponse à une requête GET (ou encore l’exécution d’une opération de modification, d’une opération de création ou de toute autre opération qui modifie des données) génère une faille de sécurité. Pour plus d’informations à ce sujet, consultez entrée de blog de Stephen Walther [ASP.net Conseil MVC #46 — n’utilisez pas supprimer les liens, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

La méthode `HttpPost` qui supprime les données est nommée `DeleteConfirmed` pour donner à la méthode HTTP POST une signature ou un nom unique. Les signatures des deux méthodes sont illustrées ci-dessous :

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

Le common language runtime (CLR) requiert que les méthodes surchargées aient une signature unique (même nom, une liste différente de paramètres). Toutefois, ici, vous avez besoin de deux méthodes DELETE : une pour obtenir et une pour la publication, qui requièrent la même signature. (Elles doivent toutes les deux accepter un entier unique comme paramètre.)

Pour le trier, vous pouvez effectuer quelques opérations. La première consiste à attribuer des noms différents aux méthodes. C’est ce que nous avons fait dans l’exemple précédent. Toutefois, elle présente un petit problème : ASP.NET mappe des segments d’une URL à des méthodes d’action par nom. Si vous renommez une méthode, il est probable que le routage ne pourra pas trouver cette méthode. La solution consiste à faire ce que vous voyez dans l’exemple, c’est-à-dire à ajouter l’attribut `ActionName("Delete")` à la méthode `DeleteConfirmed`. Cela permet d’effectuer le mappage pour le système de routage de sorte qu’une URL qui comprend <em>/Delete/</em>pour une requête de publication trouve la méthode `DeleteConfirmed`.

Une autre façon d’éviter un problème avec les méthodes qui ont des noms et des signatures identiques consiste à modifier artificiellement la signature de la méthode de publication pour inclure un paramètre inutilisé. Par exemple, certains développeurs ajoutent un type de paramètre `FormCollection` qui est passé à la méthode de publication, puis n’utilisent pas le paramètre :

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Conclusion

Vous disposez maintenant d’une application MVC ASP.NET complète qui stocke les données dans une base de données SQL Server Compact. Vous pouvez créer, lire, mettre à jour, supprimer et Rechercher des films.

![](improving-the-details-and-delete-methods/_static/image1.png)

Ce didacticiel de base vous a fait commencer à créer des contrôleurs, à les associer à des vues et à transmettre des données codées en dur. Ensuite, vous avez créé et conçu un modèle de données. Entity Framework Code First créé une base de données à partir du modèle de données à la volée, et le système de génération de modèles automatique ASP.NET MVC a généré automatiquement les méthodes et les vues d’action pour les opérations CRUD de base. Vous avez ensuite ajouté un formulaire de recherche qui permet aux utilisateurs de rechercher dans la base de données. Vous avez modifié la base de données pour inclure une nouvelle colonne de données, puis mis à jour deux pages pour créer et afficher ces nouvelles données. Vous avez ajouté la validation en marquant le modèle de données avec les attributs de l’espace de noms `DataAnnotations`. La validation résultante s’exécute sur le client et sur le serveur.

Si vous souhaitez déployer votre application, il est utile de commencer par tester l’application sur votre serveur IIS 7 local. Vous pouvez utiliser ce [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) lien pour activer le paramètre IIS pour les applications ASP.net. Consultez les liens de déploiement suivants :

- [Plan du contenu de déploiement ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Activation d’IIS 7. x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Déploiement des projets d’application Web](https://msdn.microsoft.com/library/dd394698.aspx)

Je vous encourage maintenant à passer à notre niveau intermédiaire pour [créer un modèle de données Entity Framework pour une application mvc ASP.net](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) et des didacticiels du [magasin de musique MVC](../../mvc-music-store/mvc-music-store-part-1.md) , pour explorer les [Articles ASP.net sur MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx), et pour consulter les nombreuses vidéos et ressources disponibles sur [https://asp.net/mvc](https://asp.net/mvc) pour en savoir plus sur ASP.NET MVC ! Les [forums ASP.NET MVC](https://forums.asp.net/1146.aspx) sont l’endroit idéal pour poser des questions.

Vous n’avez plus qu’à l’utiliser !

— Scott Hanselman ([http://hanselman.com](http://hanselman.com) et [@shanselman](http://twitter.com/shanselman) sur Twitter) et Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Précédent](adding-validation-to-the-model.md)
