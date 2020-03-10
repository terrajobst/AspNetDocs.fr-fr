---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Accès aux données de votre modèle à partir d’un contrôleur | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Créer une application Web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 207ed880977d794d81efdc1ea458d17a68d501d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543749"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Accès aux données de votre modèle à partir d’un contrôleur

par [Scott Hanselman](https://github.com/shanselman)

> Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données. Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.

Dans cette section, nous allons créer une nouvelle classe MoviesController et écrire du code qui récupère nos données de film et les affiche à nouveau dans le navigateur à l’aide d’un modèle de vue.

Cliquez avec le bouton droit sur le dossier Controllers et créez un nouveau MoviesController.

[![ajouter un contrôleur](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Cela créera un nouveau fichier « MoviesController.cs » sous notre dossier \Controllers au sein de notre projet. Nous allons mettre à jour MovieController pour récupérer la liste des films à partir de notre base de données qui vient d’être remplie.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Nous effectuons une requête LINQ pour que nous puissions uniquement récupérer les films publiés après l’été de 1984. Nous aurons besoin d’un modèle de vue pour restituer cette liste de films. par conséquent, cliquez avec le bouton droit dans la méthode, puis sélectionnez Ajouter une vue pour la créer.

Dans la boîte de dialogue Ajouter une vue, nous indiquons que nous transmettons une liste&lt;movies. Models. Movie&gt; à notre modèle de vue. Contrairement aux heures précédentes, nous avons utilisé la boîte de dialogue Ajouter une vue et choisi de créer un modèle « vide ». cette fois-ci, nous indiquons que Visual Studio doit automatiquement « échafauder » un modèle de vue pour nous avec un contenu par défaut. Pour ce faire, nous allons sélectionner l’élément « liste » dans le menu déroulant Afficher le contenu.

N’oubliez pas qu’une fois que vous avez créé une nouvelle classe, vous devez compiler votre application pour qu’elle s’affiche dans la boîte de dialogue Ajouter une vue.

![Ajouter une vue](getting-started-with-mvc-part5/_static/image3.png)

Cliquez sur Ajouter pour que le système génère automatiquement le code pour une vue pour nous qui affiche notre liste de films. C’est le moment idéal pour modifier l’en-tête &lt;H2&gt; par un texte tel que « ma liste de films » comme nous l’avons fait précédemment avec la vue de Hello World.

[Films ![-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Exécutez votre application et visitez/movies dans la barre d’adresses. À présent, nous avons extrait les données de la base de données à l’aide d’une requête de base dans le contrôleur et j’ai renvoyé les données à une vue qui connaît les films. Cette vue tourne ensuite dans la liste des films et crée une table de données pour nous.

[Liste des films ![-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Nous n’implémenterons pas les fonctionnalités de modification, de détails et de suppression avec cette application. nous n’avons donc pas besoin des liens par défaut créés pour nous. Ouvrez le fichier/Movies/Index.aspx et supprimez-le.

Voici le code source de ce à quoi doit ressembler notre modèle de vue mis à jour une fois que nous apportons ces modifications :

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Il crée des liens dont nous n’aurons pas besoin. nous allons donc les supprimer pour cet exemple. Nous allons conserver notre lien créer, car c’est le cas. Voici à quoi ressemble votre application avec cette colonne supprimée.

[Liste des films ![-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Nous disposons à présent d’une simple liste de nos données de films. Toutefois, si nous cliquons sur le lien « créer nouveau », nous obtenons une erreur, car elle n’est pas raccrochée. Nous allons implémenter une méthode de création d’action et permettre à un utilisateur d’entrer de nouveaux films dans notre base de données.

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part4.md)
> [Suivant](getting-started-with-mvc-part6.md)
