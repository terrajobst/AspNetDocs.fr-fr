---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Optimisation des performances avec la Entity Framework 4,0 dans une application Web ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le Prise en main avec la série de didacticiels Entity Framework 4,0. ...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5630200a1ad1d30f6d89b38e15179f15b699fa9f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78545961"
---
# <a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Optimisation des performances avec la Entity Framework 4,0 dans une application Web ASP.NET 4

par [Tom Dykstra](https://github.com/tdykstra)

> Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le [prise en main avec la](https://asp.net/entity-framework/tutorials#Getting%20Started) série de didacticiels Entity Framework 4,0. Si vous n’avez pas terminé les didacticiels précédents, comme point de départ pour ce didacticiel, vous pouvez [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous auriez créée. Vous pouvez également [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créée par la série de didacticiels complète. Si vous avez des questions sur les didacticiels, vous pouvez les poster sur le [Forum de Entity Framework ASP.net](https://forums.asp.net/1227.aspx).

Dans le didacticiel précédent, vous avez vu comment gérer les conflits d’accès concurrentiel. Ce didacticiel présente les options permettant d’améliorer les performances d’une application Web ASP.NET qui utilise l’Entity Framework. Vous apprendrez plusieurs méthodes pour optimiser les performances ou pour diagnostiquer les problèmes de performances.

Les informations présentées dans les sections suivantes sont susceptibles d’être utiles dans un large éventail de scénarios :

- Chargez efficacement les données associées.
- Gérer l’état d’affichage.

Les informations présentées dans les sections suivantes peuvent être utiles si vous avez des requêtes individuelles qui présentent des problèmes de performances :

- Utilisez l’option de fusion `NoTracking`.
- Précompilez des requêtes LINQ.
- Examinez les commandes de requête envoyées à la base de données.

Les informations présentées dans la section suivante sont potentiellement utiles pour les applications qui ont des modèles de données extrêmement volumineux :

- Prégénérez les vues.

> [!NOTE]
> Les performances de l’application Web sont affectées par de nombreux facteurs, notamment la taille des données de demande et de réponse, la vitesse des requêtes de base de données, le nombre de demandes que le serveur peut mettre en file d’attente et la rapidité avec laquelle il peut les traiter, et même l’efficacité de tout les bibliothèques de scripts client que vous utilisez peut-être. Si les performances sont critiques dans votre application, ou si les tests ou l’expérience montrent que les performances de l’application ne sont pas satisfaisantes, vous devez suivre le protocole normal pour le réglage des performances. Mesurez les cas où les goulots d’étranglement de performances se produisent, puis traitez les zones qui auront le plus grand impact sur les performances globales de l’application.
> 
> Cette rubrique se concentre principalement sur la façon dont vous pouvez potentiellement améliorer les performances de la Entity Framework dans ASP.NET. Les suggestions ici sont utiles si vous déterminez que l’accès aux données est l’un des goulots d’étranglement de performances dans votre application. Sauf mention contraire, les méthodes expliquées ici ne doivent pas être considérées comme &quot;meilleures pratiques&quot; en général. beaucoup d’entre elles sont appropriées uniquement dans des situations exceptionnelles ou pour résoudre des types spécifiques de goulots d’étranglement de performances.

Pour démarrer le didacticiel, démarrez Visual Studio et ouvrez l’application Web Contoso University avec laquelle vous travailliez dans le didacticiel précédent.

## <a name="efficiently-loading-related-data"></a>Chargement efficace de données associées

La Entity Framework peut charger des données associées de plusieurs façons dans les propriétés de navigation d’une entité :

- *Chargement différé*. Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées. Toutefois, la première fois que vous essayez d’accéder à une propriété de navigation, les données requises pour cette propriété de navigation sont récupérées automatiquement. Cela entraîne l’envoi de plusieurs requêtes à la base de données : une pour l’entité elle-même et une autre chaque fois que les données associées pour l’entité doivent être récupérées. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Chargement hâtif*. Quand l’entité est lue, ses données associées sont également récupérées. Cela génère en général une requête de jointure unique qui récupère toutes les données nécessaires. Vous spécifiez le chargement hâtif à l’aide de la méthode `Include`, comme vous l’avez déjà vu dans ces didacticiels.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Chargement explicite*. Cela est similaire au chargement différé, à ceci près que vous récupérez explicitement les données associées dans le code ; elle ne se produit pas automatiquement lorsque vous accédez à une propriété de navigation. Vous chargez les données associées manuellement à l’aide de la méthode `Load` de la propriété de navigation pour les collections, ou vous utilisez la méthode `Load` de la propriété de référence pour les propriétés qui contiennent un seul objet. (Par exemple, vous appelez la méthode `PersonReference.Load` pour charger la propriété de navigation `Person` d’une entité `Department`.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Étant donné qu’ils ne récupèrent pas immédiatement les valeurs de propriété, le chargement différé et le chargement explicite sont également connus sous le nom de *chargement différé*.

Le chargement différé est le comportement par défaut d’un contexte d’objet qui a été généré par le concepteur. Si vous ouvrez le fichier *SchoolModel.Designer.cs* qui définit la classe de contexte de l’objet, vous trouverez trois méthodes de constructeur, chacune incluant l’instruction suivante :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

En général, si vous savez que vous avez besoin de données associées pour chaque entité Récupérée, le chargement hâtif offre les meilleures performances, car une seule requête envoyée à la base de données est généralement plus efficace que les requêtes distinctes pour chaque entité récupérée. En revanche, si vous devez accéder aux propriétés de navigation d’une entité rarement ou uniquement pour un petit ensemble d’entités, le chargement différé ou le chargement explicite peut être plus efficace, car le chargement hâtif récupérerait plus de données que nécessaire.

Dans une application Web, le chargement différé peut être relativement peu utile, car les actions de l’utilisateur qui affectent le besoin de données associées ont lieu dans le navigateur, qui n’a pas de connexion au contexte de l’objet qui a rendu la page. En revanche, lorsque vous liez un contrôle, vous connaissez généralement les données dont vous avez besoin, et il est généralement préférable de choisir un chargement hâtif ou un chargement différé en fonction de ce qui est approprié dans chaque scénario.

En outre, un contrôle DataBound peut utiliser un objet d’entité après la suppression du contexte de l’objet. Dans ce cas, une tentative de chargement différé d’une propriété de navigation échoue. Le message d’erreur que vous recevez est clair : &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

Le contrôle `EntityDataSource` désactive le chargement différé par défaut. Pour le contrôle de `ObjectDataSource` que vous utilisez pour le didacticiel actuel (ou si vous accédez au contexte de l’objet à partir du code de la page), il existe plusieurs façons de désactiver le chargement différé par défaut. Vous pouvez la désactiver quand vous instanciez un contexte d’objet. Par exemple, vous pouvez ajouter la ligne suivante à la méthode de constructeur de la classe `SchoolRepository` :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Pour l’application Contoso University, vous allez faire en sorte que le contexte de l’objet désactive automatiquement le chargement différé afin qu’il ne soit pas nécessaire de définir cette propriété chaque fois qu’un contexte est instancié.

Ouvrez le modèle de données *SchoolModel. edmx* , cliquez sur l’aire de conception, puis dans le volet Propriétés, affectez à la propriété **chargement différé activé** la valeur `False`. Enregistrez et fermez le modèle de données.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Gestion de l’état d’affichage

Pour fournir une fonctionnalité de mise à jour, une page Web ASP.NET doit stocker les valeurs de propriété d’origine d’une entité lors du rendu d’une page. Pendant le traitement de la publication (postback), le contrôle peut recréer l’état d’origine de l’entité et appeler la méthode `Attach` de l’entité avant d’appliquer les modifications et d’appeler la méthode `SaveChanges`. Par défaut, ASP.NET Web Forms contrôles de données utilisent l’état d’affichage pour stocker les valeurs d’origine. Toutefois, l’état d’affichage peut affecter les performances, car il est stocké dans des champs masqués qui peuvent augmenter considérablement la taille de la page qui est envoyée vers et depuis le navigateur.

Les techniques de gestion de l’état d’affichage, ou d’autres solutions telles que l’état de session, ne sont pas uniques à la Entity Framework. ce didacticiel n’aborde pas ce sujet en détail. Pour plus d’informations, consultez les liens à la fin du didacticiel.

Toutefois, la version 4 de ASP.NET offre une nouvelle façon de travailler avec l’état d’affichage que chaque développeur ASP.NET d’Web Forms applications doit connaître : la propriété `ViewStateMode`. Cette nouvelle propriété peut être définie au niveau de la page ou du contrôle, et elle vous permet de désactiver par défaut l’état d’affichage d’une page et de l’activer uniquement pour les contrôles qui en ont besoin.

Pour les applications dont les performances sont critiques, il est recommandé de toujours désactiver l’état d’affichage au niveau de la page et de l’activer uniquement pour les contrôles qui l’exigent. La taille de l’état d’affichage dans les pages de l’Université contoso n’aurait pas été sensiblement réduite par cette méthode, mais pour voir comment elle fonctionne, vous le ferez pour la page *enseignants. aspx* . Cette page contient de nombreux contrôles, y compris un contrôle `Label` dont l’état d’affichage est désactivé. Aucun des contrôles de cette page n’a réellement besoin d’avoir l’état d’affichage activé. (La propriété `DataKeyNames` du contrôle `GridView` spécifie l’État qui doit être maintenu entre les publications, mais ces valeurs sont conservées dans l’état du contrôle, ce qui n’est pas affecté par la propriété `ViewStateMode`.)

La directive `Page` et le balisage de contrôle `Label` se comparent actuellement de l’exemple suivant :

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Effectuez les modifications suivantes :

- Ajoutez `ViewStateMode="Disabled"` à la directive `Page`.
- Supprimez `ViewStateMode="Disabled"` du contrôle `Label`.

Le balisage ressemble maintenant à l’exemple suivant :

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

L’état d’affichage est maintenant désactivé pour tous les contrôles. Si, par la suite, vous ajoutez un contrôle qui n’a pas besoin d’utiliser l’état d’affichage, il vous suffit d’inclure l’attribut `ViewStateMode="Enabled"` pour ce contrôle.

## <a name="using-the-notracking-merge-option"></a>Utilisation de l’option de fusion NoTracking

Lorsqu’un contexte d’objet récupère des lignes de base de données et crée des objets d’entité qui les représentent, par défaut, il effectue également le suivi de ces objets d’entité à l’aide de son gestionnaire d’état d’objet. Ces données de suivi jouent le rôle de cache et sont utilisées lorsque vous mettez à jour une entité. Étant donné qu’une application Web possède généralement des instances de contexte d’objet éphémères, les requêtes retournent souvent des données qui n’ont pas besoin d’être suivies, car le contexte de l’objet qui les lit est supprimé avant d’être réutilisé ou mis à jour.

Dans la Entity Framework, vous pouvez spécifier si le contexte de l’objet effectue le suivi des objets d’entité en définissant une *option de fusion*. Vous pouvez définir l’option de fusion pour des requêtes individuelles ou pour des jeux d’entités. Si vous la définissez pour un jeu d’entités, cela signifie que vous définissez l’option de fusion par défaut pour toutes les requêtes créées pour ce jeu d’entités.

Pour l’application Contoso University, le suivi n’est pas nécessaire pour les jeux d’entités auxquels vous accédez à partir du référentiel. vous pouvez donc définir l’option de fusion sur `NoTracking` pour ces jeux d’entités lorsque vous instanciez le contexte de l’objet dans la classe de référentiel. (Notez que dans ce didacticiel, la définition de l’option de fusion n’aura pas d’effet notable sur les performances de l’application. L’option `NoTracking` est susceptible d’améliorer les performances observables uniquement dans certains scénarios de volume de données élevé.)

Dans le dossier DAL, ouvrez le fichier *SchoolRepository.cs* et ajoutez une méthode de constructeur qui définit l’option de fusion pour les jeux d’entités auxquels le référentiel accède :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Précompilation de requêtes LINQ

La première fois que l’Entity Framework exécute une requête Entity SQL dans la durée de vie d’une instance de `ObjectContext` donnée, la compilation de la requête prend un certain temps. Le résultat de la compilation est mis en cache, ce qui signifie que les exécutions ultérieures de la requête sont nettement plus rapides. Les requêtes LINQ suivent un modèle similaire, à ceci près qu’une partie du travail nécessaire à la compilation de la requête est effectuée chaque fois que la requête est exécutée. En d’autres termes, pour les requêtes LINQ, par défaut, tous les résultats de la compilation ne sont pas tous mis en cache.

Si vous avez une requête LINQ que vous prévoyez d’exécuter à plusieurs reprises dans la vie d’un contexte d’objet, vous pouvez écrire du code qui entraîne la mise en cache de tous les résultats de la compilation lors de la première exécution de la requête LINQ.

À titre d’illustration, vous effectuerez cette opération pour deux méthodes `Get` dans la classe `SchoolRepository`, l’une d’entre elles ne prenant aucun paramètre (la méthode `GetInstructorNames`) et l’autre qui nécessite un paramètre (la méthode `GetDepartmentsByAdministrator`). En réalité, ces méthodes ne doivent pas être compilées car elles ne sont pas des requêtes LINQ :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Toutefois, pour pouvoir essayer des requêtes compilées, vous devez procéder comme si elles avaient été écrites en tant que requêtes LINQ suivantes :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Vous pouvez modifier le code dans ces méthodes en fonction de ce qui est indiqué ci-dessus, puis exécuter l’application pour vérifier qu’elle fonctionne avant de continuer. Toutefois, les instructions suivantes vous guident directement dans la création de versions précompilées.

Créez un fichier de classe dans le dossier *dal* , nommez-le *SchoolEntities.cs*et remplacez le code existant par le code suivant :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Ce code crée une classe partielle qui étend la classe de contexte d’objet générée automatiquement. La classe partielle comprend deux requêtes LINQ compilées à l’aide de la méthode `Compile` de la classe `CompiledQuery`. Il crée également des méthodes que vous pouvez utiliser pour appeler les requêtes. Enregistrez et fermez ce fichier.

Ensuite, dans *SchoolRepository.cs*, modifiez les méthodes `GetInstructorNames` et `GetDepartmentsByAdministrator` existantes dans la classe de référentiel afin qu’elles appellent les requêtes compilées :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Exécutez la page *Departments. aspx* pour vérifier qu’elle fonctionne comme auparavant. La méthode `GetInstructorNames` est appelée pour remplir la liste déroulante administrateur et la méthode `GetDepartmentsByAdministrator` est appelée lorsque vous cliquez sur **mettre à jour** afin de vérifier qu’aucun formateur n’est un administrateur de plusieurs départements.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Vous avez des requêtes précompilées dans l’application Contoso University uniquement pour voir comment procéder, pas parce que cela permet d’améliorer considérablement les performances. La précompilation de requêtes LINQ ajoute un niveau de complexité à votre code. Assurez-vous de le faire uniquement pour les requêtes qui représentent en fait des goulots d’étranglement de performances dans votre application.

## <a name="examining-queries-sent-to-the-database"></a>Examen des requêtes envoyées à la base de données

Lorsque vous examinez les problèmes de performances, il est parfois utile de connaître les commandes SQL exactes que l’Entity Framework envoie à la base de données. Si vous utilisez un objet `IQueryable`, vous pouvez utiliser la méthode `ToTraceString` pour ce faire.

Dans *SchoolRepository.cs*, modifiez le code de la méthode `GetDepartmentsByName` pour qu’il corresponde à l’exemple suivant :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

La variable `departments` doit être convertie en un type `ObjectQuery` uniquement parce que la méthode `Where` à la fin de la ligne précédente crée un objet `IQueryable` ; sans la méthode `Where`, le cast n’est pas nécessaire.

Définissez un point d’arrêt sur la ligne de `return`, puis exécutez la page *Departments. aspx* dans le débogueur. Quand vous atteignez le point d’arrêt, examinez la variable `commandText` dans la fenêtre **variables locales** et utilisez le visualiseur de texte (la loupe dans la colonne **valeur** ) pour afficher sa valeur dans la fenêtre du **visualiseur de texte** . Vous pouvez voir l’intégralité de la commande SQL qui résulte de ce code :

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

En guise d’alternative, la fonctionnalité IntelliTrace de Visual Studio Ultimate permet d’afficher les commandes SQL générées par le Entity Framework qui ne vous oblige pas à modifier votre code ou même à définir un point d’arrêt.

> [!NOTE]
> Vous pouvez effectuer les procédures suivantes uniquement si vous avez Visual Studio Ultimate.

Restaurez le code d’origine dans la méthode `GetDepartmentsByName`, puis exécutez la page *Departments. aspx* dans le débogueur.

Dans Visual Studio, sélectionnez le menu **Déboguer** , **IntelliTrace**, puis **événements IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Dans la fenêtre **IntelliTrace** , cliquez sur **arrêter tout**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

La fenêtre **IntelliTrace** affiche une liste des événements récents :

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Cliquez sur la ligne **ADO.net** . Il se développe pour afficher le texte de la commande :

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Vous pouvez copier l’intégralité de la chaîne de texte de la commande dans le presse-papiers à partir de la fenêtre **variables locales** .

Supposons que vous travailliez avec une base de données contenant plus de tables, de relations et de colonnes que la base de données `School` simple. Vous pouvez constater qu’une requête qui rassemble toutes les informations dont vous avez besoin dans une seule instruction `Select` contenant plusieurs clauses `Join` devient trop complexe pour fonctionner efficacement. Dans ce cas, vous pouvez passer du chargement hâtif au chargement explicite pour simplifier la requête.

Par exemple, essayez de modifier le code dans la méthode `GetDepartmentsByName` dans *SchoolRepository.cs*. Dans cette méthode, vous avez une requête d’objet qui a `Include` méthodes pour les propriétés de navigation `Person` et `Courses`. Remplacez l’instruction `return` par un code qui effectue un chargement explicite, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Exécutez la page *Departments. aspx* dans le débogueur et vérifiez à nouveau la fenêtre **IntelliTrace** comme vous l’avez fait précédemment. Maintenant, lorsqu’il y avait une seule requête, vous voyez une longue séquence de ces requêtes.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Cliquez sur la première ligne **ADO.net** pour voir ce qui s’est passé à la requête complexe que vous avez vue précédemment.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

La requête de Departments est devenue une simple requête de `Select` sans clause `Join`, mais elle est suivie de requêtes distinctes qui récupèrent les cours connexes et un administrateur, à l’aide d’un ensemble de deux requêtes pour chaque service retourné par la requête d’origine.

> [!NOTE]
> Si vous laissez le chargement différé activé, le modèle que vous voyez ici, avec la même requête répétée plusieurs fois, peut être dû à un chargement différé. Un modèle que vous souhaitez généralement éviter est le chargement différé des données associées pour chaque ligne de la table primaire. À moins que vous ayez vérifié qu’une seule requête de jointure est trop complexe pour être efficace, vous pouvez généralement améliorer les performances dans de tels cas en modifiant la requête principale afin d’utiliser le chargement hâtif.

## <a name="pre-generating-views"></a>Prégénération de vues

Quand un objet `ObjectContext` est créé pour la première fois dans un nouveau domaine d’application, le Entity Framework génère un ensemble de classes qu’il utilise pour accéder à la base de données. Ces classes sont appelées *vues*et, si vous disposez d’un modèle de données très volumineux, la génération de ces vues peut retarder la réponse du site Web à la première demande d’une page après l’initialisation d’un nouveau domaine d’application. Vous pouvez réduire ce délai de première demande en créant les vues au moment de la compilation plutôt qu’au moment de l’exécution.

> [!NOTE]
> Si votre application n’a pas de modèle de données extrêmement volumineux, ou si elle a un modèle de données volumineux, mais que vous ne vous inquiétez pas d’un problème de performances qui affecte uniquement la première demande de page après le recyclage d’IIS, vous pouvez ignorer cette section. La création d’une vue ne se produit pas chaque fois que vous instanciez un objet `ObjectContext`, car les vues sont mises en cache dans le domaine d’application. Par conséquent, à moins que vous ne recycliez fréquemment votre application dans IIS, très peu de demandes de page tireraient parti des vues prégénérées.

Vous pouvez prégénérer des vues à l’aide de l’outil en ligne de commande *EdmGen. exe* ou à l’aide d’un modèle *Text Template Transformation Toolkit* (T4). Dans ce didacticiel, vous allez utiliser un modèle T4.

Dans le dossier *dal* , ajoutez un fichier à l’aide du modèle de **modèle de texte** (il se trouve sous le nœud **général** dans la liste **modèles installés** ), puis nommez-le *SchoolModel.views.TT*. Remplacez le code existant dans le fichier par le code suivant :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Ce code génère des vues pour un fichier *. edmx* situé dans le même dossier que le modèle et portant le même nom que le fichier de modèle. Par exemple, si votre fichier de modèle est nommé *SchoolModel.views.TT*, il recherche un fichier de modèle de données nommé *SchoolModel. edmx*.

Enregistrez le fichier, puis cliquez avec le bouton droit sur le fichier dans **Explorateur de solutions** et sélectionnez **exécuter un outil personnalisé**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio génère un fichier de code qui crée les vues, nommées *SchoolModel.views.cs* en fonction du modèle. (Vous avez peut-être remarqué que le fichier de code est généré même avant de sélectionner **exécuter un outil personnalisé**, dès que vous enregistrez le fichier de modèle.)

[![image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Vous pouvez maintenant exécuter l’application et vérifier qu’elle fonctionne comme auparavant.

Pour plus d’informations sur les affichages prégénérés, consultez les ressources suivantes :

- [Comment : prégénérer des vues pour améliorer les performances des requêtes](https://msdn.microsoft.com/library/bb896240.aspx) sur le site Web MSDN. Explique comment utiliser l’outil en ligne de commande `EdmGen.exe` pour prégénérer des vues.
- [Isolation des performances avec les vues précompilées/prégénérées dans le Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) du blog de l’équipe de conseil clientèle de Windows Server AppFabric.

Cela termine la présentation de l’amélioration des performances dans une application Web ASP.NET qui utilise le Entity Framework. Pour plus d'informations, reportez-vous aux ressources suivantes :

- [Considérations sur les performances (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) sur le site Web MSDN.
- [Publications relatives aux performances sur le blog de l’équipe Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [Options de fusion EF et requêtes compilées](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Billet de blog qui explique les comportements inattendus des requêtes compilées et des options de fusion, telles que les `NoTracking`. Si vous envisagez d’utiliser des requêtes compilées ou de manipuler des paramètres d’option de fusion dans votre application, lisez-les en premier.
- [Publications relatives à Entity Framework dans le blog de l’équipe de Conseil des clients de données et de modélisation](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Comprend des publications sur les requêtes compilées et l’utilisation du profileur Visual Studio 2010 pour détecter les problèmes de performances.
- [Entity Framework thread de forum avec des conseils sur l’amélioration des performances des requêtes très complexes](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Recommandations relatives à la gestion d’état ASP.net](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Utilisation des Entity Framework et ObjectDataSource : la pagination personnalisée](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Billet de blog qui s’appuie sur l’application ContosoUniversity créée dans ces didacticiels pour expliquer comment implémenter la pagination dans la page *Departments. aspx* .

Le didacticiel suivant passe en revue certaines des améliorations importantes apportées à la Entity Framework qui sont nouvelles dans la version 4.

> [!div class="step-by-step"]
> [Précédent](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [Suivant](what-s-new-in-the-entity-framework-4.md)
