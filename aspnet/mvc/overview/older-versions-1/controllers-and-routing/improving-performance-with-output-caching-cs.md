---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Amélioration des performances avec la miseC#en cache de sortie () | Microsoft Docs
author: microsoft
description: Dans ce didacticiel, vous allez apprendre comment améliorer considérablement les performances de vos applications Web ASP.NET MVC en tirant parti de la mise en cache de sortie. Vous...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 548c5bea2e9cf26e0574e72d2c0ea204dbd90f9c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601268"
---
# <a name="improving-performance-with-output-caching-c"></a>Amélioration des performances avec la mise en cache de la sortie (C#)

par [Microsoft](https://github.com/microsoft)

> Dans ce didacticiel, vous allez apprendre comment améliorer considérablement les performances de vos applications Web ASP.NET MVC en tirant parti de la mise en cache de sortie. Vous apprenez à mettre en cache le résultat retourné par une action de contrôleur afin que le même contenu n’ait pas besoin d’être créé chaque fois qu’un nouvel utilisateur appelle l’action.

L’objectif de ce didacticiel est de vous expliquer comment vous pouvez améliorer considérablement les performances d’une application ASP.NET MVC en tirant parti du cache de sortie. Le cache de sortie vous permet de mettre en cache le contenu retourné par une action de contrôleur. De cette façon, le même contenu n’a pas besoin d’être généré chaque fois que la même action de contrôleur est appelée.

Imaginez, par exemple, que votre application ASP.NET MVC affiche une liste d’enregistrements de base de données dans une vue nommée index. Normalement, chaque fois qu’un utilisateur appelle l’action de contrôleur qui retourne la vue index, l’ensemble des enregistrements de base de données doit être récupéré de la base de données en exécutant une requête de base de données.

Si, en revanche, vous tirez parti du cache de sortie, vous pouvez éviter d’exécuter une requête de base de données chaque fois qu’un utilisateur appelle la même action de contrôleur. La vue peut être récupérée à partir du cache au lieu d’être régénérée à partir de l’action du contrôleur. La mise en cache vous permet d’éviter d’effectuer des tâches redondantes sur le serveur.

## <a name="enabling-output-caching"></a>Activation de la mise en cache de sortie

Vous activez la mise en cache de sortie en ajoutant un attribut [OutputCache] à une action de contrôleur individuelle ou à une classe de contrôleur entière. Par exemple, le contrôleur de la liste 1 expose une action nommée index (). La sortie de l’action d’index () est mise en cache pendant 10 secondes.

**Liste 1 – Controllers\HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

Dans les versions bêta de ASP.NET MVC, la mise en cache de sortie ne fonctionne pas pour une URL telle que [http://www.MySite.com/](http://www.mysite.com/). Au lieu de cela, vous devez entrer une URL telle que [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index). 

Dans la liste 1, la sortie de l’action index () est mise en cache pendant 10 secondes. Si vous préférez, vous pouvez spécifier une durée de cache bien plus longue. Par exemple, si vous souhaitez mettre en cache la sortie d’une action de contrôleur pendant un jour, vous pouvez spécifier une durée de cache de 86400 secondes (60 secondes \* 60 minutes \* 24 heures).

Il n’y a aucune garantie que le contenu sera mis en cache pendant la durée que vous spécifiez. Lorsque les ressources mémoire sont insuffisantes, le cache commence à supprimer automatiquement le contenu.

Le contrôleur d’hébergement de la liste 1 retourne la vue index dans la liste 2. Il n’y a rien de spécial sur cette vue. La vue index affiche simplement l’heure actuelle (voir figure 1).

**Liste 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Figure 1 : vue d’index mise en cache**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Si vous appelez l’action index () plusieurs fois en entrant l’URL/Home/Index dans la barre d’adresses de votre navigateur et en appuyant sur le bouton actualiser/recharger dans votre navigateur de façon répétée, l’heure affichée par la vue index ne changera pas pendant 10 secondes. La même heure s’affiche, car la vue est mise en cache.

Il est important de comprendre que la même vue est mise en cache pour toute personne qui visite votre application. Toute personne qui appelle l’action index () obtiendra la même version mise en cache de la vue index. Cela signifie que la quantité de travail que le serveur Web doit effectuer pour servir la vue d’index est considérablement réduite.

La vue de la liste 2 se déroule comme un processus très simple. La vue affiche simplement l’heure actuelle. Toutefois, vous pouvez tout aussi facilement mettre en cache une vue qui affiche un ensemble d’enregistrements de base de données. Dans ce cas, l’ensemble des enregistrements de la base de données n’a pas besoin d’être récupéré de la base de données chaque fois que l’action du contrôleur qui retourne la vue est appelée. La mise en cache peut réduire la quantité de travail que votre serveur Web et votre serveur de base de données doivent effectuer.

N’utilisez pas la page &lt;la directive% @ OutputCache%&gt; dans une vue MVC. Cette directive est un saignement du monde Web Forms et ne doit pas être utilisée dans une application ASP.NET MVC.

## <a name="where-content-is-cached"></a>Emplacement de mise en cache du contenu

Par défaut, lorsque vous utilisez l’attribut [OutputCache], le contenu est mis en cache dans trois emplacements : le serveur Web, les serveurs proxy et le navigateur Web. Vous pouvez contrôler exactement où le contenu est mis en cache en modifiant la propriété Location de l’attribut [OutputCache].

Vous pouvez définir la propriété Location sur l’une des valeurs suivantes :

> · Aux
> 
> · Client
> 
> · Situé
> 
> · Serveurs
> 
> · None
> 
> · ServerAndClient

Par défaut, la propriété Location a la valeur any. Toutefois, dans certaines situations, vous souhaiterez peut-être mettre en cache uniquement sur le navigateur ou uniquement sur le serveur. Par exemple, si vous mettez en cache des informations personnalisées pour chaque utilisateur, vous ne devez pas mettre en cache les informations sur le serveur. Si vous affichez des informations différentes pour différents utilisateurs, vous devez mettre en cache les informations uniquement sur le client.

Par exemple, le contrôleur de la liste 3 expose une action nommée GetName () qui retourne le nom d’utilisateur actuel. Si Jack se connecte au site Web et appelle l’action GetName (), l’action retourne la chaîne « Hi Jack ». Si, par la suite, Jill se connecte au site Web et appelle l’action GetName (), il obtiendra également la chaîne « Hi Jack ». La chaîne est mise en cache sur le serveur Web pour tous les utilisateurs après que la prise Jack appelle initialement l’action du contrôleur.

**Liste 3 – Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Probablement, le contrôleur de la liste 3 ne fonctionne pas comme vous le souhaitez. Vous ne souhaitez pas afficher le message « Hi Jack » à Jill.

Vous ne devez jamais mettre en cache du contenu personnalisé dans le cache du serveur. Toutefois, vous souhaiterez peut-être mettre en cache le contenu personnalisé dans le cache du navigateur pour améliorer les performances. Si vous mettez en cache du contenu dans le navigateur et qu’un utilisateur appelle la même action de contrôleur plusieurs fois, le contenu peut être récupéré à partir du cache du navigateur au lieu du serveur.

Le contrôleur modifié dans la liste 4 met en cache la sortie de l’action GetName (). Toutefois, le contenu est mis en cache uniquement sur le navigateur et non sur le serveur. Ainsi, lorsque plusieurs utilisateurs appellent la méthode GetName (), chaque personne obtient son propre nom d’utilisateur et non pas le nom d’utilisateur d’une autre personne.

**Liste 4 – Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Notez que l’attribut [OutputCache] dans la liste 4 comprend une propriété Location définie sur la valeur OutputCacheLocation. client. L’attribut [OutputCache] comprend également une propriété NoStore. La propriété NoStore est utilisée pour informer les serveurs proxy et le navigateur qu’ils ne doivent pas stocker une copie permanente du contenu mis en cache.

## <a name="varying-the-output-cache"></a>Variation du cache de sortie

Dans certains cas, vous souhaiterez peut-être avoir différentes versions mises en cache du même contenu. Imaginez, par exemple, que vous créez une page maître/détail. La page maître affiche la liste des titres de films. Lorsque vous cliquez sur un titre, vous recevez des détails sur le film sélectionné.

Si vous mettez en cache la page de détails, les détails du même film s’affichent quel que soit le film sur lequel vous cliquez. Le premier film sélectionné par le premier utilisateur sera affiché à tous les utilisateurs futurs.

Vous pouvez résoudre ce problème en tirant parti de la propriété VaryByParam de l’attribut [OutputCache]. Cette propriété vous permet de créer différentes versions mises en cache du même contenu lorsqu’un paramètre de formulaire ou un paramètre de chaîne de requête varie.

Par exemple, le contrôleur de la liste 5 expose deux actions nommées Master () et Details (). L’action Master () renvoie une liste des titres de films et l’action Details () renvoie les détails de la vidéo sélectionnée.

**Liste 5 – Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

L’action Master () comprend une propriété VaryByParam avec la valeur « None ». Lorsque l’action Master () est appelée, la même version mise en cache de la vue principale est retournée. Les paramètres de formulaire ou les paramètres de chaîne de requête sont ignorés (voir figure 2).

**Figure 2 : vue/Movies/Master**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Figure 3 : vue/Movies/Details**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

L’action Details () comprend une propriété VaryByParam avec la valeur « ID ». Lorsque différentes valeurs du paramètre ID sont transmises à l’action du contrôleur, différentes versions mises en cache de la vue détails sont générées.

Il est important de comprendre que l’utilisation de la propriété VaryByParam entraîne une plus grande mise en cache et non pas moins. Une autre version mise en cache de la vue Détails est créée pour chaque version différente du paramètre ID.

Vous pouvez définir la propriété VaryByParam sur les valeurs suivantes :

> \* = créer une version mise en cache différente chaque fois qu’un paramètre de chaîne de requête ou de formulaire varie.
> 
> None = ne jamais créer des versions mises en cache différentes
> 
> Liste de points-virgules des paramètres = créer différentes versions mises en cache chaque fois que les paramètres de la forme ou de la chaîne de requête dans la liste varient

## <a name="creating-a-cache-profile"></a>Création d’un profil de cache

En guise d’alternative à la configuration des propriétés du cache de sortie en modifiant les propriétés de l’attribut [OutputCache], vous pouvez créer un profil de cache dans le fichier de configuration Web (Web. config). La création d’un profil de cache dans le fichier de configuration Web offre quelques avantages importants.

Tout d’abord, en configurant la mise en cache de sortie dans le fichier de configuration Web, vous pouvez contrôler la façon dont les actions du contrôleur cachent le contenu dans un emplacement central. Vous pouvez créer un profil de cache et appliquer le profil à plusieurs contrôleurs ou actions de contrôleur.

Deuxièmement, vous pouvez modifier le fichier de configuration Web sans recompiler votre application. Si vous devez désactiver la mise en cache d’une application qui a déjà été déployée en production, vous pouvez simplement modifier les profils de cache définis dans le fichier de configuration Web. Toutes les modifications apportées au fichier de configuration Web sont détectées automatiquement et appliquées.

Par exemple, la section &lt;Caching&gt; configuration Web de la liste 6 définit un profil de cache nommé Cache1Hour. La section&gt; de mise en cache &lt;doit apparaître dans la section &lt;System. Web&gt; d’un fichier de configuration Web.

**Liste 6 – section Caching pour Web. config**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

Le contrôleur de la liste 7 montre comment vous pouvez appliquer le profil Cache1Hour à une action de contrôleur avec l’attribut [OutputCache].

**Liste 7 – Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Si vous appelez l’action d’index () exposée par le contrôleur dans la liste 7, la même heure sera retournée pendant 1 heure.

## <a name="summary"></a>Récapitulatif

La mise en cache de sortie vous offre une méthode très simple pour améliorer considérablement les performances de vos applications ASP.NET MVC. Dans ce didacticiel, vous avez appris à utiliser l’attribut [OutputCache] pour mettre en cache la sortie des actions du contrôleur. Vous avez également appris à modifier les propriétés de l’attribut [OutputCache], telles que les propriétés Duration et VaryByParam, afin de modifier la façon dont le contenu est mis en cache. Enfin, vous avez appris à définir des profils de cache dans le fichier de configuration Web.

> [!div class="step-by-step"]
> [Précédent](understanding-action-filters-cs.md)
> [Suivant](adding-dynamic-content-to-a-cached-page-cs.md)
