---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Validation de modèle dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Vue d’ensemble de la validation de modèle dans API Web ASP.NET pour ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557238"
---
# <a name="model-validation-in-aspnet-web-api"></a>Validation de modèle dans API Web ASP.NET

par [Mike Wasson](https://github.com/MikeWasson)

Cet article explique comment annoter vos modèles, utiliser les annotations pour la validation des données et gérer les erreurs de validation dans votre API Web. Lorsqu’un client envoie des données à votre API Web, vous devez souvent valider les données avant d’effectuer un traitement. 

## <a name="data-annotations"></a>Annotations de données

Dans API Web ASP.NET, vous pouvez utiliser les attributs de l’espace de noms [System. ComponentModel. DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) pour définir des règles de validation pour les propriétés de votre modèle. Considérez le modèle suivant :

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Si vous avez utilisé la validation de modèle dans ASP.NET MVC, cela devrait vous paraître familier. L’attribut **Required** indique que la propriété `Name` ne doit pas être null. L’attribut de **plage** indique que `Weight` doit être compris entre zéro et 999.

Supposons qu’un client envoie une demande de publication avec la représentation JSON suivante :

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Vous pouvez voir que le client n’a pas inclus la propriété `Name`, qui est marquée comme étant obligatoire. Lorsque l’API Web convertit le JSON en instance `Product`, il valide le `Product` par rapport aux attributs de validation. Dans votre action de contrôleur, vous pouvez vérifier si le modèle est valide :

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

La validation de modèle ne garantit pas la sécurité des données du client. Une validation supplémentaire peut être nécessaire dans d’autres couches de l’application. (Par exemple, la couche de données peut appliquer des contraintes de clé étrangère.) Le didacticiel [sur l’utilisation de l’API Web avec Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explore certains de ces problèmes.

**« Sous-publication »** : sous-publication se produit lorsque le client quitte certaines propriétés. Par exemple, supposons que le client envoie les éléments suivants :

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Ici, le client n’a pas spécifié de valeurs pour `Price` ou `Weight`. Le module de formatage JSON assigne une valeur par défaut de zéro aux propriétés manquantes.

![](model-validation-in-aspnet-web-api/_static/image1.png)

L’état du modèle est valide, car zéro est une valeur valide pour ces propriétés. Le fait qu’il s’agisse d’un problème dépend de votre scénario. Par exemple, dans une opération de mise à jour, vous souhaiterez peut-être faire la distinction entre « zéro » et « non défini ». Pour forcer les clients à définir une valeur, rendez la propriété Nullable et définissez l’attribut **requis** :

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**« Dépassement de la publication »** : un client peut également envoyer *plus* de données que prévu. Exemple :

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Ici, le JSON comprend une propriété (« Color ») qui n’existe pas dans le modèle `Product`. Dans ce cas, le module de formatage JSON ignore simplement cette valeur. (Le formateur XML fait de même.) La survalidation entraîne des problèmes si votre modèle a des propriétés que vous souhaitez être en lecture seule. Exemple :

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Vous ne voulez pas que les utilisateurs mettent à jour la propriété `IsAdmin` et s’élèvent aux administrateurs ! La stratégie la plus sûre consiste à utiliser une classe de modèle qui correspond exactement à ce que le client est autorisé à envoyer :

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Le billet de blog de Brad Wilson «[validation des entrées et validation de modèle dans ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)» présente une bonne présentation de la sous-publication et de la survalidation. Bien que la publication concerne ASP.NET MVC 2, les problèmes sont toujours pertinents pour l’API Web.

## <a name="handling-validation-errors"></a>Gestion des erreurs de validation

L’API Web ne renvoie pas automatiquement une erreur au client lorsque la validation échoue. Il revient à l’action du contrôleur de vérifier l’état du modèle et de répondre de manière appropriée.

Vous pouvez également créer un filtre d’action pour vérifier l’état du modèle avant l’appel de l’action du contrôleur. Le code suivant montre un exemple :

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Si la validation du modèle échoue, ce filtre retourne une réponse HTTP qui contient les erreurs de validation. Dans ce cas, l’action du contrôleur n’est pas appelée.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Pour appliquer ce filtre à tous les contrôleurs d’API Web, ajoutez une instance du filtre à la collection **HttpConfiguration. filters** au cours de la configuration :

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Une autre option consiste à définir le filtre en tant qu’attribut sur des contrôleurs ou des actions de contrôleur individuels :

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
