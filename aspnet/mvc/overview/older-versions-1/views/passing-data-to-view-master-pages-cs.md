---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: Passage de données pour afficher les pagesC#maîtres () | Microsoft Docs
author: microsoft
description: L’objectif de ce didacticiel est d’expliquer comment vous pouvez transmettre des données d’un contrôleur à une page maître de vue. Nous examinons deux stratégies pour passer des données à une vue m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1492175812b0a092cd1594a770e348efe9b4122b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600141"
---
# <a name="passing-data-to-view-master-pages-c"></a>Passage de données à des pages maîtres de vue (C#)

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> L’objectif de ce didacticiel est d’expliquer comment vous pouvez transmettre des données d’un contrôleur à une page maître de vue. Nous examinons deux stratégies pour passer des données à une page maître de vue. Tout d’abord, nous aborderons une solution simple qui produit une application difficile à gérer. Ensuite, nous examinons une solution bien meilleure qui nécessite un peu plus de travail initial, mais génère une application beaucoup plus facile à gérer.

## <a name="passing-data-to-view-master-pages"></a>Passage de données pour afficher les pages maîtres

L’objectif de ce didacticiel est d’expliquer comment vous pouvez transmettre des données d’un contrôleur à une page maître de vue. Nous examinons deux stratégies pour passer des données à une page maître de vue. Tout d’abord, nous aborderons une solution simple qui produit une application difficile à gérer. Ensuite, nous examinons une solution bien meilleure qui nécessite un peu plus de travail initial, mais génère une application beaucoup plus facile à gérer.

### <a name="the-problem"></a>Le problème

Imaginez que vous générez une application de base de données de films et que vous souhaitez afficher la liste des catégories de films sur chaque page de votre application (voir figure 1). Imaginez, en outre, que la liste des catégories de films est stockée dans une table de base de données. Dans ce cas, il serait judicieux de récupérer les catégories à partir de la base de données et d’afficher la liste des catégories de films dans une page maître de vue.

[![affichage des catégories de films dans une page maître de vue](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)

**Figure 01**: affichage des catégories de films dans une page maître d’affichage ([cliquez pour afficher l’image en taille réelle](passing-data-to-view-master-pages-cs/_static/image3.png))

Voici le problème. Comment récupérer la liste des catégories de films dans la page maître ? Il est tentant d’appeler directement les méthodes de vos classes de modèle dans la page maître. En d’autres termes, il est tentant d’inclure le code permettant de récupérer les données de la base de données directement dans votre page maître. Toutefois, le contournement de vos contrôleurs MVC pour accéder à la base de données violerait la séparation nette des préoccupations qui est l’un des principaux avantages de la création d’une application MVC.

Dans une application MVC, vous souhaitez que toutes les interactions entre vos vues MVC et votre modèle MVC soient gérées par vos contrôleurs MVC. Cette séparation des préoccupations aboutit à une application plus gérable, adaptable et testable.

Dans une application MVC, toutes les données transmises à une vue, y compris une page maître de vue, doivent être passées à une vue par une action de contrôleur. En outre, les données doivent être transmises en tirant parti des données d’affichage. Dans le reste de ce didacticiel, j’examinerai deux méthodes pour passer les données d’affichage à une page maître de vue.

### <a name="the-simple-solution"></a>La solution simple

Commençons par la solution la plus simple pour passer les données d’affichage d’un contrôleur à une page maître de vue. La solution la plus simple consiste à passer les données d’affichage de la page maître dans chaque action de contrôleur.

Prenons le contrôleur de la liste 1. Il expose deux actions nommées `Index()` et `Details()`. La méthode d’action `Index()` retourne chaque film dans la table de base de données de films. La méthode d’action `Details()` retourne chaque film dans une catégorie de films particulière.

**Liste 1 – `Controllers\HomeController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

Notez que les actions index () et Details () ajoutent deux éléments pour afficher les données. L’action index () ajoute deux clés : les catégories et les films. La clé catégories représente la liste des catégories de films affichées par la page maître de la vue. La clé films représente la liste des films affichés par la page affichage de l’index.

L’action détails () ajoute également deux clés nommées catégories et films. La clé catégories, une fois encore, représente la liste des catégories de films affichées par la page maître de la vue. La clé films représente la liste des films dans une catégorie particulière affichée par la page mode Détails (voir figure 2).

[![le mode Détails](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)

**Figure 02**: mode Détails ([cliquez pour afficher l’image en taille réelle](passing-data-to-view-master-pages-cs/_static/image6.png))

La vue index est contenue dans la liste 2. Elle effectue simplement une itération dans la liste des films représentés par l’élément movies dans afficher les données.

**Liste 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

La page maître de la vue est contenue dans la liste 3. La page maître d’affichage itère et affiche toutes les catégories de films représentées par l’élément catégories à partir des données d’affichage.

**Liste 3 – `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

Toutes les données sont passées à la vue et à la page maître de vue par l’intermédiaire des données d’affichage. C’est la méthode correcte pour passer des données à la page maître.

Alors, quel est le problème avec cette solution ? Le problème est que cette solution ne respecte pas le principe sec (Ne vous répétez pas). Chaque action de contrôleur doit ajouter la liste des différentes catégories de films pour afficher les données. Le fait de disposer d’un code en double dans votre application rend votre application bien plus difficile à gérer, à adapter et à modifier.

### <a name="the-good-solution"></a>La bonne solution

Dans cette section, nous examinons une solution alternative, et meilleure, pour passer des données d’une action de contrôleur à une page maître de vue. Au lieu d’ajouter les catégories de films pour la page maître dans chaque action de contrôleur, nous n’ajoutons les catégories de films aux données d’affichage qu’une seule fois. Toutes les données d’affichage utilisées par la page maître d’affichage sont ajoutées dans un contrôleur d’application.

La classe ApplicationController est contenue dans la liste 4.

**Liste 4 – `Controllers\ApplicationController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

Il y a trois choses que vous devez remarquer sur le contrôleur d’application dans la liste 4. Tout d’abord, Notez que la classe hérite de la classe de base System. Web. Mvc. Controller. Le contrôleur d’application est une classe de contrôleur.

Deuxièmement, Notez que la classe de contrôleur d’application est une classe abstraite. Une classe abstraite est une classe qui doit être implémentée par une classe concrète. Étant donné que le contrôleur d’application est une classe abstraite, vous ne pouvez pas appeler directement des méthodes définies dans la classe. Si vous tentez d’appeler la classe d’application directement, vous obtiendrez un message d’erreur indiquant que la ressource est introuvable.

Troisièmement, Notez que le contrôleur d’application contient un constructeur qui ajoute la liste des catégories de films pour afficher les données. Chaque classe de contrôleur qui hérite du contrôleur d’application appelle automatiquement le constructeur du contrôleur d’application. Chaque fois que vous appelez une action sur un contrôleur qui hérite du contrôleur d’application, les catégories de films sont automatiquement incluses dans les données d’affichage.

Le contrôleur de films de la liste 5 hérite du contrôleur d’application.

**Liste 5 – `Controllers\MoviesController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

Le contrôleur movies, comme le contrôleur d’origine abordé dans la section précédente, expose deux méthodes d’action nommées `Index()` et `Details()`. Notez que la liste des catégories de films affichées par la page maître d’affichage n’est pas ajoutée pour afficher les données dans la méthode `Index()` ou `Details()`. Étant donné que le contrôleur de films hérite du contrôleur d’application, la liste des catégories de films est ajoutée automatiquement pour afficher les données.

Notez que cette solution pour ajouter des données d’affichage pour une page maître de vue n’enfreint pas le principe sec (Ne vous répétez pas). Le code permettant d’ajouter la liste des catégories de films pour afficher les données est contenu dans un seul emplacement : le constructeur du contrôleur d’application.

### <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons abordé deux approches pour passer les données d’affichage d’un contrôleur à une page maître de vue. Tout d’abord, nous avons examiné une approche simple, mais difficile à gérer. Dans la première section, nous avons vu comment ajouter des données d’affichage pour une page maître de vue dans chaque action de contrôleur dans votre application. Nous avons conclu qu’il s’agissait d’une mauvaise approche, car elle viole le principe sec (Ne vous répétez pas).

Ensuite, nous avons examiné une stratégie bien meilleure pour ajouter les données requises par une page maître de vue pour afficher les données. Au lieu d’ajouter les données d’affichage à chaque action de contrôleur, nous avons ajouté les données d’affichage une seule fois dans un contrôleur d’application. De cette façon, vous pouvez éviter la duplication de code lors du passage de données à une page maître d’affichage dans une application MVC ASP.NET.

> [!div class="step-by-step"]
> [Précédent](creating-page-layouts-with-view-master-pages-cs.md)
> [Suivant](asp-net-mvc-views-overview-vb.md)
