---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Ajout d’une vue | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Créer une application Web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 462b1210c45da67058899193afcea973f3daf122
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581556"
---
# <a name="adding-a-view"></a>Ajout d’une vue

par [Scott Hanselman](https://github.com/shanselman)

> Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données. Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.

Dans cette section, nous allons nous pencher sur la façon dont nous pouvons faire en sorte que notre classe HelloWorldController utilise un fichier de modèle de vue pour encapsuler correctement la génération de réponses HTML sur un client.

Commençons par utiliser un modèle de vue avec notre méthode d’index. Notre méthode est appelée index et se trouve dans le HelloWorldController. Actuellement, notre méthode index () retourne une chaîne avec un message codé en dur dans la classe Controller.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Nous allons maintenant modifier la méthode d’index pour qu’elle ressemble à ceci :

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Nous allons maintenant ajouter un modèle de vue à notre projet que nous pouvons utiliser pour notre méthode index (). Pour ce faire, cliquez avec le bouton droit avec la souris quelque part au milieu de la méthode d’index, puis cliquez sur Ajouter une vue...

![image](getting-started-with-mvc-part3/_static/image1.png)

Cela permet d’afficher la boîte de dialogue « Ajouter une vue » qui fournit des options pour la création d’un modèle de vue utilisable par notre méthode d’index. Pour le moment, ne modifiez rien, puis cliquez simplement sur le bouton Ajouter.

[![boîte de dialogue Ajouter une vue](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Une fois que vous avez cliqué sur Ajouter, un nouveau dossier et un nouveau fichier s’affichent dans le dossier de la solution, comme indiqué ici. Je dispose maintenant d’un dossier HelloWorld sous vues, et d’un fichier index. aspx à l’intérieur de ce dossier.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Le nouveau fichier d’index est également déjà ouvert et prêt pour la modification. Ajoutez du texte sous le premier index &lt;H2&gt;&lt;/H2&gt; comme « Hello World ».

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Exécutez votre application et reportez-vous à [`http://localhost:xx/HelloWorld`](http://localhostxx) dans votre navigateur. La méthode index de notre contrôleur dans cet exemple n’a pas effectué de travail, mais a appelé « Return View () », ce qui indiquait que nous souhaitions utiliser un fichier de modèle de vue pour restituer une réponse au client. Étant donné que nous n’avons pas spécifié explicitement le nom du fichier de modèle de vue à utiliser, ASP.NET MVC utilise par défaut le fichier de vue index. aspx dans le dossier \Views\HelloWorld Nous voyons maintenant la chaîne que nous avons codée en dur dans notre vue.

[Index ![-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Semble assez bon. Toutefois, Notez que le titre du navigateur indique « index » et que le gros titre sur la page indique « mon application MVC ». Modifions-les.

### <a name="changing-views-and-master-pages"></a>Modification des vues et des pages maîtres

Tout d’abord, nous allons modifier le texte « mon application MVC ». Ce texte est partagé et apparaît sur chaque page. Il n’apparaît en fait qu’à un seul emplacement dans notre code, même s’il se trouve sur chaque page de l’application. Accédez au dossier/Views/Shared dans le Explorateur de solutions et ouvrez le fichier site. Master. Ce fichier s’appelle une page maître et il s’agit de l’interpréteur de commandes partagé que toutes les autres pages utilisent.

Notez le texte qui indique ContentPlaceholder « MainContent » dans ce fichier.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Il s’agit de l’emplacement où toutes les pages que vous créez s’affichent, « encapsulées » dans la page maître. Essayez de modifier le titre, puis exécutez votre application et visitez plusieurs pages. Vous remarquerez que votre modification apparaît sur plusieurs pages.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

À présent, chaque page aura le titre principal, qui est H1-de « mon application de vidéo MVC ». Qui gère le texte blanc en haut, qui est partagé entre toutes les pages.

Voici le site. Master dans son intégralité avec notre titre modifié :

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

À présent, nous allons modifier le titre de la page d’index.

Ouvrir/HelloWorld/Index.aspx. Il y a deux emplacements à modifier. Tout d’abord, le titre qui s’affiche dans le titre du navigateur, puis l’en-tête secondaire, qui est également H2. Je les rends légèrement différents afin que vous puissiez voir le bit de modification du code qui fait partie de l’application.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Exécutez votre application et visitez/movies. Notez que le titre du navigateur, l’en-tête principal et les en-têtes secondaires ont changé. Il est facile d’apporter des modifications importantes à votre application avec des modifications mineures de votre affichage.

[Liste des films ![-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Notre petit peu de « données » (dans le cas présent, le « Hello World ! » message) a bien été codé en dur. Nous disposons de V (vues) et nous avons des C (contrôleurs), mais pas encore M (Model). Nous allons bientôt découvrir comment créer une base de données et récupérer des données de modèle à partir de celle-ci.

## <a name="passing-a-viewmodel"></a>Passage d’un ViewModel

Avant de passer à une base de données et de parler des modèles, nous allons tout d’abord parler de « ViewModels ». Il s’agit d’objets qui représentent les éléments nécessaires à un modèle de vue pour restituer une réponse HTML à un client. Ils sont généralement créés et passés par une classe de contrôleur à un modèle de vue, et doivent contenir uniquement les données requises par le modèle de vue, et pas plus.

Précédemment avec notre exemple HelloWorld, notre méthode d’action Welcome () acceptait un nom et un paramètre numTimes et l’a généré dans le navigateur. Plutôt que de faire en sorte que le contrôleur continue d’afficher cette réponse directement, nous allons créer une petite classe pour stocker ces données, puis les transmettre à un modèle de vue pour afficher la réponse HTML à l’aide de celle-ci. De cette façon, le contrôleur s’inquiète d’une chose et du modèle de vue, ce qui nous permet de conserver une « séparation des préoccupations » propre dans notre application.

Revenez au fichier HelloWorldController.cs et ajoutez une nouvelle classe « WelcomeViewModel » et modifiez la méthode de bienvenue à l’intérieur de votre contrôleur. Voici le HelloWorldController.cs complet avec la nouvelle classe dans le même fichier.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Même s’il se trouve sur plusieurs lignes, notre méthode de bienvenue est en fait uniquement deux instructions de code. La première instruction reporte les deux paramètres dans un objet ViewModel, tandis que la deuxième passe l’objet résultant sur la vue.

Nous avons maintenant besoin d’un modèle de vue de bienvenue ! Cliquez avec le bouton droit dans la méthode Welcome et sélectionnez Ajouter une vue. Cette fois-ci, nous allons cocher « créer une vue fortement typée » et sélectionner notre classe WelcomeViewModel dans la liste déroulante. Cette nouvelle vue ne connaît que WelcomeViewModels et aucun autre type d’objet.

> *Remarque : vous devez avoir compilé une seule fois après avoir ajouté votre WelcomeViewModel pour pour qu’il s’affiche dans la liste déroulante.*

Voici à quoi doit ressembler votre boîte de dialogue Ajouter une vue. Cliquez sur le bouton Ajouter. ![Ajouter une vue cerclée](getting-started-with-mvc-part3/_static/image10.png)

Ajoutez ce code sous le &lt;H2&gt; dans votre nouveau welcome. aspx. Nous allons créer une boucle et disons Hello autant de fois que l’utilisateur dit que c’est le cas.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Notez également que lorsque vous tapez, parce que nous avons dit cette vue sur le WelcomeViewModel (ils sont mariés, n’oubliez pas ?) que nous obtenons IntelliSense utiles chaque fois que nous référençons notre objet de modèle comme indiqué dans la capture d’écran ci-dessous :

[![le code source NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Exécutez votre application et reportez-vous à `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`. Maintenant que nous prenons les données de l’URL, elles sont automatiquement transmises à notre contrôleur, notre contrôleur redirige les données dans un ViewModel et transmet cet objet dans notre vue. Vue qui affiche les données au format HTML pour l’utilisateur.

[![d’accueil-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Eh bien, il s’agissait d’un type de « M » pour le modèle, mais pas du type de base de données. Prenons ce que nous avons appris et créons une base de données de films.

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part2.md)
> [Suivant](getting-started-with-mvc-part4.md)
