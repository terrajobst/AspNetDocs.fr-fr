---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implémentation du référentiel et des modèles d’unité de travail dans une application ASP.NET MVC (9 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 18de9b125ee5d10795b9ce1a366918dadf4fc4e3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595245"
---
# <a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implémentation du référentiel et des modèles d’unité de travail dans une application ASP.NET MVC (9 sur 10)

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et de Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels depuis le début ou [Télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et démarrer ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [Téléchargez le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire le problème. Vous pouvez généralement trouver la solution au problème en comparant votre code au code terminé. Pour obtenir des erreurs courantes et comment les résoudre, consultez [Erreurs et solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Dans le didacticiel précédent, vous avez utilisé l’héritage pour réduire le code redondant dans les classes d’entité `Student` et `Instructor`. Dans ce didacticiel, vous allez découvrir des façons d’utiliser le référentiel et les modèles d’unité de travail pour les opérations CRUD. Comme dans le didacticiel précédent, vous allez modifier le mode de fonctionnement de votre code avec les pages que vous avez déjà créées au lieu de créer des pages.

## <a name="the-repository-and-unit-of-work-patterns"></a>Le référentiel et les modèles d’unité de travail

Le référentiel et les modèles d’unité de travail sont conçus pour créer une couche d’abstraction entre la couche d’accès aux données et la couche de logique métier d’une application. L’implémentation de ces modèles peut favoriser l’isolation de votre application face à des modifications dans le magasin de données et peut faciliter le test unitaire automatisé ou le développement piloté par les tests (TDD).

Dans ce didacticiel, vous allez implémenter une classe de référentiel pour chaque type d’entité. Pour le type d’entité `Student`, vous allez créer une interface de référentiel et une classe de référentiel. Lorsque vous instanciez le référentiel dans votre contrôleur, vous utilisez l’interface afin que le contrôleur accepte une référence à n’importe quel objet qui implémente l’interface de référentiel. Lorsque le contrôleur s’exécute sous un serveur Web, il reçoit un référentiel qui fonctionne avec le Entity Framework. Lorsque le contrôleur s’exécute sous une classe de test unitaire, il reçoit un référentiel qui fonctionne avec les données stockées d’une manière que vous pouvez facilement manipuler à des fins de test, telles qu’une collection en mémoire.

Plus loin dans ce didacticiel, vous utiliserez plusieurs référentiels et une classe d’unité de travail pour les `Course` et les types d’entités `Department` dans le contrôleur de `Course`. La classe Unit of Work coordonne le travail de plusieurs dépôts en créant une classe de contexte de base de données unique partagée par toutes. Si vous souhaitez être en mesure d’effectuer des tests unitaires automatisés, vous devez créer et utiliser des interfaces pour ces classes de la même façon que pour le référentiel `Student`. Toutefois, pour simplifier le didacticiel, vous allez créer et utiliser ces classes sans interfaces.

L’illustration suivante montre une façon de conceptualiser les relations entre le contrôleur et les classes de contexte par rapport à l’utilisation du modèle de référentiel ou d’unité de travail du tout.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Vous ne créerez pas de tests unitaires dans cette série de didacticiels. Pour une introduction au TDD avec une application MVC qui utilise le modèle de référentiel, consultez [procédure pas à pas : utilisation de TDD avec ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Pour plus d’informations sur le modèle de référentiel, consultez les ressources suivantes :

- [Le modèle de référentiel](https://msdn.microsoft.com/library/ff649690.aspx) sur MSDN.
- [Utilisation du référentiel et des modèles d’unité de travail avec Entity Framework 4,0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) sur le blog de l’équipe Entity Framework.
- Le blog de la série de publications [Agile Entity Framework 4](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) sur Julie Lerman.
- [Création du compte en un clin d’œil sur l’application HTML5/jQuery](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) sur le blog de Dan Wahlin.

> [!NOTE]
> Il existe de nombreuses façons d’implémenter le référentiel et les modèles d’unité de travail. Vous pouvez utiliser des classes de référentiel avec ou sans classe d’unité de travail. Vous pouvez implémenter un référentiel unique pour tous les types d’entité, ou un pour chaque type. Si vous implémentez un pour chaque type, vous pouvez utiliser des classes distinctes, une classe de base générique et des classes dérivées, ou une classe de base abstraite et des classes dérivées. Vous pouvez inclure la logique métier dans votre référentiel ou la limiter à la logique d’accès aux données. Vous pouvez également créer une couche d’abstraction dans votre classe de contexte de base de données en utilisant des interfaces [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) à la place de types [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) pour vos jeux d’entités. L’approche de l’implémentation d’une couche d’abstraction présentée dans ce didacticiel est une option que vous devez prendre en compte, et non une recommandation pour tous les scénarios et environnements.

## <a name="creating-the-student-repository-class"></a>Création de la classe de référentiel Student

Dans le dossier *dal* , créez un fichier de classe nommé *IStudentRepository.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ce code déclare un ensemble standard de méthodes CRUD, y compris deux méthodes de lecture (une qui retourne toutes les entités `Student`) et une autre qui trouve une seule entité `Student` par ID.

Dans le dossier *dal* , créez un fichier de classe nommé *StudentRepository.cs* . Remplacez le code existant par le code suivant, qui implémente l’interface `IStudentRepository` :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Le contexte de base de données est défini dans une variable de classe et le constructeur s’attend à ce que l’objet appelant passe dans une instance du contexte :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Vous pouvez instancier un nouveau contexte dans le référentiel, mais si vous avez utilisé plusieurs référentiels dans un contrôleur, chacun finit par un contexte distinct. Plus tard, vous utiliserez plusieurs référentiels dans le contrôleur `Course`, et vous verrez comment une classe d’unité de travail peut garantir que tous les dépôts utilisent le même contexte.

Le référentiel implémente [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) et supprime le contexte de base de données tel que vous l’avez vu précédemment dans le contrôleur, et ses méthodes CRUD effectuent des appels au contexte de la base de données de la même façon que vous l’avez vu précédemment.

## <a name="change-the-student-controller-to-use-the-repository"></a>Changer le contrôleur d’étudiant pour utiliser le référentiel

Dans *StudentController.cs*, remplacez le code actuellement dans la classe par le code suivant. Les modifications sont mises en surbrillance.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Le contrôleur déclare maintenant une variable de classe pour un objet qui implémente l’interface `IStudentRepository` au lieu de la classe de contexte :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Le constructeur par défaut (sans paramètre) crée une nouvelle instance de contexte et un constructeur facultatif permet à l’appelant de passer une instance de contexte.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Si vous utilisiez l' *injection de dépendances*, ou di, vous n’auriez pas besoin du constructeur par défaut, car le logiciel di s’assure que l’objet de référentiel correct serait toujours fourni.)

Dans les méthodes CRUD, le référentiel est maintenant appelé à la place du contexte :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

La méthode `Dispose` supprime maintenant le référentiel au lieu du contexte :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Exécutez le site et cliquez sur l’onglet **students** .

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

La page s’affiche et fonctionne de la même manière qu’avant la modification du code pour utiliser le référentiel, et les autres pages Student fonctionnent également de la même façon. Toutefois, il existe une différence importante dans la façon dont la méthode `Index` du contrôleur effectue le filtrage et le tri. La version d’origine de cette méthode contenait le code suivant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

La méthode `Index` mise à jour contient le code suivant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Seul le code en surbrillance a été modifié.

Dans la version d’origine du code, `students` est tapé comme un objet `IQueryable`. La requête n’est pas envoyée à la base de données tant qu’elle n’est pas convertie en collection à l’aide d’une méthode telle que `ToList`, ce qui ne se produit pas tant que la vue index n’accède pas au modèle Student. La méthode `Where` dans le code d’origine ci-dessus devient une clause `WHERE` dans la requête SQL qui est envoyée à la base de données. Cela signifie que seules les entités sélectionnées sont retournées par la base de données. Toutefois, suite à la modification de `context.Students` en `studentRepository.GetStudents()`, la variable `students` après cette instruction est une collection `IEnumerable` qui comprend tous les étudiants dans la base de données. Le résultat final de l’application de la méthode `Where` est le même, mais à présent le travail est effectué en mémoire sur le serveur Web et non pas par la base de données. Pour les requêtes qui retournent de gros volumes de données, cela peut s’avérer inefficace.

> [!TIP]
> 
> **IQueryable et IEnumerable**
> 
> Une fois que vous avez implémenté le référentiel comme indiqué ici, même si vous entrez un nom dans la zone de **recherche** , la requête envoyée à SQL Server retourne toutes les lignes de l’étudiant, car il n’inclut pas vos critères de recherche :
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Cette requête retourne toutes les données de l’étudiant, car le référentiel a exécuté la requête sans connaître les critères de recherche. Le processus de tri, d’application des critères de recherche et de sélection d’un sous-ensemble de données pour la pagination (avec seulement 3 lignes dans ce cas) est effectué en mémoire ultérieurement lorsque la méthode `ToPagedList` est appelée sur la collection `IEnumerable`.
> 
> Dans la version précédente du code (avant d’implémenter le référentiel), la requête n’est pas envoyée à la base de données tant que vous n’avez pas appliqué les critères de recherche, lorsque `ToPagedList` est appelé sur l’objet `IQueryable`.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Lorsque ToPagedList est appelé sur un objet `IQueryable`, la requête envoyée à SQL Server spécifie la chaîne de recherche et, par conséquent, seules les lignes qui répondent aux critères de recherche sont retournées, et aucun filtrage ne doit être effectué en mémoire.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (Le didacticiel suivant explique comment examiner les requêtes envoyées à SQL Server.)

La section suivante montre comment implémenter des méthodes de référentiel qui vous permettent de spécifier que ce travail doit être effectué par la base de données.

Vous avez maintenant créé une couche d’abstraction entre le contrôleur et le contexte de base de données Entity Framework. Si vous envisagez d’effectuer des tests unitaires automatisés avec cette application, vous pouvez créer une autre classe de référentiel dans un projet de test unitaire qui implémente `IStudentRepository` *.* Au lieu d’appeler le contexte pour lire et écrire des données, cette classe de référentiel fictive peut manipuler des collections en mémoire afin de tester les fonctions du contrôleur.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implémenter un référentiel générique et une classe d’unité de travail

La création d’une classe de référentiel pour chaque type d’entité peut entraîner une grande quantité de code redondant et peut entraîner des mises à jour partielles. Par exemple, supposons que vous devez mettre à jour deux types d’entité différents dans le cadre de la même transaction. Si chacune utilise une instance de contexte de base de données distincte, l’une peut réussir et l’autre peut échouer. L’une des façons de réduire le code redondant consiste à utiliser un référentiel générique et une façon de s’assurer que tous les dépôts utilisent le même contexte de base de données (et, par conséquent, la coordination de toutes les mises à jour) consiste à utiliser une classe d’unité de travail.

Dans cette section du didacticiel, vous allez créer une classe `GenericRepository` et une classe `UnitOfWork`, et les utiliser dans le contrôleur `Course` pour accéder à la `Department` et aux jeux d’entités `Course`. Comme expliqué précédemment, pour simplifier cette partie du didacticiel, vous ne créez pas d’interfaces pour ces classes. Toutefois, si vous deviez les utiliser pour faciliter le TDD, vous les implémentez généralement à l’aide d’interfaces de la même façon que vous l’avez fait pour le référentiel de `Student`.

### <a name="create-a-generic-repository"></a>Créer un dépôt générique

Dans le dossier *dal* , créez *GenericRepository.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Les variables de classe sont déclarées pour le contexte de base de données et pour le jeu d’entités pour lequel le dépôt est instancié :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Le constructeur accepte une instance de contexte de base de données et initialise la variable de jeu d’entités :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

La méthode `Get` utilise des expressions lambda pour permettre au code appelant de spécifier une condition de filtre et une colonne pour trier les résultats par, et un paramètre de chaîne permet à l’appelant de fournir une liste délimitée par des virgules des propriétés de navigation pour le chargement hâtif :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Le `Expression<Func<TEntity, bool>> filter` de code signifie que l’appelant fournira une expression lambda basée sur le type de `TEntity`, et cette expression renverra une valeur booléenne. Par exemple, si le référentiel est instancié pour le type d’entité `Student`, le code de la méthode d’appel peut spécifier `student => student.LastName == "Smith`&quot; pour le paramètre `filter`.

Le code `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` signifie également que l’appelant fournira une expression lambda. Mais dans ce cas, l’entrée de l’expression est un objet `IQueryable` pour le type `TEntity`. L’expression retourne une version triée de cet objet `IQueryable`. Par exemple, si le référentiel est instancié pour le type d’entité `Student`, le code dans la méthode d’appel peut spécifier `q => q.OrderBy(s => s.LastName)` pour le paramètre `orderBy`.

Le code de la méthode `Get` crée un objet `IQueryable`, puis applique l’expression de filtre, le cas échéant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Elle applique ensuite les expressions de chargement hâtif après l’analyse de la liste délimitée par des virgules :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Enfin, il applique l’expression `orderBy` s’il en existe un et retourne les résultats ; dans le cas contraire, elle retourne les résultats de la requête non triée :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Quand vous appelez la méthode `Get`, vous pouvez effectuer un filtrage et un tri sur la collection `IEnumerable` retournée par la méthode au lieu de fournir des paramètres pour ces fonctions. Toutefois, le travail de tri et de filtrage est ensuite effectué en mémoire sur le serveur Web. En utilisant ces paramètres, vous vous assurez que le travail est effectué par la base de données plutôt que par le serveur Web. Une alternative consiste à créer des classes dérivées pour des types d’entités spécifiques et à ajouter des méthodes `Get` spécialisées, telles que `GetStudentsInNameOrder` ou `GetStudentsByName`. Toutefois, dans une application complexe, cela peut entraîner un grand nombre de classes dérivées de ce type et de méthodes spécialisées, qui peuvent être plus de travail à gérer.

Le code des méthodes `GetByID`, `Insert`et `Update` est semblable à celui que vous avez vu dans le référentiel non générique. (Vous ne fournissez pas de paramètre de chargement hâtif dans la signature `GetByID`, car vous ne pouvez pas effectuer de chargement hâtif avec la méthode `Find`.)

Deux surcharges sont fournies pour la méthode `Delete` :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

L’une d’entre elles vous permet de transmettre uniquement l’ID de l’entité à supprimer, et l’autre prend une instance d’entité. Comme vous l’avez vu dans le didacticiel sur la [gestion](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) de la concurrence, vous avez besoin d’une méthode `Delete` qui prend une instance d’entité incluant la valeur d’origine d’une propriété de suivi.

Ce dépôt générique gère les exigences de la CRUD classique. Quand un type d’entité particulier a des exigences spéciales, telles que le filtrage ou le classement plus complexe, vous pouvez créer une classe dérivée qui a des méthodes supplémentaires pour ce type.

## <a name="creating-the-unit-of-work-class"></a>Création de la classe Unit of Work

La classe Unit of Work sert à un objectif : pour s’assurer que lorsque vous utilisez plusieurs référentiels, ils partagent un contexte de base de données unique. Ainsi, quand une unité de travail est terminée, vous pouvez appeler la méthode `SaveChanges` sur cette instance du contexte et être certain que toutes les modifications associées seront coordonnées. Tout ce dont a besoin la classe est une méthode `Save` et une propriété pour chaque référentiel. Chaque propriété de référentiel retourne une instance de référentiel qui a été instanciée à l’aide de la même instance de contexte de base de données que les autres instances de référentiel.

Dans le dossier *dal* , créez un fichier de classe nommé *UnitOfWork.cs* et remplacez le code du modèle par le code suivant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Le code crée des variables de classe pour le contexte de base de données et chaque référentiel. Pour la variable `context`, un nouveau contexte est instancié :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Chaque propriété de référentiel vérifie si le référentiel existe déjà. Si ce n’est pas le cas, il instancie le référentiel, en passant l’instance de contexte. Par conséquent, tous les référentiels partagent la même instance de contexte.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

La méthode `Save` appelle `SaveChanges` sur le contexte de base de données.

Comme toute classe qui instancie un contexte de base de données dans une variable de classe, la classe `UnitOfWork` implémente `IDisposable` et supprime le contexte.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Modification du contrôleur de cours pour utiliser la classe UnitOfWork et les référentiels

Remplacez le code que vous avez actuellement dans *CourseController.cs* par le code suivant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Ce code ajoute une variable de classe pour la classe `UnitOfWork`. (Si vous utilisiez des interfaces ici, vous n’auriez pas initialisé la variable ici. au lieu de cela, vous devez implémenter un modèle de deux constructeurs comme vous l’avez fait pour le référentiel `Student`.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

Dans le reste de la classe, toutes les références au contexte de base de données sont remplacées par des références au référentiel approprié, à l’aide des propriétés de `UnitOfWork` pour accéder au référentiel. La méthode `Dispose` supprime l’instance `UnitOfWork`.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Exécutez le site et cliquez sur l’onglet **courses** .

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

La page s’affiche et fonctionne comme avant vos modifications, et les autres pages de cours fonctionnent également de la même façon.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant implémenté les modèles de référentiel et d’unité de travail. Vous avez utilisé des expressions lambda comme paramètres de méthode dans le référentiel générique. Pour plus d’informations sur l’utilisation de ces expressions avec un objet `IQueryable`, consultez [IQueryable (t) interface (System. Linq)](https://msdn.microsoft.com/library/bb351562.aspx) dans MSDN Library. Dans le didacticiel suivant, vous apprendrez à gérer certains scénarios avancés.

Vous trouverez des liens vers d’autres ressources de Entity Framework dans le [plan de contenu d’accès aux données ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Suivant](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
