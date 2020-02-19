---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Ajout d’une vueC#() | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: b4a1316feb8d9b7f3ef5ca4755bf1cc5b23e6ef9
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458194"
---
# <a name="adding-a-view-c"></a>Ajout d’une vue (C#)

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

Dans cette section, vous allez modifier la classe `HelloWorldController` pour utiliser les fichiers de modèle de vue afin d’encapsuler correctement le processus de génération de réponses HTML sur un client.

Vous allez créer un fichier de modèle de vue à l’aide du nouveau [moteur d’affichage Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduit avec ASP.NET MVC 3. Les modèles de vue Razor ont une extension de fichier *. cshtml* et offrent un moyen élégant de créer une sortie HTML C#à l’aide de. Razor réduit le nombre de caractères et de frappes de touches requis lors de l’écriture d’un modèle de vue, et permet un flux de travail de codage fluide rapide.

Commencez par utiliser un modèle de vue avec la méthode `Index` dans la classe `HelloWorldController`. Actuellement, la méthode `Index` retourne une chaîne avec un message qui est codé en dur dans la classe du contrôleur. Modifiez la méthode `Index` pour retourner un objet `View`, comme indiqué ci-dessous :

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Ce code utilise un modèle de vue pour générer une réponse HTML au navigateur. Dans le projet, ajoutez un modèle de vue que vous pouvez utiliser avec la méthode `Index`. Pour ce faire, cliquez avec le bouton droit à l’intérieur de la méthode `Index`, puis cliquez sur **Ajouter une vue**.

![](adding-a-view/_static/image1.png)

La boîte de dialogue **Ajouter une vue** s’affiche. Laissez les valeurs par défaut comme elles le sont, puis cliquez sur le bouton **Ajouter** :

![](adding-a-view/_static/image2.png)

Le dossier *MvcMovie\Views\HelloWorld* et le fichier *MvcMovie\Views\HelloWorld\Index.cshtml* sont créés. Vous pouvez les afficher dans **Explorateur de solutions**:

![](adding-a-view/_static/image3.png)

L’exemple suivant montre le fichier *index. cshtml* qui a été créé :

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Ajoutez du code HTML sous la balise `<h2>`. Le fichier *MvcMovie\Views\HelloWorld\Index.cshtml* modifié est illustré ci-dessous.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Exécutez l’application et accédez au contrôleur de `HelloWorld` (`http://localhost:xxxx/HelloWorld`). La méthode `Index` de votre contrôleur n’a pas fait de nombreuses tâches. elle a simplement exécuté l’instruction `return View()`, qui spécifie que la méthode doit utiliser un fichier de modèle de vue pour afficher une réponse dans le navigateur. Étant donné que vous n’avez pas spécifié explicitement le nom du fichier de modèle de vue à utiliser, ASP.NET MVC utilise par défaut le fichier de vue *index. cshtml* dans le dossier *\Views\HelloWorld* L’image ci-dessous montre la chaîne codée en dur dans la vue.

![](adding-a-view/_static/image6.png)

Semble assez bon. Toutefois, Notez que la barre de titre du navigateur indique « index » et que le gros titre sur la page indique « mon application MVC ». Modifions-les.

## <a name="changing-views-and-layout-pages"></a>Modification des vues et des pages de disposition

Tout d’abord, vous souhaitez modifier le titre « mon application MVC » en haut de la page. Ce texte est commun à chaque page. En fait, elle est implémentée dans un seul emplacement du projet, même si elle apparaît sur chaque page de l’application. Accédez au dossier */Views/Shared* dans **Explorateur de solutions** et ouvrez le fichier *Layout. cshtml\_* . Ce fichier s’appelle une *page de disposition* et il s’agit de l’interpréteur de commandes partagé que toutes les autres pages utilisent.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Les modèles de disposition vous permettent de spécifier la disposition de conteneur HTML de votre site à un emplacement, puis de l’appliquer sur plusieurs pages de votre site. Notez la `@RenderBody()` ligne près du bas du fichier. `RenderBody` est un espace réservé dans lequel toutes les pages spécifiques de la vue que vous créez s’affichent, « encapsulé » dans la page de disposition. Remplacez l’en-tête du titre dans le modèle de disposition « mon application MVC » par « application de vidéo MVC ».

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Exécutez l’application et Notez qu’elle indique à présent « MVC Movie App ». Cliquez sur le lien **à propos** de pour voir comment cette page affiche également « application de vidéo Mvc ». Nous avons été en mesure d’apporter la modification une fois dans le modèle de disposition et de faire en sorte que toutes les pages du site reflètent le nouveau titre.

![](adding-a-view/_static/image9.png)

Le fichier *\_Layout. cshtml* complet est indiqué ci-dessous :

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

À présent, nous allons modifier le titre de la page d’index (vue).

Ouvrez *MvcMovie\Views\HelloWorld\Index.cshtml*. Il existe deux emplacements pour apporter une modification : tout d’abord, le texte qui apparaît dans le titre du navigateur, puis dans l’en-tête secondaire (l’élément `<h2>`). Vous allez les modifier légèrement pour voir quel morceau du code modifie quelle partie de l’application.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Pour indiquer le titre HTML à afficher, le code ci-dessus définit une propriété `Title` de l’objet `ViewBag` (qui se trouve dans le modèle de vue *index. cshtml* ). Si vous revenez au code source du modèle de disposition, vous remarquerez que le modèle utilise cette valeur dans l’élément `<title>` dans le cadre de la `<head>` section du code HTML. À l’aide de cette approche, vous pouvez facilement passer d’autres paramètres entre votre modèle de vue et votre fichier de disposition.

Exécutez l’application et accédez à `http://localhost:xx/HelloWorld`. Notez que le titre du navigateur, l’en-tête principal et les en-têtes secondaires ont changé. (Si vous ne voyez pas les changements dans le navigateur, vous voyez peut-être le contenu mis en cache. Appuyez sur Ctrl+F5 dans votre navigateur pour forcer le chargement de la réponse du serveur.)

Notez également que le contenu du modèle de vue *index. cshtml* a été fusionné avec le modèle de vue *\_Layout. cshtml* et qu’une réponse html unique a été envoyée au navigateur. Les modèles de disposition permettent d’apporter facilement des modifications qui s’appliquent à toutes les pages de votre application.

![](adding-a-view/_static/image10.png)

Nos quelques « données » (dans le cas présent, le message « Hello from our View Template! » ) sont toutefois codées en dur. L’application MVC a une vue (« V») et vous avez un contrôleur (« C »), mais pas encore de modèle (« M »). Nous allons bientôt vous montrer comment créer une base de données et récupérer des données de modèle.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passage de données du contrôleur vers la vue

Avant de passer à une base de données et de parler des modèles, nous allons tout d’abord parler de la transmission des informations du contrôleur à une vue. Les classes de contrôleur sont appelées en réponse à une demande d’URL entrante. Une classe de contrôleur est l’endroit où vous écrivez le code qui gère les paramètres entrants, récupère les données d’une base de données et décide finalement le type de réponse à renvoyer au navigateur. Les modèles de vue peuvent ensuite être utilisés à partir d’un contrôleur pour générer et mettre en forme une réponse HTML au navigateur.

Les contrôleurs sont chargés de fournir les données ou objets requis pour qu’un modèle de vue affiche une réponse au navigateur. Un modèle de vue ne doit jamais exécuter de logique métier ou interagir directement avec une base de données. Au lieu de cela, elle doit fonctionner uniquement avec les données qui lui sont fournies par le contrôleur. Le maintien de cette « séparation des préoccupations » permet de préserver la netteté de votre code et de la maintenir plus facile à gérer.

Actuellement, la `Welcome` méthode d’action de la classe `HelloWorldController` prend un `name` et un paramètre `numTimes`, puis génère les valeurs directement dans le navigateur. Plutôt que de faire en sorte que le contrôleur rende cette réponse sous forme de chaîne, nous allons modifier le contrôleur pour qu’il utilise un modèle de vue à la place. Le modèle de vue génère une réponse dynamique, ce qui signifie que vous devez passer les bits de données appropriés du contrôleur à la vue pour générer la réponse. Pour ce faire, vous devez faire en sorte que le contrôleur place les données dynamiques dont le modèle de vue a besoin dans un objet `ViewBag` auquel le modèle de vue peut accéder.

Revenez au fichier *HelloWorldController.cs* et modifiez la méthode `Welcome` pour ajouter une valeur de `Message` et de `NumTimes` à l’objet `ViewBag`. `ViewBag` est un objet dynamique, ce qui signifie que vous pouvez y placer tout ce que vous souhaitez. l’objet `ViewBag` n’a aucune propriété définie tant que vous ne placez pas de contenu dans celui-ci. Le fichier *HelloWorldController.cs* complet ressemble à ceci :

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

À présent, l’objet `ViewBag` contient des données qui seront transmises automatiquement à la vue.

Ensuite, vous avez besoin d’un modèle de vue de bienvenue ! Dans le menu **Déboguer** , sélectionnez **générer MvcMovie** pour vous assurer que le projet est compilé.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Cliquez ensuite avec le bouton droit à l’intérieur de la méthode `Welcome`, puis cliquez sur **Ajouter une vue**. Voici à quoi ressemble la boîte de dialogue **Ajouter une vue** :

![](adding-a-view/_static/image13.png)

Cliquez sur **Ajouter**, puis ajoutez le code suivant sous l’élément `<h2>` dans le nouveau fichier *Welcome. cshtml* . Vous allez créer une boucle qui indique « Hello » autant de fois que l’utilisateur l’indique. Le fichier *Welcome welcome. cshtml* est présenté ci-dessous.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Exécutez l’application et accédez à l’URL suivante :

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Désormais, les données sont extraites de l’URL et transmises automatiquement au contrôleur. Le contrôleur empaquette les données dans un objet `ViewBag` et transmet cet objet à la vue. La vue affiche ensuite les données au format HTML pour l’utilisateur.

![](adding-a-view/_static/image14.png)

Il s’agissait d’une sorte de « M » pour le modèle, mais pas d’une base de données en soi. Créons une base de données de films en utilisant ce que nous avons appris.

> [!div class="step-by-step"]
> [Précédent](adding-a-controller.md)
> [Suivant](adding-a-model.md)
