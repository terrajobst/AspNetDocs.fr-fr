---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Vues fortement typées Vues fortement typées | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 3e81c6381b1e280e3b74cb7eb6ea6e6c3224e655
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538156"
---
# <a name="dynamic-v-strongly-typed-views"></a>Vues fortement typées et vues dynamiques

par [Rick Anderson](https://twitter.com/RickAndMSFT)

Il existe trois façons de passer des informations d’un contrôleur à une vue dans ASP.NET MVC 3 :

1. En tant qu’objet de modèle fortement typé.
2. En tant que type dynamique (à l’aide de @model Dynamic)
3. Utilisation de ViewBag

J’ai écrit une simple application de blog MVC 3 Top pour comparer et opposer les vues dynamiques et fortement typées. Le contrôleur commence par une simple liste de blogs :

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Cliquez avec le bouton droit dans la méthode IndexNotStonglyTyped () et ajoutez une vue Razor.

[![8475. NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Assurez-vous que la case **créer un affichage fortement typé** n’est pas cochée. La vue résultante ne contient pas la plupart des éléments suivants :

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Dans la mesure où nous utilisons une vue dynamique et non typée fortement typée, IntelliSense ne nous aide pas. Le code complet est indiqué ci-dessous :

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646. NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Nous allons maintenant ajouter une vue fortement typée. Ajoutez le code suivant au contrôleur :

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]

Notez qu’il s’agit exactement de la même vue de retour (les blogs). Appelez comme vue non fortement typée. Cliquez avec le bouton droit à l’intérieur de *StonglyTypedIndex ()* et sélectionnez **Ajouter une vue**. Cette fois-ci, sélectionnez la classe de modèle de **blog** et sélectionnez **liste** comme modèle de structure.

[![5658. StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

Dans le nouveau modèle de vue, nous obtenons la prise en charge IntelliSense.

[![7002. IntelliSense [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Le projet c# peut être téléchargé [ici](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
