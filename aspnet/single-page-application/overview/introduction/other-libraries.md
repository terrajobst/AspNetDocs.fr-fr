---
uid: single-page-application/overview/introduction/other-libraries
title: Bibliothèques autres que Knockout | Microsoft Docs
author: madskristensen
description: Bibliothèques autres que Knockout
ms.author: riande
ms.date: 02/05/2013
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 64a4ad1fb411f7291a5cba634afdf4d2fdb16d55
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578546"
---
# <a name="know-a-library-other-than-knockout"></a>Bibliothèques autres que Knockout

par [Mads Kristensen](https://github.com/madskristensen)

Le [modèle d’application à page unique (Spa)](knockoutjs-template.md) est un excellent moyen de commencer à écrire des applications à page unique. Le modèle utilise [KnockoutJS](http://knockoutjs.com/) pour lier les données d’application aux éléments DOM.

Mais Knockout n’est pas la seule bibliothèque JavaScript pour créer des applications clientes riches. D’autres bibliothèques résolvent des défis similaires de différentes façons. Vous préférerez peut-être une bibliothèque plutôt qu’une autre, donc nous avons mis à disposition plusieurs modèles créés par la communauté. Chacun de ces modèles utilise une combinaison différente de bibliothèques JavaScript clientes.

Pour installer un modèle créé par la Communauté, accédez à l’une des pages de modèle ci-dessous, puis cliquez sur le bouton Télécharger. Les modèles sont fournis en tant que fichiers VSIX.

## <a name="backbonejs"></a>BackboneJS

[Modèle d’authentification de réseau principal. js](../templates/backbonejs-template.md). Ce modèle fournit un squelette initial pour le développement d’une application [backbone. js](http://backbonejs.org/) dans ASP.NET MVC. Il fournit les fonctionnalités de base de la connexion utilisateur, y compris l’inscription des utilisateurs, la connexion, la réinitialisation du mot de passe et la confirmation de l’utilisateur avec les modèles de messagerie de base.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) est une bibliothèque open source pour la gestion de données enrichies dans un client JavaScript. Breeze gère l’interrogation, la mise en cache, le suivi des modifications, la validation et bien plus encore. Deux modèles de fonctionnalités :

- Le modèle [Breeze/Knockout](../templates/breezeknockout-template.md) étend le modèle de la Spa Knockout, en vous permettant de créer facilement une application à page unique avec Breeze pour la gestion des données et KnockoutJS pour la liaison de données.
- Le modèle [Breeze/angulaire](../templates/breezeangular-template.md) étend également le modèle de la Spa de Knockout avec Breeze, mais à l’aide de la bibliothèque [AngularJS](http://angularjs.org) pour la liaison de données, l’injection de dépendances et la gestion d’écran.

En outre, le [modèle Spa de la serviette chaude](../templates/hottowel-template.md) utilise BreezeJS.

## <a name="emberjs"></a>EmberJS

[Modèle Spa EmberJS](../templates/emberjs-template.md). Ce modèle utilise [Ember](http://emberjs.com/), une bibliothèque MVC JavaScript puissante qui résout un large éventail de défis liés à la création d’applications clientes riches.

Le modèle Ember SPA est une nouvelle implémentation du modèle de SPA Knockout, avec EmberJS et des modèles de GUID.

## <a name="hot-towel"></a>Serviette chaude

[Modèle](../templates/hottowel-template.md)de l’essuie-serviettes à chaud. Ce modèle intègre plusieurs bibliothèques JavaScript, notamment Breeze, Knockout, RequireJS et Twitter bootstrap.

Par rapport aux autres modèles répertoriés ici, le modèle d’aide à chaud fournit une application plus complète à partir de laquelle vous pouvez créer vos propres modèles. Il y a plus de concepts à connaître, mais une fois que vous les comprenez, ce modèle peut simplement être ce que vous recherchez. Si vous souhaitez créer un SPA, mais que vous ne pouvez pas décider de l’emplacement de départ, utilisez une serviette et, en secondes, vous disposerez d’un SPA et de tous les outils dont vous avez besoin pour créer ce dernier.

## <a name="feature-table"></a>Tableau des fonctionnalités

Voici les fonctionnalités fournies par chaque modèle de SPA :

|                        | Application à page unique ASP.NET | Infrastructure | Breeze/angulaire | Breeze/KO |  Ember   | Serviette chaude |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Exemple ToDo       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Modèle nu      |             | &#10003; |                |           |          | &#10003;  |
| Navigation et historique |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Bibliothèques       |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Infrastructure     |             | &#10003; |                |           |          |           |
|         Breeze         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |
