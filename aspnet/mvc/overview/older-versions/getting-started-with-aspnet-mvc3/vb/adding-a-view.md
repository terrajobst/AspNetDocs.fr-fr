---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Ajout d’une vue (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: fa200935d83bb26c07b302449a6eba6fd67b5322
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457317"
---
# <a name="adding-a-view-vb"></a>Ajout d’une vue (VB)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :
> 
> - [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez C#, passez à la [ C# version](../cs/adding-a-view.md) de ce didacticiel.

Dans cette section, nous allons modifier la classe `HelloWorldController` pour utiliser un fichier de modèle de vue afin d’encapsuler correctement le processus de génération de réponses HTML sur un client.

Commençons par utiliser un modèle de vue avec la méthode `Index` dans la classe `HelloWorldController`. Actuellement, la méthode `Index` retourne une chaîne avec un message codé en dur dans la classe Controller. Modifiez la méthode `Index` pour retourner un objet `View`, comme indiqué ci-dessous :

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Nous allons maintenant ajouter un modèle de vue à notre projet que nous pouvons appeler avec la méthode `Index`. Pour ce faire, cliquez avec le bouton droit à l’intérieur de la méthode `Index`, puis cliquez sur **Ajouter une vue**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

La boîte de dialogue **Ajouter une vue** s’affiche. Laissez les entrées par défaut et cliquez sur le bouton **Ajouter** .

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

Le dossier *MvcMovie\Views\HelloWorld* et le fichier *MvcMovie\Views\HelloWorld\Index.vbhtml* sont créés. Vous pouvez les afficher dans **Explorateur de solutions**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Ajoutez du code HTML sous la balise `<h2>`. Le fichier *MvcMovie\Views\HelloWorld\Index.vbhtml* modifié est illustré ci-dessous.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Exécutez l’application et accédez au &quot;le contrôleur de&quot; Hello World (`http://localhost:xxxx/HelloWorld`). La méthode `Index` de votre contrôleur n’a pas fait de nombreuses tâches. elle exécutait simplement l’instruction `return View()`, ce qui indiquait que nous souhaitions utiliser un fichier de modèle de vue pour afficher une réponse au client. Étant donné que nous n’avons pas spécifié explicitement le nom du fichier de modèle de vue à utiliser, ASP.NET MVC utilise par défaut le fichier de vue *index. vbhtml* dans le dossier *\Views\HelloWorld* L’image ci-dessous montre la chaîne codée en dur dans la vue.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Semble assez bon. Toutefois, Notez que la barre de titre du navigateur indique &quot;index&quot; et que le gros titre sur la page indique &quot;mon application MVC.&quot; modifions-les.

## <a name="changing-views-and-layout-pages"></a>Changement de vues et de pages de disposition

Tout d’abord, nous allons modifier le texte &quot;mon application MVC.&quot; ce texte est partagé et apparaît sur chaque page. En fait, il n’apparaît que dans un seul emplacement de notre projet, même s’il se trouve sur chaque page de notre application. Accédez au dossier */Views/Shared* dans **Explorateur de solutions** et ouvrez le fichier *\_Layout. vbhtml* . Ce fichier s’appelle une page de disposition et il s’agit de l’interpréteur de commandes &quot;partagé&quot; que toutes les autres pages utilisent.

Notez la ligne de code `@RenderBody()` en bas du fichier. `RenderBody` est un espace réservé dans lequel toutes les pages que vous créez s’affichent, &quot;&quot; encapsulées dans la page de disposition. Remplacez l’en-tête `<h1>` de **&quot;** &quot; de l’application mvc par &quot;application de film MVC&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Exécutez l’application et Notez qu’elle indique à présent &quot;&quot;d’application de film MVC. Cliquez sur le lien **à propos** de et cette page affiche également &quot;&quot;d’application de film Mvc.

Le fichier *\_Layout. vbhtml* complet est illustré ci-dessous :

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

À présent, nous allons modifier le titre de la page d’index (vue).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Ouvrez *MvcMovie\Views\HelloWorld\Index.vbhtml*. Il existe deux emplacements pour apporter une modification : tout d’abord, le texte qui apparaît dans le titre du navigateur, puis dans l’en-tête secondaire (l’élément `<h2>`). Nous les rendrons légèrement différents afin que vous puissiez voir le bit de modification du code qui fait partie de l’application.

Exécutez l’application et accédez à`http://localhost:xx/HelloWorld`. Notez que le titre du navigateur, l’en-tête principal et les en-têtes secondaires ont changé. Il est facile d’apporter des modifications importantes à votre application avec de petites modifications apportées à une vue. (Si vous ne voyez pas les changements dans le navigateur, vous voyez peut-être le contenu mis en cache. Appuyez sur Ctrl+F5 dans votre navigateur pour forcer le chargement de la réponse du serveur.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Notre petite &quot;&quot; de données (dans ce cas, le &quot;Hello World !&quot; message) est toutefois codée en dur. Notre application MVC a V (views) et nous disposons de C (Controllers), mais pas encore de M (Model). Nous allons bientôt vous montrer comment créer une base de données et récupérer des données de modèle.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passage de données du contrôleur vers la vue

Avant de passer à une base de données et de parler des modèles, nous allons tout d’abord parler de la transmission des informations du contrôleur à une vue. Nous souhaitons transmettre ce qu’un modèle de vue a besoin pour afficher une réponse HTML à un client. Ces objets sont généralement créés et passés par une classe de contrôleur à un modèle de vue, et ils doivent contenir uniquement les données dont le modèle de vue a besoin, et pas plus.

Précédemment avec la classe `HelloWorldController`, la méthode d’action `Welcome` prenait un `name` et un paramètre `numTimes`, puis en sortie les valeurs des paramètres dans le navigateur. Au lieu que le contrôleur continue à afficher cette réponse directement, nous allons placer ces données dans un conteneur pour la vue. Les contrôleurs et les vues peuvent utiliser un objet `ViewBag` pour stocker ces données. Qui sera transmis automatiquement à un modèle de vue et utilisé pour restituer la réponse HTML en utilisant le contenu du conteneur en tant que données. De cette façon, le contrôleur s’inquiète d’une chose et du modèle de vue avec un autre, ce qui nous permet de conserver une séparation &quot;claire des préoccupations&quot; au sein de l’application.

Vous pouvez également définir une classe personnalisée, puis créer une instance de cet objet, la remplir avec des données et la passer à la vue. C’est ce que l’on appelle souvent un ViewModel, car il s’agit d’un modèle personnalisé pour la vue. Toutefois, pour les petites quantités de données, ViewBag fonctionne très bien.

Revenir au fichier *HelloWorldController. vb* modifiez la méthode `Welcome` à l’intérieur du contrôleur pour placer le message et NumTimes dans ViewBag. ViewBag est un objet dynamique. Cela signifie que vous pouvez y placer tout ce que vous souhaitez. ViewBag n’a pas de propriétés définies tant que vous ne placez pas un contenu dans celui-ci.

`HelloWorldController.vb` complète avec la nouvelle classe dans le même fichier.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

À présent, ViewBag contient des données qui seront transmises automatiquement à la vue. Là encore, nous pourrions avoir passé notre propre objet comme celui-ci si nous l’avons aimé :

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Nous avons maintenant besoin d’un modèle de `WelcomeView` ! Exécutez l’application pour compiler le nouveau code. Fermez le navigateur, cliquez avec le bouton droit à l’intérieur de la méthode `Welcome`, puis cliquez sur **Ajouter une vue**.

Voici à quoi ressemble la boîte de dialogue **Ajouter une vue** .

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Ajoutez le code suivant sous l’élément `<h2>` dans la nouvelle <em>page d’accueil.</em> fichier vbhtml. Nous allons créer une boucle et indiquer &quot;Hello&quot; autant de fois que l’utilisateur dit que c’est le cas !

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Exécutez l’application et accédez à `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Désormais, les données sont extraites de l’URL et transmises automatiquement au contrôleur. Le contrôleur reporte les données dans un objet `Model` et transmet cet objet à la vue. Vue qui affiche les données au format HTML pour l’utilisateur.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Eh bien, il s’agit d’un type de &quot;M&quot; pour le modèle, mais pas le type de base de données. Créons une base de données de films en utilisant ce que nous avons appris.

> [!div class="step-by-step"]
> [Précédent](adding-a-controller.md)
> [Suivant](adding-a-model.md)
