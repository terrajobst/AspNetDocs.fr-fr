---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Ajout d’un contrôleurC#() | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, que j’ai...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 959116ff773f4ef466cda6b172e8321590b50e5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540942"
---
# <a name="adding-a-controller-c"></a>Ajout d’un contrôleur (C#)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../../getting-started/introduction/getting-started.md) et utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sûr, bien plus simple à suivre et qui illustre davantage de fonctionnalités.
> 
> 
> Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :
> 
> - [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet Visual Web Developer C# avec le code source est disponible pour accompagner cette rubrique. [Téléchargez la C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez Visual Basic, passez à la [version Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de ce didacticiel.

MVC est l’acronyme de *Model-View-Controller*. MVC est un modèle qui permet de développer des applications bien conçues et faciles à gérer. Les applications basées sur MVC contiennent les éléments suivants :

- Contrôleurs : classes qui gèrent les demandes entrantes à l’application, récupèrent les données du modèle, puis spécifient les modèles de vue qui retournent une réponse au client.
- Models : classes qui représentent les données de l’application et qui utilisent une logique de validation pour appliquer des règles d’entreprise pour ces données.
- Vues : fichiers modèles que votre application utilise pour générer dynamiquement des réponses HTML.

Nous allons aborder tous ces concepts dans cette série de didacticiels et vous montrerons comment les utiliser pour créer une application.

Commençons par créer une classe de contrôleur. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Controllers* , puis sélectionnez **Ajouter un contrôleur**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Nommez votre nouveau contrôleur « HelloWorldController ». Laissez le modèle par défaut **vide comme contrôleur** , puis cliquez sur **Ajouter**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Notez dans **Explorateur de solutions** qu’un nouveau fichier a été créé nommé *HelloWorldController.cs*. Le fichier est ouvert dans l’IDE.

![](adding-a-controller/_static/image5.png)

À l’intérieur du bloc `public class HelloWorldController`, créez deux méthodes qui ressemblent au code suivant. Le contrôleur retourne une chaîne de code HTML à titre d’exemple.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Votre contrôleur est nommé `HelloWorldController` et la première méthode ci-dessus est nommée `Index`. Nous allons l’appeler à partir d’un navigateur. Exécutez l’application (appuyez sur F5 ou CTRL + F5). Dans le navigateur, ajoutez « HelloWorld » au chemin d’accès dans la barre d’adresses. (Par exemple, dans l’illustration ci-dessous, il s’agit de `http://localhost:43246/HelloWorld.`) La page dans le navigateur ressemble à la capture d’écran suivante. Dans la méthode ci-dessus, le code a retourné une chaîne directement. Vous avez demandé au système de retourner simplement du code HTML, et c’est le fait !

![](adding-a-controller/_static/image6.png)

ASP.NET MVC appelle différentes classes de contrôleur (et différentes méthodes d’action qu’ils contiennent) en fonction de l’URL entrante. La logique de mappage par défaut utilisée par ASP.NET MVC utilise un format similaire à celui-ci pour déterminer le code à appeler :

`/[Controller]/[ActionName]/[Parameters]`

La première partie de l’URL détermine la classe de contrôleur à exécuter. */HelloWorld* est donc mappé à la classe `HelloWorldController`. La deuxième partie de l’URL détermine la méthode d’action sur la classe à exécuter. Par conséquent, */HelloWorld/index* provoque l’exécution de la méthode `Index` de la classe `HelloWorldController`. Notez que nous avons uniquement accès à */HelloWorld* et que la méthode `Index` a été utilisée par défaut. Cela est dû au fait qu’une méthode nommée `Index` est la méthode par défaut qui sera appelée sur un contrôleur, si aucune n’est explicitement spécifiée.

Accédez à `http://localhost:xxxx/HelloWorld/Welcome`. La méthode `Welcome` s’exécute et retourne la chaîne « This is the Welcome action method... ». Le mappage MVC par défaut est `/[Controller]/[ActionName]/[Parameters]`. Pour cette URL, le contrôleur est `HelloWorld`, et `Welcome` est la méthode d’action. Vous n’avez pas encore utilisé la partie `[Parameters]` de l’URL.

![](adding-a-controller/_static/image7.png)

Nous allons modifier légèrement l’exemple pour pouvoir passer des informations de paramètres de l’URL au contrôleur (par exemple, */HelloWorld/Welcome ? Name = Scott&amp;numtimes = 4*). Modifiez votre méthode `Welcome` pour inclure deux paramètres comme indiqué ci-dessous. Notez que le code utilise la C# fonctionnalité de paramètre facultatif pour indiquer que le paramètre `numTimes` doit avoir la valeur 1 par défaut si aucune valeur n’est passée pour ce paramètre.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Exécutez votre application et accédez à l’exemple d’URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Vous pouvez essayer différentes valeurs pour `name` et `numtimes` dans l’URL. Le système mappe automatiquement les paramètres nommés de la chaîne de requête dans la barre d’adresse aux paramètres de votre méthode.

![](adding-a-controller/_static/image8.png)

Dans ces deux exemples, le contrôleur a fait la partie « VC » de MVC, à savoir le fonctionnement de la vue et du contrôleur. Le contrôleur retourne directement du HTML. En règle générale, vous ne souhaitez pas que les contrôleurs renvoient du code HTML directement, car cela devient très fastidieux au code. Au lieu de cela, nous utiliserons généralement un fichier de modèle de vue distinct pour faciliter la génération de la réponse HTML. Nous allons maintenant voir comment nous pouvons le faire.

> [!div class="step-by-step"]
> [Précédent](intro-to-aspnet-mvc-3.md)
> [Suivant](adding-a-view.md)
