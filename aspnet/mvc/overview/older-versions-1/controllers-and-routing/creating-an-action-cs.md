---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Création d’une actionC#() | Microsoft Docs
author: microsoft
description: Découvrez comment ajouter une nouvelle action à un contrôleur MVC ASP.NET. En savoir plus sur la configuration requise pour qu’une méthode soit une action.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: ebba935383819935ad85c95245666f4eaf6a0dca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582053"
---
# <a name="creating-an-action-c"></a>Création d’une action (C#)

par [Microsoft](https://github.com/microsoft)

> Découvrez comment ajouter une nouvelle action à un contrôleur MVC ASP.NET. En savoir plus sur la configuration requise pour qu’une méthode soit une action.

L’objectif de ce didacticiel est d’expliquer comment vous pouvez créer une nouvelle action de contrôleur. Vous en saurez plus sur les exigences d’une méthode d’action. Vous apprendrez également comment empêcher une méthode d’être exposée en tant qu’action.

## <a name="adding-an-action-to-a-controller"></a>Ajout d’une action à un contrôleur

Pour ajouter une nouvelle action à un contrôleur, ajoutez une nouvelle méthode au contrôleur. Par exemple, le contrôleur de la liste 1 contient une action nommée index () et une action nommée SayHello (). Les deux méthodes sont exposées en tant qu’actions.

**Liste 1-Controllers\HomeController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

Pour être exposé à l’univers en tant qu’action, une méthode doit répondre à certaines exigences :

- La méthode doit être publique.
- La méthode ne peut pas être une méthode statique.
- La méthode ne peut pas être une méthode d’extension.
- La méthode ne peut pas être un constructeur, un accesseur Get ou un accesseur Set.
- La méthode ne peut pas avoir de types génériques ouverts.
- La méthode n’est pas une méthode de la classe de base du contrôleur.
- La méthode ne peut pas contenir de paramètres **ref** ou **out** .

Notez qu’il n’existe aucune restriction sur le type de retour d’une action de contrôleur. Une action de contrôleur peut retourner une chaîne, une valeur DateTime, une instance de la classe Random ou void. L’infrastructure MVC ASP.NET convertit tout type de retour qui n’est pas un résultat d’action en une chaîne et affiche la chaîne dans le navigateur.

Lorsque vous ajoutez une méthode qui ne viole pas ces exigences à un contrôleur, la méthode est exposée en tant qu’action de contrôleur. Soyez prudent ici. Une action de contrôleur peut être appelée par toute personne connectée à Internet. Ne créez pas, par exemple, une action de contrôleur DeleteMyWebsite ().

## <a name="preventing-a-public-method-from-being-invoked"></a>Empêcher l’appel d’une méthode publique

Si vous avez besoin de créer une méthode publique dans une classe de contrôleur et que vous ne souhaitez pas exposer la méthode en tant qu’action de contrôleur, vous pouvez empêcher la méthode d’être appelée à l’aide de l’attribut [non-action]. Par exemple, le contrôleur de la liste 2 contient une méthode publique nommée CompanySecrets () qui est décorée avec l’attribut [non-action].

**Liste 2-Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Si vous tentez d’appeler l’action du contrôleur CompanySecrets () en tapant/Work/CompanySecrets dans la barre d’adresses de votre navigateur, vous obtiendrez le message d’erreur dans la figure 1.

[![de l’appel d’une méthode qui n’est pas une action](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Figure 01**: appel d’une méthode non-action ([cliquez pour afficher l’image en taille réelle](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Précédent](creating-a-controller-cs.md)
> [Suivant](asp-net-mvc-routing-overview-vb.md)
