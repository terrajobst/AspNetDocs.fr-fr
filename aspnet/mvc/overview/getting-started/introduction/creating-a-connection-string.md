---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Création d’une chaîne de connexion et utilisation de SQL Server base de données locale | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 20781ad760d3a0e4559ec4c7e18528f3686dcc02
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615667"
---
# <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Création d’une chaîne de connexion et utilisation de SQL Server LocalDB

par [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Création d’une chaîne de connexion et utilisation de SQL Server LocalDB

La classe `MovieDBContext` que vous avez créée gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de base de données. L’une des questions que vous pouvez poser, cependant, est la façon de spécifier la base de données à laquelle elle doit se connecter. Vous n’avez pas besoin de spécifier la base de données à utiliser, [Entity Framework utilisera](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)par défaut la base de données locale. Dans cette section, nous allons ajouter de manière explicite une chaîne de connexion dans le fichier *Web. config* de l’application.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

La base de données [locale est une](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) version allégée de l’SQL Server Express moteur de base de données qui démarre à la demande et s’exécute en mode utilisateur. La base de données locale s’exécute dans un mode d’exécution spécial de SQL Server Express qui vous permet d’utiliser des bases de données en tant que fichiers *. mdf* . En règle générale, les fichiers de base de données de base de données locale sont conservés dans le dossier *application\_Data* d’un projet Web.

SQL Server Express n’est pas recommandé pour une utilisation dans les applications Web de production. En particulier, la base de données locale ne doit pas être utilisée pour la production avec une application Web, car elle n’est pas conçue pour fonctionner avec IIS. Toutefois, une base de données de base de données locale peut être facilement migrée vers SQL Server ou SQL Azure.

Dans Visual Studio 2017, la base de données locale est installée par défaut dans Visual Studio.

Par défaut, le Entity Framework recherche une chaîne de connexion nommée identique à la classe de contexte de l’objet (`MovieDBContext` pour ce projet). Pour plus d’informations, consultez [SQL Server des chaînes de connexion pour les applications Web ASP.net](https://msdn.microsoft.com/library/jj653752.aspx).

Ouvrez le fichier *Web. config* racine de l’application, comme indiqué ci-dessous. (Pas le fichier *Web. config* dans le dossier *views* .)

![](creating-a-connection-string/_static/image1.png)

Recherchez l’élément `<connectionStrings>` :

![](creating-a-connection-string/_static/image2.png)

Ajoutez la chaîne de connexion suivante à l’élément `<connectionStrings>` dans le fichier *Web. config* .

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

L’exemple suivant montre une partie du fichier *Web. config* avec la nouvelle chaîne de connexion ajoutée :

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Les deux chaînes de connexion sont très similaires. La première chaîne de connexion est nommée `DefaultConnection` et est utilisée pour la base de données d’appartenance afin de contrôler qui peut accéder à l’application. La chaîne de connexion que vous avez ajoutée spécifie une base de données de base de données locale nommée *Movie. mdf* située dans le dossier de données de l' *application\_* . Nous n’utiliserons pas la base de données d’appartenance dans ce didacticiel. pour plus d’informations sur l’appartenance, l’authentification et la sécurité, consultez mon didacticiel [créer une application ASP.NET MVC avec auth et SQL DB et déployer sur Azure App service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Le nom de la chaîne de connexion doit correspondre au nom de la classe [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) .

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Vous n’avez pas besoin d’ajouter la chaîne de connexion `MovieDBContext`. Si vous ne spécifiez pas de chaîne de connexion, Entity Framework créera une base de données de base de données locale dans le répertoire Users avec le nom complet de la classe [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) (dans ce cas `MvcMovie.Models.MovieDBContext`). Vous pouvez nommer la base de données comme vous le souhaitez, tant qu’elle a la valeur *.* Suffixe MDF. Par exemple, nous pourrions nommer la base de données *MyFilms. mdf*.

Ensuite, vous allez générer une nouvelle classe de `MoviesController` que vous pouvez utiliser pour afficher les données de film et permettre aux utilisateurs de créer des listes de films.

> [!div class="step-by-step"]
> [Précédent](adding-a-model.md)
> [Suivant](accessing-your-models-data-from-a-controller.md)
