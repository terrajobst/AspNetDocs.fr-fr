---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Validation avec une couche de service (VB) | Microsoft Docs
author: StephenWalther
description: Découvrez comment déplacer votre logique de validation hors de vos actions de contrôleur et dans une couche de service distincte. Dans ce didacticiel, Stephen Walther vous explique comment...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 704657ffe6f50eaf3eb0d91d0d334567003ab7f4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542825"
---
# <a name="validating-with-a-service-layer-vb"></a>Validation avec une couche de service (VB)

par [Stephen Walther](https://github.com/StephenWalther)

> Découvrez comment déplacer votre logique de validation hors de vos actions de contrôleur et dans une couche de service distincte. Dans ce didacticiel, Stephen Walther explique comment vous pouvez conserver une séparation nette des préoccupations en isolant votre couche de service de votre couche de contrôleur.

L’objectif de ce didacticiel est de décrire une méthode d’exécution de la validation dans une application MVC ASP.NET. Dans ce didacticiel, vous allez apprendre à déplacer votre logique de validation hors de vos contrôleurs et dans une couche de service distincte.

## <a name="separating-concerns"></a>Séparation des préoccupations

Lorsque vous générez une application ASP.NET MVC, vous ne devez pas placer votre logique de base de données dans vos actions de contrôleur. La combinaison de votre base de données et de votre logique de contrôleur rend votre application plus difficile à gérer au fil du temps. Il est recommandé de placer l’ensemble de la logique de votre base de données dans une couche de référentiel distincte.

Par exemple, la liste 1 contient un référentiel simple nommé ProductRepository. Le référentiel du produit contient tout le code d’accès aux données de l’application. La liste comprend également l’interface IProductRepository implémentée par le référentiel du produit.

**Liste 1-Models\ProductRepository.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

Le contrôleur de la liste 2 utilise la couche de référentiel dans ses actions index () et Create (). Notez que ce contrôleur ne contient pas de logique de base de données. La création d’une couche de référentiel vous permet de conserver une séparation claire des préoccupations. Les contrôleurs sont responsables de la logique de contrôle de workflow d’application et le référentiel est responsable de la logique d’accès aux données.

**Liste 2-Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a>Création d’une couche de service

Par conséquent, la logique de contrôle de workflow d’application appartient à un contrôleur et la logique d’accès aux données appartient à un référentiel. Dans ce cas, où placer votre logique de validation ? L’une des options consiste à placer votre logique de validation dans une *couche de service*.

Une couche de service est une couche supplémentaire dans une application ASP.NET MVC qui sert d’intermédiaire pour la communication entre un contrôleur et une couche de référentiel. La couche de service contient la logique métier. En particulier, il contient une logique de validation.

Par exemple, la couche de service du produit dans la liste 3 a une méthode CreateProduct (). La méthode CreateProduct () appelle la méthode ValidateProduct () pour valider un nouveau produit avant de passer le produit au référentiel du produit.

**Liste 3-Models\ProductService.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

Le contrôleur de produit a été mis à jour dans la liste 4 afin d’utiliser la couche de service à la place de la couche de référentiel. La couche de contrôleur communique avec la couche de service. La couche de service communique avec la couche de référentiel. Chaque couche a une responsabilité distincte.

**Liste 4-Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

Notez que le service de produit est créé dans le constructeur du contrôleur de produit. Lorsque le service de produit est créé, le dictionnaire d’États de modèles est transmis au service. Le service de produit utilise l’état du modèle pour renvoyer les messages d’erreur de validation au contrôleur.

## <a name="decoupling-the-service-layer"></a>Découplage de la couche de service

Nous n’avons pas pu isoler les couches de contrôleur et de service en un seul respect. Les couches de contrôleur et de service communiquent par l’intermédiaire de l’état du modèle. En d’autres termes, la couche de service a une dépendance vis-à-vis d’une fonctionnalité particulière de l’infrastructure MVC ASP.NET.

Nous voulons isoler autant que possible la couche de service de notre couche de contrôleur. En théorie, nous devrions être en mesure d’utiliser la couche de service avec n’importe quel type d’application et non seulement une application ASP.NET MVC. Par exemple, à l’avenir, nous pourrions vouloir créer un serveur frontal WPF pour notre application. Nous devrions trouver un moyen de supprimer la dépendance vis-à-vis de l’état du modèle ASP.NET MVC à partir de notre couche de service.

Dans le Listing 5, la couche de service a été mise à jour afin qu’elle n’utilise plus l’état du modèle. Au lieu de cela, il utilise une classe qui implémente l’interface IValidationDictionary.

**Liste 5-Models\ProductService.vb (découplé)**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

L’interface IValidationDictionary est définie dans la liste 6. Cette interface simple a une seule méthode et une seule propriété.

**Liste 6-Models\IValidationDictionary.cs**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

La classe de la liste 7, nommée la classe ModelStateWrapper, implémente l’interface IValidationDictionary. Vous pouvez instancier la classe ModelStateWrapper en passant un dictionnaire d’États de modèles au constructeur.

**Liste 7-Models\ModelStateWrapper.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

Enfin, le contrôleur mis à jour dans la liste 8 utilise le ModelStateWrapper lors de la création de la couche de service dans son constructeur.

**Liste 8-Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

L’utilisation de l’interface IValidationDictionary et de la classe ModelStateWrapper nous permet d’isoler complètement notre couche de service de notre couche de contrôleur. La couche de service n’est plus dépendante de l’état du modèle. Vous pouvez passer toute classe qui implémente l’interface IValidationDictionary à la couche de service. Par exemple, une application WPF peut implémenter l’interface IValidationDictionary avec une classe de collection simple.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel était d’aborder une approche de la validation dans une application ASP.NET MVC. Dans ce didacticiel, vous avez appris à déplacer toute la logique de validation de vos contrôleurs vers une couche de service distincte. Vous avez également appris à isoler votre couche de service de votre couche de contrôleur en créant une classe ModelStateWrapper.

> [!div class="step-by-step"]
> [Précédent](validating-with-the-idataerrorinfo-interface-vb.md)
> [Suivant](validation-with-the-data-annotation-validators-vb.md)
