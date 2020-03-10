---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Utiliser Migrations Code First pour amorcer la base de données | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557455"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a>Utiliser Migrations Code First pour amorcer la base de données

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez utiliser [migrations code First](https://msdn.microsoft.com/data/jj591621) dans EF pour amorcer la base de données avec des données de test.

Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :

[!code-console[Main](part-3/samples/sample1.cmd)]

Cette commande ajoute un dossier nommé migrations à votre projet, ainsi qu’un fichier de code nommé Configuration.cs dans le dossier migrations.

![](part-3/_static/image1.png)

Ouvrez le fichier Configuration.cs. Ajoutez l’instruction **using** suivante.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Ajoutez ensuite le code suivant à la méthode **Configuration. Seed** :

[!code-csharp[Main](part-3/samples/sample3.cs)]

Dans la fenêtre console du gestionnaire de package, tapez les commandes suivantes :

[!code-console[Main](part-3/samples/sample4.cmd)]

La première commande génère du code qui crée la base de données, et la deuxième commande exécute ce code. La base de données est créée localement [à l'](https://msdn.microsoft.com/library/hh510202.aspx)aide de la base de données locale.

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Explorez l’API (facultatif)

Appuyez sur F5 pour exécuter l'application en mode débogage. Visual Studio démarre IIS Express et exécute votre application Web. Visual Studio lance ensuite un navigateur et ouvre la page d’hébergement de l’application.

Lorsque Visual Studio exécute un projet Web, il attribue un numéro de port. Dans l’image ci-dessous, le numéro de port est 50524. Lorsque vous exécutez l’application, vous verrez un numéro de port différent.

![](part-3/_static/image3.png)

La page d’hébergement est implémentée à l’aide de ASP.NET MVC. En haut de la page se trouve un lien indiquant « API ». Ce lien vous amène à une page d’aide générée automatiquement pour l’API Web. (Pour savoir comment cette page d’aide est générée et comment vous pouvez ajouter votre propre documentation à la page, consultez [création de pages d’aide pour API Web ASP.net](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Vous pouvez cliquer sur la page d’aide Liens pour afficher des détails sur l’API, y compris le format de la demande et de la réponse.

![](part-3/_static/image4.png)

L’API permet les opérations CRUD sur la base de données. Voici un résumé de l’API.

| Auteurs |  |
| --- | -- |
| GET api/authors | Récupération de tous les auteurs. |
| GET api/authors/{id} | Obtient un auteur par ID. |
| POST /api/authors | Créez un auteur. |
| PUT /api/authors/{id} | Mettre à jour un auteur existant. |
| DELETE /api/authors/{id} | Supprimer un auteur. |

| Livres |  |
| --- | -- |
| GET /api/books | Récupération de tous les livres. |
| GET /api/books/{id} | Obtenir un livre par ID. |
| POST /api/books | Créez un livre. |
| PUT /api/books/{id} | Mettre à jour un livre existant. |
| DELETE /api/books/{id} | Supprimer un livre. |

## <a name="view-the-database-optional"></a>Afficher la base de données (facultatif)

Lorsque vous avez exécuté la commande Update-Database, EF a créé la base de données et a appelé la méthode `Seed`. Lorsque vous exécutez l’application localement, [EF utilise la](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)base de données locale. Vous pouvez afficher la base de données dans Visual Studio. Dans le menu **Vue**, sélectionnez **Explorateur d’objets SQL Server**.

![](part-3/_static/image5.png)

Dans la boîte de dialogue **se connecter au serveur** , dans la zone de texte **nom du serveur** , tapez « (base de données locale) \v11.0 ». Laissez l’option **d’authentification** « authentification Windows ». Cliquez sur **Connexion**.

![](part-3/_static/image6.png)

Visual Studio se connecte à la base de données locale et affiche vos bases de données existantes dans la fenêtre de Explorateur d’objets SQL Server. Vous pouvez développer les nœuds pour afficher les tables que EF a créées.

![](part-3/_static/image7.png)

Pour afficher les données, cliquez avec le bouton droit sur une table, puis sélectionnez **afficher les données**.

![](part-3/_static/image8.png)

La capture d’écran suivante montre les résultats de la table Books. Notez que EF a rempli la base de données avec les données de départ et que la table contient la clé étrangère à la table authors.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Précédent](part-2.md)
> [Suivant](part-4.md)
