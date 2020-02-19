---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Ajout d’un contrôleur | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 194a8a7398e163f0c37164a8724f98b16444984b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457217"
---
# <a name="adding-a-controller"></a>Ajout d'un contrôleur

par [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

MVC est l’acronyme de *Model-View-Controller*. MVC est un modèle qui permet de développer des applications bien conçues, testables et faciles à entretenir. Les applications basées sur MVC contiennent les éléments suivants :

- **M** odèles : classes qui représentent les données de l’application et qui utilisent une logique de validation pour appliquer des règles d’entreprise pour ces données.
- **V** ues : fichiers modèles que votre application utilise pour générer dynamiquement des réponses html.
- **C** Ontrôleurs : classes qui gèrent les demandes de navigateur entrantes, récupèrent les données de modèle, puis spécifient les modèles de vue qui retournent une réponse au navigateur.

Nous allons aborder tous ces concepts dans cette série de didacticiels et vous montrerons comment les utiliser pour créer une application.

Commençons par créer une classe de contrôleur. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Controllers* , puis cliquez sur **Ajouter**, puis sur **contrôleur**.

![](adding-a-controller/_static/image1.png)

Dans la boîte de dialogue **Ajouter une structure** , cliquez sur **contrôleur MVC 5-vide**, puis cliquez sur **Ajouter**.

![](adding-a-controller/_static/image2.png)  

Nommez votre nouveau contrôleur « HelloWorldController », puis cliquez sur **Ajouter**.

![Ajouter un contrôleur](adding-a-controller/_static/image3.png)

Notez dans **Explorateur de solutions** qu’un nouveau fichier a été créé nommé *HelloWorldController.cs* et un nouveau dossier *Views\HelloWorld*. Le contrôleur est ouvert dans l’IDE.

![](adding-a-controller/_static/image4.png)

Remplacez le contenu du fichier par le code suivant.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Les méthodes de contrôleur retournent une chaîne de code HTML à titre d’exemple. Le contrôleur est nommé `HelloWorldController` et la première méthode est nommée `Index`. Nous allons l’appeler à partir d’un navigateur. Exécutez l’application (appuyez sur F5 ou CTRL + F5). Dans le navigateur, ajoutez &quot;&quot; HelloWorld au chemin d’accès dans la barre d’adresses. (Par exemple, dans l’illustration ci-dessous, il s’agit de `http://localhost:1234/HelloWorld.`) La page dans le navigateur ressemble à la capture d’écran suivante. Dans la méthode ci-dessus, le code a retourné une chaîne directement. Vous avez demandé au système de retourner simplement du code HTML, et c’est le fait !

![](adding-a-controller/_static/image5.png)

ASP.NET MVC appelle différentes classes de contrôleur (et différentes méthodes d’action qu’ils contiennent) en fonction de l’URL entrante. La logique de routage d’URL par défaut utilisée par ASP.NET MVC utilise un format similaire à celui-ci pour déterminer le code à appeler :

`/[Controller]/[ActionName]/[Parameters]`

Vous définissez le format pour le routage dans le fichier *App\_Start/RouteConfig. cs* .

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Lorsque vous exécutez l’application et que vous ne fournissez aucun segment d’URL, la valeur par défaut est le contrôleur « d’origine » et la méthode d’action « index » est spécifiée dans la section valeurs par défaut du code ci-dessus.

La première partie de l’URL détermine la classe de contrôleur à exécuter. */HelloWorld* est donc mappé à la classe `HelloWorldController`. La deuxième partie de l’URL détermine la méthode d’action sur la classe à exécuter. Par conséquent, */HelloWorld/index* provoque l’exécution de la méthode `Index` de la classe `HelloWorldController`. Notez que nous avons uniquement accès à */HelloWorld* et que la méthode `Index` a été utilisée par défaut. Cela est dû au fait qu’une méthode nommée `Index` est la méthode par défaut qui sera appelée sur un contrôleur, si aucune n’est explicitement spécifiée. La troisième partie du segment d’URL (`Parameters`) concerne les données de routage. Nous verrons les données de routage plus loin dans ce didacticiel.

Accédez à `http://localhost:xxxx/HelloWorld/Welcome`. La méthode `Welcome` s’exécute et retourne la chaîne &quot;il s’agit de la méthode d’action welcome...&quot;. Le mappage MVC par défaut est `/[Controller]/[ActionName]/[Parameters]`. Pour cette URL, le contrôleur est `HelloWorld`, et `Welcome` est la méthode d’action. Vous n’avez pas encore utilisé la partie `[Parameters]` de l’URL.

![](adding-a-controller/_static/image6.png)

Nous allons modifier légèrement l’exemple pour pouvoir passer des informations de paramètres de l’URL au contrôleur (par exemple, */HelloWorld/Welcome ? Name = Scott&amp;numtimes = 4*). Modifiez votre méthode `Welcome` pour inclure deux paramètres comme indiqué ci-dessous. Notez que le code utilise la C# fonctionnalité de paramètre facultatif pour indiquer que le paramètre `numTimes` doit avoir la valeur 1 par défaut si aucune valeur n’est passée pour ce paramètre.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Note de sécurité : le code ci-dessus utilise [HttpUtility. HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) pour protéger l’application contre les entrées malveillantes (à savoir JavaScript). Pour plus d’informations [, consultez Comment : protéger contre les attaques de script dans une application Web en appliquant l’encodage HTML aux chaînes](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).

 Exécutez votre application et accédez à l’exemple d’URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Vous pouvez essayer différentes valeurs pour `name` et `numtimes` dans l’URL. Le [système de liaison de modèle MVC ASP.net](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mappe automatiquement les paramètres nommés de la chaîne de requête dans la barre d’adresse aux paramètres de votre méthode.

![](adding-a-controller/_static/image7.png)

Dans l’exemple ci-dessus, le segment d’URL (`Parameters`) n’est pas utilisé, les paramètres `name` et `numTimes` sont passés en tant que [chaînes de requête](http://en.wikipedia.org/wiki/Query_string). Le point d’interrogation, ?, (point d’interrogation) dans l’URL ci-dessus, il s’agit d’un séparateur, et les chaînes de requête suivent. Le caractère &amp; sépare les chaînes de requête.

Remplacez la méthode Welcome par le code suivant :

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Exécutez l’application et entrez l’URL suivante : `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Cette fois, le troisième segment d’URL correspondait au paramètre d’itinéraire `ID.` la méthode d’action `Welcome` contient un paramètre (`ID`) qui correspondait à la spécification de l’URL dans la méthode `RegisterRoutes`.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

Dans les applications ASP.NET MVC, il est plus courant de transmettre des paramètres en tant que données d’itinéraire (comme nous l’avons fait avec l’ID ci-dessus) que de les passer en tant que chaînes de requête. Vous pouvez également ajouter un itinéraire pour transmettre les `name` et `numtimes` dans les paramètres en tant que données de routage dans l’URL. Dans le fichier *App\_Start\RouteConfig.cs* , ajoutez l’itinéraire « Hello » :

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Exécutez l’application et accédez à `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Pour de nombreuses applications MVC, l’itinéraire par défaut fonctionne correctement. Vous en apprendrez plus loin dans ce didacticiel pour passer des données à l’aide du classeur de modèles, et vous n’aurez pas à modifier l’itinéraire par défaut pour cela.

Dans ces exemples, le contrôleur a fait le &quot;&quot; VC du MVC, à savoir le fonctionnement de la vue et du contrôleur. Le contrôleur retourne directement du HTML. En règle générale, vous ne souhaitez pas que les contrôleurs renvoient du code HTML directement, car cela devient très fastidieux au code. Au lieu de cela, nous utiliserons généralement un fichier de modèle de vue distinct pour faciliter la génération de la réponse HTML. Nous allons maintenant voir comment nous pouvons le faire.

> [!div class="step-by-step"]
> [Précédent](getting-started.md)
> [Suivant](adding-a-view.md)
