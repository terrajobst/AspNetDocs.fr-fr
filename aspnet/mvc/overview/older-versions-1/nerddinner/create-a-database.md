---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Créer une base de données | Microsoft Docs
author: microsoft
description: L’étape 2 montre les étapes permettant de créer la base de données contenant toutes les données de dîner et RSVP pour notre application NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b0aa7c8cdf741f44e09ed18e2b2f73fe6bf786ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581003"
---
# <a name="create-a-database"></a>Créer une base de données

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 2 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.
> 
> L’étape 2 montre les étapes permettant de créer la base de données contenant toutes les données de dîner et RSVP pour notre application NerdDinner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner étape 2 : création de la base de données

Nous utiliserons une base de données pour stocker toutes les données de dîner et RSVP pour notre application NerdDinner.

Les étapes ci-dessous illustrent la création de la base de données à l’aide de l’édition gratuite SQL Server Express (que vous pouvez facilement installer à l’aide de V2 du [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)). Tout le code que nous allons écrire fonctionne avec SQL Server Express et la SQL Server complète.

### <a name="creating-a-new-sql-server-express-database"></a>Création d’une nouvelle base de données SQL Server Express

Nous allons commencer par cliquer avec le bouton droit sur notre projet Web, puis sélectionner la commande de menu **Ajouter-&gt;nouvel élément** :

![](create-a-database/_static/image1.png)

Cela permet d’afficher la boîte de dialogue « Ajouter un nouvel élément » de Visual Studio. Nous allons filtrer par la catégorie « données » et sélectionner le modèle d’élément « base de données SQL Server » :

![](create-a-database/_static/image2.png)

Nous allons nommer la base de données SQL Server Express que nous voulons créer « NerdDinner. mdf » et cliquer sur OK. Visual Studio vous demandera ensuite si nous souhaitons ajouter ce fichier à notre répertoire de données \App\_(qui est un répertoire déjà configuré avec des ACL de sécurité en lecture et en écriture) :

![](create-a-database/_static/image3.png)

Nous cliquons sur « Oui » et la nouvelle base de données sera créée et ajoutée à notre Explorateur de solutions :

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Création de tables au sein de notre base de données

Nous avons maintenant une nouvelle base de données vide. Nous allons y ajouter des tables.

Pour ce faire, nous allons accéder à la fenêtre « Explorateur de serveurs » de l’onglet dans Visual Studio, qui nous permet de gérer les serveurs et les bases de données. SQL Server Express bases de données stockées dans le dossier \App\_Data de notre application s’affichent automatiquement dans le Explorateur de serveurs. Vous pouvez également utiliser l’icône « se connecter à la base de données » en haut de la fenêtre « Explorateur de serveurs » pour ajouter d’autres bases de données SQL Server (à la fois locales et distantes) à la liste :

![](create-a-database/_static/image5.png)

Nous allons ajouter deux tables à notre base de données NerdDinner : une pour stocker nos dîners, et l’autre pour suivre les acceptations des RSVP. Nous pouvons créer des tables en cliquant avec le bouton droit sur le dossier « tables » dans notre base de données et en choisissant la commande de menu « Ajouter une nouvelle table » :

![](create-a-database/_static/image6.png)

Cela ouvre un concepteur de tables qui nous permet de configurer le schéma de notre table. Pour notre table « dîners », nous ajouterons 10 colonnes de données :

![](create-a-database/_static/image7.png)

Nous voulons que la colonne « DinnerID » soit une clé primaire unique pour la table. Pour ce faire, vous pouvez cliquer avec le bouton droit sur la colonne « DinnerID » et choisir l’élément de menu « définir la clé primaire » :

![](create-a-database/_static/image8.png)

En plus de faire de DinnerID une clé primaire, nous voulons également la configurer en tant que colonne « identity » dont la valeur est incrémentée automatiquement à mesure que de nouvelles lignes de données sont ajoutées à la table (ce qui signifie que la première ligne de dîner insérée aura un DinnerID de 1, la deuxième ligne insérée aura un DinnerID de 2, etc.).

Pour ce faire, vous pouvez sélectionner la colonne « DinnerID », puis utiliser l’éditeur de propriétés de colonne pour définir la propriété « (is Identity) » sur la colonne sur « Oui ». Nous utiliserons les valeurs par défaut de l’identité standard (à partir de 1 et incrément 1 sur chaque nouvelle ligne de dîner) :

![](create-a-database/_static/image9.png)

Nous allons ensuite enregistrer notre table en tapant Ctrl-S ou à l’aide de la commande **de menu fichier-&gt;enregistrer** . Cela nous invite à nommer la table. Nous allons le nommer « dîners » :

![](create-a-database/_static/image10.png)

Notre nouveau tableau des dîners s’affiche alors dans la base de données de l’Explorateur de serveurs.

Nous répéterons ensuite les étapes ci-dessus et créons une table « RSVP ». Cette table avec 3 colonnes. Nous allons configurer la colonne RsvpID comme clé primaire et en faire une colonne d’identité :

![](create-a-database/_static/image11.png)

Nous allons l’enregistrer et lui attribuer le nom « RSVP ».

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Configuration d’une relation de clé étrangère entre des tables

Nous avons maintenant deux tables au sein de notre base de données. Notre dernière étape de conception de schéma consiste à configurer une relation « un-à-plusieurs » entre ces deux tables, afin que nous puissions associer chaque ligne de dîner à zéro, une ou plusieurs lignes RSVP qui s’y appliquent. Pour ce faire, nous allons configurer la colonne « DinnerID » de la table RSVP de façon à ce qu’elle ait une relation de clé étrangère avec la colonne « DinnerID » de la table « dîners ».

Pour ce faire, nous allons ouvrir la table RSVP dans le concepteur de tables en double-cliquant dessus dans l’Explorateur de serveurs. Nous allons ensuite sélectionner la colonne « DinnerID » dans celle-ci, cliquer avec le bouton droit et sélectionner « relations... ». commande de menu contextuel :

![](create-a-database/_static/image12.png)

Cela affiche une boîte de dialogue que nous pouvons utiliser pour configurer les relations entre les tables :

![](create-a-database/_static/image13.png)

Nous allons cliquer sur le bouton « Ajouter » pour ajouter une nouvelle relation à la boîte de dialogue. Une fois qu’une relation a été ajoutée, nous allons développer le nœud de vue d’arborescence « tables et spécification de colonne » dans la grille des propriétés, à droite de la boîte de dialogue, puis cliquer sur le bouton « ... » à droite du bouton :

![](create-a-database/_static/image14.png)

En cliquant sur le bouton « ... » affichera une autre boîte de dialogue qui nous permet de spécifier les tables et les colonnes impliquées dans la relation, et de nous permettre de nommer la relation.

Nous allons modifier la table de clé primaire en « dîners », puis sélectionner la colonne « DinnerID » dans la table des dîners comme clé primaire. Notre table RSVP est la table de clé étrangère et le RSVP. La colonne DinnerID sera associée en tant que clé étrangère :

![](create-a-database/_static/image15.png)

À présent, chaque ligne de la table RSVP sera associée à une ligne de la table dinner. SQL Server conserve l’intégrité référentielle pour nous, et nous empêche d’ajouter une nouvelle ligne RSVP si elle ne pointe pas vers une ligne de dîner valide. Cela nous empêchera également de supprimer une ligne dîner si des lignes RSVP y font référence.

### <a name="adding-data-to-our-tables"></a>Ajout de données à nos tables

Commençons par ajouter des exemples de données à notre table dîners. Nous pouvons ajouter des données à une table en cliquant dessus avec le bouton droit dans le Explorateur de serveurs et en choisissant la commande « Afficher les données de la table » :

![](create-a-database/_static/image16.png)

Nous allons ajouter quelques lignes de données de dîner que nous pouvons utiliser ultérieurement lorsque nous commencerons à implémenter l’application :

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Étape suivante

Nous avons terminé la création de notre base de données. Créons maintenant des classes de modèle que nous pouvons utiliser pour interroger et mettre à jour.

> [!div class="step-by-step"]
> [Précédent](create-a-new-aspnet-mvc-project.md)
> [Suivant](build-a-model-with-business-rule-validations.md)
