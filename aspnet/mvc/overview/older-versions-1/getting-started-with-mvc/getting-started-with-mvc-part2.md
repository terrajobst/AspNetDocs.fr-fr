---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Ajout d’un contrôleur | Microsoft Docs
author: shanselman
description: Une version mise à jour si ce didacticiel est disponible ici à l’aide de Visual Studio 2013. Le nouveau didacticiel utilise ASP.NET MVC 5, qui offre de nombreuses améliorations par rapport à t...
ms.author: riande
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: e2a298584473f57c2b14edf507f0f6886d906ea3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543980"
---
# <a name="adding-a-controller"></a>Ajour d’un contrôleur

par [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Une version mise à jour si ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) à l’aide de [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). Le nouveau didacticiel utilise ASP.NET MVC 5, qui offre de nombreuses améliorations par rapport à ce didacticiel.
>
>
> Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données. Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.

MVC est l’acronyme de Model, View, Controller. MVC est un modèle pour le développement d’applications, de sorte que chaque partie a une responsabilité différente de celle d’une autre.

- Modèle : les données de votre application
- Vues : fichiers modèles que votre application utilisera pour générer dynamiquement des réponses HTML.
- Contrôleurs : classes qui gèrent les demandes d’URL entrantes pour l’application, récupèrent les données du modèle, puis spécifient les modèles de vue qui restituent une réponse au client

Nous allons aborder tous ces concepts dans ce didacticiel et vous montrerons comment les utiliser pour créer une application.

Nous allons créer un nouveau contrôleur en cliquant avec le bouton droit sur le dossier Controllers dans l’Explorateur de solutions et en sélectionnant Ajouter un contrôleur.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Nommez votre nouveau contrôleur « HelloWorldController », puis cliquez sur Ajouter.

[![boîte de dialogue Ajouter un contrôleur](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Notez dans le Explorateur de solutions à droite qu’un nouveau fichier a été créé pour vous appelé HelloWorldController.cs et que ce fichier est maintenant ouvert dans l' **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Créez deux nouvelles méthodes qui ressemblent à ceci à l’intérieur de votre nouvelle classe publique HelloWorldController. Nous allons retourner une chaîne de code HTML directement à partir de notre contrôleur en guise d’exemple.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Votre contrôleur est nommé HelloWorldController et votre nouvelle méthode est appelée index. Exécutez à nouveau votre application, comme auparavant (cliquez sur le bouton lecture ou appuyez sur F5 pour effectuer cette opération). Une fois que votre navigateur a démarré, remplacez le chemin d’accès dans la barre d’adresses par `http://localhost:xx/HelloWorld` où xx correspond au nombre que votre ordinateur a choisi. Votre navigateur doit maintenant ressembler à la capture d’écran ci-dessous. Dans notre méthode ci-dessus, nous avons retourné une chaîne transmise dans une méthode appelée « Content ». Nous avons dit que le système renvoyait simplement du code HTML, et cela faisait !

ASP.NET MVC appelle différentes classes de contrôleur (et différentes méthodes d’action qu’ils contiennent) en fonction de l’URL entrante. La logique de mappage par défaut utilisée par ASP.NET MVC utilise un format similaire à celui-ci pour contrôler l’exécution du code :

/[Contrôleur]/[ActionName]/[paramètres]

La première partie de l’URL détermine la classe de contrôleur à exécuter. /HelloWorld est donc mappé à la classe HelloWorldController. La deuxième partie de l’URL détermine la méthode d’action sur la classe à exécuter. Par conséquent,/HelloWorld/Index provoque l’exécution de la méthode index () de la classe HelloWorldController. Notez que nous n’avons eu qu’à visiter/HelloWorld ci-dessus et que l’index de méthode était implicite. Cela est dû au fait qu’une méthode nommée « index » est la méthode par défaut qui sera appelée sur un contrôleur si aucune n’est explicitement spécifiée.

[![il s’agit de mon action par défaut](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

À présent, nous allons visiter `http://localhost:xx/HelloWorld/Welcome.` maintenant notre méthode Welcome a exécuté et retourné sa chaîne HTML.

Encore une fois,/[Controller]/[ActionName]/[Parameters], le contrôleur est HelloWorld et Welcome est la méthode dans ce cas. Nous n’avons pas encore effectué de paramètres.

[![il s’agit de la méthode d’action Welcome](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Nous allons légèrement modifier notre exemple pour que nous puissions transmettre des informations de l’URL à notre contrôleur, par exemple:/HelloWorld/Welcome ? Name = Scott&amp;numtimes = 4. Modifiez votre méthode d’accueil pour inclure deux paramètres et mettez-le à jour comme indiqué ci-dessous. Notez que nous avons utilisé la C# fonctionnalité de paramètre facultative pour indiquer que le paramètre numTimes doit avoir la valeur 1 par défaut si elle n’est pas passée.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Exécutez votre application et accédez `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` en modifiant la valeur de nom et de numtimes comme vous le souhaitez. Le système a automatiquement mappé les paramètres nommés de votre chaîne de requête dans la barre d’adresse aux paramètres de votre méthode.

Dans ces deux exemples, le contrôleur a fait tout le travail et a renvoyé du code HTML directement. En règle générale, nous ne souhaitons pas que nos contrôleurs renvoient du code HTML directement, car cela finit par être très fastidieux au code. Au lieu de cela, nous utiliserons généralement un fichier de modèle de vue distinct pour faciliter la génération de la réponse HTML. Nous allons voir comment nous pouvons le faire. Fermez votre navigateur et revenez à l’IDE.

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part1.md)
> [Suivant](getting-started-with-mvc-part3.md)
