---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Héritage de type complexe dans OData v4 avec API Web ASP.NET | Microsoft Docs
author: microsoft
description: Selon la spécification OData v4, un type complexe peut hériter d’un autre type complexe. (Un type complexe est un type structuré sans clé.) API Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 3d90216c8e594055f77577eb6d8b1d978ae4c24d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556307"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Héritage de type complexe dans OData v4 avec API Web ASP.NET

par [Microsoft](https://github.com/microsoft)

> Selon la [spécification](http://www.odata.org/documentation/odata-version-4-0/)OData v4, un type complexe peut hériter d’un autre type complexe. (Un type *complexe* est un type structuré sans clé.) L’API Web OData 5,3 prend en charge l’héritage de type complexe.
> 
> Cette rubrique montre comment générer un modèle EDM (Entity Data Model) avec des types d’héritage complexes. Pour obtenir le code source complet, consultez [exemple d’héritage de type complexe OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - API Web OData 5,3
> - OData v4

## <a name="model-hierarchy"></a>Hiérarchie du modèle

Pour illustrer l’héritage de type complexe, nous allons utiliser la hiérarchie de classes suivante.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` est un type complexe abstrait. `Rectangle`, `Triangle`et `Circle` sont des types complexes dérivés de `Shape`, et `RoundRectangle` dérive de `Rectangle`. `Window` est un type d’entité et contient une instance `Shape`.

Voici les classes CLR qui définissent ces types.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Générer le modèle EDM

Pour créer le modèle EDM, vous pouvez utiliser **ODataConventionModelBuilder**, qui déduit les relations d’héritage des types CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Vous pouvez également générer explicitement le modèle EDM à l’aide de **ODataModelBuilder**. Cela nécessite davantage de code, mais vous donne davantage de contrôle sur le modèle EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Ces deux exemples créent le même schéma EDM.

## <a name="metadata-document"></a>Document de métadonnées

Voici le document de métadonnées OData, qui montre l’héritage de type complexe.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

À partir du document de métadonnées, vous pouvez voir que :

- Le type complexe `Shape` est abstract.
- Les `Rectangle`, `Triangle`et `Circle` type complexe ont le type de base `Shape`.
- Le type de `RoundRectangle` a le type de base `Rectangle`.

## <a name="casting-complex-types"></a>Cast de types complexes

La conversion de types complexes est désormais prise en charge. Par exemple, la requête suivante effectue un cast d’un `Shape` en `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Voici la charge utile de la réponse :

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
