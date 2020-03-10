---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: 'Didacticiel : prise en main d’EF Database First à l’aide de MVC 5'
description: Ce didacticiel montre comment démarrer avec une base de données existante et créer rapidement une application Web qui permet aux utilisateurs d’interagir avec les données.
author: Rick-Anderson
ms.author: riande
ms.date: 01/15/2019
ms.topic: tutorial
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: a760767839a834a9c7e9fe358a3fd806a833261f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583523"
---
# <a name="tutorial-get-started-with-ef-database-first-using-mvc-5"></a>Didacticiel : prise en main d’EF Database First à l’aide de MVC 5

À l’aide de la génération de modèles automatique MVC, Entity Framework et ASP.NET, vous pouvez créer une application Web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer automatiquement du code qui permet aux utilisateurs d’afficher, de modifier, de créer et de supprimer des données qui se trouvent dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données. Dans la dernière partie de la série, vous allez apprendre à ajouter des annotations de données au modèle de données pour spécifier les exigences de validation et la mise en forme d’affichage. Lorsque vous avez terminé, vous pouvez passer à un article Azure pour apprendre à déployer une application .NET et une base de données SQL dans Azure App Service.

Ce didacticiel montre comment démarrer avec une base de données existante et créer rapidement une application Web qui permet aux utilisateurs d’interagir avec les données. Il utilise les Entity Framework 6 et MVC 5 pour créer l’application Web. La fonctionnalité de génération de modèles automatique ASP.NET vous permet de générer automatiquement du code pour afficher, mettre à jour, créer et supprimer des données. À l’aide des outils de publication dans Visual Studio, vous pouvez facilement déployer le site et la base de données dans Azure.

Cette partie de la série se concentre sur la création d’une base de données et son remplissage avec des données.

Cette série a été écrite avec les contributions de Tom Dykstra et Rick Anderson. Il a été amélioré en fonction des commentaires des utilisateurs figurant dans la section commentaires.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Configurer la base de données

## <a name="prerequisites"></a>Conditions préalables requises

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)

## <a name="set-up-the-database"></a>Configurer la base de données

Pour imiter l’environnement d’une base de données existante, vous allez d’abord créer une base de données avec des données pré-remplies, puis créer votre application Web qui se connecte à la base de données.

Ce didacticiel a été développé à l’aide de la base de données locale avec Visual Studio 2017. Vous pouvez utiliser un serveur de base de données existant au lieu de la base de données locale, mais en fonction de votre version de Visual Studio et de votre type de base de données, tous les outils de données de Visual Studio peuvent ne pas être pris en charge. Si les outils ne sont pas disponibles pour votre base de données, vous devrez peut-être effectuer certaines des étapes spécifiques à la base de données dans la suite de gestion de votre base de données.

Si vous rencontrez un problème avec les outils de base de données dans votre version de Visual Studio, vérifiez que vous avez installé la dernière version des outils de base de données. Pour plus d’informations sur la mise à jour ou l’installation des outils de base de données, consultez [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Lancez Visual Studio et créez un **projet de base de données SQL Server**. Nommez le projet **ContosoUniversityData**.

![créer un projet de base de données](setting-up-database/_static/image1.png)

Vous disposez maintenant d’un projet de base de données vide. Pour vous assurer que vous pouvez déployer cette base de données sur Azure, vous devez définir Azure SQL Database comme plateforme cible pour le projet. La définition de la plateforme cible ne déploie pas réellement la base de données ; Cela signifie uniquement que le projet de base de données doit vérifier que la conception de la base de données est compatible avec la plateforme cible. Pour définir la plateforme cible, ouvrez les **Propriétés** du projet et sélectionnez **Microsoft Azure SQL Database** pour la plateforme cible.

Vous pouvez créer les tables nécessaires pour ce didacticiel en ajoutant des scripts SQL qui définissent les tables. Cliquez avec le bouton droit sur votre projet et ajoutez un nouvel élément. Sélectionnez **tables et vues** > **table** et nommez-la *Student*.

Dans le fichier de table, remplacez la commande T-SQL par le code suivant pour créer la table.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Notez que la fenêtre de conception se synchronise automatiquement avec le code. Vous pouvez utiliser le code ou le concepteur.

![afficher le code et la conception](setting-up-database/_static/image5.png)

Ajoutez une autre table. Cette fois-ci, nommez-le course et utilisez la commande T-SQL suivante.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

Et recommencez une fois pour créer une table nommée inscription.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Vous pouvez remplir votre base de données avec des données à l’aide d’un script qui s’exécute après le déploiement de la base de données. Ajoutez un script de publication au projet. Cliquez avec le bouton droit sur votre projet et ajoutez un nouvel élément. Sélectionnez **scripts utilisateur** > **script de publication**. Vous pouvez utiliser le nom par défaut.

Ajoutez le code T-SQL suivant au script de publication. Ce script ajoute simplement des données à la base de données lorsqu’aucun enregistrement correspondant n’est trouvé. Il ne remplace pas les données que vous avez entrées dans la base de données.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Il est important de noter que le script de publication est exécuté chaque fois que vous déployez votre projet de base de données. Par conséquent, vous devez prendre en compte soigneusement vos besoins lors de l’écriture de ce script. Dans certains cas, vous souhaiterez peut-être recommencer à partir d’un jeu de données connu chaque fois que le projet est déployé. Dans d’autres cas, vous ne souhaiterez peut-être pas modifier les données existantes. En fonction de vos besoins, vous pouvez décider si vous avez besoin d’un script de publication ou de ce que vous devez inclure dans le script. Pour plus d’informations sur le remplissage de votre base de données avec un script de publication, consultez [inclusion de données dans un projet de base de données SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Vous avez maintenant 4 fichiers de script SQL, mais pas de tables réelles. Vous êtes prêt à déployer votre projet de base de données sur la base de données locale. Dans Visual Studio, cliquez sur le bouton Démarrer (ou F5) pour générer et déployer votre projet de base de données. Vérifiez l’onglet **sortie** pour vérifier que la génération et le déploiement ont réussi.

Pour voir que la nouvelle base de données a été créée, ouvrez **Explorateur d’objets SQL Server** et recherchez le nom du projet dans le serveur de base de données local correct (en l’occurrence **, \ProjectsV13**).

Pour voir que les tables sont remplies avec des données, cliquez avec le bouton droit sur une table, puis sélectionnez **afficher les données**.

![afficher les données de la table](setting-up-database/_static/image9.png)

Une vue modifiable des données de la table est affichée. Par exemple, si vous sélectionnez des **Tables** > **dbo. course** > **afficher des données**, vous voyez une table avec trois colonnes (**course**, **title**et **credits**) et quatre lignes.

## <a name="additional-resources"></a>Ressources supplémentaires

Pour obtenir un exemple d’introduction de Code First développement, consultez [prise en main avec ASP.NET MVC 5](../introduction/getting-started.md). Pour obtenir un exemple plus avancé, consultez [création d’un modèle de données Entity Framework pour une application ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Pour obtenir des conseils sur la sélection de l’approche Entity Framework à utiliser, consultez [Entity Framework approches de développement](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Configurer la base de données

Passez au didacticiel suivant pour apprendre à créer l’application Web et les modèles de données.
> [!div class="nextstepaction"]
> [Créer l’application Web et les modèles de données](creating-the-web-application.md)
