---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Création d’une base de données | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Créer une application Web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 995db714ce6365415d06dc458aee84a31c7c8fd6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581437"
---
# <a name="creating-a-database"></a>Création d’une base de données

par [Scott Hanselman](https://github.com/shanselman)

> Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données. Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.

Dans cette section, nous allons créer une nouvelle base de données SQL Express que nous allons utiliser pour stocker et récupérer les données de nos films. Dans l’environnement de développement intégré (IDE) de Visual Web Developer, sélectionnez Afficher | Explorateur de serveurs. Cliquez avec le bouton droit sur connexions de données, puis cliquez sur Ajouter une connexion...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

Dans la boîte de dialogue Choisir la source de données, sélectionnez Microsoft SQL Server, puis sélectionnez Continuer.

![](getting-started-with-mvc-part4/_static/image2.png)

Dans la boîte de dialogue Ajouter une connexion, entrez « .\SQLEXPRESS » comme nom de serveur, puis entrez « films » comme nom de votre nouvelle base de données.

[![boîte de dialogue Ajouter une connexion](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Cliquez sur OK. il vous sera demandé si vous souhaitez créer cette base de données. Sélectionnez Oui.

[![créer des films ?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Vous disposez maintenant d’une base de données vide dans Explorateur de serveurs.

![Ajouter une nouvelle table](getting-started-with-mvc-part4/_static/image7.png)

Cliquez avec le bouton droit sur tables, puis cliquez sur Ajouter une table. Le Concepteur de tables s’affiche. Ajoutez des colonnes pour ID, title, Released, genre et Price. Cliquez avec le bouton droit sur la colonne ID, puis cliquez sur définir la clé primaire. Voici à quoi ressemblent mes zones de conception.

[Éditeur de table de base de données ![](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

En outre, sélectionnez la colonne ID et, sous propriétés de la colonne sous, modifiez « spécification d’identité » en « Oui ».

[![IsIdentity-propriétés de colonne](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Une fois que vous avez terminé, cliquez sur l’icône Enregistrer dans la barre d’outils ou sélectionnez fichier | Enregistrez dans le menu et nommez votre table «**Movie**» (singulier). Nous disposons d’une base de données et d’une table !

[![choisir un nom](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Revenez à Explorateur de serveurs et cliquez avec le bouton droit sur la table Movie, puis sélectionnez « Afficher les données de la table ». Entrez quelques films pour que notre base de données comporte des données.

[Modification des tables de base de données ![](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Création d’un modèle

À présent, revenez au Explorateur de solutions sur le côté droit de l’IDE et cliquez avec le bouton droit sur le dossier Models, puis sélectionnez Ajouter | Nouvel élément.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Nous allons créer un modèle d’entité à partir de notre nouvelle base de données. Cela permet d’ajouter un ensemble de classes à notre projet qui facilite l’interrogation et la manipulation des données dans notre base de données. Sélectionnez le nœud de données dans la partie gauche de la boîte de dialogue, puis sélectionnez le modèle d’élément ADO.NET Entity Data Model. Nommez-le movies. edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Cliquez sur le bouton « Ajouter ». Cette opération lance ensuite l' « Assistant Entity Data Model ».

Dans la boîte de dialogue Nouveau qui s’affiche, sélectionnez générer à partir de la base de données. Étant donné que nous venons de créer une base de données, nous devons uniquement indiquer à la Entity Framework à propos de notre nouvelle base de données et de sa table. Cliquez sur suivant pour enregistrer la connexion à la base de données dans la configuration de l’application Web. Maintenant, activez la case à cocher tables et films, puis cliquez sur Terminer.

[Assistant Entity Data Model ![](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Nous pouvons maintenant voir notre nouvelle table Movie dans le Entity Framework Designer et y accéder à partir du code.

[Films ![-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Sur l’aire de conception, vous pouvez voir une classe « film ». Cette classe est mappée à la table « Movie » de notre base de données, et chaque propriété qu’elle contient est mappée à une colonne avec la table. Chaque instance d’une classe « Movie » correspond à une ligne dans la table « Movie ».

Si vous n’aimez pas les conventions de nommage et de mappage par défaut utilisées par le Entity Framework, vous pouvez utiliser le concepteur de Entity Framework pour les modifier ou les personnaliser. Pour cette application, nous allons utiliser les valeurs par défaut et enregistrer simplement le fichier tel quel.

Nous allons maintenant travailler avec des données réelles !

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part3.md)
> [Suivant](getting-started-with-mvc-part5.md)
