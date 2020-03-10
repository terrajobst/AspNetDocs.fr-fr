---
uid: single-page-application/overview/templates/breezeknockout-template
title: Modèle Breeze/Knockout | Microsoft Docs
author: madskristensen
description: Modèle d’application à page unique Breeze/Knockout
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 5bb9ee8f758a25afa6baf3ccbaf7d5864754c7df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558190"
---
# <a name="breezeknockout-template"></a>Modèle Breeze/Knockout

par [Mads Kristensen](https://github.com/madskristensen)

> Le modèle MVC Breeze/Knockout a été écrit à l’intérieur de la cloche
> 
> [Télécharger le modèle MVC Breeze/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)

Vous avez entendu parler de l’application à page unique (SPA) et vous avez demandé ce qu’elle était. Bien que vous puissiez en savoir plus sur ce sujet, il est préférable de l’expérimenter par vous-même. Mais qui a le temps de télécharger un exemple ? Si vous disposez de Visual Studio, vous disposerez d’un exemple de SPA opérationnel en moins de 60 secondes avec le modèle ASP.NET MVC 4 « application à page unique de Breeze/Knockout ».

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Qu’est-ce que le modèle SPA Breeze/Knockout ?

La plupart des modèles de projet génèrent un squelette d’application. Vous placez la chair sur ces segments en ajoutant votre code et en configurant une application opérationnelle. Le modèle SPA Breeze/Knockout est différent. Il génère un exemple d’application que vous pouvez étudier. Il illustre la conception d’une application SPA et de nombreuses techniques pour la création d’un SPA.

Le modèle Breeze/Knockout est une variante du [modèle KNOCKOUTJS Spa](../introduction/knockoutjs-template.md) inclus dans la mise à jour ASP.net et Web Tools 2012,2. Le modèle de la SPA Breeze génère une application avec la même expérience utilisateur, mais elle a une implémentation différente, avec Breeze pour la gestion des données.

Le modèle SPA KnockoutJS effectue des demandes de service avec jQuery AJAX brut, ce qui est approprié pour une application simple. Mais des applications plus sophistiquées ont des exigences de gestion des données plus exigeantes. Par exemple, la plupart des applications :

- Interroger et relancer une requête sur le serveur pendant une session utilisateur étendue.
- Ajoutez des filtres de requête, le tri et la pagination.
- Partager les mêmes données sur plusieurs écrans.
- Accumulez les modifications apportées à de nombreux objets, puis enregistrez-les sous la forme d’une transaction unique.
- Valider les modifications sur le client, afin que l’utilisateur puisse corriger les erreurs avant de valider les modifications apportées à la base de données.

La bibliothèque BreezeJS gère ces tâches pour vous, ce qui vous permet de développer la logique d’application et l’expérience utilisateur les plus importantes.

[**Breeze**](http://www.breezejs.com/?utm_source=ms-spa) est une bibliothèque open source pour la création d’applications de données riches en JavaScript et HTML, les types d’applications qui ont été fournies historiquement comme des applications de bureau autonomes.

Le modèle Breeze/Knockout vous aide à prendre la première étape cruciale dans une infrastructure de gestion des données plus robuste. Il génère un exemple d’application todo qui est en haut de la même façon que le modèle SPA KnockoutJS. À l’intérieur, il remplace la couche de données AJAX par Breeze, ce qui vous permet de comparer les deux approches côte à côte. Bien entendu, il touche à peine le potentiel d’une application Breeze. Toutefois, vous verrez le fonctionnement de Breeze et le minimum requis pour effectuer cette transition.

Allons-y.

## <a name="create-a-breezeknockout-template-project"></a>Créer un projet de modèle Breeze/Knockout

Téléchargez et installez le modèle en cliquant sur le bouton de téléchargement ci-dessus. Le modèle est empaqueté sous la forme d’un fichier d’extension Visual Studio (VSIX). Vous devrez peut-être redémarrer Visual Studio.

Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  . Sous **visuel C#** , sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **application Web ASP.NET MVC 4**. Nommez le projet, puis cliquez sur **OK**.

Dans l’Assistant **nouveau projet** , sélectionnez **Breeze Knockout « Spa**».

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Appuyez sur CTRL + F5 pour générer et exécuter l’application sans débogage, ou appuyez sur F5 pour exécuter le débogage.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

Lorsque l’application s’exécute pour la première fois, elle affiche un écran de connexion. Cliquez sur le lien « s’inscrire » pour accéder à une nouvelle page, dans laquelle vous pouvez entrer un nom d’utilisateur et un mot de passe. (Les pages de connexion et d’inscription sont créées à l’aide de ASP.NET MVC.) Lorsque vous soumettez le formulaire d’inscription, le serveur génère un TodoList avec deux éléments pour votre compte. Vous les Affichez ensuite sur une note jaune.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Vous êtes à présent à l’intérieur du SPA. Tout ce que vous voyez et que vous rencontrez lors de la manipulation de éléments todo est rendu et géré sur le client à l’aide de Knockout et Breeze. Explorez l’application en tant qu’utilisateur... mais avec l’œil d’un développeur. Utilisez les outils de développement de votre navigateur pour capturer le trafic réseau. (Dans Internet Explorer : Appuyez sur F12, sélectionnez l’onglet **réseau** , puis cliquez sur **Démarrer la capture**.) Maintenant, essayez ce qui suit :

- Ajoutez un nouvel élément TODO.
- Cliquez sur l’étiquette et modifiez le titre de l’élément TODO
- Cochez une case pour marquer l’élément comme terminé. Notez que la zone de texte est désactivée ; le titre n’est donc plus modifiable.
- Cliquez sur le signe « x » à droite de l’étiquette. L’élément disparaît et est supprimé de la base de données.
- Choisissez un autre élément et effacez son titre. Vous obtiendrez une erreur de validation indiquant que le titre est obligatoire. Après une courte pause, le titre précédent est restauré.
- Tapez un titre ridiculement long. Vous obtiendrez une autre erreur de validation indiquant que le titre est trop long.
- Cliquez sur le bouton « Ajouter une liste TODO ». Une nouvelle liste apparaît à gauche de la liste précédente.
- Jouez avec le titre TodoList, en déclenchant ses validations obligatoires et de longueur.
- Cliquez dans la zone de texte titre pour effacer le message d’erreur.
- Cliquez sur le « x » dans le cercle dans le coin supérieur droit pour supprimer TodoList et son éléments todo.

La logique de validation est exécutée par Breeze côté client. Les attributs de validation sur les classes de modèle de serveur sont propagés au client et exécutés automatiquement avant que le client ne contacte le serveur.

Examinez le trafic réseau. Notez qu’il n’y avait aucun appel au serveur lorsque Breeze a détecté une erreur. Chaque modification valide a entraîné une demande de publication à « /api/Todo/SaveChanges ». Breeze rassemble les modifications et les envoie sous la forme d’une requête unique à la méthode `SaveChanges` du contrôleur d’API Web. Cela est différent du modèle KnockoutJS SPA, qui permet de placer, de poster et de supprimer des demandes pour chaque élément individuellement.

## <a name="peek-inside"></a>Aperçu à l’intérieur

Cette application est côté client et côté serveur. La pile côté client se compose d’un peu de code HTML et d’une combinaison de modules JavaScript d’application (dans le dossier « App »), ainsi que de bibliothèques JavaScript tierces (dans le dossier « scripts »).

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Si vous avez étudié le modèle KnockoutJS SPA, cela devrait vous sembler très familier. Concentrez-vous sur les cases bleues. L’architecture d’interface utilisateur est Model-View-ViewModel (MVVM), dans laquelle les widgets HTML de la vue sont séparés proprement du code de présentation de prise en charge dans le modèle d’affichage. Un système de liaison de données (Knockout dans ce cas) coordonne la vue et le modèle de vue afin que chacun puisse faire son travail sans connaissance approfondie de l’autre.

Le modèle encapsule les données todo. Les entités dans le modèle sont construites par Breeze avec des propriétés observables Knockout, afin qu’elles puissent être directement liées aux widgets dans la vue. Le modèle d’affichage demande au contexte de données d’acquérir et d’enregistrer les entités du modèle. Le contexte de données délègue la majeure partie du travail à Breeze.

La pile côté serveur se compose d’un code de développeur et de trois bibliothèques .NET principales : API Web, Entity Framework et Breeze.NET :

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

L’architecture de base est la même que le modèle SPA KnockoutJS. Toutefois, l’implémentation est bien plus simple : les DTO ont été supprimés, et la plupart des détails de Entity Framework ont été délégués à Breeze.NET.

## <a name="next-steps"></a>Étapes suivantes

Nous vous suggérons d’explorer le code, guidé par la [discussion complète](http://www.breezejs.com/spa-template?utm_source=ms-spa) sur les piles client et serveur sur le site Web de Breeze.

Vous pouvez essayer de lancer une requête côté client Breeze. Ajoutez des filtres et des tris. Vous pouvez ajouter d’autres propriétés de modèle et d’autres entités pour avoir une meilleure idée du développement de SPA de bout en bout. Lorsque vous êtes certain de la conception, vous pouvez détacher les fonctionnalités TODO et les remplacer par les vôtres.

Vous serez bientôt prêt pour la prochaine étape principale : l’ajout d’écrans côté client et la navigation entre eux. Vous allez conserver ce modèle SPA en arrière-plan et passer à une pile SPA plus complète, telle que la [serviette chaude de John Papa](https://github.com/johnpapa/HotTowel#readme "Serviette chaude"), qui ajoute Durandal à la combinaison Breeze et Knockout.
