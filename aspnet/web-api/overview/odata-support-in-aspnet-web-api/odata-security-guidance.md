---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Guide de sécurité pour API Web ASP.NET 2 OData-ASP.NET 4. x
author: MikeWasson
description: Décrit les problèmes de sécurité à prendre en compte lors de l’exposition d’un jeu de données par le biais d’OData pour API Web ASP.NET 2 sur ASP.NET 4. x.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556496"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a>Guide de sécurité pour API Web ASP.NET 2 OData

par [Mike Wasson](https://github.com/MikeWasson)

Cette rubrique décrit certains des problèmes de sécurité que vous devez prendre en compte lors de l’exposition d’un jeu de données par le biais d’OData pour API Web ASP.NET 2 sur ASP.NET 4. x.

## <a name="edm-security"></a>Sécurité EDM

La sémantique de requête est basée sur le modèle EDM (Entity Data Model), et non sur les types de modèles sous-jacents. Vous pouvez exclure une propriété du modèle EDM et celle-ci ne sera pas visible par la requête. Par exemple, supposons que votre modèle comprenne un type d’employé avec une propriété de salaire. Vous souhaiterez peut-être exclure cette propriété de l’EDM pour la masquer sur les clients.

Il existe deux façons d’exclure une propriété du modèle EDM. Vous pouvez définir l’attribut **[IgnoreDataMember]** sur la propriété dans la classe de modèle :

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

Vous pouvez également supprimer la propriété de l’EDM par programme :

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Sécurité des requêtes

Un client malveillant ou naïve peut être en mesure de créer une requête dont l’exécution prend beaucoup de temps. Dans le pire des cas, cela peut perturber l’accès à votre service.

L’attribut **[interrogeable]** est un filtre d’action qui analyse, valide et applique la requête. Le filtre convertit les options de requête en une expression LINQ. Lorsque le contrôleur OData retourne un type **IQueryable** , le fournisseur LINQ **IQueryable** convertit l’expression LINQ en une requête. Par conséquent, les performances dépendent du fournisseur LINQ utilisé, ainsi que des caractéristiques particulières de votre schéma de base de données ou de base de données.

Pour plus d’informations sur l’utilisation des options de requête OData dans API Web ASP.NET, consultez [prise en charge des options de requête OData](supporting-odata-query-options.md).

Si vous savez que tous les clients sont approuvés (par exemple, dans un environnement d’entreprise), ou si votre jeu de données est petit, les performances des requêtes peuvent ne pas être un problème. Dans le cas contraire, vous devez prendre en compte les recommandations suivantes.

- Testez votre service avec diverses requêtes et profilez la base de la base de
- Activez la pagination pilotée par le serveur pour éviter de retourner un jeu de données volumineux dans une requête. Pour plus d’informations, consultez [pagination pilotée par le serveur](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- Avez-vous besoin d' $filter et de $orderby ? Certaines applications peuvent autoriser la pagination du client, en utilisant $top et $skip, mais désactiver les autres options de requête. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Envisagez de limiter les $orderby aux propriétés dans un index cluster. Le tri des données volumineuses sans index cluster est lent. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Nombre maximal de nœuds : la propriété **maxNodeCount** sur **[Queryable]** définit le nombre maximal de nœuds autorisés dans l’arborescence de syntaxe $Filter. La valeur par défaut est 100, mais vous pouvez définir une valeur inférieure, car un grand nombre de nœuds peut être lent à compiler. Cela est particulièrement vrai si vous utilisez des LINQ to Objects (par exemple, des requêtes LINQ sur une collection en mémoire, sans utiliser de fournisseur LINQ intermédiaire). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Envisagez de désactiver les fonctions any () et All (), car elles peuvent être lentes. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Si des propriétés de chaîne contiennent des&#8212;chaînes volumineuses, par exemple, une description de&#8212;produit ou une entrée de blog, envisagez de désactiver les fonctions de chaîne. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Envisagez de ne pas autoriser le filtrage sur les propriétés de navigation. Le filtrage sur les propriétés de navigation peut entraîner une jointure, qui peut être lente, en fonction de votre schéma de base de données. Le code suivant illustre un validateur de requête qui empêche le filtrage sur les propriétés de navigation. Pour plus d’informations sur les validateurs de requête, consultez [validation de requête](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Envisagez de limiter $filter requêtes en écrivant un validateur personnalisé pour votre base de données. Par exemple, considérez ces deux requêtes : 

  - Tous les films avec des acteurs dont le nom commence par « A ».
  - Tous les films publiés dans 1994.

    À moins que les films ne soient indexés par des acteurs, la première requête peut nécessiter que le moteur de base de base de base de l’analyse la liste complète des films. Tandis que la deuxième requête peut être acceptable, en supposant que les films sont indexés par année de sortie.

    Le code suivant illustre un validateur qui autorise le filtrage sur les propriétés « ReleaseYear » et « title », mais pas sur d’autres propriétés.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- En général, réfléchissez aux fonctions de $filter dont vous avez besoin. Si vos clients n’ont pas besoin de l’expressivité complète de $filter, vous pouvez limiter les fonctions autorisées.
