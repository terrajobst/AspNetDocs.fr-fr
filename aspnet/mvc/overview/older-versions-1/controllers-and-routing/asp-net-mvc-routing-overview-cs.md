---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Vue d’ensemble du routageC#ASP.NET MVC () | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther montre comment l’infrastructure ASP.NET MVC mappe les demandes de navigateur aux actions de contrôleur.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e1155ca676e7a25b5bfc63e251c6387a010eb34
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544141"
---
# <a name="aspnet-mvc-routing-overview-c"></a>Vue d’ensemble du routage ASP.NET MVC (C#)

par [Stephen Walther](https://github.com/StephenWalther)

> Dans ce didacticiel, Stephen Walther montre comment l’infrastructure ASP.NET MVC mappe les demandes de navigateur aux actions de contrôleur.

Dans ce didacticiel, vous allez découvrir une fonctionnalité importante de chaque application ASP.NET MVC appelée *routage ASP.net*. Le module de routage ASP.NET est chargé de mapper les demandes de navigateur entrantes à des actions de contrôleur MVC particulières. À la fin de ce didacticiel, vous comprendrez comment la table de routage standard mappe les demandes aux actions de contrôleur.

## <a name="using-the-default-route-table"></a>Utilisation de la table de routage par défaut

Lorsque vous créez une application MVC ASP.NET, l’application est déjà configurée pour utiliser le routage ASP.NET. Le routage ASP.NET est configuré à deux emplacements.

Tout d’abord, le routage ASP.NET est activé dans le fichier de configuration Web de votre application (fichier Web. config). Quatre sections du fichier de configuration sont pertinentes pour le routage : la section System. Web. httpModules, la section System. Web. httpHandlers, la section System. webserver. modules et la section System. webserver. Handlers. Veillez à ne pas supprimer ces sections, car sans ces sections le routage ne fonctionnera plus.

Deuxièmement, et plus important encore, une table de routage est créée dans le fichier global. asax de l’application. Le fichier global. asax est un fichier spécial qui contient des gestionnaires d’événements pour les événements de cycle de vie d’application ASP.NET. La table de routage est créée lors de l’événement de démarrage de l’application.

Le fichier dans la liste 1 contient le fichier global. asax par défaut pour une application MVC ASP.NET.

**Liste 1-Global.asax.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

Lors du premier démarrage d’une application MVC, l’application\_méthode Start () est appelée. Cette méthode appelle à son tour la méthode RegisterRoutes (). La méthode RegisterRoutes () crée la table de routage.

La table de routage par défaut contient un seul itinéraire (nommé Default). L’itinéraire par défaut mappe le premier segment d’une URL à un nom de contrôleur, le deuxième segment d’une URL à une action de contrôleur et le troisième segment à un paramètre nommé **ID**.

Imaginez que vous entrez l’URL suivante dans la barre d’adresse de votre navigateur Web :

/Home/Index/3

L’itinéraire par défaut mappe cette URL aux paramètres suivants :

- contrôleur = page d’hébergement

- action = index

- id = 3

Lorsque vous demandez l’URL/Home/Index/3, le code suivant est exécuté :

HomeController.Index(3)

L’itinéraire par défaut comprend les valeurs par défaut pour les trois paramètres. Si vous ne fournissez pas de contrôleur, le paramètre de contrôleur a par défaut la valeur **orig**. Si vous ne fournissez pas d’action, le paramètre d’action est défini par défaut sur l' **index**de valeur. Enfin, si vous ne fournissez pas d’ID, le paramètre ID est une chaîne vide par défaut.

Examinons quelques exemples de la façon dont l’itinéraire par défaut mappe les URL aux actions de contrôleur. Imaginez que vous entrez l’URL suivante dans la barre d’adresse de votre navigateur :

/Home

En raison des valeurs par défaut des paramètres d’itinéraire par défaut, l’entrée de cette URL entraîne l’appel de la méthode index () de la classe HomeController dans Listing 2.

**Liste 2-HomeController.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

Dans la liste 2, la classe HomeController contient une méthode nommée index () qui accepte un paramètre unique nommé ID. L’URL/Home provoque l’appel de la méthode index () avec une chaîne vide comme valeur du paramètre ID.

En raison de la façon dont l’infrastructure MVC appelle les actions de contrôleur, l’URL/Home correspond également à la méthode index () de la classe HomeController dans le Listing 3.

**Liste 3-HomeController.cs (action d’index sans paramètre)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

La méthode index () de la liste 3 n’accepte aucun paramètre. L’URL/Home entraînera l’appel de cette méthode index (). L’URL/Home/Index/3 appelle également cette méthode (l’ID est ignoré).

L’URL/Home correspond également à la méthode index () de la classe HomeController de la liste 4.

**Liste 4-HomeController.cs (action d’index avec paramètre Nullable)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

Dans la liste 4, la méthode index () a un paramètre entier. Étant donné que le paramètre est un paramètre Nullable (peut avoir la valeur null), l’index () peut être appelé sans déclencher d’erreur.

Enfin, l’appel de la méthode index () dans la liste 5 avec l’URL/Home provoque une exception, car le paramètre ID *n’est pas* un paramètre Nullable. Si vous tentez d’appeler la méthode index (), vous recevez l’erreur affichée à la figure 1.

**Liste 5-HomeController.cs (action d’index avec paramètre ID)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]

[![de l’appel d’une action de contrôleur qui attend une valeur de paramètre](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**Figure 01**: appel d’une action de contrôleur qui attend une valeur de paramètre ([cliquez pour afficher l’image en taille réelle](asp-net-mvc-routing-overview-cs/_static/image2.png))

L’URL/Home/Index/3, en revanche, fonctionne parfaitement avec l’action du contrôleur d’index dans la liste 5. La requête/Home/Index/3 provoque l’appel de la méthode index () avec un paramètre ID qui a la valeur 3.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel était de vous fournir une brève présentation du routage ASP.NET. Nous avons examiné la table de routage par défaut que vous avez obtenue avec une nouvelle application ASP.NET MVC. Vous avez appris comment l’itinéraire par défaut mappe les URL aux actions du contrôleur.

> [!div class="step-by-step"]
> [Next](understanding-action-filters-cs.md)
