---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Ajout d’un contrôleur (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 2e77f62a9796211b0e59a99c71bc532659b7cb92
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540515"
---
# <a name="adding-a-controller-vb"></a>Ajout d’un contrôleur (VB)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :
> 
> - [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez C#, passez à la [ C# version](../cs/adding-a-controller.md) de ce didacticiel.

MVC est l’acronyme de *Model-View-Controller*. MVC est un modèle pour le développement d’applications de telle sorte que chaque partie a une responsabilité distincte :

- Modèle : les données de votre application.
- Vues : fichiers modèles que votre application utilisera pour générer dynamiquement des réponses HTML.
- Contrôleurs : classes qui gèrent les demandes d’URL entrantes pour l’application, récupèrent les données de modèle, puis spécifient les modèles de vue qui restituent une réponse au client.

Nous allons aborder tous ces concepts dans ce didacticiel et vous montrerons comment les utiliser pour créer une application.

Pour créer un nouveau contrôleur, cliquez avec le bouton droit sur le dossier *Controllers* dans **Explorateur de solutions** puis sélectionnez **Ajouter un contrôleur**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Nommez votre nouveau contrôleur &quot;HelloWorldController&quot; puis cliquez sur **Ajouter**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Notez dans **Explorateur de solutions** à droite qu’un nouveau fichier a été créé pour vous, nommé *HelloWorldController.cs* , et que le fichier est ouvert dans l’IDE.

Dans le nouveau bloc de `public class HelloWorldController`, créez deux nouvelles méthodes qui ressemblent au code suivant. Nous allons retourner une chaîne de code HTML directement à partir du contrôleur à titre d’exemple.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Votre contrôleur est nommé `HelloWorldController` et votre nouvelle méthode est nommée `Index`. Exécutez l’application (appuyez sur F5 ou CTRL + F5). Une fois que votre navigateur a démarré, ajoutez &quot;&quot; HelloWorld au chemin d’accès dans la barre d’adresses. (Sur mon ordinateur, il est `http://localhost:43246/HelloWorld`) Votre navigateur ressemble à la capture d’écran ci-dessous. Dans la méthode ci-dessus, le code a retourné une chaîne directement. Nous avons dit au système de retourner simplement du code HTML, et c’est le fait !

![](adding-a-controller/_static/image5.png)

ASP.NET MVC appelle différentes classes de contrôleur (et différentes méthodes d’action qu’ils contiennent) en fonction de l’URL entrante. La logique de mappage par défaut utilisée par ASP.NET MVC utilise un format similaire à celui-ci pour contrôler le code appelé :

`/[Controller]/[ActionName]/[Parameters]`

La première partie de l’URL détermine la classe de contrôleur à exécuter. */HelloWorld* est donc mappé à la classe `HelloWorldController`. La deuxième partie de l’URL détermine la méthode d’action sur la classe à exécuter. Par conséquent, */HelloWorld/index* provoque l’exécution de la méthode `Index` de la classe `HelloWorldController`. Notez que nous n’avons eu qu’à visiter */HelloWorld* ci-dessus et que la méthode `Index` a été utilisée par défaut. Cela est dû au fait qu’une méthode nommée `Index` est la méthode par défaut qui sera appelée sur un contrôleur, si aucune n’est explicitement spécifiée.

Accédez à `http://localhost:xxxx/HelloWorld/Welcome`. La méthode `Welcome` s’exécute et retourne la chaîne &quot;il s’agit de la méthode d’action welcome...&quot;. Le mappage MVC par défaut est `/[Controller]/[ActionName]/[Parameters]`. Pour cette URL, le contrôleur est `HelloWorld` et `Welcome` est la méthode. Nous n’avons pas encore utilisé la `[Parameters]` partie de l’URL.

![](adding-a-controller/_static/image6.png)

Nous allons modifier légèrement l’exemple pour que nous puissions transmettre des informations de paramètres à partir de l’URL vers le contrôleur (par exemple, */HelloWorld/Welcome ? Name = Scott&amp;numtimes = 4*). Modifiez votre méthode `Welcome` pour inclure deux paramètres comme indiqué ci-dessous. Notez que nous avons utilisé la fonctionnalité de paramètre facultatif VB pour indiquer que le paramètre `numTimes` doit avoir la valeur 1 par défaut si aucune valeur n’est passée pour ce paramètre.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Exécutez votre application et accédez à `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Vous pouvez essayer différentes valeurs pour `name` et `numtimes`. Le système mappe automatiquement les paramètres nommés de votre chaîne de requête dans la barre d’adresse aux paramètres de votre méthode.

![](adding-a-controller/_static/image7.png)

Dans ces deux exemples, le contrôleur a fait la partie VC de MVC, qui est le travail de la vue et du contrôleur. Le contrôleur retourne directement du HTML. En règle générale, nous ne voulons pas que les contrôleurs renvoient du code HTML directement, car cela devient très fastidieux au code. Au lieu de cela, nous utiliserons généralement un fichier de modèle de vue distinct pour faciliter la génération de la réponse HTML. Nous allons voir comment nous pouvons le faire.

> [!div class="step-by-step"]
> [Précédent](intro-to-aspnet-mvc-3.md)
> [Suivant](adding-a-view.md)
