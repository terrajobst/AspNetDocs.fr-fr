---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: Débogage des procédures stockées (C#) | Microsoft Docs
author: rick-anderson
description: Les éditions Visual Studio Professional et Team System vous permettent de définir des points d’arrêt et un pas à pas détaillé des procédures stockées dans SQL Server, ce qui rend le débogage stocké...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: 12a8500b107345b9cc9ab457016fdef09ca1bb9d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74604864"
---
# <a name="debugging-stored-procedures-c"></a>Débogage des procédures stockées (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip) ou [Télécharger le PDF](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Les éditions Visual Studio Professional et Team System vous permettent de définir des points d’arrêt et un pas à pas détaillé des procédures stockées dans SQL Server, ce qui rend le débogage des procédures stockées aussi simple que le débogage du code de l’application. Ce didacticiel illustre le débogage de base de données directe et le débogage d’applications des procédures stockées.

## <a name="introduction"></a>Introduction

Visual Studio offre une expérience de débogage enrichie. Avec quelques séquences de touches ou clics de souris, il est possible d’utiliser des points d’arrêt pour arrêter l’exécution d’un programme et examiner son état et son fluide de contrôle. Outre le débogage du code d’application, Visual Studio offre une prise en charge du débogage des procédures stockées à partir de SQL Server. Tout comme les points d’arrêt peuvent être définis dans le code d’une classe code-behind ASP.NET ou d’une classe de couche de logique métier, il est également possible de les placer dans des procédures stockées.

Dans ce didacticiel, nous allons examiner pas à pas les procédures stockées du Explorateur de serveurs dans Visual Studio, ainsi que la façon de définir des points d’arrêt qui sont atteints lorsque la procédure stockée est appelée à partir de l’application ASP.NET en cours d’exécution.

> [!NOTE]
> Malheureusement, les procédures stockées ne peuvent être exécutées pas à pas et déboguées par le biais des versions Professional et Team Systems de Visual Studio. Si vous utilisez Visual Web Developer ou la version standard de Visual Studio, vous êtes invité à lire les étapes à suivre pour déboguer les procédures stockées, mais vous ne pourrez pas répliquer ces étapes sur votre ordinateur.

## <a name="sql-server-debugging-concepts"></a>Concepts de débogage SQL Server

Microsoft SQL Server 2005 a été conçu pour permettre l’intégration au [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), qui est le runtime utilisé par tous les assemblys .net. Par conséquent, SQL Server 2005 prend en charge les objets de base de données managés. Autrement dit, vous pouvez créer des objets de base de données tels que des procédures stockées et des fonctions définies par l' C# utilisateur (UDF) en tant que méthodes dans une classe. Cela permet à ces procédures stockées et fonctions définies par l’utilisateur d’utiliser les fonctionnalités de la .NET Framework et de vos propres classes personnalisées. Bien entendu, SQL Server 2005 fournit également la prise en charge des objets de base de données T-SQL.

SQL Server 2005 offre une prise en charge du débogage pour les objets T-SQL et les objets de base de données managés. Toutefois, ces objets peuvent être débogués uniquement par le biais des éditions Visual Studio 2005 Professional et Team Systems. Dans ce didacticiel, nous allons examiner le débogage des objets de base de données T-SQL. Le didacticiel suivant examine le débogage des objets de base de données managés.

La [vue d’ensemble du débogage T-SQL et CLR dans SQL Server](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) entrée de blog 2005 de l' [équipe d’intégration de SQL Server 2005 CLR](https://blogs.msdn.com/sqlclr/default.aspx) met en évidence les trois façons de déboguer SQL Server 2005 objets à partir de Visual Studio :

- **Débogage de base de données direct (DDD)** -à partir de Explorateur de serveurs nous pouvons parcourir n’importe quel objet de base de données T-SQL, comme les procédures stockées et les fonctions définies par l’utilisateur. Nous allons examiner DDD à l’étape 1.
- **Débogage d’application** : nous pouvons définir des points d’arrêt dans un objet de base de données, puis exécuter notre application ASP.net. Lorsque l’objet de base de données est exécuté, le point d’arrêt est atteint et le contrôle est réactivé sur le débogueur. Notez que le débogage d’application ne peut pas effectuer de pas à pas détaillé dans un objet de base de données à partir du code d’application. Nous devons définir explicitement des points d’arrêt dans les procédures stockées ou les fonctions définies par l’utilisateur où nous voulons que le débogueur s’arrête. Le débogage de l’application est examiné à partir de l’étape 2.
- Le **débogage à partir d’un projet SQL Server** Visual Studio Professional et les éditions de Team Systems incluent un type de projet SQL Server qui est couramment utilisé pour créer des objets de base de données managés. Nous allons examiner l’utilisation des projets SQL Server et le débogage de leur contenu dans le didacticiel suivant.

Visual Studio peut déboguer des procédures stockées sur des instances locales et distantes de SQL Server. Une instance de SQL Server locale est une instance installée sur le même ordinateur que Visual Studio. Si la base de données SQL Server que vous utilisez ne se trouve pas sur votre ordinateur de développement, elle est considérée comme une instance distante. Pour ces didacticiels, nous utilisons des instances de SQL Server locales. Le débogage de procédures stockées sur une instance SQL Server distante requiert davantage d’étapes de configuration que lors du débogage de procédures stockées sur une instance locale.

Si vous utilisez une instance de SQL Server locale, vous pouvez commencer par l’étape 1 et suivre ce didacticiel jusqu’à la fin. Toutefois, si vous utilisez une instance de SQL Server distante, vous devez d’abord vous assurer que lors du débogage, vous êtes connecté à votre ordinateur de développement à l’aide d’un compte d’utilisateur Windows disposant d’une connexion SQL Server sur l’instance distante. En outre, la connexion à la base de données et la connexion de base de données utilisée pour se connecter à la base de données à partir de l’application ASP.NET en cours d’exécution doivent être membres du rôle `sysadmin`. Pour plus d’informations sur la configuration de Visual Studio et SQL Server pour déboguer une instance distante, consultez la section débogage d’objets T-SQL Database sur des instances distantes à la fin de ce didacticiel.

Enfin, sachez que la prise en charge du débogage pour les objets de base de données T-SQL n’est pas aussi riche que la prise en charge du débogage pour les applications .NET. Par exemple, les conditions de point d’arrêt et les filtres ne sont pas pris en charge, seul un sous-ensemble des fenêtres de débogage sont disponibles, vous ne pouvez pas utiliser modifier & continuer, la fenêtre exécution est rendue inutilisable, et ainsi de suite. Pour plus d’informations, consultez [limitations sur les commandes et les fonctionnalités du débogueur](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) .

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Étape 1 : effectuer un pas à pas détaillé dans une procédure stockée

Visual Studio simplifie le débogage direct d’un objet de base de données. Voyons comment utiliser la fonctionnalité de débogage de base de données directe (DDD) pour effectuer un pas à pas détaillé dans la procédure stockée `Products_SelectByCategoryID` dans la base de données Northwind. Comme son nom l’indique, `Products_SelectByCategoryID` retourne des informations sur les produits pour une catégorie particulière ; Il a été créé dans le didacticiel [utilisation de procédures stockées existantes pour les TableAdapters de DataSet typé](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) . Commencez par accéder au Explorateur de serveurs et développez le nœud de la base de données Northwind. Explorez ensuite le dossier procédures stockées, cliquez avec le bouton droit sur la procédure stockée `Products_SelectByCategoryID`, puis choisissez l’option pas à pas détaillé dans la procédure stockée dans le menu contextuel. Le débogueur est alors démarré.

Étant donné que la procédure stockée `Products_SelectByCategoryID` attend un paramètre d’entrée `@CategoryID`, nous sommes invités à fournir cette valeur. Entrez 1, qui renverra des informations sur les boissons.

![Utilisez la valeur 1 pour le paramètre @CategoryID](debugging-stored-procedures-cs/_static/image1.png)

**Figure 1**: utiliser la valeur 1 pour le paramètre `@CategoryID`

Une fois que vous avez fourni la valeur pour le paramètre `@CategoryID`, la procédure stockée est exécutée. Au lieu d’exécuter jusqu’à la fin, toutefois, le débogueur arrête l’exécution sur la première instruction. Notez la flèche jaune dans la marge, indiquant l’emplacement actuel dans la procédure stockée. Vous pouvez afficher et modifier les valeurs des paramètres par le biais de la Fenêtre Espion ou en plaçant le curseur sur le nom du paramètre dans la procédure stockée.

[![le débogueur s’est arrêté sur la première instruction de la procédure stockée](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**Figure 2**: le débogueur s’est arrêté sur la première instruction de la procédure stockée ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image4.png))

Pour exécuter pas à pas la procédure stockée une instruction à la fois, cliquez sur le bouton pas à pas principal dans la barre d’outils ou appuyez sur la touche F10. La procédure stockée `Products_SelectByCategoryID` contient une seule instruction `SELECT`. en appuyant sur F10, vous allez effectuer un pas à pas principal dans l’instruction unique et terminer l’exécution de la procédure stockée. Une fois la procédure stockée terminée, sa sortie s’affiche dans la fenêtre sortie et le débogueur se termine.

> [!NOTE]
> Le débogage T-SQL se produit au niveau de l’instruction ; vous ne pouvez pas effectuer un pas à pas détaillé dans une instruction `SELECT`.

## <a name="step-2-configuring-the-website-for-application-debugging"></a>Étape 2 : configuration du site Web pour le débogage des applications

Bien que le débogage d’une procédure stockée directement à partir de l’Explorateur de serveurs soit pratique, dans de nombreux scénarios, nous sommes plus intéressés à déboguer la procédure stockée lorsqu’elle est appelée à partir de notre application ASP.NET. Nous pouvons ajouter des points d’arrêt à une procédure stockée à partir de Visual Studio, puis démarrer le débogage de l’application ASP.NET. Lorsqu’une procédure stockée avec des points d’arrêt est appelée à partir de l’application, l’exécution s’arrête au point d’arrêt et vous pouvez afficher et modifier les valeurs des paramètres de la procédure stockée et parcourir ses instructions, comme nous l’avons fait à l’étape 1.

Avant de pouvoir démarrer le débogage des procédures stockées appelées à partir de l’application, nous devons demander à l’application Web ASP.NET de s’intégrer au débogueur SQL Server. Commencez par cliquer avec le bouton droit sur le nom du site Web dans le Explorateur de solutions (`ASPNET_Data_Tutorial_74_CS`). Choisissez l’option pages de propriétés dans le menu contextuel, sélectionnez l’élément options de démarrage à gauche, puis activez la case à cocher SQL Server dans la section débogueurs (voir figure 3).

[![cochez la case SQL Server dans les pages de propriétés de l’application](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**Figure 3**: activer la case à cocher SQL Server dans les pages de propriétés de l’application ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image7.png))

En outre, nous devons mettre à jour la chaîne de connexion de base de données utilisée par l’application afin que le regroupement de connexions soit désactivé. Lorsqu’une connexion à une base de données est fermée, l’objet `SqlConnection` correspondant est placé dans un pool de connexions disponibles. Lors de l’établissement d’une connexion à une base de données, un objet de connexion disponible peut être récupéré à partir de ce pool au lieu d’avoir à créer et à établir une nouvelle connexion. Ce regroupement d’objets de connexion est une amélioration des performances et est activé par défaut. Toutefois, lors du débogage, nous voulons désactiver le regroupement de connexions, car l’infrastructure de débogage n’est pas correctement rétablie lors de l’utilisation d’une connexion qui a été effectuée à partir du pool.

Pour désactiver le regroupement de connexions, mettez à jour le `NORTHWNDConnectionString` dans `Web.config` afin qu’il comprenne le paramètre `Pooling=false`.

[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> Une fois que vous avez terminé le débogage SQL Server par le biais de l’application ASP.NET, veillez à rétablir le regroupement de connexions en supprimant le paramètre `Pooling` de la chaîne de connexion (ou en lui affectant la valeur `Pooling=true`).

À ce stade, l’application ASP.NET a été configurée pour permettre à Visual Studio de déboguer SQL Server objets de base de données lorsqu’elle est appelée par le biais de l’application Web. Tout ce qui reste à présent, c’est d’ajouter un point d’arrêt à une procédure stockée et de démarrer le débogage !

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Étape 3 : ajout d’un point d’arrêt et débogage

Ouvrez la procédure stockée `Products_SelectByCategoryID` et définissez un point d’arrêt au début de l’instruction `SELECT` en cliquant dans la marge à l’emplacement approprié ou en plaçant votre curseur au début de l’instruction `SELECT` et en appuyant sur F9. Comme illustré à la figure 4, le point d’arrêt apparaît sous la forme d’un cercle rouge dans la marge.

[![définir un point d’arrêt dans la procédure stockée Products_SelectByCategoryID](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**Figure 4**: définir un point d’arrêt dans la procédure stockée `Products_SelectByCategoryID` ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image10.png))

Pour qu’un objet SQL Database soit débogué par le biais d’une application cliente, il est impératif que la base de données soit configurée pour prendre en charge le débogage d’application. Lorsque vous définissez un point d’arrêt pour la première fois, ce paramètre doit être automatiquement activé, mais il est prudent de procéder à une double vérification. Cliquez avec le bouton droit sur le nœud `NORTHWND.MDF` dans le Explorateur de serveurs. Le menu contextuel doit inclure un élément de menu coché nommé débogage de l’application.

![Vérifier que l’option de débogage de l’application est activée](debugging-stored-procedures-cs/_static/image11.png)

**Figure 5**: vérifier que l’option débogage de l’application est activée

Lorsque le point d’arrêt est défini et que l’option débogage de l’application est activée, nous sommes prêts à déboguer la procédure stockée quand elle est appelée à partir de l’application ASP.NET. Démarrez le débogueur en accédant au menu Déboguer et en choisissant Démarrer le débogage, en appuyant sur F5 ou en cliquant sur l’icône de lecture verte dans la barre d’outils. Cette opération démarre le débogueur et lance le site Web.

La procédure stockée `Products_SelectByCategoryID` a été créée dans le didacticiel [utilisation de procédures stockées existantes pour les TableAdapters de DataSet typé](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) . Sa page Web correspondante (`~/AdvancedDAL/ExistingSprocs.aspx`) contient un GridView qui affiche les résultats retournés par cette procédure stockée. Visitez cette page via le navigateur. Quand vous atteignez la page, le point d’arrêt dans la procédure stockée `Products_SelectByCategoryID` est atteint et le contrôle est retourné à Visual Studio. Comme dans l’étape 1, vous pouvez parcourir les instructions des procédures stockées et afficher et modifier les valeurs des paramètres.

[![la page ExistingSprocs. aspx affiche initialement les boissons](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**Figure 6**: la page `ExistingSprocs.aspx` affiche initialement les boissons ([cliquez pour afficher l’image en plein écran](debugging-stored-procedures-cs/_static/image14.png))

[![le point d’arrêt des procédures stockées a été atteint](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**Figure 7**: le point d’arrêt de la procédure stockée a été atteint ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image17.png))

Comme le montre la Fenêtre Espion de la figure 7, la valeur du paramètre `@CategoryID` est 1. Cela est dû au fait que la page `ExistingSprocs.aspx` affiche initialement les produits de la catégorie boissons, qui ont une valeur `CategoryID` de 1. Choisissez une autre catégorie dans la liste déroulante. Cela provoque une publication (postback) et réexécute la procédure stockée `Products_SelectByCategoryID`. Le point d’arrêt est de nouveau atteint, mais cette fois, la valeur du paramètre `@CategoryID` correspond à la `CategoryID`des éléments de liste déroulante sélectionnés.

[![choisir une autre catégorie dans la liste déroulante](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**Figure 8**: choisir une autre catégorie dans la liste déroulante ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image20.png))

[![le paramètre @CategoryID reflète la catégorie sélectionnée à partir de la page Web](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**Figure 9**: le paramètre `@CategoryID` reflète la catégorie sélectionnée à partir de la page Web ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image23.png))

> [!NOTE]
> Si le point d’arrêt de la procédure stockée `Products_SelectByCategoryID` n’est pas atteint lors de la visite de la page de `ExistingSprocs.aspx`, assurez-vous que la case à cocher SQL Server a été activée dans la section débogueurs de la page de propriétés de l’application ASP.NET, que le regroupement de connexions a été désactivé et que l’option de débogage de l’application base de données est activée. Si vous rencontrez encore des problèmes, redémarrez Visual Studio et réessayez.

## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Débogage d’objets T-SQL Database sur des instances distantes

Le débogage des objets de base de données par le biais de Visual Studio est relativement simple lorsque l’instance de base de données SQL Server se trouve sur le même ordinateur que Visual Studio. Toutefois, si SQL Server et Visual Studio résident sur des ordinateurs différents, une configuration minutieuse est nécessaire pour que tout fonctionne correctement. Nous avons rencontré deux tâches principales :

- Assurez-vous que la connexion utilisée pour se connecter à la base de données via ADO.NET appartient au rôle `sysadmin`.
- Assurez-vous que le compte d’utilisateur Windows utilisé par Visual Studio sur l’ordinateur de développement est un compte de connexion SQL Server valide qui appartient au rôle `sysadmin`.

La première étape est relativement simple. Tout d’abord, identifiez le compte d’utilisateur utilisé pour se connecter à la base de données à partir de l’application ASP.NET, puis, dans SQL Server Management Studio, ajoutez ce compte de connexion au rôle `sysadmin`.

La deuxième tâche nécessite que le compte d’utilisateur Windows que vous utilisez pour déboguer l’application soit une connexion valide sur la base de données distante. Toutefois, il est probable que le compte Windows avec lequel vous vous êtes connecté à votre station de travail n’est pas une connexion valide sur SQL Server. Au lieu d’ajouter votre compte de connexion particulier à SQL Server, il est préférable de désigner un compte d’utilisateur Windows comme compte de débogage SQL Server. Ensuite, pour déboguer les objets de base de données d’une instance de SQL Server distante, vous devez exécuter Visual Studio en utilisant les informations d’identification de ce compte de connexion Windows.

Un exemple doit vous aider à clarifier les choses. Imaginez qu’il existe un compte Windows nommé `SQLDebug` dans le domaine Windows. Ce compte doit être ajouté à l’instance SQL Server distante en tant que connexion valide et en tant que membre du rôle `sysadmin`. Ensuite, pour déboguer l’instance SQL Server distante à partir de Visual Studio, vous devez exécuter Visual Studio en tant qu’utilisateur `SQLDebug`. Pour ce faire, vous devez vous déconnecter de notre station de travail, vous reconnecter en tant que `SQLDebug`, puis démarrer Visual Studio, mais une approche plus simple consiste à se connecter à notre station de travail à l’aide de vos propres informations d’identification, puis à utiliser `runas.exe` pour lancer Visual Studio en tant qu' `SQLDebug` utilisateur. `runas.exe` permet l’exécution d’une application particulière sous le forme d’un compte d’utilisateur différent. Pour lancer Visual Studio en tant que `SQLDebug`, vous pouvez entrer l’instruction suivante sur la ligne de commande :

[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

Pour obtenir une explication plus détaillée de ce processus, consultez [William R. Vaughn](http://betav.com/BLOG/billva/) s *hitchhiker’s s Guide to Visual Studio and SQL Server, septième édition,* ainsi que [comment : définir des autorisations SQL Server pour le débogage](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Si votre ordinateur de développement exécute Windows XP Service Pack 2, vous devez configurer le pare-feu de connexion Internet pour autoriser le débogage distant. L’article Guide pratique [pour activer le débogage SQL Server 2005](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) note que cela implique deux étapes : (a) sur l’ordinateur hôte Visual Studio, vous devez ajouter `Devenv.exe` à la liste des exceptions et ouvrir le port TCP 135. et (b) sur l’ordinateur distant (SQL), vous devez ouvrir le port TCP 135 et ajouter `sqlservr.exe` à la liste des exceptions. Si votre stratégie de domaine nécessite que la communication réseau soit effectuée via IPSec, vous devez ouvrir les ports UDP 4500 et UDP 500.

## <a name="summary"></a>Récapitulatif

En plus de fournir une prise en charge du débogage pour le code d’application .NET, Visual Studio fournit également une variété d’options de débogage pour SQL Server 2005. Dans ce didacticiel, nous avons vu deux de ces options : débogage de base de données direct et débogage d’application. Pour déboguer directement un objet de base de données T-SQL, recherchez l’objet à l’aide de l’Explorateur de serveurs cliquez dessus avec le bouton droit et choisissez pas à pas détaillé. Cela démarre le débogueur et s’arrête sur la première instruction de l’objet de base de données. à ce stade, vous pouvez parcourir les instructions de l’objet et afficher et modifier les valeurs des paramètres. À l’étape 1, nous avons utilisé cette approche pour effectuer un pas à pas détaillé dans la procédure stockée `Products_SelectByCategoryID`.

Le débogage d’application permet de définir des points d’arrêt directement dans les objets de base de données. Lorsqu’un objet de base de données avec des points d’arrêt est appelé à partir d’une application cliente (telle qu’une application Web ASP.NET), le programme s’arrête lorsque le débogueur prend le relais. Le débogage d’application est utile, car il montre plus clairement quelle action de l’application entraîne l’appel d’un objet de base de données particulier. Toutefois, elle nécessite un peu plus de configuration et de configuration que le débogage de base de données directe.

Les objets de base de données peuvent également être débogués via des projets SQL Server. Nous allons examiner l’utilisation de SQL Server projets et comment les utiliser pour créer et déboguer des objets de base de données managés dans le didacticiel suivant.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](protecting-connection-strings-and-other-configuration-information-cs.md)
> [Suivant](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
