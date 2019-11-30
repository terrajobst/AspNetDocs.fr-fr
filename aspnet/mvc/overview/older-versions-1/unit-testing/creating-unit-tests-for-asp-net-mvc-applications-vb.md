---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Création de tests unitaires pour les applications ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Découvrez comment créer des tests unitaires pour les actions du contrôleur. Dans ce didacticiel, Stephen Walther montre comment tester si une action de contrôleur retourne un parti...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 643faddaf6f9cd075131e8e5a9cccb303e355ceb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594700"
---
# <a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Création de tests unitaires pour les applications ASP.NET MVC (VB)

par [Stephen Walther](https://github.com/StephenWalther)

[Télécharger PDF](https://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Découvrez comment créer des tests unitaires pour les actions du contrôleur. Dans ce didacticiel, Stephen Walther montre comment tester si une action de contrôleur retourne une vue particulière, retourne un jeu de données particulier ou retourne un type différent de résultat d’action.

L’objectif de ce didacticiel est de montrer comment vous pouvez écrire des tests unitaires pour les contrôleurs dans vos applications MVC ASP.NET. Nous expliquons comment créer trois types différents de tests unitaires. Vous apprenez à tester la vue retournée par une action de contrôleur, à tester les données d’affichage retournées par une action de contrôleur et à tester si une action de contrôleur vous redirige vers une deuxième action de contrôleur.

## <a name="creating-the-controller-under-test"></a>Création du contrôleur testé

Commençons par créer le contrôleur que nous avons l’intention de tester. Le contrôleur, appelé le `ProductController`, est contenu dans la liste 1.

**Liste 1 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

Le `ProductController` contient deux méthodes d’action nommées `Index()` et `Details()`. Les deux méthodes d’action retournent une vue. Notez que l’action `Details()` accepte un paramètre nommé ID.

## <a name="testing-the-view-returned-by-a-controller"></a>Test de la vue retournée par un contrôleur

Imaginez que nous souhaitons tester si le `ProductController` retourne la vue de droite. Nous voulons nous assurer que lors de l’appel de l’action `ProductController.Details()`, la vue Détails est retournée. La classe de test dans la liste 2 contient un test unitaire pour tester la vue retournée par l’action `ProductController.Details()`.

**Liste 2 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

La classe de la liste 2 comprend une méthode de test nommée `TestDetailsView()`. Cette méthode contient trois lignes de code. La première ligne de code crée une nouvelle instance de la classe `ProductController`. La deuxième ligne de code appelle la méthode d’action `Details()` du contrôleur. Enfin, la dernière ligne de code vérifie si la vue retournée par l’action `Details()` est le mode Détails.

La propriété `ViewResult.ViewName` représente le nom de la vue retournée par un contrôleur. Un grand avertissement concernant le test de cette propriété. Un contrôleur peut retourner une vue de deux manières. Un contrôleur peut retourner explicitement une vue de la façon suivante :

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

Le nom de la vue peut également être déduit à partir du nom de l’action de contrôleur comme suit :

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Cette action de contrôleur retourne également une vue nommée `Details`. Toutefois, le nom de la vue est déduit à partir du nom de l’action. Si vous souhaitez tester le nom de la vue, vous devez retourner explicitement le nom de la vue à partir de l’action du contrôleur.

Vous pouvez exécuter le test unitaire dans la liste 2 en entrant la combinaison de touches **Ctrl-R, A** ou en cliquant sur le bouton **exécuter tous les tests de la solution** (voir la figure 1). Si le test réussit, vous verrez la fenêtre de Résultats des tests dans la figure 2.

[![exécuter tous les tests de la solution](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Figure 01**: exécuter tous les tests de la solution ([cliquez pour afficher l’image en taille réelle](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))

[![réussie !](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Figure 02**: réussite ! ([Cliquez pour afficher l’image en taille réelle](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))

## <a name="testing-the-view-data-returned-by-a-controller"></a>Test des données d’affichage retournées par un contrôleur

Un contrôleur MVC transmet des données à une vue à l’aide d’un nom appelé *`View Data`* . Par exemple, imaginez que vous souhaitez afficher les détails d’un produit particulier lorsque vous appelez l’action `ProductController Details()`. Dans ce cas, vous pouvez créer une instance d’une classe `Product` (définie dans votre modèle) et passer l’instance à la vue `Details` en tirant parti de `View Data`.

Le `ProductController` modifié dans la liste 3 comprend une action de `Details()` mise à jour qui retourne un produit.

**Liste 3 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

Tout d’abord, l’action `Details()` crée une nouvelle instance de la classe `Product` qui représente un ordinateur portable. Ensuite, l’instance de la classe `Product` est passée comme deuxième paramètre à la méthode `View()`.

Vous pouvez écrire des tests unitaires pour tester si les données attendues sont contenues dans les données d’affichage. Le test unitaire dans la liste 4 teste si un produit représentant un ordinateur portable est retourné lorsque vous appelez la méthode d’action `ProductController Details()`.

**Liste 4 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

Dans la liste 4, la méthode `TestDetailsView()` teste les données de vue retournées en appelant la méthode `Details()`. Le `ViewData` est exposé en tant que propriété sur la `ViewResult` retournée en appelant la méthode `Details()`. La propriété `ViewData.Model` contient le produit passé à la vue. Le test vérifie simplement que le produit contenu dans les données de la vue a le nom portable.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Test du résultat de l’action retourné par un contrôleur

Une action de contrôleur plus complexe peut retourner différents types de résultats d’action en fonction des valeurs des paramètres transmis à l’action du contrôleur. Une action de contrôleur peut retourner divers types de résultats d’action, notamment un `ViewResult`, `RedirectToRouteResult`ou `JsonResult`.

Par exemple, l’action modifiée `Details()` dans la liste 5 retourne la vue de `Details` lorsque vous transmettez un ID de produit valide à l’action. Si vous transmettez un ID de produit non valide (un ID dont la valeur est inférieure à 1), vous êtes redirigé vers l’action `Index()`.

**Liste 5 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

Vous pouvez tester le comportement de l’action `Details()` avec le test unitaire dans la liste 6. Le test unitaire dans la liste 6 vérifie que vous êtes redirigé vers la vue `Index` lorsqu’un ID avec la valeur-1 est passé à la méthode `Details()`.

**Liste 6 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Lorsque vous appelez la méthode `RedirectToAction()` dans une action de contrôleur, l’action du contrôleur retourne un `RedirectToRouteResult`. Le test vérifie si l' `RedirectToRouteResult` redirige l’utilisateur vers une action de contrôleur nommée `Index`.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à créer des tests unitaires pour les actions du contrôleur MVC. Tout d’abord, vous avez appris à vérifier si la vue de droite est retournée par une action de contrôleur. Vous avez appris à utiliser la propriété `ViewResult.ViewName` pour vérifier le nom d’une vue.

Ensuite, nous avons examiné comment vous pouvez tester le contenu de `View Data`. Vous avez appris à vérifier si le produit approprié a été retourné dans `View Data` après l’appel d’une action de contrôleur.

Enfin, nous avons abordé la manière dont vous pouvez tester si les différents types de résultats d’action sont retournés à partir d’une action de contrôleur. Vous avez appris à tester si un contrôleur retourne un `ViewResult` ou un `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Précédent](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
