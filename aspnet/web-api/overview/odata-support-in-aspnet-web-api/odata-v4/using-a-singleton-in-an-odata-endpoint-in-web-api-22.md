---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Créer un singleton dans OData v4 à l’aide de l’API Web 2,2 | Microsoft Docs
author: rick-anderson
description: Cette rubrique montre comment définir un singleton dans un point de terminaison OData dans l’API Web 2,2.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 218449c18759b306e425c55f8e7b573d837b4658
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622086"
---
# <a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Créer un singleton dans OData v4 à l’aide de l’API Web 2,2

par Zoe Luo

> Traditionnellement, une entité n’est accessible que si elle était encapsulée dans un jeu d’entités. Toutefois, OData v4 fournit deux options supplémentaires, Singleton et relation contenant-contenu, que WebAPI 2,2 prend en charge.

Cet article explique comment définir un singleton dans un point de terminaison OData dans l’API Web 2,2. Pour plus d’informations sur ce qu’est un singleton et sur la façon dont vous pouvez l’utiliser, consultez [utilisation d’un singleton pour définir votre entité spéciale](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Pour créer un point de terminaison OData v4 dans l’API Web, consultez [créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 2,2](create-an-odata-v4-endpoint.md). 

Nous allons créer un singleton dans votre projet d’API Web à l’aide du modèle de données suivant :

![Modèle de données](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Un singleton nommé `Umbrella` sera défini en fonction du type `Company`, et un jeu d’entités nommé `Employees` sera défini en fonction du type `Employee`.

La solution utilisée dans ce didacticiel peut être téléchargée à partir de [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Définir le modèle de données

1. Définissez les types CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Générez le modèle EDM basé sur les types CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Ici, `builder.Singleton<Company>("Umbrella")` indique au générateur de modèles de créer un singleton nommé `Umbrella` dans le modèle EDM.

    Les métadonnées générées se présentent comme suit :

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    À partir des métadonnées, nous pouvons voir que la propriété de navigation `Company` dans le jeu d’entités `Employees` est liée à la `Umbrella`Singleton. La liaison est effectuée automatiquement par `ODataConventionModelBuilder`, puisque seul `Umbrella` a le type de `Company`. En cas d’ambiguïté dans le modèle, vous pouvez utiliser `HasSingletonBinding` pour lier explicitement une propriété de navigation à un singleton ; `HasSingletonBinding` a le même effet que l’utilisation de l’attribut `Singleton` dans la définition de type CLR :

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Définir le contrôleur Singleton

À l’instar du contrôleur EntitySet, le contrôleur Singleton hérite de `ODataController`, et le nom du contrôleur Singleton doit être `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Afin de gérer différents types de requêtes, les actions doivent être prédéfinies dans le contrôleur. Le **routage des attributs** est activé par défaut dans WebApi 2,2. Par exemple, pour définir une action pour gérer l’interrogation des `Revenue` à partir d' `Company` à l’aide du routage d’attributs, utilisez ce qui suit :

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Si vous n’êtes pas disposé à définir des attributs pour chaque action, définissez simplement vos actions en suivant les [conventions de routage OData](../odata-routing-conventions.md). Étant donné qu’une clé n’est pas requise pour l’interrogation d’un singleton, les actions définies dans le contrôleur Singleton diffèrent légèrement des actions définies dans le contrôleur EntitySet.

Pour référence, les signatures de méthode pour chaque définition d’action dans le contrôleur Singleton sont répertoriées ci-dessous.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Fondamentalement, c’est tout ce que vous devez faire côté service. L' [exemple de projet](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contient tout le code de la solution et le client OData qui montre comment utiliser le singleton. Le client est créé en suivant les étapes décrites dans [créer une application cliente OData v4](create-an-odata-v4-client-app.md).

. 

*Merci à Leo HU pour le contenu d’origine de cet article.*
