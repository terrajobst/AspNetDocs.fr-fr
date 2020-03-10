---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Ajout d’une colonne au modèle | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Créer une application Web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 1cf092c3db3959d6f47006f1be2ba82833c5dc06
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543581"
---
# <a name="adding-a-column-to-the-model"></a>Ajout d’une colonne au modèle

par [Scott Hanselman](https://github.com/shanselman)

> Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données. Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.

Dans cette section, nous allons découvrir comment nous pouvons apporter des modifications au schéma de notre base de données et gérer les modifications au sein de notre application.

Nous allons ajouter une colonne « Rating » à la table Movie. Revenez à l’IDE, puis cliquez sur le explorateur de base de données. Cliquez avec le bouton droit sur la table Movie et sélectionnez Ouvrir la définition de table.

Ajoutez une colonne « Rating » comme indiqué ci-dessous. Étant donné que nous n’avons pas de classification, la colonne peut accepter les valeurs NULL. Cliquez sur Enregistrer.

[Tableau des films de modification ![](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Ensuite, revenez au Explorateur de solutions et ouvrez le fichier movies. edmx (qui se trouve dans le dossier \Models). Cliquez avec le bouton droit sur l’aire de conception (zone blanche) et sélectionnez mettre à jour le modèle à partir de la base de données.

[Films ![-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Cette opération lance l' « Assistant Mise à jour ». Cliquez sur l’onglet actualiser dans celui-ci, puis sur Terminer. Notre classe Movie Model sera ensuite mise à jour avec la nouvelle colonne.

![Assistant Mise à jour (2)](getting-started-with-mvc-part8/_static/image5.png)

Après avoir cliqué sur terminer, vous pouvez voir que la nouvelle colonne évaluation a été ajoutée à l’entité film dans notre modèle.

[Entité film ![](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Nous avons ajouté une colonne dans le modèle de base de données, mais les vues ne le savent pas.

## <a name="update-views-with-model-changes"></a>Mettre à jour les vues avec des modifications de modèle

Il existe plusieurs façons de mettre à jour nos modèles de vue pour refléter la nouvelle colonne d’évaluation. Étant donné que nous avons créé ces vues en les générant via la boîte de dialogue Ajouter une vue, nous pourrions les supprimer et les recréer. Toutefois, en général, les utilisateurs auront déjà apporté des modifications à leurs modèles de vue à partir de la génération générée automatiquement et voudront ajouter ou supprimer des champs manuellement, comme nous l’avons fait avec le champ ID pour Create.

Ouvrez le modèle \Views\Movies\Index.aspx et ajoutez un &lt;&gt;évaluation&lt;/th&gt; à l’en-tête de la table Movie. J’ai ajouté la mine après le genre. Ensuite, dans la même position de colonne mais en bas, ajoutez une ligne pour sortir notre nouvelle évaluation.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Notre dernier modèle index. aspx ressemble à ce qui suit :

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Nous allons ensuite ouvrir le modèle \Views\Movies\Create.aspx et ajouter une étiquette et une zone de texte pour notre nouvelle propriété d’évaluation :

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Notre dernier modèle Create. aspx ressemblera à ce qui suit, et nous allons modifier le titre de votre navigateur et le nom secondaire &lt;H2&gt; titre comme « créer un film », alors que nous sommes ici !

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Exécutez votre application. à présent, vous disposez d’un nouveau champ dans la base de données qui a été ajouté à la page créer. Ajoutez un nouveau film-cette fois avec une évaluation, puis cliquez sur créer.

[![créer un film-Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Une fois que vous avez cliqué sur créer, vous êtes dirigé vers la page d’index dans laquelle le nouveau film est listé avec la nouvelle colonne d’évaluation de la base de données.

[Liste des films ![-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Ce didacticiel de base vous a fait commencer à créer des contrôleurs, à les associer à des vues et à transmettre des données codées en dur. Ensuite, nous avons créé et conçu une base de données et nous y avons mis des données. Nous avons extrait les données de la base de données et les avons affichées dans une table HTML. Ensuite, nous avons ajouté un formulaire créer qui permet à l’utilisateur d’ajouter des données à la base de données à partir de l’application Web. Nous avons ajouté une validation, puis effectué la validation à l’aide de JavaScript côté client. Enfin, nous avons modifié la base de données pour inclure une nouvelle colonne de données, puis mis à jour nos deux pages pour créer et afficher ces nouvelles données.

Je vous encourage à présent à passer à notre didacticiel de niveau intermédiaire «[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)», ainsi qu’aux nombreuses vidéos et ressources de [https://asp.net/mvc](https://asp.net/mvc) pour en savoir plus sur ASP.NET MVC !

Bonne lecture !

- Scott Hanselman- [http://hanselman.com](http://hanselman.com) et [@shanselman](http://twitter.com/shanselman) sur Twitter.

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part7.md)
