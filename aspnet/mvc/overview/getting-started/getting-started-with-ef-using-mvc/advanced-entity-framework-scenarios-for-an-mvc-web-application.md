---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: 'Tutoriel : En savoir plus sur les scénarios EF avancés pour une application Web MVC 5'
description: Ce didacticiel comprend plusieurs rubriques qui sont utiles lorsque vous allez au-delà des principes de base du développement d’applications Web ASP.NET qui utilisent Entity Framework Code First.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d7cc83a5b78a60f575f5c3065079679189296a0c
ms.sourcegitcommit: fe5c7512383a9b0a05d321ff10d3cca1611556f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2019
ms.locfileid: "58425273"
---
# <a name="tutorial-learn-about-advanced-ef-scenarios-for-an-mvc-5-web-app"></a>Tutoriel : En savoir plus sur les scénarios EF avancés pour une application Web MVC 5

Dans le didacticiel précédent, vous avez implémenté l’héritage TPH (table par hiérarchie). Ce didacticiel comprend plusieurs rubriques qui sont utiles lorsque vous allez au-delà des principes de base du développement d’applications Web ASP.NET qui utilisent Entity Framework Code First. Les premières sections contiennent des instructions pas à pas qui vous guident pas à pas dans le code et l’utilisation de Visual Studio pour effectuer des tâches. les sections qui suivent présentent plusieurs rubriques avec des présentations rapides, suivies de liens vers des ressources pour plus d’informations.

Pour la plupart de ces rubriques, vous utiliserez les pages que vous avez déjà créées. Pour utiliser des données SQL brutes pour effectuer des mises à jour en bloc, vous allez créer une nouvelle page qui met à jour le nombre de crédits de tous les cours dans la base de données :

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Exécuter des requêtes SQL brutes
> * Exécuter des requêtes sans suivi
> * Examiner les requêtes SQL envoyées à la base de données

Vous en apprendrez également plus sur :

> [!div class="checklist"]
> * Création d’une couche d’abstraction
> * Classes proxy
> * Détection automatique des modifications
> * Validation automatique
> * Outils Power Tools Entity Framework
> * Code source Entity Framework

## <a name="prerequisite"></a>Configuration requise

* [Implémentation de l’héritage](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="perform-raw-sql-queries"></a>Exécuter des requêtes SQL brutes

L’API Entity Framework Code First comprend des méthodes qui vous permettent de transmettre des commandes SQL directement à la base de données. Les options suivantes sont disponibles :

- Utilisez la méthode [DbSet. SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) pour les requêtes qui retournent des types d’entité. Les objets retournés doivent être du type attendu par `DbSet` l’objet, et ils sont suivis automatiquement par le contexte de base de données, sauf si vous désactivez le suivi. (Consultez la section suivante sur la méthode [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) .)
- Utilisez la méthode [Database. SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) pour les requêtes qui retournent des types qui ne sont pas des entités. Les données renvoyées ne font pas l’objet d’un suivi par le contexte de base de données, même si vous utilisez cette méthode pour récupérer des types d’entités.
- Utilisez la [base de données. ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) pour les commandes qui ne sont pas des requêtes.

L’un des avantages d’utiliser Entity Framework est que cela évite de lier votre code trop étroitement à une méthode particulière de stockage des données. Il le fait en générant des requêtes et des commandes SQL pour vous, ce qui vous évite d’avoir à les écrire vous-même. Toutefois, il existe des scénarios exceptionnels lorsque vous devez exécuter des requêtes SQL spécifiques que vous avez créées manuellement, et ces méthodes vous permettent de gérer ces exceptions.

Comme c’est toujours le cas lorsque vous exécutez des commandes SQL dans une application web, vous devez prendre des précautions pour protéger votre site contre des attaques par injection de code SQL. Une manière de procéder consiste à utiliser des requêtes paramétrables pour vous assurer que les chaînes soumises par une page web ne peuvent pas être interprétées comme des commandes SQL. Dans ce didacticiel, vous utiliserez des requêtes paramétrables lors de l’intégration de l’entrée utilisateur dans une requête.

### <a name="calling-a-query-that-returns-entities"></a>Appel d’une requête qui retourne des entités

La [classe&lt;DbSet TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) fournit une méthode que vous pouvez utiliser pour exécuter une requête qui retourne une entité de type `TEntity`. Pour voir comment cela fonctionne, vous allez modifier le code dans `Details` la méthode `Department` du contrôleur.

Dans *DepartmentController.cs*, dans la `Details` méthode, remplacez l' `db.Departments.FindAsync` appel de méthode par `db.Departments.SqlQuery` un appel de méthode, comme indiqué dans le code en surbrillance suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Pour vérifier que le nouveau code fonctionne correctement, sélectionnez l’onglet **Departments**, puis **Details** pour l’un des services. Assurez-vous que toutes les données s’affichent comme prévu.

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Appel d’une requête qui retourne d’autres types d’objets

Précédemment, vous avez créé une grille de statistiques des étudiants pour la page About, qui montrait le nombre d’étudiants pour chaque date d’inscription. Le code qui le fait dans *HomeController.cs* utilise LINQ :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Supposons que vous souhaitiez écrire le code qui récupère ces données directement dans SQL au lieu d’utiliser LINQ. Pour ce faire, vous devez exécuter une requête qui retourne autre chose que des objets d’entité, ce qui signifie que vous devez utiliser la méthode [Database. SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) .

Dans *HomeController.cs*, remplacez l’instruction LINQ dans la `About` méthode par une instruction SQL, comme indiqué dans le code en surbrillance suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Exécutez la page about. Vérifiez qu’il affiche les mêmes données qu’avant.

### <a name="calling-an-update-query"></a>Appel d’une requête Update

Supposons que les administrateurs de Contoso University souhaitent pouvoir effectuer des modifications en bloc dans la base de données, telles que la modification du nombre de crédits pour chaque cours. Si l’université a un grand nombre de cours, il serait inefficace de les récupérer tous sous forme d’entités et de les modifier individuellement. Dans cette section, vous allez implémenter une page Web qui permet à l’utilisateur de spécifier un facteur par lequel modifier le nombre de crédits pour tous les cours, et vous apprendrez la modification en `UPDATE` exécutant une instruction SQL. 

Dans *CourseController.cs*, ajoutez `UpdateCourseCredits` les méthodes `HttpGet` pour `HttpPost`et :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Lorsque le contrôleur traite une `HttpGet` demande, rien n’est retourné dans `ViewBag.RowsAffected` la variable, et la vue affiche une zone de texte vide et un bouton Envoyer.

Lorsque l’utilisateur clique sur le bouton **mettre à jour** , la `HttpPost` méthode `multiplier` est appelée et la valeur est entrée dans la zone de texte. Le code exécute ensuite le SQL qui met à jour les cours et retourne le nombre de lignes affectées à la vue dans `ViewBag.RowsAffected` la variable. Lorsque la vue obtient une valeur dans cette variable, elle affiche le nombre de lignes mises à jour à la place de la zone de texte et du bouton Envoyer.

Dans *CourseController.cs*, cliquez avec le bouton droit sur `UpdateCourseCredits` l’une des méthodes, puis cliquez sur **Ajouter une vue**. La boîte de dialogue **Ajouter une vue** s’affiche. Laissez les valeurs par défaut et sélectionnez **Ajouter**.

Dans *Views\Course\UpdateCourseCredits.cshtml*, remplacez le code du modèle par le code suivant :

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Exécutez la méthode `UpdateCourseCredits` en sélectionnant l’onglet **Courses**, puis en ajoutant « /UpdateCourseCredits » à la fin de l’URL dans la barre d’adresse du navigateur (par exemple : `http://localhost:50205/Course/UpdateCourseCredits`). Entrez un nombre dans la zone de texte :

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Cliquez sur **Mettre à jour**. Vous voyez le nombre de lignes affectées.

Cliquez sur **Revenir à la liste** pour afficher la liste des cours avec le nombre révisé de crédits.

Pour plus d’informations sur les requêtes SQL brutes, consultez [requêtes SQL brutes](https://msdn.microsoft.com/data/jj592907) sur MSDN.

## <a name="no-tracking-queries"></a>Pas de suivi des requêtes

Quand un contexte de base de données récupère des lignes de table et crée des objets entité qui les représentent, par défaut, il effectue le suivi du fait que les entités en mémoire sont ou non synchronisées avec ce qui se trouve dans la base de données. Les données en mémoire agissent comme un cache et sont utilisées quand vous mettez à jour une entité. Cette mise en cache est souvent inutile dans une application web, car les instances de contexte ont généralement une durée de vie courte (une instance est créée puis supprimée pour chaque requête) et le contexte qui lit une entité est généralement supprimé avant que cette entité soit réutilisée.

Vous pouvez désactiver le suivi des objets d’entité en mémoire à l’aide de la méthode [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) . Voici des scénarios classiques où vous voulez procéder ainsi :

- Une requête récupère un volume important de données qui désactive le suivi, ce qui peut améliorer considérablement les performances.
- Vous souhaitez attacher une entité afin de la mettre à jour, mais vous avez précédemment récupéré la même entité dans un but différent. Comme l’entité est déjà suivie par le contexte de base de données, vous ne pouvez pas attacher l’entité que vous voulez modifier. Une façon de gérer cette situation consiste à utiliser l' `AsNoTracking` option avec la requête précédente.

Pour obtenir un exemple qui montre comment utiliser la méthode [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) , consultez [la version antérieure de ce didacticiel](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Cette version du didacticiel ne définit pas l’indicateur modifié sur une entité créée par le Binder de modèle dans la méthode Edit, donc cela n’est `AsNoTracking`pas nécessaire.

## <a name="examine-sql-sent-to-database"></a>Examiner SQL envoyé à la base de données

Il est parfois utile de pouvoir voir les requêtes SQL réelles qui sont envoyées à la base de données. Dans un didacticiel précédent, vous avez vu comment faire cela dans le code d’intercepteur. à présent, vous allez voir comment le faire sans écrire de code d’intercepteur. Pour essayer cela, vous allez examiner une requête simple, puis examiner ce qui se passe au fur et à mesure que vous ajoutez des options telles que le chargement, le filtrage et le tri hâtif.

Dans *Controllers/CourseController*, remplacez la `Index` méthode par le code suivant, afin d’arrêter temporairement le chargement hâtif :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

À présent, définissez un point `return` d’arrêt sur l’instruction (F9 avec le curseur sur cette ligne). Appuyez sur **F5** pour exécuter le projet en mode débogage, puis sélectionnez la page index du cours. Lorsque le code atteint le point d’arrêt, `sql` examinez la variable. Vous voyez la requête qui est envoyée à SQL Server. Il s’agit d' `Select` une simple instruction.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Cliquez sur la loupe pour afficher la requête dans le **visualiseur de texte**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Vous allez maintenant ajouter une liste déroulante à la page d’index des cours afin que les utilisateurs puissent filtrer pour un service particulier. Vous allez trier les cours par titre et spécifier le chargement hâtif pour la `Department` propriété de navigation.

Dans *CourseController.cs*, remplacez la `Index` méthode par le code suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Restaurez le point d' `return` arrêt sur l’instruction.

La méthode reçoit la valeur sélectionnée de la liste déroulante dans le `SelectedDepartment` paramètre. Si rien n’est sélectionné, ce paramètre a la valeur null.

Une `SelectList` collection contenant tous les services est passée à la vue pour la liste déroulante. Les paramètres passés au `SelectList` constructeur spécifient le nom du champ de valeur, le nom du champ de texte et l’élément sélectionné.

Pour la `Get` méthode `Course` du référentiel, le code spécifie une expression de filtre, un ordre de tri et un chargement hâtif pour la `Department` propriété de navigation. L’expression de filtre retourne `true` toujours si rien n’est sélectionné dans la liste déroulante (autrement `SelectedDepartment` dit, est null).

Dans *Views\Course\Index.cshtml*, juste avant la balise d’ouverture `table` , ajoutez le code suivant pour créer la liste déroulante et un bouton Envoyer :

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Le point d’arrêt étant toujours défini, exécutez la page index du cours. Poursuivez la première fois que le code atteint un point d’arrêt, afin que la page s’affiche dans le navigateur. Sélectionnez un service dans la liste déroulante, puis cliquez sur **Filtrer**.

Cette fois, le premier point d’arrêt est destiné à la requête Departments pour la liste déroulante. Ignorez cette valeur et `query` Affichez la variable la prochaine fois que le code atteint le point d’arrêt `Course` afin de voir à quoi ressemble la requête. Vous verrez un résultat similaire à ce qui suit :

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Vous pouvez voir que la requête est désormais une `JOIN` requête qui charge `Department` des données avec les `Course` données et qu’elle comprend une `WHERE` clause.

Supprimez `var sql = courses.ToString()` la ligne.

## <a name="create-an-abstraction-layer"></a>Créer une couche d’abstraction

De nombreux développeurs écrivent du code pour implémenter les modèles d’unité de travail et de référentiel comme un wrapper autour du code qui fonctionne avec Entity Framework. Ces modèles sont destinés à créer une couche d’abstraction entre la couche d’accès aux données et la couche de logique métier d’une application. L’implémentation de ces modèles peut favoriser l’isolation de votre application face à des modifications dans le magasin de données et peut faciliter le test unitaire automatisé ou le développement piloté par les tests (TDD). Toutefois, l’écriture de code supplémentaire pour implémenter ces modèles n’est pas toujours le meilleur choix pour les applications qui utilisent EF, pour plusieurs raisons :

- La classe de contexte EF elle-même isole votre code face au code spécifique de magasin de données.
- La classe de contexte EF peut agir comme une classe d’unité de travail pour les mises à jour de base de données que vous effectuez à l’aide d’EF.
- Les fonctionnalités introduites dans Entity Framework 6 facilitent l’implémentation de TDD sans écrire de code de référentiel.

Pour plus d’informations sur la façon d’implémenter le référentiel et les modèles d’unité de travail, consultez [la version Entity Framework 5 de cette série de didacticiels](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Pour plus d’informations sur les méthodes d’implémentation de TDD dans Entity Framework 6, consultez les ressources suivantes :

- [Comment EF6 permet d’identifier plus facilement les DbSets factices](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Test avec un Framework fictif](https://msdn.microsoft.com/data/dn314429)
- [Test avec vos propres doubles de test](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>

## <a name="proxy-classes"></a>Classes proxy

Lorsque le Entity Framework crée des instances d’entité (par exemple, lorsque vous exécutez une requête), il les crée souvent comme instances d’un type dérivé généré de manière dynamique qui agit comme un proxy pour l’entité. Par exemple, consultez les deux images de débogueur suivantes. Dans la première image, vous constatez `student` que la variable est `Student` le type attendu immédiatement après l’instanciation de l’entité. Dans la deuxième image, une fois que EF a été utilisé pour lire une entité Student à partir de la base de données, la classe proxy s’affiche.

![Avant la classe proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Après la classe proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Cette classe proxy remplace certaines propriétés virtuelles de l’entité pour insérer des raccordements pour l’exécution automatique des actions lors de l’accès à la propriété. L’une des fonctions pour lesquelles ce mécanisme est utilisé est le chargement différé.

La plupart du temps, vous n’avez pas besoin d’être conscient de cette utilisation des proxys, mais il existe des exceptions :

- Dans certains scénarios, vous souhaiterez peut-être empêcher le Entity Framework de créer des instances de proxy. Par exemple, quand vous sérialisez des entités, vous souhaitez généralement les classes POCO, et non les classes proxy. Pour éviter les problèmes de sérialisation, il est possible de sérialiser des objets DTO (Data Transfer Objects) au lieu d’objets d’entité, comme indiqué dans le didacticiel utilisation de l' [API Web avec Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) . Une autre méthode consiste à [désactiver la création de proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Lorsque vous instanciez une classe d’entité `new` à l’aide de l’opérateur, vous n’avez pas d’instance de proxy. Cela signifie que vous ne bénéficiez pas de fonctionnalités telles que le chargement différé et le suivi automatique des modifications. C’est généralement parfait. en général, vous n’avez pas besoin de chargement différé, car vous créez une nouvelle entité qui n’est pas dans la base de données, et vous n’avez généralement pas besoin `Added`de suivi des modifications si vous marquez explicitement l’entité comme. Toutefois, si vous avez besoin d’un chargement différé et que vous avez besoin d’un suivi des modifications, vous pouvez créer de nouvelles instances d’entité `DbSet` avec des proxies à l’aide de la méthode [Create](https://msdn.microsoft.com/library/gg679504.aspx) de la classe.
- Vous pouvez obtenir un type d’entité réel à partir d’un type de proxy. Vous pouvez utiliser la méthode [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) de la `ObjectContext` classe pour obtenir le type d’entité réel d’une instance de type de proxy.

Pour plus d’informations, consultez [utilisation des proxies](https://msdn.microsoft.com/data/JJ592886.aspx) sur MSDN.

## <a name="automatic-change-detection"></a>Détection automatique des modifications

Entity Framework détermine la manière dont une entité a changé (et par conséquent les mises à jour qui doivent être envoyées à la base de données) en comparant les valeurs en cours d’une entité avec les valeurs d’origine. Les valeurs d’origine sont stockées lorsque l’entité fait l’objet d’une requête ou d’une jointure. Certaines des méthodes qui provoquent la détection automatique des modifications sont les suivantes :

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Si vous effectuez le suivi d’un grand nombre d’entités et que vous appelez une de ces méthodes plusieurs fois dans une boucle, vous pouvez obtenir des améliorations significatives en matière de performances en désactivant temporairement la détection automatique des modifications à l’aide de la propriété [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) . Pour plus d’informations, consultez [détection automatique des modifications](https://msdn.microsoft.com/data/jj556205) sur MSDN.

## <a name="automatic-validation"></a>Validation automatique

Lorsque vous appelez la `SaveChanges` méthode, par défaut, la Entity Framework valide les données de toutes les propriétés de toutes les entités modifiées avant de mettre à jour la base de données. Si vous avez mis à jour un grand nombre d’entités et que vous avez déjà validé les données, ce travail n’est pas nécessaire et vous pouvez faire en sorte que le processus d’enregistrement des modifications prenne moins de temps en désactivant temporairement la validation. Vous pouvez le faire à l’aide de la propriété [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) . Pour plus d’informations, consultez [validation](https://msdn.microsoft.com/data/gg193959) sur MSDN.

## <a name="entity-framework-power-tools"></a>Outils Power Tools Entity Framework

[Entity Framework Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) est un complément Visual Studio qui a été utilisé pour créer les diagrammes de modèle de données présentés dans ces didacticiels. Les outils peuvent également effectuer d’autres fonctions, telles que générer des classes d’entité en fonction des tables d’une base de données existante, afin que vous puissiez utiliser la base de données avec Code First. Après avoir installé les outils, certaines options supplémentaires s’affichent dans les menus contextuels. Par exemple, lorsque vous cliquez avec le bouton droit sur votre classe de contexte dans **Explorateur de solutions**, vous voyez et **Entity Framework** option. Cela vous donne la possibilité de générer un diagramme. Lorsque vous utilisez Code First vous ne pouvez pas modifier le modèle de données dans le diagramme, mais vous pouvez déplacer des éléments pour en faciliter la compréhension.

![Diagramme EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

## <a name="entity-framework-source-code"></a>Code source Entity Framework

Le code source de Entity Framework 6 est disponible sur [GitHub](https://github.com/aspnet/EntityFramework6). Vous pouvez classer les bogues et vous pouvez apporter vos propres améliorations au code source EF.

Bien que le code source soit ouvert, Entity Framework est entièrement pris en charge en tant que produit Microsoft. L’équipe Microsoft Entity Framework garde le contrôle sur le choix des contributions qui sont acceptées et teste toutes les modifications du code pour garantir la qualité de chaque version.

## <a name="acknowledgments"></a>Remerciements

- Tom Dykstra a écrit la version d’origine de ce didacticiel, a co-créé la mise à jour d’EF 5 et écrit la mise à jour d’EF 6. Tom est un rédacteur de programmation Senior sur l’équipe de contenu Microsoft Web Platform and Tools.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) la plupart du travail a mis à jour le didacticiel pour EF 5 et MVC 4 et co-créé la mise à jour d’EF 6. Rick est un auteur de programmation senior pour Microsoft axé sur Azure et MVC.
- [Rowan Miller](http://www.romiller.com) et les autres membres de l’équipe Entity Framework assistent à la révision du code et ont aidé à déboguer de nombreux problèmes liés aux migrations qui se sont produits lors de la mise à jour du didacticiel pour EF 5 et EF 6.

## <a name="troubleshoot-common-errors"></a>Résolution des erreurs courantes

### <a name="cannot-createshadow-copy"></a>Impossible de créer un cliché instantané

Message d’erreur :

> Impossible de créer le cliché instantané&lt;'&gt;nom_fichier’quand ce fichier existe déjà.

Solution

Patientez quelques secondes, puis actualisez la page.

### <a name="update-database-not-recognized"></a>Mise à jour-base de données non reconnue

Message d’erreur (à `Update-Database` partir de la commande dans le PMC) :

> Le terme « Update-Database » n’est pas reconnu comme le nom d’une applet de commande, d’une fonction, d’un fichier de script ou d’un programme exécutable. Vérifiez l’orthographe du nom ou, si un chemin d’accès a été inclus, vérifiez que le chemin d’accès est correct et réessayez.

Solution

Quittez Visual Studio. Rouvrez le projet et réessayez.

### <a name="validation-failed"></a>Échec de la validation

Message d’erreur (à `Update-Database` partir de la commande dans le PMC) :

> La validation a échoué pour une ou plusieurs entités. Pour plus d’informations, consultez la propriété « EntityValidationErrors ».

Solution

Ce problème peut être dû à des erreurs de validation `Seed` lors de l’exécution de la méthode. Pour obtenir des conseils sur le débogage de la méthode, consultez bases de l' `Seed` [amorçage et débogage Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) .

### <a name="http-50019-error"></a>Erreur HTTP 500,19

Message d’erreur :

> Erreur HTTP 500,19-erreur de serveur interne. impossible d’accéder à la page demandée, car les données de configuration associées à la page ne sont pas valides.

Solution

L’une des façons d’obtenir cette erreur est d’avoir plusieurs copies de la solution, chacune utilisant le même numéro de port. Vous pouvez généralement résoudre ce problème en quittant toutes les instances de Visual Studio, puis en redémarrant le projet sur lequel vous travaillez. Si cela ne fonctionne pas, essayez de modifier le numéro de port. Cliquez avec le bouton droit sur le fichier projet, puis cliquez sur Propriétés. Sélectionnez l’onglet **Web** , puis modifiez le numéro de port dans la zone de texte **URL du projet** .

### <a name="error-locating-sql-server-instance"></a>Erreur lors de la localisation de l’instance SQL Server

Message d’erreur :

> Une erreur liée au réseau ou spécifique à l’instance s’est produite lors de l’établissement d’une connexion à SQL Server. Le serveur est introuvable ou n’est pas accessible. Vérifiez que le nom de l’instance est correct et que SQL Server est configuré pour autoriser les connexions distantes. (fournisseur : Interfaces réseau SQL, erreur : 26 - Erreur lors de la localisation du serveur/de l’instance spécifiés)

Solution

Vérifiez la chaîne de connexion. Si vous avez supprimé manuellement la base de données, modifiez le nom de la base de données dans la chaîne de construction.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

 Pour plus d’informations sur l’utilisation des données à l’aide du Entity Framework, consultez la [page de documentation EF sur MSDN](https://msdn.microsoft.com/data/ee712907) et [ASP.NET Data Access-Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).

Pour plus d’informations sur la façon de déployer votre application Web une fois que vous l’avez créée, consultez [déploiement web ASP.net-ressources recommandées](../../../../whitepapers/aspnet-web-deployment-content-map.md) dans MSDN Library.

Pour plus d’informations sur les autres rubriques liées à MVC, telles que l’authentification et l’autorisation, consultez les [ressources recommandées pour ASP.NET MVC](../recommended-resources-for-mvc.md).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Exécuter des requêtes SQL brutes
> * Aucune requête de suivi effectuée
> * Requêtes SQL examinées envoyées à la base de données

Vous avez également appris ce qui suit :

> [!div class="checklist"]
> * Création d’une couche d’abstraction
> * Classes proxy
> * Détection automatique des modifications
> * Validation automatique
> * Outils Power Tools Entity Framework
> * Code source Entity Framework

Cette série de didacticiels s’achève sur l’utilisation de la Entity Framework dans une application MVC ASP.NET. Si vous souhaitez en savoir plus sur le Database First EF, consultez la série de didacticiels sur la base de connaissances.
> [!div class="nextstepaction"]
> [Entity Framework Database First](../database-first-development/setting-up-database.md)