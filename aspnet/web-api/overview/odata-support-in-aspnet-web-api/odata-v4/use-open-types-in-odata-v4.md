---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Types ouverts dans OData v4 avec API Web ASP.NET | Microsoft Docs
author: microsoft
description: Dans OData v4, un type ouvert est un type structuré qui contient des propriétés dynamiques, en plus de toutes les propriétés déclarées dans la définition de type. Ouvrir…
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 950442c071bf50d2c8c1588971f13f85c4891436
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622177"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Types ouverts dans OData v4 avec API Web ASP.NET

par [Microsoft](https://github.com/microsoft)

> Dans OData v4, un *type ouvert* est un type structuré qui contient des propriétés dynamiques, en plus de toutes les propriétés déclarées dans la définition de type. Les types ouverts vous permettent d’ajouter de la flexibilité à vos modèles de données. Ce didacticiel montre comment utiliser des types ouverts dans API Web ASP.NET OData.
> 
> Ce didacticiel part du principe que vous savez déjà comment créer un point de terminaison OData dans API Web ASP.NET. Si ce n’est pas le cas, commencez par lire [créer un point de terminaison OData v4](create-an-odata-v4-endpoint.md) en premier.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - API Web OData 5,3
> - OData v4

Tout d’abord, la terminologie OData :

- Type d’entité : type structuré avec une clé.
- Type complexe : type structuré sans clé.
- Open type : type avec des propriétés dynamiques. Les types d’entité et les types complexes peuvent être ouverts.

La valeur d’une propriété dynamique peut être un type primitif, un type complexe ou un type énumération ; ou une collection de l’un de ces types. Pour plus d’informations sur les types ouverts, consultez la [spécification OData v4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Installer les bibliothèques Web OData

Utilisez le gestionnaire de package NuGet pour installer les dernières bibliothèques OData de l’API Web. À partir de la fenêtre de la console du gestionnaire de package :

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Définir les types CLR

Commencez par définir les modèles EDM en tant que types CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Lorsque le Entity Data Model (EDM) est créé,

- `Category` est un type d’énumération.
- `Address` est un type complexe. (Elle n’a pas de clé, donc il ne s’agit pas d’un type d’entité.)
- `Customer` est un type d’entité. (Elle a une clé.)
- `Press` est un type complexe ouvert.
- `Book` est un type d’entité ouvert.

Pour créer un type ouvert, le type CLR doit avoir une propriété de type `IDictionary<string, object>`, qui contient les propriétés dynamiques.

## <a name="build-the-edm-model"></a>Générer le modèle EDM

Si vous utilisez **ODataConventionModelBuilder** pour créer le modèle EDM, `Press` et `Book` sont automatiquement ajoutés en tant que types ouverts, en fonction de la présence d’une propriété `IDictionary<string, object>`.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Vous pouvez également générer explicitement le modèle EDM à l’aide de **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Ajouter un contrôleur OData

Ensuite, ajoutez un contrôleur OData. Pour ce didacticiel, nous allons utiliser un contrôleur simplifié qui prend uniquement en charge les demandes d’extraction et de publication, et utilise une liste en mémoire pour stocker les entités.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Notez que la première `Book` instance n’a pas de propriétés dynamiques. La deuxième instance de `Book` a les propriétés dynamiques suivantes :

- « Publié » : type primitif
- "Authors" : collection de types primitifs
- "OtherCategories" : collection de types énumération.

En outre, la propriété `Press` de cette `Book` instance a les propriétés dynamiques suivantes :

- « Blog » : type primitif
- « Address » : type complexe

## <a name="query-the-metadata"></a>Interroger les métadonnées

Pour obtenir le document de métadonnées OData, envoyez une demande de récupération à `~/$metadata`. Le corps de la réponse doit ressembler à ceci :

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

À partir du document de métadonnées, vous pouvez voir que :

- Pour les types `Book` et `Press`, la valeur de l’attribut `OpenType` est true. Les types `Customer` et `Address` n’ont pas cet attribut.
- Le type d’entité `Book` possède trois propriétés déclarées : ISBN, title et Press. Les métadonnées OData n’incluent pas la propriété `Book.Properties` de la classe CLR.
- De même, le `Press` type complexe n’a que deux propriétés déclarées : Name et Category. Les métadonnées n’incluent pas la propriété `Press.DynamicProperties` de la classe CLR.

## <a name="query-an-entity"></a>Interroger une entité

Pour obtenir le livre avec ISBN égal à « 978-0-7356-7942-9 », envoyez une demande de récupération à `~/Books('978-0-7356-7942-9')`. Le corps de la réponse doit ressembler à ce qui suit. (En retrait pour le rendre plus lisible.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Notez que les propriétés dynamiques sont incluses inline avec les propriétés déclarées.

## <a name="post-an-entity"></a>PUBLICATION d’une entité

Pour ajouter une entité de livre, envoyez une demande de publication à `~/Books`. Le client peut définir des propriétés dynamiques dans la charge utile de la demande.

Voici un exemple de demande. Notez les propriétés « Price » et « published ».

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Si vous définissez un point d’arrêt dans la méthode du contrôleur, vous pouvez voir que l’API Web a ajouté ces propriétés au dictionnaire `Properties`.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Ressources supplémentaires

[Exemple d’ouverture de type OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
