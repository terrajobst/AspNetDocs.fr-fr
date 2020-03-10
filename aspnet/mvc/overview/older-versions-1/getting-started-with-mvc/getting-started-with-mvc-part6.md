---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Ajout d’une méthode Create et création d’une vue | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Créer une application Web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 05a281720f76b107fe8d902ef60d5d2e72af3ef5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543658"
---
# <a name="adding-a-create-method-and-create-view"></a>Ajout d’une méthode de création et d’une vue de création

par [Scott Hanselman](https://github.com/shanselman)

> Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données. Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.

Dans cette section, nous allons implémenter la prise en charge nécessaire pour permettre aux utilisateurs de créer des films dans notre base de données. Pour ce faire, nous allons implémenter l’action d’URL/Movies/Create.

L’implémentation de l’URL/Movies/Create est un processus en deux étapes. Lorsqu’un utilisateur visite pour la première fois l’URL/Movies/Create, nous souhaitons afficher un formulaire HTML qu’il peut remplir pour entrer dans un nouveau film. Ensuite, lorsque l’utilisateur envoie le formulaire et publie les données sur le serveur, nous souhaitons récupérer le contenu publié et l’enregistrer dans la base de données.

Nous allons implémenter ces deux étapes dans deux méthodes Create () au sein de notre classe MoviesController. Une méthode affiche le formulaire &lt;&gt; que l’utilisateur doit remplir pour créer un nouveau film. La deuxième méthode gère le traitement des données publiées lorsque l’utilisateur soumet le formulaire de &lt;&gt; à nouveau au serveur et enregistre un nouveau film dans notre base de données.

Voici le code que nous ajouterons à notre classe MoviesController pour l’implémenter :

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Le code ci-dessus contient tout le code dont nous aurons besoin au sein de notre contrôleur.

Nous allons maintenant implémenter le modèle de vue créer que nous allons utiliser pour afficher un formulaire à l’utilisateur. Nous allons cliquer avec le bouton droit dans la première méthode Create et sélectionner « Add View » (ajouter une vue) pour créer le modèle de vue pour notre formulaire de film.

Nous allons sélectionner que nous allons passer le modèle de vue « Movie » comme classe de données de vue, et indiquer que nous voulons « échafauder » un modèle « Create » (créer).

[![ajouter une vue](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Une fois que vous avez cliqué sur le bouton Ajouter, le modèle de vue \Movies\Create.aspx est créé pour vous. Étant donné que nous avons sélectionné « créer » dans la liste déroulante « Afficher le contenu », la boîte de dialogue Ajouter une vue « échafaudait » automatiquement le contenu par défaut pour nous. La génération de modèles automatique a créé un formulaire de &lt;HTML&gt;, un emplacement pour les messages d’erreur de validation, et puisque la génération de modèles automatique connaît les films, elle a créé des étiquettes et des champs pour chaque propriété de notre classe.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Dans la mesure où notre base de données donne automatiquement un ID à un film, nous allons supprimer ces champs qui font référence au modèle. ID de notre vue Create. Supprimez les 7 lignes après &lt;&gt;champs de légende&lt;la&gt;/Legend, car ils affichent le champ d’ID que nous ne souhaitons pas.

Créons maintenant un nouveau film et l’ajoutons à la base de données. Pour ce faire, nous allons réexécuter l’application et accéder à l’URL « /movies », puis cliquer sur le lien « créer » pour ajouter un nouveau film.

[![créer-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Lorsque nous cliquons sur le bouton créer, nous publierons (via HTTP POST) les données de ce formulaire vers la méthode/Movies/Create que nous venons de créer. Tout comme lorsque le système a pris les paramètres « numTimes » et « Name » de l’URL et les a mappés à des paramètres sur une méthode précédemment, le système prend automatiquement les champs de formulaire d’une publication et les mappe à un objet. Dans ce cas, les valeurs des champs en HTML, telles que « Released » et « title », sont automatiquement placées dans les propriétés appropriées d’une nouvelle instance d’un film.

Examinons à nouveau la deuxième méthode Create de notre MoviesController. Notez qu’il prend un objet « Movie » comme argument :

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Cet objet film a ensuite été transmis à la version [HttpPost] de notre méthode Create action et nous l’avons enregistré dans la base de données, puis Redirigé l’utilisateur vers la méthode d’action index (), qui affiche le résultat enregistré dans la liste de films :

[Liste des films ![-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Toutefois, nous ne vérifions pas si nos films sont corrects et la base de données ne nous permettra pas d’enregistrer un film sans titre. C’est agréable si nous pouvions dire à l’utilisateur avant que la base de données ne relevait une erreur. Nous allons le faire ensuite en ajoutant la prise en charge de la validation à notre application.

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part5.md)
> [Suivant](getting-started-with-mvc-part7.md)
