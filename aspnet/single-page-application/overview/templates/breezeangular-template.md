---
uid: single-page-application/overview/templates/breezeangular-template
title: Modèle de Breeze/angulaire | Microsoft Docs
author: madskristensen
description: Modèle d’application à page unique de Breeze/angulaire
ms.author: riande
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 3e4e63d385a56d51d3d08696782b43d6228f6201
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578539"
---
# <a name="breezeangular-template"></a>Modèle Breeze/Angular

par [Mads Kristensen](https://github.com/madskristensen)

> Le modèle MVC Breeze/angulaire a été écrit à l’intérieur de la cloche
> 
> [Télécharger le modèle MVC Breeze/angulaire](https://go.microsoft.com/fwlink/?LinkId=286437)

[AngularJS](http://angularjs.org) est une bibliothèque open source de Google pour la création d’applications à page unique (spas). Il offre une liaison de données, une injection de dépendances et une gestion d’écran. Associez-le à [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), une autre bibliothèque open source pour la modélisation des données et la gestion des données, et vous avez les ingrédients essentiels pour une application cliente HTML/JavaScript exceptionnelle.

Le modèle SPA Breeze/angulaire est une variante du [modèle KNOCKOUTJS Spa](../introduction/knockoutjs-template.md) inclus dans la mise à jour ASP.net et Web Tools 2012,2. Si vous disposez de Visual Studio, vous disposerez d’un exemple de SPA opérationnel en moins de 60 secondes.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

À l’extérieur, l’application semble très similaire au modèle SPA KnockoutJS. Mais c’est très différent en coulisses. Le modèle KnockoutJS utilise Knockout pour la liaison de données et l’AJAX brut pour l’accès aux données. Le modèle Breeze/angulaire utilise l’angle pour la liaison de données et Breeze pour l’accès aux données. Ces bibliothèques activent des fonctionnalités supplémentaires, notamment la navigation entre les pages et l’historique.

Voici la page à propos de de l’application :

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Cette page affiche un journal des événements en cours d’exécution pendant la session utilisateur active, notamment :

- Déplacement. Notez la création du contrôleur todo à #2 et #7.
- Requêtes distantes (#3) et requêtes de cache local (#7).
- Enregistrement des nouvelles entités (#5, #6) et des entités modifiées (#4).
- Les modifications validées sur le client (#9), afin que l’utilisateur puisse corriger les erreurs avant de valider les modifications apportées à la base de données.

Il y a plus d’exploration dans ce modèle, notamment :

- Chargement dynamique de modèles de vue HTML.
- Liaison de données personnalisée via des directives angulaires.
- Modularité et injection de dépendances.
- Filtres de requête, tris, pagination, projections et inclusion d’entités associées.
- Partage de données sur plusieurs écrans.
- Enregistrement de plusieurs modifications sous la forme d’une transaction unique.
- Les règles de validation sont propagées automatiquement du serveur vers le client JavaScript.

Allons-y.

## <a name="create-a-breezeangular-template-project"></a>Créer un projet de modèle de Breeze/angulaire

Téléchargez et installez le modèle en cliquant sur le bouton de téléchargement ci-dessus. Le modèle est empaqueté sous la forme d’un fichier d’extension Visual Studio (VSIX). Vous devrez peut-être redémarrer Visual Studio.

Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  . Sous **visuel C#** , sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **application Web ASP.NET MVC 4**. Nommez le projet, puis cliquez sur **OK**.

Dans l’Assistant **nouveau projet** , sélectionnez le **Spa angulaire Breeze**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Appuyez sur CTRL + F5 pour générer et exécuter l’application sans débogage, ou appuyez sur F5 pour exécuter le débogage.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- Cliquez sur le lien « à propos de » dans le coin supérieur droit pour afficher un journal de ces activités.

La logique de validation est exécutée par Breeze côté client. Les attributs de validation sur les classes de modèle de serveur sont propagés au client et exécutés automatiquement avant que le client ne contacte le serveur.

Examinez le trafic réseau. Notez qu’il n’y avait aucun appel au serveur lorsque Breeze a détecté une erreur. Chaque modification valide a entraîné une demande de publication à « /api/Todo/SaveChanges ». Breeze rassemble les modifications et les envoie sous la forme d’une requête unique à la méthode `SaveChanges` du contrôleur d’API Web. Cela est différent du modèle KnockoutJS SPA, qui permet de placer, de poster et de supprimer des demandes pour chaque élément individuellement.

Notez également qu’il n’y a pas de trafic réseau lorsque vous basculez entre les pages TodoList et about. Cela est dû au fait que la requête a été limitée au cache de Breeze local.

## <a name="peek-inside"></a>Aperçu à l’intérieur

Cette application est côté client et côté serveur. La pile côté client se compose d’un peu de code HTML et d’une combinaison de modules JavaScript d’application (dans le dossier « App »), ainsi que de bibliothèques JavaScript tierces (dans le dossier « scripts »).

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

L’architecture d’interface utilisateur sépare les widgets HTML des vues du code de présentation de prise en charge dans les contrôleurs. Le système de liaison de données angulaire coordonne les vues et les contrôleurs afin que chacun puisse faire son travail sans connaissance approfondie de l’autre.

Le contrôleur demande au contexte de données d’acquérir et d’enregistrer les entités du modèle. Le contexte de données délègue la majeure partie du travail à Breeze, qui crée des objets de modèle de suivi automatique à partir des résultats de requête JSON.

La pile côté serveur se compose d’un code de développeur et de trois bibliothèques .NET principales : API Web, Entity Framework et Breeze.NET :

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

L’architecture de base est la même que le modèle SPA KnockoutJS. Toutefois, l’implémentation est bien plus simple : les DTO ont été supprimés, et la plupart des détails de Entity Framework ont été délégués à Breeze.NET.

## <a name="next-steps"></a>Étapes suivantes

Nous vous suggérons d’explorer le code, guidé par la [discussion complète](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) sur les piles client et serveur sur le site Web de Breeze.

Vous pouvez essayer de lancer une requête côté client Breeze. Ajoutez des filtres et des tris. Vous pouvez ajouter d’autres propriétés de modèle et d’autres entités pour avoir une meilleure idée du développement de SPA de bout en bout. Lorsque vous êtes certain de la conception, vous pouvez détacher les fonctionnalités TODO et les remplacer par les vôtres.

Bon développement !
