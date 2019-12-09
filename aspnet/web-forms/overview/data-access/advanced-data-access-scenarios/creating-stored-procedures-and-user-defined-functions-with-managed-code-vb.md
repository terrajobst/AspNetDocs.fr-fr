---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
title: Création de procédures stockées et de fonctions définies par l’utilisateur avec du code managé (VB) | Microsoft Docs
author: rick-anderson
description: Microsoft SQL Server 2005 s’intègre au Common Language Runtime .NET pour permettre aux développeurs de créer des objets de base de données par le biais de code managé. Ce didacticiel...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 8be9a51b-ea6b-46c7-bfa2-476d9b14c24c
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ac5f71d519689a9dc84fb82a04196d520cca6e1
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74610390"
---
# <a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-vb"></a>Création de procédures stockées et de fonctions définies par l’utilisateur avec du code managé (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_VB.zip) ou [Télécharger le PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/datatutorial75vb1.pdf)

> Microsoft SQL Server 2005 s’intègre au Common Language Runtime .NET pour permettre aux développeurs de créer des objets de base de données par le biais de code managé. Ce didacticiel montre comment créer des procédures stockées gérées et des fonctions définies par l’utilisateur gérées C# avec votre Visual Basic ou votre code. Nous voyons également comment ces éditions de Visual Studio vous permettent de déboguer des objets de base de données managés.

## <a name="introduction"></a>Introduction

Les bases de données telles que Microsoft s SQL Server 2005 utilisent l' [instruction Transact-langage SQL (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) pour l’insertion, la modification et l’extraction de données. La plupart des systèmes de base de données incluent des constructions pour le regroupement d’une série d’instructions SQL qui peuvent ensuite être exécutées comme une seule unité réutilisable. Les procédures stockées en sont un exemple. Une autre est une *fonction définie par l’utilisateur*(UDF), une construction que nous examinerons plus en détail à l’étape 9.

À son cœur, SQL est conçu pour utiliser des ensembles de données. Les instructions `SELECT`, `UPDATE`et `DELETE` s’appliquent de manière intrinsèque à tous les enregistrements de la table correspondante et sont limitées uniquement par leurs clauses de `WHERE`. Pourtant, il existe de nombreuses fonctionnalités de langage conçues pour travailler avec un seul enregistrement à la fois et pour manipuler les données scalaires. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) permettent de parcourir un ensemble d’enregistrements à la fois. Les fonctions de manipulation de chaînes telles que `LEFT`, `CHARINDEX`et `PATINDEX` fonctionnent avec des données scalaires. SQL comprend également des instructions de contrôle de workflow comme `IF` et `WHILE`.

Avant Microsoft SQL Server 2005, les procédures stockées et les fonctions définies par l’utilisateur pouvaient uniquement être définies en tant que collection d’instructions T-SQL. SQL Server 2005, toutefois, a été conçu pour permettre l’intégration au [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), qui est le runtime utilisé par tous les assemblys .net. Par conséquent, les procédures stockées et les fonctions définies par l’utilisateur dans une base de données SQL Server 2005 peuvent être créées à l’aide de code managé. Autrement dit, vous pouvez créer une procédure stockée ou une fonction définie par l’opération (UDF) comme méthode dans une classe Visual Basic. Cela permet à ces procédures stockées et fonctions définies par l’utilisateur d’utiliser les fonctionnalités de la .NET Framework et de vos propres classes personnalisées.

Dans ce didacticiel, nous allons examiner comment créer des procédures stockées gérées et des fonctions définies par l’utilisateur et comment les intégrer dans la base de données Northwind. Commençons !

> [!NOTE]
> Les objets de base de données managés présentent des avantages par rapport à leurs équivalents SQL. La richesse et la connaissance du langage, ainsi que la possibilité de réutiliser le code et la logique existants, sont les principaux avantages. Toutefois, les objets de base de données managés sont susceptibles d’être moins efficaces quand vous utilisez des jeux de données qui n’impliquent pas une logique procédurale. Pour une discussion plus approfondie sur les avantages de l’utilisation du code managé par rapport à T-SQL, Découvrez les [avantages de l’utilisation du code managé pour créer des objets de base de données](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).

## <a name="step-1-moving-the-northwind-database-out-ofapp_data"></a>Étape 1 : déplacement de la base de données Northwind à partir de`App_Data`

Tous nos didacticiels, jusqu’à présent, ont utilisé un fichier de base de données Microsoft SQL Server 2005 Express Edition dans le dossier des `App_Data` de l’application Web. Le fait de placer la base de données dans `App_Data` simplifiée la distribution et l’exécution de ces didacticiels, car tous les fichiers se trouvaient dans un répertoire et aucune étape de configuration supplémentaire n’est nécessaire pour tester le didacticiel.

Toutefois, pour ce didacticiel, nous allons déplacer la base de données Northwind de `App_Data` et l’inscrire explicitement auprès de l’instance de base de données SQL Server 2005 Express Edition. Bien que nous puissions effectuer les étapes de ce didacticiel avec la base de données dans le dossier `App_Data`, un certain nombre d’étapes sont considérablement plus simples en inscrivant explicitement la base de données auprès de l’instance de base de données SQL Server 2005 Express Edition.

Le téléchargement de ce didacticiel contient les deux fichiers de base de données : `NORTHWND.MDF` et `NORTHWND_log.LDF`-placés dans un dossier nommé `DataFiles`. Si vous suivez votre propre implémentation des didacticiels, fermez Visual Studio et déplacez les fichiers `NORTHWND.MDF` et `NORTHWND_log.LDF` du dossier site Web `App_Data` s vers un dossier en dehors du site Web. Une fois que les fichiers de base de données ont été déplacés vers un autre dossier, nous devons inscrire la base de données Northwind auprès de l’instance de base de données SQL Server 2005 Express Edition. Pour ce faire, vous pouvez utiliser SQL Server Management Studio. Si vous disposez d’une édition non Express de SQL Server 2005 installée sur votre ordinateur, vous avez probablement déjà installé Management Studio. Si vous n’avez SQL Server 2005 Express Edition sur votre ordinateur, prenez un moment pour télécharger et installer [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Lancez SQL Server Management Studio. Comme le montre la figure 1, Management Studio commence par demander le serveur auquel se connecter. Entrez localhost\SQLExpress pour le nom du serveur, choisissez authentification Windows dans la liste déroulante authentification, puis cliquez sur se connecter.

![Se connecter à l’instance de base de données appropriée](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image1.png)

**Figure 1**: connexion à l’instance de base de données appropriée

Une fois connecté, la fenêtre Explorateur d’objets affiche des informations sur l’instance de base de données SQL Server 2005 Express Edition, notamment ses bases de données, les informations de sécurité, les options de gestion, etc.

Nous devons attacher la base de données Northwind dans le dossier `DataFiles` (ou à l’endroit où vous l’avez éventuellement déplacée) dans l’instance de base de données SQL Server 2005 Express Edition. Cliquez avec le bouton droit sur le dossier bases de données et choisissez l’option attacher dans le menu contextuel. La boîte de dialogue attacher des bases de données s’affiche. Cliquez sur le bouton Ajouter, explorez le fichier `NORTHWND.MDF` approprié, puis cliquez sur OK. À ce stade, votre écran doit ressembler à la figure 2.

[![se connecter à l’instance de base de données appropriée](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image2.png)

**Figure 2**: connexion à l’instance de base de données appropriée ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image4.png))

> [!NOTE]
> Lors de la connexion à l’instance de SQL Server 2005 Express Edition via Management Studio la boîte de dialogue attacher des bases de données ne vous permet pas d’accéder aux répertoires des profils utilisateur, tels que mes documents. Par conséquent, veillez à placer les fichiers `NORTHWND.MDF` et `NORTHWND_log.LDF` dans un répertoire de profil non-utilisateur.

Cliquez sur le bouton OK pour attacher la base de données. La boîte de dialogue attacher des bases de données se ferme et l’Explorateur d’objets doit à présent répertorier la base de données qui vient d’être attachée. Il est probable que la base de données Northwind ait un nom comme `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Renommez la base de données en Northwind en cliquant avec le bouton droit sur la base de données et en choisissant renommer.

![Renommez la base de données en Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image5.png)

**Figure 3**: renommer la base de données en Northwind

## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Étape 2 : création d’une solution et d’SQL Server projet dans Visual Studio

Pour créer des procédures stockées ou des fonctions définies par l’utilisateur dans SQL Server 2005, nous allons écrire la procédure stockée et la logique UDF comme Visual Basic code dans une classe. Une fois le code écrit, nous devons compiler cette classe dans un assembly (un fichier `.dll`), inscrire l’assembly auprès de la base de données SQL Server, puis créer une procédure stockée ou un objet UDF dans la base de données qui pointe vers la méthode correspondante dans l’assembly. Ces étapes peuvent toutes être effectuées manuellement. Nous pouvons créer le code dans n’importe quel éditeur de texte, le compiler à partir de la ligne de commande à l’aide du compilateur de Visual Basic (`vbc.exe`), l’inscrire auprès de la base de données à l’aide de la commande [`CREATE ASSEMBLY`](https://msdn.microsoft.com/library/ms189524.aspx) ou de Management Studio, et ajouter la procédure stockée ou l’objet UDF par le biais de moyens similaires. Heureusement, les versions Professional et Team Systems de Visual Studio incluent un type de projet SQL Server qui automatise ces tâches. Dans ce didacticiel, nous allons utiliser le type de projet SQL Server pour créer une procédure stockée managée et une fonction définie par l’opération.

> [!NOTE]
> Si vous utilisez Visual Web Developer ou l’édition standard de Visual Studio, vous devrez utiliser l’approche manuelle à la place. L’étape 13 fournit des instructions détaillées pour exécuter ces étapes manuellement. Je vous encourage à lire les étapes 2 à 12 avant de lire l’étape 13, car ces étapes incluent des instructions de configuration SQL Server importantes qui doivent être appliquées, quelle que soit la version de Visual Studio que vous utilisez.

Commencez par ouvrir Visual Studio. Dans le menu fichier, choisissez nouveau projet pour afficher la boîte de dialogue Nouveau projet (voir figure 4). Explorez le type de projet de base de données, puis, à partir des modèles figurant sur la droite, choisissez de créer un projet de SQL Server. J’ai choisi de nommer ce projet `ManagedDatabaseConstructs` et de l’avoir placé dans une solution nommée `Tutorial75`.

[![créer un projet de SQL Server](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image6.png)

**Figure 4**: créer un projet de SQL Server ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image8.png))

Cliquez sur le bouton OK dans la boîte de dialogue Nouveau projet pour créer la solution et SQL Server projet.

Un projet de SQL Server est lié à une base de données particulière. Par conséquent, après la création du projet de SQL Server, nous avons immédiatement demandé de spécifier ces informations. La figure 5 montre la boîte de dialogue nouvelle référence de base de données qui a été remplie pour pointer vers la base de données Northwind que nous avons inscrite dans l’instance de base de données SQL Server 2005 Express Edition à l’étape 1.

![Associer le projet SQL Server à la base de données Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image9.png)

**Figure 5**: associer le projet SQL Server à la base de données Northwind

Afin de déboguer les procédures stockées managées et les fonctions définies par l’utilisateur créées dans ce projet, nous devons activer la prise en charge du débogage SQL/CLR pour la connexion. Lorsque vous associez un projet de SQL Server à une nouvelle base de données (comme nous l’avons fait dans la figure 5), Visual Studio nous demande si vous souhaitez activer le débogage SQL/CLR sur la connexion (voir la figure 6). Cliquez sur Oui.

![Activer le débogage SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image10.png)

**Figure 6**: activer le débogage SQL/CLR

À ce stade, le nouveau projet de SQL Server a été ajouté à la solution. Il contient un dossier nommé `Test Scripts` avec un fichier nommé `Test.sql`, qui est utilisé pour déboguer les objets de base de données managés créés dans le projet. Nous allons examiner le débogage à l’étape 12.

Nous pouvons maintenant ajouter de nouvelles procédures stockées gérées et des fonctions définies par l’utilisateur à ce projet. Toutefois, avant de commencer, vous incluez d’abord notre application Web existante dans la solution. Dans le menu fichier, sélectionnez l’option Ajouter et choisissez site Web existant. Accédez au dossier du site Web approprié, puis cliquez sur OK. Comme le montre la figure 7, cette opération met à jour la solution pour inclure deux projets : le site Web et le projet `ManagedDatabaseConstructs` SQL Server.

![Le Explorateur de solutions comprend maintenant deux projets](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image11.png)

**Figure 7**: le Explorateur de solutions contient maintenant deux projets

La valeur `NORTHWNDConnectionString` dans `Web.config` référence actuellement le fichier `NORTHWND.MDF` dans le dossier `App_Data`. Étant donné que nous avons supprimé cette base de données de `App_Data` et que vous l’avez inscrite explicitement dans l’instance de base de données SQL Server 2005 Express Edition, nous devons mettre à jour la valeur de `NORTHWNDConnectionString`. Ouvrez le fichier `Web.config` dans le site Web et modifiez la valeur de `NORTHWNDConnectionString` afin que la chaîne de connexion indique : `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Après cette modification, votre section `<connectionStrings>` dans `Web.config` doit ressembler à ce qui suit :

[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample1.xml)]

> [!NOTE]
> Comme indiqué dans le [didacticiel précédent](debugging-stored-procedures-vb.md), lors du débogage d’un objet SQL Server à partir d’une application cliente, telle qu’un site Web ASP.net, nous devons désactiver le regroupement de connexions. La chaîne de connexion présentée ci-dessus désactive le regroupement de connexions (`Pooling=false`). Si vous n’envisagez pas de déboguer les procédures stockées managées et les fonctions définies par l’utilisateur sur le site Web ASP.NET, activez le regroupement de connexions.

## <a name="step-3-creating-a-managed-stored-procedure"></a>Étape 3 : création d’une procédure stockée managée

Pour ajouter une procédure stockée managée à la base de données Northwind, vous devez d’abord créer la procédure stockée en tant que méthode dans le projet SQL Server. Dans le Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet `ManagedDatabaseConstructs` et choisissez d’ajouter un nouvel élément. La boîte de dialogue Ajouter un nouvel élément s’affiche et répertorie les types d’objets de base de données managés qui peuvent être ajoutés au projet. Comme le montre la figure 8, cela comprend les procédures stockées et les fonctions définies par l’utilisateur, entre autres.

Commençons par ajouter une procédure stockée qui retourne simplement tous les produits qui ont été abandonnés. Nommez le nouveau fichier de procédure stockée `GetDiscontinuedProducts.vb`.

[![ajouter une nouvelle procédure stockée nommée GetDiscontinuedProducts. vb](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image12.png)

**Figure 8**: ajouter une nouvelle procédure stockée nommée `GetDiscontinuedProducts.vb` ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image14.png))

Cette opération crée un fichier de classe Visual Basic avec le contenu suivant :

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample2.vb)]

Notez que la procédure stockée est implémentée en tant que méthode `Shared` dans un fichier de classe `Partial` nommé `StoredProcedures`. En outre, la méthode `GetDiscontinuedProducts` est décorée avec l' [attribut`SqlProcedure`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlprocedureattribute.aspx), qui marque la méthode comme une procédure stockée.

Le code suivant crée un objet `SqlCommand` et définit son `CommandText` sur une requête `SELECT` qui retourne toutes les colonnes de la table `Products` pour les produits dont le champ `Discontinued` est égal à 1. Il exécute ensuite la commande et renvoie les résultats à l’application cliente. Ajoutez ce code à la méthode `GetDiscontinuedProducts`.

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample3.vb)]

Tous les objets de base de données managés ont accès à un [objet`SqlContext`](https://msdn.microsoft.com/library/ms131108.aspx) qui représente le contexte de l’appelant. Le `SqlContext` permet d’accéder à un [objet`SqlPipe`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) par le biais de sa [propriété`Pipe`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Cet objet `SqlPipe` est utilisé pour ferry des informations entre la base de données SQL Server et l’application appelante. Comme son nom l’indique, la [méthode`ExecuteAndSend`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) exécute un objet `SqlCommand` passé et renvoie les résultats à l’application cliente.

> [!NOTE]
> Les objets de base de données managés conviennent mieux aux procédures stockées et aux fonctions définies par l’utilisateur qui utilisent la logique procédurale plutôt que la logique basée sur un jeu. La logique procédurale implique l’utilisation de jeux de données ligne par ligne ou l’utilisation de données scalaires. Toutefois, la méthode `GetDiscontinuedProducts` que nous venons de créer n’implique aucune logique procédurale. Par conséquent, il serait idéalement implémenté comme une procédure stockée T-SQL. Elle est implémentée comme une procédure stockée managée pour illustrer les étapes nécessaires à la création et au déploiement de procédures stockées managées.

## <a name="step-4-deploying-the-managed-stored-procedure"></a>Étape 4 : déploiement de la procédure stockée managée

Une fois ce code terminé, nous sommes prêts à le déployer dans la base de données Northwind. Le déploiement d’une SQL Server projet compile le code dans un assembly, inscrit l’assembly auprès de la base de données et crée les objets correspondants dans la base de données, en les liant aux méthodes appropriées dans l’assembly. L’ensemble exact de tâches effectuées par l’option de déploiement est plus précisément orthographié à l’étape 13. Cliquez avec le bouton droit sur le nom du projet `ManagedDatabaseConstructs` dans le Explorateur de solutions et choisissez l’option déployer. Toutefois, le déploiement échoue avec l’erreur suivante : syntaxe incorrecte à côté de « EXTERNAL ». Vous devrez peut-être définir le niveau de compatibilité de la base de données actuelle sur une valeur plus élevée pour activer cette fonctionnalité. Consultez l’aide de la procédure stockée `sp_dbcmptlevel`.

Ce message d’erreur se produit lorsque vous tentez d’inscrire l’assembly auprès de la base de données Northwind. Pour pouvoir inscrire un assembly avec une base de données SQL Server 2005, le niveau de compatibilité de la base de données doit être défini sur 90. Par défaut, les nouvelles bases de données SQL Server 2005 ont un niveau de compatibilité de 90. Toutefois, les bases de données créées à l’aide de Microsoft SQL Server 2000 ont un niveau de compatibilité par défaut de 80. Étant donné que la base de données Northwind était initialement une base de données Microsoft SQL Server 2000, son niveau de compatibilité est actuellement défini sur 80 et doit donc être augmenté à 90 pour pouvoir inscrire des objets de base de données gérés.

Pour mettre à jour le niveau de compatibilité de la base de données, ouvrez une nouvelle fenêtre de requête dans Management Studio et entrez :

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample4.sql)]

Cliquez sur l’icône exécuter dans la barre d’outils pour exécuter la requête ci-dessus.

[![mettre à jour le niveau de compatibilité des bases de données Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image15.png)

**Figure 9**: mettre à jour le niveau de compatibilité des bases de données Northwind ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image17.png))

Après avoir mis à jour le niveau de compatibilité, Redéployez le projet SQL Server. Cette fois-ci, le déploiement doit se terminer sans erreur.

Revenez à SQL Server Management Studio, cliquez avec le bouton droit sur la base de données Northwind dans l’Explorateur d’objets, puis choisissez actualiser. Ensuite, explorez le dossier programmabilité, puis développez le dossier Assemblies. Comme le montre la figure 10, la base de données Northwind comprend désormais l’assembly généré par le projet `ManagedDatabaseConstructs`.

![L’assembly ManagedDatabaseConstructs est maintenant inscrit auprès de la base de données Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image18.png)

**Figure 10**: l’assembly de `ManagedDatabaseConstructs` est maintenant inscrit auprès de la base de données Northwind

Développez également le dossier procédures stockées. Vous verrez une procédure stockée nommée `GetDiscontinuedProducts`. Cette procédure stockée a été créée par le processus de déploiement et pointe vers la méthode `GetDiscontinuedProducts` dans l’assembly `ManagedDatabaseConstructs`. Lorsque la `GetDiscontinuedProducts` procédure stockée est exécutée, elle exécute à son tour la méthode `GetDiscontinuedProducts`. Étant donné qu’il s’agit d’une procédure stockée managée, elle ne peut pas être modifiée via Management Studio (par conséquent, l’icône de verrou en regard du nom de la procédure stockée).

![La procédure stockée GetDiscontinuedProducts est indiquée dans le dossier procédures stockées.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image19.png)

**Figure 11**: la procédure stockée `GetDiscontinuedProducts` est indiquée dans le dossier procédures stockées

Il reste encore un obstacle à surmonter avant de pouvoir appeler la procédure stockée gérée : la base de données est configurée pour empêcher l’exécution du code managé. Pour vérifier cela, ouvrez une nouvelle fenêtre de requête et exécutez la procédure stockée `GetDiscontinuedProducts`. Vous recevrez le message d’erreur suivant : l’exécution du code utilisateur dans le .NET Framework est désactivée. Activez l’option de configuration CLR activé.

Pour examiner les informations de configuration de la base de données Northwind, entrez et exécutez la commande `exec sp_configure` dans la fenêtre de requête. Cela indique que le paramètre CLR Enabled a actuellement la valeur 0.

[![le paramètre CLR Enabled a actuellement la valeur 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image20.png)

**Figure 12**: le paramètre CLR activé est actuellement défini sur 0 ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image22.png))

Notez que chaque paramètre de configuration de la figure 12 est associé à quatre valeurs : les valeurs minimale et maximale, ainsi que les valeurs de configuration et d’exécution. Pour mettre à jour la valeur de configuration du paramètre CLR activé, exécutez la commande suivante :

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample5.sql)]

Si vous réexécutez la `exec sp_configure`, vous verrez que l’instruction ci-dessus a mis à jour la valeur de configuration du paramètre s du CLR avec la valeur 1, mais que la valeur d’exécution est toujours définie sur 0. Pour que cette modification de configuration prenne effet, nous devons exécuter la [commande`RECONFIGURE`](https://msdn.microsoft.com/library/ms176069.aspx), qui définira la valeur d’exécution sur la valeur de configuration actuelle. Entrez simplement `RECONFIGURE` dans la fenêtre de requête et cliquez sur l’icône exécuter dans la barre d’outils. Si vous exécutez `exec sp_configure` maintenant, vous devriez voir une valeur de 1 pour les valeurs de configuration et d’exécution du paramètre CLR activé.

Une fois la configuration CLR activée terminée, nous sommes prêts à exécuter la procédure stockée `GetDiscontinuedProducts` gérée. Dans la fenêtre de requête, entrez et exécutez la commande `exec` `GetDiscontinuedProducts`. L’appel de la procédure stockée provoque l’exécution du code managé correspondant dans la méthode `GetDiscontinuedProducts`. Ce code émet une requête `SELECT` pour retourner tous les produits qui sont abandonnés et retourne ces données à l’application appelante, qui est SQL Server Management Studio dans cette instance. Management Studio reçoit ces résultats et les affiche dans la fenêtre résultats.

[![la procédure stockée GetDiscontinuedProducts retourne tous les produits abandonnés](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image23.png)

**Figure 13**: la procédure stockée `GetDiscontinuedProducts` retourne tous les produits abandonnés ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image25.png))

## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Étape 5 : création de procédures stockées managées acceptant les paramètres d’entrée

La plupart des requêtes et procédures stockées que nous avons créées dans ces didacticiels ont utilisé des *paramètres*. Par exemple, dans le didacticiel [création de nouvelles procédures stockées pour les TableAdapters de DataSet typé](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) , nous avons créé une procédure stockée nommée `GetProductsByCategoryID` qui acceptait un paramètre d’entrée nommé `@CategoryID`. La procédure stockée retournait alors tous les produits dont le champ `CategoryID` correspondait à la valeur du paramètre `@CategoryID` fourni.

Pour créer une procédure stockée managée qui accepte les paramètres d’entrée, il vous suffit de spécifier ces paramètres dans la définition de la méthode s. Pour illustrer cela, ajoutez une autre procédure stockée managée au projet `ManagedDatabaseConstructs` nommé `GetProductsWithPriceLessThan`. Cette procédure stockée managée accepte un paramètre d’entrée qui spécifie un prix et retourne tous les produits dont le champ `UnitPrice` est inférieur à la valeur du paramètre s.

Pour ajouter une nouvelle procédure stockée au projet, cliquez avec le bouton droit sur le nom du projet `ManagedDatabaseConstructs` et choisissez d’ajouter une nouvelle procédure stockée. Nommez le fichier `GetProductsWithPriceLessThan.vb`. Comme nous l’avons vu à l’étape 3, cette opération crée un fichier de classe Visual Basic avec une méthode nommée `GetProductsWithPriceLessThan` placée dans la classe `Partial` `StoredProcedures`.

Mettez à jour la définition de la méthode `GetProductsWithPriceLessThan` pour qu’elle accepte un paramètre d’entrée [`SqlMoney`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) nommé `price` et écrivez le code pour exécuter et retourner les résultats de la requête :

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample6.vb)]

La définition et le code de la méthode `GetProductsWithPriceLessThan` ressemble étroitement à la définition et au code de la méthode `GetDiscontinuedProducts` créée à l’étape 3. Les seules différences sont que la méthode `GetProductsWithPriceLessThan` accepte comme paramètre d’entrée (`price`), que la requête `SqlCommand` s comprend un paramètre (`@MaxPrice`) et qu’un paramètre est ajouté à la collection `SqlCommand` s `Parameters` est et que la valeur de la variable `price` leur est assignée.

Après avoir ajouté ce code, Redéployez le projet SQL Server. Ensuite, revenez à SQL Server Management Studio et actualisez le dossier procédures stockées. Vous devez voir une nouvelle entrée, `GetProductsWithPriceLessThan`. Dans une fenêtre de requête, entrez et exécutez la commande `exec GetProductsWithPriceLessThan 25`, qui répertorie tous les produits inférieurs à $25, comme illustré dans la figure 14.

[![produits sous $25 sont affichés](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image26.png)

**Figure 14**: affichage des produits sous $25 ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image28.png))

## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Étape 6 : appel de la procédure stockée managée à partir de la couche d’accès aux données

À ce stade, nous avons ajouté le `GetDiscontinuedProducts` et `GetProductsWithPriceLessThan` procédures stockées managées au projet `ManagedDatabaseConstructs` et vous les avez inscrits auprès de la base de données Northwind SQL Server. Nous avons également appelé ces procédures stockées managées à partir de SQL Server Management Studio (voir les figures 13 et 14). Toutefois, pour que notre application ASP.NET utilise ces procédures stockées managées, nous devons les ajouter aux couches de logique métier et d’accès aux données dans l’architecture. Dans cette étape, nous allons ajouter deux nouvelles méthodes au `ProductsTableAdapter` dans le `NorthwindWithSprocs` DataSet typé, qui a été initialement créé dans le didacticiel [création de nouvelles procédures stockées pour les TableAdapters de DataSet typé](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) . À l’étape 7, nous ajouterons les méthodes correspondantes à la couche BLL.

Ouvrez le `NorthwindWithSprocs` DataSet typé dans Visual Studio et commencez par ajouter une nouvelle méthode au `ProductsTableAdapter` nommé `GetDiscontinuedProducts`. Pour ajouter une nouvelle méthode à un TableAdapter, cliquez avec le bouton droit sur le nom du TableAdapter dans le concepteur, puis choisissez l’option Ajouter une requête dans le menu contextuel.

> [!NOTE]
> Étant donné que nous avons déplacé la base de données Northwind du dossier `App_Data` vers l’instance de base de données SQL Server 2005 Express Edition, il est impératif que la chaîne de connexion correspondante dans Web. config soit mise à jour pour refléter cette modification. À l’étape 2, nous avons abordé la mise à jour de la valeur `NORTHWNDConnectionString` dans `Web.config`. Si vous avez oublié d’effectuer cette mise à jour, vous verrez le message d’erreur Échec de l’ajout de la requête. Impossible de trouver `NORTHWNDConnectionString` de connexion pour l’objet `Web.config` dans une boîte de dialogue lors de la tentative d’ajout d’une nouvelle méthode au TableAdapter. Pour résoudre cette erreur, cliquez sur OK, puis accédez à `Web.config` et mettez à jour la valeur de `NORTHWNDConnectionString`, comme indiqué à l’étape 2. Essayez ensuite d’ajouter à nouveau la méthode au TableAdapter. Cette fois-ci, elle doit fonctionner sans erreur.

L’ajout d’une nouvelle méthode lance l’Assistant Configuration de requêtes TableAdapter, que nous avons utilisé plusieurs fois dans les didacticiels précédents. La première étape nous invite à spécifier comment le TableAdapter doit accéder à la base de données : par le biais d’une instruction SQL ad hoc ou via une procédure stockée nouvelle ou existante. Étant donné que nous avons déjà créé et inscrit le `GetDiscontinuedProducts` procédure stockée gérée avec la base de données, choisissez l’option utiliser une procédure stockée existante et appuyez sur suivant.

[![choisissez l’option utiliser une procédure stockée existante](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image29.png)

**Figure 15**: sélectionner l’option utiliser une procédure stockée existante ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image31.png))

L’écran suivant nous invite à entrer la procédure stockée que la méthode appellera. Choisissez la `GetDiscontinuedProducts` procédure stockée gérée dans la liste déroulante et appuyez sur suivant.

[![sélectionnez la procédure stockée managée GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image32.png)

**Figure 16**: sélectionner la procédure stockée gérée `GetDiscontinuedProducts` ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image34.png))

Nous sommes ensuite invités à spécifier si la procédure stockée retourne des lignes, une seule valeur ou rien. Étant donné que `GetDiscontinuedProducts` retourne le jeu de lignes de produits abandonnées, choisissez la première option (données tabulaires), puis cliquez sur suivant.

[![sélectionnez l’option données tabulaires](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image35.png)

**Figure 17**: sélectionner l’option données tabulaires ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image37.png))

L’écran final de l’Assistant nous permet de spécifier les modèles d’accès aux données utilisés et les noms des méthodes qui en résultent. Laissez les cases cochées et nommez les méthodes `FillByDiscontinued` et `GetDiscontinuedProducts`. Cliquez sur Terminer pour terminer l'Assistant.

[![nommez les méthodes FillByDiscontinued et GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image38.png)

**Figure 18**: nommer les méthodes `FillByDiscontinued` et `GetDiscontinuedProducts` ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image40.png))

Répétez ces étapes pour créer des méthodes nommées `FillByPriceLessThan` et `GetProductsWithPriceLessThan` dans le `ProductsTableAdapter` pour la `GetProductsWithPriceLessThan` procédure stockée managée.

La figure 19 illustre une capture d’écran du concepteur de DataSet après l’ajout des méthodes au `ProductsTableAdapter` pour le `GetDiscontinuedProducts` et `GetProductsWithPriceLessThan` les procédures stockées managées.

[![ProductsTableAdapter comprend les nouvelles méthodes ajoutées à cette étape.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image41.png)

**Figure 19**: le `ProductsTableAdapter` comprend les nouvelles méthodes ajoutées à cette étape ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image43.png))

## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Étape 7 : ajout de méthodes correspondantes à la couche de logique métier

Maintenant que nous avons mis à jour la couche d’accès aux données pour inclure des méthodes permettant d’appeler les procédures stockées managées ajoutées aux étapes 4 et 5, nous devons ajouter les méthodes correspondantes à la couche de logique métier. Ajoutez les deux méthodes suivantes à la classe `ProductsBLLWithSprocs` :

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample7.vb)]

Les deux méthodes appellent simplement la méthode DAL correspondante et retournent l’instance `ProductsDataTable`. Le balisage `DataObjectMethodAttribute` au-dessus de chaque méthode entraîne l’inclusion de ces méthodes dans la liste déroulante de l’onglet sélectionner de l’Assistant Configuration de la source de données ObjectDataSource s.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Étape 8 : appel des procédures stockées managées à partir de la couche de présentation

Avec la logique métier et les couches d’accès aux données augmentées pour inclure la prise en charge de l’appel de la `GetDiscontinuedProducts` et `GetProductsWithPriceLessThan` les procédures stockées managées, nous pouvons maintenant afficher les résultats de ces procédures stockées dans une page ASP.NET.

Ouvrez la page `ManagedFunctionsAndSprocs.aspx` dans le dossier `AdvancedDAL` et, à partir de la boîte à outils, faites glisser un contrôle GridView sur le concepteur. Définissez la propriété GridView s `ID` sur `DiscontinuedProducts` et, à partir de sa balise active, liez-la à un nouvel ObjectDataSource nommé `DiscontinuedProductsDataSource`. Configurez le ObjectDataSource pour extraire ses données de la méthode de `GetDiscontinuedProducts` de la classe `ProductsBLLWithSprocs`.

[![configurer ObjectDataSource pour utiliser la classe ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image44.png)

**Figure 20**: configurer ObjectDataSource pour utiliser la classe `ProductsBLLWithSprocs` ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image46.png))

[![choisir la méthode GetDiscontinuedProducts dans la liste déroulante de l’onglet sélectionner](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image47.png)

**Figure 21**: choisir la méthode de `GetDiscontinuedProducts` dans la liste déroulante de l’onglet sélectionner ([cliquez pour afficher l’image en plein écran](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image49.png))

Étant donné que cette grille sera utilisée pour afficher uniquement des informations sur les produits, définissez les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun), puis cliquez sur Terminer.

À la fin de l’Assistant, Visual Studio ajoute automatiquement un BoundField ou un CheckBoxField pour chaque champ de données dans la `ProductsDataTable`. Prenez un moment pour supprimer tous ces champs à l’exception de `ProductName` et `Discontinued`, à partir duquel vos balises déclaratives GridView et ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample8.aspx)]

Prenez un moment pour afficher cette page par le biais d’un navigateur. Lorsque la page est visitée, ObjectDataSource appelle la méthode de `GetDiscontinuedProducts` de la classe `ProductsBLLWithSprocs`. Comme nous l’avons vu à l’étape 7, cette méthode appelle la méthode DAL s `ProductsDataTable` classe s `GetDiscontinuedProducts`, qui appelle la procédure stockée `GetDiscontinuedProducts`. Cette procédure stockée est une procédure stockée gérée qui exécute le code que nous avons créé à l’étape 3, retournant les produits abandonnés.

Les résultats retournés par la procédure stockée managée sont regroupés dans un `ProductsDataTable` par la couche DAL, puis retournés à la couche BLL, qui les retourne à la couche de présentation où ils sont liés au GridView et affichés. Comme prévu, la grille répertorie les produits qui ont été abandonnés.

[![les produits abandonnés sont répertoriés](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image50.png)

**Figure 22**: les produits abandonnés sont répertoriés ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image52.png))

Pour une meilleure pratique, ajoutez une zone de texte et un autre GridView à la page. Faire en sorte que ce GridView affiche les produits inférieurs à la quantité entrée dans la zone de texte en appelant la méthode `ProductsBLLWithSprocs` classe s `GetProductsWithPriceLessThan`.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Étape 9 : création et appel des fonctions définies par l’utilisateur T-SQL

Les fonctions définies par l’utilisateur, ou UDF, sont des objets de base de données qui reproduisent fidèlement la sémantique des fonctions dans les langages de programmation. À l’instar d’une fonction dans Visual Basic, les fonctions définies par l’utilisateur peuvent inclure un nombre variable de paramètres d’entrée et retourner une valeur d’un type particulier. Une fonction définie par l’attaquant peut retourner des données scalaires (une chaîne, un entier, etc.) ou des données tabulaires. Voyons rapidement les deux types de fonctions définies par l’utilisateur, à partir d’une FDU qui retourne un type de données scalaire.

La FDU suivante calcule la valeur estimée de l’inventaire pour un produit particulier. Pour ce faire, il prend trois paramètres d’entrée : les valeurs `UnitPrice`, `UnitsInStock`et `Discontinued` pour un produit particulier, et retourne une valeur de type `money`. Il calcule la valeur estimée de l’inventaire en multipliant le `UnitPrice` par le `UnitsInStock`. Pour les éléments abandonnés, cette valeur est divisée par deux.

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample9.sql)]

Une fois cette FDU ajoutée à la base de données, elle peut être trouvée via Management Studio en développant le dossier programmabilité, les fonctions, puis les fonctions scalaires. Il peut être utilisé dans une requête de `SELECT` comme suit :

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample10.sql)]

J’ai ajouté le `udf_ComputeInventoryValue` UDF à la base de données Northwind. La figure 23 illustre la sortie de la requête `SELECT` ci-dessus lorsqu’elle est consultée via Management Studio. Notez également que la FDU est listée sous le dossier scalaire-value Functions dans l’Explorateur d’objets.

[![chaque valeur d’inventaire des produits est indiquée](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image53.png)

**Figure 23**: les valeurs d’inventaire de chaque produit sont listées ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image55.png))

Les fonctions définies par l’utilisateur peuvent également retourner des données tabulaires. Par exemple, nous pouvons créer une fonction UDF qui retourne des produits appartenant à une catégorie particulière :

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample11.sql)]

Le `udf_GetProductsByCategoryID` UDF accepte un paramètre d’entrée `@CategoryID` et retourne les résultats de la requête de `SELECT` spécifiée. Une fois créé, cette FDU peut être référencée dans la clause `FROM` (ou `JOIN`) d’une requête `SELECT`. L’exemple suivant retourne les valeurs `ProductID`, `ProductName`et `CategoryID` pour chacune des boissons.

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample12.sql)]

J’ai ajouté le `udf_GetProductsByCategoryID` UDF à la base de données Northwind. La figure 24 affiche la sortie de la requête `SELECT` ci-dessus lorsqu’elle est consultée via Management Studio. Les fonctions définies par l’utilisateur qui retournent des données tabulaires se trouvent dans le dossier de l’Explorateur d’objets.

[![ProductID, ProductName et CategoryID sont répertoriés pour chaque boisson](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image56.png)

**Figure 24**: `ProductID`, `ProductName`et `CategoryID` sont répertoriés pour chaque boisson ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image58.png))

> [!NOTE]
> Pour plus d’informations sur la création et l’utilisation des [fonctions définies par l’utilisateur, consultez Introduction aux fonctions définies par l’utilisateur](http://www.sqlteam.com/item.asp?ItemID=1955). Découvrez également les [avantages et les inconvénients des fonctions définies par l’utilisateur](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).

## <a name="step-10-creating-a-managed-udf"></a>Étape 10 : création d’une FDU managée

Les `udf_ComputeInventoryValue` et `udf_GetProductsByCategoryID` les fonctions définies par l’utilisateur créées dans les exemples ci-dessus sont des objets de base de données T-SQL. SQL Server 2005 prend également en charge les fonctions définies par l’utilisateur managées, qui peuvent être ajoutées au projet `ManagedDatabaseConstructs` comme les procédures stockées managées des étapes 3 et 5. Pour cette étape, nous allons implémenter le `udf_ComputeInventoryValue` UDF dans du code managé.

Pour ajouter une FDU managée au projet `ManagedDatabaseConstructs`, cliquez avec le bouton droit sur le nom du projet dans Explorateur de solutions et choisissez d’ajouter un nouvel élément. Sélectionnez le modèle défini par l’utilisateur dans la boîte de dialogue Ajouter un nouvel élément et nommez le nouveau fichier UDF `udf_ComputeInventoryValue_Managed.vb`.

[![ajouter une nouvelle FDU managée au projet ManagedDatabaseConstructs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image59.png)

**Figure 25**: ajouter une nouvelle FDU managée au projet `ManagedDatabaseConstructs` ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image61.png))

Le modèle de fonction définie par l’utilisateur crée une classe `Partial` nommée `UserDefinedFunctions` avec une méthode dont le nom est le même que le nom du fichier de classe s (`udf_ComputeInventoryValue_Managed`, dans cette instance). Cette méthode est décorée à l’aide de l' [attribut`SqlFunction`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), qui marque la méthode comme une fonction UDF managée.

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample13.vb)]

La méthode `udf_ComputeInventoryValue` retourne actuellement un [objet`SqlString`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) et n’accepte aucun paramètre d’entrée. Nous devons mettre à jour la définition de la méthode afin qu’elle accepte trois paramètres d’entrée : `UnitPrice`, `UnitsInStock`et `Discontinued`-et retourne un objet `SqlMoney`. La logique de calcul de la valeur d’inventaire est identique à celle de l' `udf_ComputeInventoryValue` UDF T-SQL.

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample14.vb)]

Notez que les paramètres d’entrée de la méthode UDF ont les types SQL correspondants : `SqlMoney` pour le champ `UnitPrice`, [`SqlInt16`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) pour `UnitsInStock`et [`SqlBoolean`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) pour `Discontinued`. Ces types de données reflètent les types définis dans la table `Products` : la colonne `UnitPrice` est de type `money`, la colonne `UnitsInStock` de type `smallint`et la colonne `Discontinued` de type `bit`.

Le code commence par créer une instance `SqlMoney` nommée `inventoryValue` à laquelle la valeur 0 lui est assignée. La table `Products` permet les valeurs de `NULL` de base de données dans les colonnes `UnitsInPrice` et `UnitsInStock`. Par conséquent, nous devons d’abord vérifier si ces valeurs contiennent `NULL` s, que nous faisons par le biais de la propriété `SqlMoney` des objets [`IsNull`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Si `UnitPrice` et `UnitsInStock` contiennent des valeurs non-`NULL`, nous calculons le `inventoryValue` pour être le produit des deux. Ensuite, si `Discontinued` a la valeur true, nous allons diviser la valeur.

> [!NOTE]
> L’objet `SqlMoney` autorise la multiplication simultanée de deux instances `SqlMoney`. Elle n’autorise pas la multiplication d’une instance `SqlMoney` par un nombre à virgule flottante littéral. Par conséquent, pour diviser `inventoryValue` nous la multiplions par une nouvelle instance `SqlMoney` qui a la valeur 0,5.

## <a name="step-11-deploying-the-managed-udf"></a>Étape 11 : déploiement de la FDU managée

Maintenant que la fonction UDF managée a été créée, nous sommes prêts à la déployer dans la base de données Northwind. Comme nous l’avons vu à l’étape 4, les objets gérés d’un SQL Server projet sont déployés en cliquant avec le bouton droit sur le nom du projet dans la Explorateur de solutions et en choisissant l’option déployer dans le menu contextuel.

Une fois que vous avez déployé le projet, revenez à SQL Server Management Studio et actualisez le dossier des fonctions à valeurs scalaires. Vous devez maintenant voir deux entrées :

- `dbo.udf_ComputeInventoryValue`-le fichier UDF T-SQL créé à l’étape 9, et
- `dbo.udf ComputeInventoryValue_Managed`-la FDU managée créée à l’étape 10 qui vient d’être déployée.

Pour tester cette FDU managée, exécutez la requête suivante à partir de Management Studio :

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample15.sql)]

Cette commande utilise la fonction UDF `udf ComputeInventoryValue_Managed` managée au lieu de la fonction définie par l' `udf_ComputeInventoryValue` T-SQL, mais la sortie est la même. Reportez-vous à la figure 23 pour afficher une capture d’écran de la sortie UDF s.

## <a name="step-12-debugging-the-managed-database-objects"></a>Étape 12 : débogage des objets de base de données managés

Dans le didacticiel [débogage des procédures stockées](debugging-stored-procedures-vb.md) , nous avons abordé les trois options de débogage SQL Server par le biais de Visual Studio : débogage de base de données direct, débogage d’application et débogage à partir d’un projet SQL Server. Les objets de base de données managés ne peuvent pas être débogués via le débogage de base de données direct, mais peuvent être débogués à partir d’une application cliente et directement à partir du projet SQL Server. Toutefois, pour que le débogage fonctionne, la base de données SQL Server 2005 doit autoriser le débogage SQL/CLR. Rappelez-vous que lorsque nous avons créé le projet `ManagedDatabaseConstructs` Visual Studio nous a demandé si nous voulions activer le débogage SQL/CLR (voir la figure 6 à l’étape 2). Ce paramètre peut être modifié en cliquant avec le bouton droit sur la base de données à partir de la fenêtre Explorateur de serveurs.

![Vérifier que la base de données autorise le débogage SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image62.png)

**Figure 26**: vérifier que la base de données autorise le débogage SQL/CLR

Imaginez que nous voulions déboguer le `GetProductsWithPriceLessThan` procédure stockée managée. Nous commençons par définir un point d’arrêt dans le code de la méthode `GetProductsWithPriceLessThan`.

[![définir un point d’arrêt dans la méthode GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image63.png)

**Figure 27**: définir un point d’arrêt dans la méthode `GetProductsWithPriceLessThan` ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image65.png))

Commençons par examiner le débogage des objets de base de données managés à partir du projet SQL Server. Dans la mesure où notre solution comprend deux projets : le `ManagedDatabaseConstructs` SQL Server projet avec notre site Web, afin de déboguer à partir du projet SQL Server, nous devons demander à Visual Studio de lancer le projet `ManagedDatabaseConstructs` SQL Server quand nous commençons le débogage. Cliquez avec le bouton droit sur le projet `ManagedDatabaseConstructs` dans Explorateur de solutions et choisissez l’option définir comme projet de démarrage dans le menu contextuel.

Lorsque le projet `ManagedDatabaseConstructs` est lancé à partir du débogueur, il exécute les instructions SQL dans le fichier `Test.sql`, qui se trouve dans le dossier `Test Scripts`. Par exemple, pour tester le `GetProductsWithPriceLessThan` procédure stockée managée, remplacez le contenu du fichier `Test.sql` existant par l’instruction suivante, qui appelle la procédure stockée managée `GetProductsWithPriceLessThan` en passant la valeur `@CategoryID` de 14,95 :

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample16.sql)]

Une fois que vous avez entré le script ci-dessus dans `Test.sql`, démarrez le débogage en accédant au menu Déboguer et en choisissant Démarrer le débogage ou en appuyant sur F5 ou en cliquant sur l’icône de lecture verte dans la barre d’outils. Cela génère les projets dans la solution, déploie les objets de base de données managés dans la base de données Northwind, puis exécute le script `Test.sql`. À ce stade, le point d’arrêt est atteint et nous pouvons parcourir la méthode `GetProductsWithPriceLessThan`, examiner les valeurs des paramètres d’entrée, et ainsi de suite.

[![le point d’arrêt dans la méthode GetProductsWithPriceLessThan a été atteint](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image66.png)

**Figure 28**: le point d’arrêt dans la méthode `GetProductsWithPriceLessThan` a été atteint ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image68.png))

Pour qu’un objet SQL Database soit débogué par le biais d’une application cliente, il est impératif que la base de données soit configurée pour prendre en charge le débogage d’application. Dans Explorateur de serveurs, cliquez avec le bouton droit sur la base de données et assurez-vous que l’option débogage de l’application est cochée. En outre, nous devons configurer l’application ASP.NET pour qu’elle s’intègre au débogueur SQL et désactiver le regroupement de connexions. Ces étapes ont été abordées en détail à l’étape 2 du didacticiel [débogage des procédures stockées](debugging-stored-procedures-vb.md) .

Une fois que vous avez configuré l’application et la base de données ASP.NET, définissez le site Web ASP.NET comme projet de démarrage et démarrez le débogage. Si vous visitez une page qui appelle l’un des objets managés qui a un point d’arrêt, l’application s’arrêtera et le contrôle sera basculé vers le débogueur, où vous pouvez parcourir le code, comme illustré dans la figure 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Étape 13 : compilation et déploiement manuels des objets de base de données managés

Les projets SQL Server facilitent la création, la compilation et le déploiement d’objets de base de données managés. Malheureusement, les projets SQL Server ne sont disponibles que dans les éditions Professional et Team Systems de Visual Studio. Si vous utilisez Visual Web Developer ou l’édition standard de Visual Studio et souhaitez utiliser des objets de base de données managés, vous devez les créer et les déployer manuellement. Cela implique quatre étapes :

1. Créez un fichier qui contient le code source de l’objet de base de données managé,
2. Compilez l’objet dans un assembly,
3. Enregistrez l’assembly avec la base de données SQL Server 2005 et
4. Créez un objet de base de données dans SQL Server qui pointe vers la méthode appropriée dans l’assembly.

Pour illustrer ces tâches, créons une nouvelle procédure stockée managée qui retourne les produits dont la `UnitPrice` est supérieure à une valeur spécifiée. Créez un fichier sur votre ordinateur nommé `GetProductsWithPriceGreaterThan.vb` et entrez le code suivant dans le fichier (vous pouvez utiliser Visual Studio, le bloc-notes ou n’importe quel éditeur de texte pour y parvenir) :

[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample17.vb)]

Ce code est quasiment identique à celui de la méthode `GetProductsWithPriceLessThan` créée à l’étape 5. Les seules différences sont les noms de méthode, la clause `WHERE` et le nom de paramètre utilisé dans la requête. De retour dans la méthode `GetProductsWithPriceLessThan`, la clause `WHERE` Read : `WHERE UnitPrice < @MaxPrice`. Ici, dans `GetProductsWithPriceGreaterThan`, nous utilisons : `WHERE UnitPrice > @MinPrice`.

Nous devons maintenant compiler cette classe dans un assembly. À partir de la ligne de commande, accédez au répertoire où vous avez enregistré le fichier `GetProductsWithPriceGreaterThan.vb` C# et utilisez le compilateur (`csc.exe`) pour compiler le fichier de classe dans un assembly :

[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample18.cmd)]

Si le dossier contenant v `bc.exe` dans not du système `PATH`, vous devrez référencer entièrement son chemin d’accès, `%WINDOWS%\Microsoft.NET\Framework\version\`, comme suit :

[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample19.cmd)]

[![compiler GetProductsWithPriceGreaterThan. vb dans un assembly](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image69.png)

**Figure 29**: compiler `GetProductsWithPriceGreaterThan.vb` dans un assembly ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image71.png))

L’indicateur `/t` spécifie que le fichier de classe Visual Basic doit être compilé dans une DLL (plutôt que dans un exécutable). L’indicateur `/out` spécifie le nom de l’assembly résultant.

> [!NOTE]
> Au lieu de compiler le fichier de classe `GetProductsWithPriceGreaterThan.vb` à partir de la ligne de commande, vous pouvez également utiliser [Visual Basic Express Edition](https://msdn.microsoft.com/vstudio/express/vb/) ou créer un projet de bibliothèque de classes distinct dans Visual Studio Standard Edition. S Ren Jacob Lauritsen a fourni un projet de Visual Basic Express Edition avec le code pour la procédure stockée `GetProductsWithPriceGreaterThan` et les deux procédures stockées managées et UDF créées aux étapes 3, 5 et 10. Le projet s Ren s comprend également les commandes T-SQL nécessaires pour ajouter les objets de base de données correspondants.

Une fois le code compilé dans un assembly, nous sommes prêts à inscrire l’assembly dans la base de données SQL Server 2005. Cela peut être effectué via T-SQL, à l’aide de la `CREATE ASSEMBLY`de commande ou via SQL Server Management Studio. Nous allons nous concentrer sur l’utilisation de Management Studio.

Dans Management Studio, développez le dossier programmabilité dans la base de données Northwind. L’un de ses sous-dossiers est Assemblies. Pour ajouter manuellement un nouvel assembly à la base de données, cliquez avec le bouton droit sur le dossier assemblys, puis choisissez nouvel assembly dans le menu contextuel. La boîte de dialogue nouvel assembly s’affiche (voir figure 30). Cliquez sur le bouton Parcourir, sélectionnez l’assembly `ManuallyCreatedDBObjects.dll` que nous venons de compiler, puis cliquez sur OK pour ajouter l’assembly à la base de données. Vous ne devriez pas voir l’assembly `ManuallyCreatedDBObjects.dll` dans l’Explorateur d’objets.

[![ajouter l’assembly ManuallyCreatedDBObjects. dll à la base de données](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image72.png)

**Figure 30**: ajouter l’assembly `ManuallyCreatedDBObjects.dll` à la base de données ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image74.png))

![ManuallyCreatedDBObjects. dll est listé dans l’Explorateur d’objets](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image75.png)

**Figure 31**: le `ManuallyCreatedDBObjects.dll` est listé dans l’Explorateur d’objets

Pendant que nous avons ajouté l’assembly à la base de données Northwind, nous n’avons pas encore associé une procédure stockée à la méthode `GetProductsWithPriceGreaterThan` de l’assembly. Pour ce faire, ouvrez une nouvelle fenêtre de requête et exécutez le script suivant :

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample20.sql)]

Cela crée une procédure stockée dans la base de données Northwind nommée `GetProductsWithPriceGreaterThan` et l’associe à la méthode managée `GetProductsWithPriceGreaterThan` (qui se trouve dans la classe `StoredProcedures`, qui se trouve dans l’assembly `ManuallyCreatedDBObjects`).

Après l’exécution du script ci-dessus, actualisez le dossier procédures stockées dans l’Explorateur d’objets. Vous devez voir une nouvelle entrée de procédure stockée, `GetProductsWithPriceGreaterThan`, à laquelle est associée une icône de verrou. Pour tester cette procédure stockée, entrez et exécutez le script suivant dans la fenêtre de requête :

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample21.sql)]

Comme le montre la figure 32, la commande ci-dessus affiche des informations pour ces produits avec une `UnitPrice` supérieure à $24,95.

[![ManuallyCreatedDBObjects. dll est listé dans l’Explorateur d’objets](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image76.png)

**Figure 32**: le `ManuallyCreatedDBObjects.dll` est listé dans l’Explorateur d’objets ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image78.png))

## <a name="summary"></a>Récapitulatif

Microsoft SQL Server 2005 fournit une intégration avec le Common Language Runtime (CLR), qui permet de créer des objets de base de données à l’aide de code managé. Auparavant, ces objets de base de données pouvaient uniquement être créés à l’aide de T-SQL, mais nous pouvons maintenant créer ces objets à l’aide de langages de programmation .NET comme Visual Basic. Dans ce didacticiel, nous avons créé deux procédures stockées managées et une fonction définie par l’utilisateur gérée.

Le type de projet SQL Server Visual Studio facilite la création, la compilation et le déploiement d’objets de base de données managés. En outre, il offre une prise en charge riche du débogage. Toutefois, SQL Server types de projets ne sont disponibles que dans les éditions Professional et Team Systems de Visual Studio. Pour ceux qui utilisent Visual Web Developer ou l’édition standard de Visual Studio, les étapes de création, de compilation et de déploiement doivent être effectuées manuellement, comme nous l’avons vu à l’étape 13.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Avantages et inconvénients des fonctions définies par l’utilisateur](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Création d’objets SQL Server 2005 dans du code managé](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Création de déclencheurs à l’aide de code managé dans SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Comment : créer et exécuter une procédure stockée SQL Server CLR](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Comment : créer et exécuter une fonction CLR SQL Server définie par l’utilisateur](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Comment : modifier le script de `Test.sql` pour exécuter des objets SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Présentation des fonctions définies par l’utilisateur](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Code managé et SQL Server 2005 (vidéo)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Référence Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Procédure pas à pas : création d’une procédure stockée dans du code managé](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était S Ren Jacob Lauritsen. En plus d’examiner cet article, S Ren a également créé le C# projet Visual Express Edition inclus dans cet article pour la compilation manuelle des objets de base de données managés. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](debugging-stored-procedures-vb.md)
