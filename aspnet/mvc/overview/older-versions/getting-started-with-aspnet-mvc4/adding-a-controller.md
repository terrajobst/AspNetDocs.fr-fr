---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Ajout d’un contrôleur | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : une version mise à jour de ce didacticiel est disponible ici et utilise ASP.NET MVC 5 et Visual Studio 2013. C’est plus sécurisé, bien plus simple à suivre et à faire une démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f528c56435976c7f31fce453c834ef9eaebe6244
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78599840"
---
# <a name="adding-a-controller"></a>Ajour d’un contrôleur

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) et utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sûr, bien plus simple à suivre et qui illustre davantage de fonctionnalités.

MVC est l’acronyme de *Model-View-Controller*. MVC est un modèle qui permet de développer des applications bien conçues, testables et faciles à entretenir. Les applications basées sur MVC contiennent les éléments suivants :

- **M** odèles : classes qui représentent les données de l’application et qui utilisent une logique de validation pour appliquer des règles d’entreprise pour ces données.
- **V** ues : fichiers modèles que votre application utilise pour générer dynamiquement des réponses html.
- **C** Ontrôleurs : classes qui gèrent les demandes de navigateur entrantes, récupèrent les données de modèle, puis spécifient les modèles de vue qui retournent une réponse au navigateur.

Nous allons aborder tous ces concepts dans cette série de didacticiels et vous montrerons comment les utiliser pour créer une application.

Commençons par créer une classe de contrôleur. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Controllers* , puis sélectionnez **Ajouter un contrôleur**.

![](adding-a-controller/_static/image1.png)

Nommez votre nouveau contrôleur &quot;HelloWorldController&quot;. Laissez le modèle par défaut en tant que **contrôleur MVC vide** , puis cliquez sur **Ajouter**.

![Ajouter un contrôleur](adding-a-controller/_static/image2.png)

Notez dans **Explorateur de solutions** qu’un nouveau fichier a été créé nommé *HelloWorldController.cs*. Le fichier est ouvert dans l’IDE.

![](adding-a-controller/_static/image3.png)

Remplacez le contenu du fichier par le code suivant.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Les méthodes de contrôleur retournent une chaîne de code HTML à titre d’exemple. Le contrôleur est nommé `HelloWorldController` et la première méthode ci-dessus est nommée `Index`. Nous allons l’appeler à partir d’un navigateur. Exécutez l’application (appuyez sur F5 ou CTRL + F5). Dans le navigateur, ajoutez &quot;&quot; HelloWorld au chemin d’accès dans la barre d’adresses. (Par exemple, dans l’illustration ci-dessous, il s’agit de `http://localhost:1234/HelloWorld.`) La page dans le navigateur ressemble à la capture d’écran suivante. Dans la méthode ci-dessus, le code a retourné une chaîne directement. Vous avez demandé au système de retourner simplement du code HTML, et c’est le fait !

![](adding-a-controller/_static/image4.png)

ASP.NET MVC appelle différentes classes de contrôleur (et différentes méthodes d’action qu’ils contiennent) en fonction de l’URL entrante. La logique de routage d’URL par défaut utilisée par ASP.NET MVC utilise un format similaire à celui-ci pour déterminer le code à appeler :

`/[Controller]/[ActionName]/[Parameters]`

La première partie de l’URL détermine la classe de contrôleur à exécuter. */HelloWorld* est donc mappé à la classe `HelloWorldController`. La deuxième partie de l’URL détermine la méthode d’action sur la classe à exécuter. Par conséquent, */HelloWorld/index* provoque l’exécution de la méthode `Index` de la classe `HelloWorldController`. Notez que nous avons uniquement accès à */HelloWorld* et que la méthode `Index` a été utilisée par défaut. Cela est dû au fait qu’une méthode nommée `Index` est la méthode par défaut qui sera appelée sur un contrôleur, si aucune n’est explicitement spécifiée.

Accédez à `http://localhost:xxxx/HelloWorld/Welcome`. La méthode `Welcome` s’exécute et retourne la chaîne &quot;il s’agit de la méthode d’action welcome...&quot;. Le mappage MVC par défaut est `/[Controller]/[ActionName]/[Parameters]`. Pour cette URL, le contrôleur est `HelloWorld`, et `Welcome` est la méthode d’action. Vous n’avez pas encore utilisé la partie `[Parameters]` de l’URL.

![](adding-a-controller/_static/image5.png)

Nous allons modifier légèrement l’exemple pour pouvoir passer des informations de paramètres de l’URL au contrôleur (par exemple, */HelloWorld/Welcome ? Name = Scott&amp;numtimes = 4*). Modifiez votre méthode `Welcome` pour inclure deux paramètres comme indiqué ci-dessous. Notez que le code utilise la C# fonctionnalité de paramètre facultatif pour indiquer que le paramètre `numTimes` doit avoir la valeur 1 par défaut si aucune valeur n’est passée pour ce paramètre.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Exécutez votre application et accédez à l’exemple d’URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Vous pouvez essayer différentes valeurs pour `name` et `numtimes` dans l’URL. Le [système de liaison de modèle MVC ASP.net](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mappe automatiquement les paramètres nommés de la chaîne de requête dans la barre d’adresse aux paramètres de votre méthode.

![](adding-a-controller/_static/image6.png)

Dans ces deux exemples, le contrôleur a fait le &quot;&quot; VC du MVC, à savoir le fonctionnement de la vue et du contrôleur. Le contrôleur retourne directement du HTML. En règle générale, vous ne souhaitez pas que les contrôleurs renvoient du code HTML directement, car cela devient très fastidieux au code. Au lieu de cela, nous utiliserons généralement un fichier de modèle de vue distinct pour faciliter la génération de la réponse HTML. Nous allons maintenant voir comment nous pouvons le faire.

> [!div class="step-by-step"]
> [Précédent](intro-to-aspnet-mvc-4.md)
> [Suivant](adding-a-view.md)
