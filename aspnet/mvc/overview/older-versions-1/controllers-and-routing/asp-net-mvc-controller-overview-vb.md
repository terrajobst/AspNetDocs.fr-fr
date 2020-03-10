---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Vue d’ensemble du contrôleur ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther vous présente les contrôleurs MVC ASP.NET. Vous allez apprendre à créer de nouveaux contrôleurs et à retourner différents types de ressources d’action res...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: f19e7dd7fc025de2e0c387db898d36623e790e6a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601562"
---
# <a name="aspnet-mvc-controller-overview-vb"></a>Vue d’ensemble du contrôleur ASP.NET MVC (VB)

par [Stephen Walther](https://github.com/StephenWalther)

> Dans ce didacticiel, Stephen Walther vous présente les contrôleurs MVC ASP.NET. Vous allez apprendre à créer de nouveaux contrôleurs et à retourner différents types de résultats d’action.

Ce didacticiel explore le sujet des contrôleurs MVC ASP.NET, des actions du contrôleur et des résultats d’action. Une fois ce didacticiel terminé, vous comprendrez comment les contrôleurs sont utilisés pour contrôler la façon dont un visiteur interagit avec un site Web MVC ASP.NET.

## <a name="understanding-controllers"></a>Fonctionnement des contrôleurs

Les contrôleurs MVC sont chargés de répondre aux demandes effectuées sur un site Web MVC ASP.NET. Chaque demande de navigateur est mappée à un contrôleur particulier. Par exemple, imaginez que vous entrez l’URL suivante dans la barre d’adresses de votre navigateur :

`http://localhost/Product/Index/3`

Dans ce cas, un contrôleur nommé ProductController est appelé. Le ProductController est responsable de la génération de la réponse à la demande du navigateur. Par exemple, le contrôleur peut retourner une vue particulière au navigateur ou le contrôleur peut rediriger l’utilisateur vers un autre contrôleur.

La liste 1 contient un contrôleur simple nommé ProductController.

**Listing1 - Controllers\ProductController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

Comme vous pouvez le voir dans la liste 1, un contrôleur n’est qu’une classe (un C# Visual Basic .net ou une classe). Un contrôleur est une classe qui dérive de la classe System. Web. Mvc. Controller de base. Comme un contrôleur hérite de cette classe de base, un contrôleur hérite de plusieurs méthodes utiles gratuites (nous aborderons ces méthodes dans un instant).

## <a name="understanding-controller-actions"></a>Compréhension des actions du contrôleur

Un contrôleur expose des actions de contrôleur. Une action est une méthode sur un contrôleur qui est appelée lorsque vous entrez une URL particulière dans la barre d’adresse de votre navigateur. Par exemple, imaginez que vous faites une demande pour l’URL suivante :

`http://localhost/Product/Index/3`

Dans ce cas, la méthode index () est appelée sur la classe ProductController. La méthode index () est un exemple d’action de contrôleur.

Une action de contrôleur doit être une méthode publique d’une classe de contrôleur. Par défaut, les méthodes Visual Basic.NET sont des méthodes publiques. Sachez que toute méthode publique que vous ajoutez à une classe de contrôleur est exposée automatiquement en tant qu’action de contrôleur (vous devez faire attention à cela, car une action de contrôleur peut être appelée par n’importe qui dans l’univers en tapant simplement l’URL appropriée dans une barre d’adresses de navigateur).

Certaines exigences supplémentaires doivent être satisfaites par une action de contrôleur. Une méthode utilisée comme action de contrôleur ne peut pas être surchargée. En outre, une action de contrôleur ne peut pas être une méthode statique. À part cela, vous pouvez utiliser quasiment n’importe quelle méthode en tant qu’action de contrôleur.

## <a name="understanding-action-results"></a>Compréhension des résultats des actions

Une action de contrôleur retourne un événement appelé *résultat d’action*. Un résultat d’action est ce qu’une action de contrôleur retourne en réponse à une demande de navigateur.

L’infrastructure MVC ASP.NET prend en charge plusieurs types de résultats d’action, notamment :

1. ViewResult : représente le code HTML et le balisage.
2. EmptyResult : ne représente aucun résultat.
3. RedirectResult : représente une redirection vers une nouvelle URL.
4. JsonResult : représente un résultat JavaScript Object Notation qui peut être utilisé dans une application AJAX.
5. JavaScriptResult : représente un script JavaScript.
6. ContentResult : représente un résultat de texte.
7. FileContentResult : représente un fichier téléchargeable (avec le contenu binaire).
8. FilePathResult : représente un fichier téléchargeable (avec un chemin d’accès).
9. FileStreamResult : représente un fichier téléchargeable (avec un flux de fichier).

Tous ces résultats d’action héritent de la classe de base ActionResult.

Dans la plupart des cas, une action de contrôleur retourne un ViewResult. Par exemple, l’action du contrôleur d’index dans la liste 2 retourne un ViewResult.

**Liste 2-Controllers\BookController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

Lorsqu’une action renvoie un ViewResult, le code HTML est renvoyé au navigateur. La méthode index () de la liste 2 retourne une vue nommée index au navigateur.

Notez que l’action index () dans la liste 2 ne retourne pas de ViewResult (). Au lieu de cela, la méthode View () de la classe de base Controller est appelée. Normalement, vous ne retournez pas directement un résultat d’action. Au lieu de cela, vous appelez l’une des méthodes suivantes de la classe de base du contrôleur :

1. View : retourne un résultat d’action ViewResult.
2. Redirect-retourne un résultat d’action RedirectResult.
3. RedirectToAction-retourne un résultat d’action RedirectToRouteResult.
4. RedirectToRoute-retourne un résultat d’action RedirectToRouteResult.
5. JSON-retourne un résultat d’action JsonResult.
6. JavaScriptResult-retourne un JavaScriptResult.
7. Content-renvoie un résultat d’action ContentResult.
8. File : retourne un FileContentResult, FilePathResult ou FileStreamResult selon les paramètres passés à la méthode.

Par conséquent, si vous souhaitez retourner une vue au navigateur, vous appelez la méthode View (). Si vous souhaitez rediriger l’utilisateur d’une action de contrôleur vers une autre, vous appelez la méthode RedirectToAction (). Par exemple, l’action détails () dans la liste 3 permet d’afficher une vue ou de rediriger l’utilisateur vers l’action index (), selon que le paramètre ID a une valeur.

**Liste 3-CustomerController. vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

Le résultat de l’action ContentResult est spécial. Vous pouvez utiliser le résultat de l’action ContentResult pour retourner un résultat d’action sous forme de texte brut. Par exemple, la méthode index () de la liste 4 retourne un message sous forme de texte brut et non de code HTML.

**Liste 4-Controllers\StatusController.vb**

> StatusController
> 
> 
> System.Web.Mvc.Controller

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

Lorsque l’action StatusController. index () est appelée, une vue n’est pas retournée. Au lieu de cela, le texte brut « Hello World ! » est retourné au navigateur.

Si une action de contrôleur retourne un résultat qui n’est pas un résultat d’action (par exemple, une date ou un entier), le résultat est automatiquement encapsulé dans un ContentResult. Par exemple, lorsque l’action index () de WorkController dans la liste 5 est appelée, la date est retournée automatiquement en tant que ContentResult.

**Liste 5-WorkController. vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

L’action index () dans la liste 5 retourne un objet DateTime. L’infrastructure MVC ASP.NET convertit l’objet DateTime en une chaîne et encapsule automatiquement la valeur DateTime dans un ContentResult. Le navigateur reçoit la date et l’heure sous forme de texte brut.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel était de vous présenter les concepts des contrôleurs MVC ASP.NET, des actions du contrôleur et des résultats des actions du contrôleur. Dans la première section, vous avez appris à ajouter de nouveaux contrôleurs à un projet MVC ASP.NET. Ensuite, vous avez appris comment les méthodes publiques d’un contrôleur sont exposées à l’univers comme des actions de contrôleur. Enfin, nous avons abordé les différents types de résultats d’action qui peuvent être retournés à partir d’une action de contrôleur. Nous avons notamment abordé la manière de retourner un ViewResult, RedirectToActionResult et ContentResult à partir d’une action de contrôleur.

> [!div class="step-by-step"]
> [Précédent](creating-a-custom-route-constraint-cs.md)
> [Suivant](creating-custom-routes-vb.md)
