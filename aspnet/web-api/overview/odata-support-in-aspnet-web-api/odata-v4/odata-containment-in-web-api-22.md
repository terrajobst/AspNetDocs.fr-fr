---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Relation contenant-contenu dans OData v4 à l’aide de l’API Web 2,2 | Microsoft Docs
author: rick-anderson
description: Traditionnellement, une entité n’est accessible que si elle était encapsulée dans un jeu d’entités. Mais OData v4 fournit deux options supplémentaires, Singleton et con...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 50050e40c4c42bf6d769d077c27864ee6417d4db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525122"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a>Relation contenant-contenu dans OData v4 à l’aide de l’API Web 2,2

par Jinfu brun

> Traditionnellement, une entité n’est accessible que si elle était encapsulée dans un jeu d’entités. Toutefois, OData v4 fournit deux options supplémentaires, Singleton et relation contenant-contenu, que WebAPI 2,2 prend en charge.

Cette rubrique montre comment définir une relation contenant-contenu dans un point de terminaison OData dans WebApi 2,2. Pour plus d’informations sur la relation contenant-contenu, consultez [relation contenant-contenu dans OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Pour créer un point de terminaison OData v4 dans l’API Web, consultez [créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 2,2](create-an-odata-v4-endpoint.md).

Tout d’abord, nous allons créer un modèle de domaine de relation contenant-contenu dans le service OData, à l’aide de ce modèle de données :

![Modèle de données](odata-containment-in-web-api-22/_static/image1.png)

Un compte contient de nombreux PaymentInstruments (PI), mais nous ne définissons pas de jeu d’entités pour un PI. Au lieu de cela, les instructions de l’utilisateur sont accessibles uniquement via un compte.

Vous pouvez télécharger la solution utilisée dans cette rubrique à partir de [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Définition du modèle de données

1. Définissez les types CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    L’attribut `Contained` est utilisé pour les propriétés de navigation de la relation contenant-contenu.
2. Générez le modèle EDM basé sur les types CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    Le `ODataConventionModelBuilder` gérera la génération du modèle EDM si l’attribut `Contained` est ajouté à la propriété de navigation correspondante. Si la propriété est un type de collection, une fonction de `GetCount(string NameContains)` est également créée.

    Les métadonnées générées se présentent comme suit :

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    L’attribut `ContainsTarget` indique que la propriété de navigation est un conteneur.

## <a name="define-the-containing-entity-set-controller"></a>Définir le contrôleur de jeu d’entités conteneur

Les entités contenues n’ont pas leur propre contrôleur ; l’action est définie dans le contrôleur de jeu d’entités conteneur. Dans cet exemple, il existe un AccountsController, mais pas de PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Si le chemin d’accès OData est de 4 segments ou plus, seul le routage des attributs fonctionne, par exemple `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` dans le contrôleur ci-dessus. Dans le cas contraire, l’attribut et le routage conventionnel fonctionnent : par exemple, `GetPayInPIs(int key)` correspond à `GET ~/Accounts(1)/PayinPIs`.

*Merci à Leo HU pour le contenu d’origine de cet article.*
