---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Création d’un contrôleur (VB) | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther montre comment vous pouvez ajouter un contrôleur à une application ASP.NET MVC.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 60636b79ab5fc06ca904dee90ce74f256e046d12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601457"
---
# <a name="creating-a-controller-vb"></a>Création d’un contrôleur (VB)

par [Stephen Walther](https://github.com/StephenWalther)

> Dans ce didacticiel, Stephen Walther montre comment vous pouvez ajouter un contrôleur à une application ASP.NET MVC.

L’objectif de ce didacticiel est d’expliquer comment vous pouvez créer des contrôleurs ASP.NET MVC. Vous allez apprendre à créer des contrôleurs à la fois à l’aide de l’option de menu Ajouter un contrôleur de Visual Studio et en créant un fichier de classe manuellement.

### <a name="using-the-add-controller-menu-option"></a>Utilisation de l’option de menu Ajouter un contrôleur

Le moyen le plus simple de créer un contrôleur consiste à cliquer avec le bouton droit sur le dossier Controllers dans la fenêtre Explorateur de solutions Visual Studio et à sélectionner l’option de menu **Ajouter, contrôleur** (voir figure 1). La sélection de cette option de menu ouvre la boîte de dialogue **Ajouter un contrôleur** (voir figure 2).

[![la boîte de dialogue Nouveau projet](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Figure 01**: ajout d’un nouveau contrôleur ([cliquez pour afficher l’image en taille réelle](creating-a-controller-vb/_static/image2.png))

[![la boîte de dialogue Nouveau projet](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Figure 02**: boîte de dialogue Ajouter un contrôleur ([cliquez pour afficher l’image en taille réelle](creating-a-controller-vb/_static/image4.png))

Notez que la première partie du nom du contrôleur est mise en surbrillance dans la boîte de dialogue **Ajouter un contrôleur** . Chaque nom de contrôleur doit se terminer par le *contrôleur*de suffixe. Par exemple, vous pouvez créer un contrôleur nommé *ProductController* , mais pas un contrôleur nommé *Product*.

Si vous créez un contrôleur qui ne contient pas le suffixe du *contrôleur* , vous ne pourrez pas appeler le contrôleur. Ne le faites pas--j’ai perdu des heures innombrables de ma vie après avoir fait cette erreur.

**Liste 1-Controllers\ProductController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Vous devez toujours créer des contrôleurs dans le dossier Controllers. Dans le cas contraire, vous violerez les conventions de ASP.NET MVC et d’autres développeurs auront du mal à comprendre votre application.

### <a name="scaffolding-action-methods"></a>Méthodes d’action de génération de modèles automatique

Lorsque vous créez un contrôleur, vous avez la possibilité de générer automatiquement des méthodes d’action créer, mettre à jour et détails (voir figure 3). Si vous sélectionnez cette option, la classe de contrôleur dans la liste 2 est générée.

[![créer automatiquement des méthodes d’action](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Figure 03**: création automatique des méthodes d’action ([cliquez pour afficher l’image en taille réelle](creating-a-controller-vb/_static/image6.png))

**Liste 2-Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Ces méthodes générées sont des méthodes stub. Vous devez ajouter la logique réelle pour la création, la mise à jour et l’indication des détails d’un client. Toutefois, les méthodes stub vous offrent un bon point de départ.

### <a name="creating-a-controller-class"></a>Création d’une classe de contrôleur

Le contrôleur MVC ASP.NET est simplement une classe. Si vous préférez, vous pouvez ignorer la génération de modèles automatique du contrôleur Visual Studio et créer une classe de contrôleur à la main. Procédez comme suit :

1. Cliquez avec le bouton droit sur le dossier Controllers et sélectionnez l’option de menu **Ajouter, nouvel élément,** puis sélectionnez le modèle de **classe** (voir figure 4).
2. Nommez la nouvelle classe PersonController. vb, puis cliquez sur le bouton **Ajouter** .
3. Modifiez le fichier de classe résultant afin que la classe hérite de la classe de base System. Web. Mvc. Controller (voir la liste 3).

[![de la création d’une classe](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Figure 04**: création d’une nouvelle classe ([cliquez pour afficher l’image en taille réelle](creating-a-controller-vb/_static/image8.png))

**Liste 3-Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

Le contrôleur de la liste 3 expose une action nommée index () qui retourne la chaîne « Hello World ! ». Vous pouvez appeler cette action de contrôleur en exécutant votre application et en demandant une URL telle que la suivante :

`http://localhost:40071/Person`

> [!NOTE]
> 
> Le Serveur de développement ASP.NET utilise un numéro de port aléatoire (par exemple, 40071). Lorsque vous entrez une URL pour appeler un contrôleur, vous devez fournir le numéro de port approprié. Vous pouvez déterminer le numéro de port en plaçant le curseur de la souris sur l’icône de la Serveur de développement ASP.NET dans la zone de notification Windows (en bas à droite de votre écran).
> 
> [!div class="step-by-step"]
> [Précédent](adding-dynamic-content-to-a-cached-page-vb.md)
> [Suivant](creating-an-action-vb.md)
