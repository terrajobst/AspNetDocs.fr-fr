---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Création d’itinéraires personnalisésC#() | Microsoft Docs
author: microsoft
description: Découvrez comment ajouter des itinéraires personnalisés à une application ASP.NET MVC. Dans ce didacticiel, vous allez apprendre à modifier la table de routage par défaut dans le fichier global. asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 58f72e390f0053d136ef00ddbda0b071ba225d98
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601331"
---
# <a name="creating-custom-routes-c"></a>Création de routes personnalisées (C#)

par [Microsoft](https://github.com/microsoft)

> Découvrez comment ajouter des itinéraires personnalisés à une application ASP.NET MVC. Dans ce didacticiel, vous allez apprendre à modifier la table de routage par défaut dans le fichier global. asax.

Dans ce didacticiel, vous allez apprendre à ajouter un itinéraire personnalisé à une application ASP.NET MVC. Vous allez apprendre à modifier la table de routage par défaut dans le fichier global. asax avec un itinéraire personnalisé.

Pour de nombreuses applications ASP.NET MVC simples, la table de routage par défaut fonctionne parfaitement. Toutefois, vous pouvez découvrir que vous avez des besoins de routage spécialisés. Dans ce cas, vous pouvez créer un itinéraire personnalisé.

Imaginez, par exemple, que vous créez une application de blog. Vous souhaiterez peut-être gérer les demandes entrantes qui ressemblent à ceci :

/Archive/12-25-2009

Quand un utilisateur entre cette demande, vous souhaitez retourner l’entrée de blog qui correspond à la date 12/25/2009. Pour gérer ce type de demande, vous devez créer un itinéraire personnalisé.

Le fichier global. asax de la liste 1 contient un nouvel itinéraire personnalisé, nommé blog, qui gère les demandes qui ressemblent*à la date d’entrée*de/Archive/.

**Liste 1-global. asax (avec itinéraire personnalisé)**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

L’ordre des itinéraires que vous ajoutez à la table de routage est important. Notre nouvel itinéraire de blog personnalisé est ajouté avant l’itinéraire par défaut existant. Si vous avez inversé l’ordre, l’itinéraire par défaut est toujours appelé à la place de l’itinéraire personnalisé.

L’itinéraire de blog personnalisé correspond à toutes les demandes qui commencent par/Archive/. Par conséquent, elle correspond à toutes les URL suivantes :

- /Archive/12-25-2009

- /Archive/10-6-2004

- /Archive/apple

L’itinéraire personnalisé mappe la requête entrante à un contrôleur nommé Archive et appelle l’action d’entrée (). Lorsque la méthode Entry () est appelée, la date d’entrée est passée en tant que paramètre nommé entryDate.

Vous pouvez utiliser l’itinéraire personnalisé du blog avec le contrôleur dans la liste 2.

**Liste 2-ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

Notez que la méthode Entry () de la liste 2 accepte un paramètre de type DateTime. L’infrastructure MVC est suffisamment intelligente pour convertir automatiquement la date d’entrée de l’URL en valeur DateTime. Si le paramètre de date d’entrée de l’URL ne peut pas être converti en DateTime, une erreur est générée (voir la figure 1).

**Figure 1-erreur lors de la conversion du paramètre**

[![la boîte de dialogue Nouveau projet](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**Figure 01**: erreur de conversion du paramètre ([cliquez pour afficher l’image en taille réelle](creating-custom-routes-cs/_static/image2.png))

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel était de montrer comment vous pouvez créer un itinéraire personnalisé. Vous avez appris à ajouter un itinéraire personnalisé à la table de routage dans le fichier global. asax qui représente les entrées de blog. Nous avons vu comment mapper des demandes d’entrées de blog à un contrôleur nommé ArchiveController et une action de contrôleur nommée Entry ().

> [!div class="step-by-step"]
> [Précédent](aspnet-mvc-controllers-overview-cs.md)
> [Suivant](creating-a-route-constraint-cs.md)
