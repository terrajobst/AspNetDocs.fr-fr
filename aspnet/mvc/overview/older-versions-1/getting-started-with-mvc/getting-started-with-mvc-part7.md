---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Ajout de la validation au modèle | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Créer une application Web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 9403be574324c34edf93bef1e0e4fd7ba68a3a9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543644"
---
# <a name="adding-validation-to-the-model"></a>Ajout de la validation au modèle

par [Scott Hanselman](https://github.com/shanselman)

> Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données. Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.

Dans cette section, nous allons implémenter la prise en charge nécessaire pour activer la validation des entrées dans notre application. Nous garantissons que notre contenu de base de données est toujours correct et fournissez des messages d’erreur utiles aux utilisateurs finaux lorsqu’ils essaient d’entrer des données de film qui ne sont pas valides. Nous allons commencer par ajouter une petite logique de validation à la classe Movie.

Cliquez avec le bouton droit sur le dossier du modèle et sélectionnez Ajouter une classe. Nommez votre classe Movie.

Lorsque nous avons créé le modèle d’entité Movie précédemment, l’IDE a créé une classe Movie. En fait, une partie de la classe Movie peut figurer dans un fichier et faire partie d’une autre. C’est ce que l’on appelle une classe partielle. Nous allons étendre la classe Movie à partir d’un autre fichier.

Nous allons créer une classe de film partielle qui pointe vers une « classe associée » avec des attributs qui fourniront des indications de validation au système. Nous allons marquer le titre et le prix comme requis et insister sur le prix dans une certaine plage. Cliquez avec le bouton droit sur le dossier modèles, puis sélectionnez Ajouter une classe. Nommez votre film de classe, puis cliquez sur le bouton OK. Voici à quoi ressemble la classe de films partiels.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Réexécutez votre application et essayez d’entrer un film dont le prix est supérieur à 100. Une erreur s’affiche une fois que vous avez envoyé le formulaire. L’erreur est interceptée côté serveur et se produit après la publication du formulaire. Notez que les applications d’assistance HTML intégrées à ASP.NET MVC étaient suffisamment intelligentes pour afficher le message d’erreur et maintenir les valeurs pour nous dans les éléments de la zone de texte :

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Cela fonctionne bien, mais il s’agit d’une bonne chose si nous pouvions dire à l’utilisateur côté client, immédiatement avant que le serveur ne soit impliqué.

Nous allons activer la validation côté client avec JavaScript.

## <a name="adding-client-side-validation"></a>Ajout de la validation côté client

Étant donné que notre classe Movie a déjà des attributs de validation, nous devons simplement ajouter quelques fichiers JavaScript à notre modèle de vue Create. aspx et ajouter une ligne de code pour permettre la validation côté client.

Dans VWD existant, accédez à notre dossier views/Movie et ouvrez Create. aspx.

Ouvrez le dossier scripts dans le Explorateur de solutions, puis faites glisser les trois scripts suivants dans la balise de&gt; &lt;Head.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Vous souhaitez que ces fichiers de script s’affichent dans cet ordre.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Ajoutez également cette seule ligne au-dessus du fichier html. BeginForm :

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Voici le code affiché dans l’IDE.

[Films ![-Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Exécutez votre application et visitez à nouveau/Movies/Create, puis cliquez sur créer sans entrer de données. Les messages d’erreur s’affichent immédiatement sans le flash de page que nous associons à l’envoi de données vers le serveur. Cela est dû au fait que ASP.NET MVC valide à présent l’entrée à la fois sur le client (à l’aide de JavaScript) et sur le serveur.

[![créer-Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

C’est parfait ! Nous allons maintenant ajouter une colonne supplémentaire à la base de données.

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part6.md)
> [Suivant](getting-started-with-mvc-part8.md)
