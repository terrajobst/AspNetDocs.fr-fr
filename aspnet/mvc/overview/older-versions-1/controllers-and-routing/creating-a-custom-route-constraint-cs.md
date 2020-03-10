---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Création d’une contrainte de routageC#personnalisée () | Microsoft Docs
author: StephenWalther
description: Stephen Walther montre comment vous pouvez créer une contrainte de routage personnalisée. Nous implémentons une contrainte personnalisée simple qui empêche un itinéraire d’être mis en correspondance avec w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 98d5839e3d2623665770ccc5689c28f9eb29c8f6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601450"
---
# <a name="creating-a-custom-route-constraint-c"></a>Création d’une contrainte de route personnalisée (C#)

par [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther montre comment vous pouvez créer une contrainte de routage personnalisée. Nous implémentons une contrainte personnalisée simple qui empêche un itinéraire d’être mis en correspondance lorsqu’une demande de navigateur est effectuée à partir d’un ordinateur distant.

L’objectif de ce didacticiel est de montrer comment vous pouvez créer une contrainte de routage personnalisée. Une contrainte d’itinéraire personnalisée vous permet d’empêcher la mise en correspondance d’un itinéraire, sauf si une condition personnalisée est mise en correspondance.

Dans ce didacticiel, nous créons une contrainte d’itinéraire localhost. La contrainte d’itinéraire localhost correspond uniquement aux demandes effectuées à partir de l’ordinateur local. Les demandes distantes à partir d’Internet ne sont pas mises en correspondance.

Implémentez une contrainte d’itinéraire personnalisée en implémentant l’interface IRouteConstraint. Il s’agit d’une interface extrêmement simple qui décrit une méthode unique :

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

La méthode retourne une valeur booléenne. Si vous renvoyez la valeur false, l’itinéraire associé à la contrainte ne correspondra pas à la demande du navigateur.

La contrainte localhost est contenue dans la liste 1.

**Liste 1-LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

La contrainte de la liste 1 tire parti de la propriété IsLocal exposée par la classe HttpRequest. Cette propriété renvoie la valeur true lorsque l’adresse IP de la demande est 127.0.0.1 ou lorsque l’adresse IP de la demande est identique à l’adresse IP du serveur.

Vous utilisez une contrainte personnalisée dans un itinéraire défini dans le fichier global. asax. Le fichier global. asax de la liste 2 utilise la contrainte localhost pour empêcher quiconque de demander une page d’administration à moins qu’il n’effectue la demande à partir du serveur local. Par exemple, une demande pour/Admin/DeleteAll échoue lorsqu’elle est effectuée à partir d’un serveur distant.

**Liste 2-global. asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

La contrainte localhost est utilisée dans la définition de l’itinéraire d’administration. Cet itinéraire n’est pas mis en correspondance par une demande de navigateur à distance. Sachez toutefois que les autres itinéraires définis dans global. asax peuvent correspondre à la même requête. Il est important de comprendre qu’une contrainte empêche un itinéraire particulier de correspondre à une requête et non à tous les itinéraires définis dans le fichier global. asax.

Notez que l’itinéraire par défaut a été mis en commentaire à partir du fichier global. asax dans la liste 2. Si vous incluez l’itinéraire par défaut, l’itinéraire par défaut correspondrait aux demandes du contrôleur d’administration. Dans ce cas, les utilisateurs distants peuvent appeler des actions du contrôleur d’administration même si leurs demandes ne correspondent pas à l’itinéraire de l’administrateur.

> [!div class="step-by-step"]
> [Précédent](creating-a-route-constraint-cs.md)
> [Suivant](asp-net-mvc-controller-overview-vb.md)
