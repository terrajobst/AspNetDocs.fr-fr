---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: Fonctionnement des filtres d’action (VB) | Microsoft Docs
author: microsoft
description: L’objectif de ce didacticiel est d’expliquer les filtres d’action. Un filtre d’action est un attribut que vous pouvez appliquer à une action de contrôleur ou à un contrôleur entier...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 263658231ccaa7863508c691a3570bc00b9e8039
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74590018"
---
# <a name="understanding-action-filters-vb"></a>Présentation des filtres d’actions (VB)

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> L’objectif de ce didacticiel est d’expliquer les filtres d’action. Un filtre d’action est un attribut que vous pouvez appliquer à une action de contrôleur, ou à un contrôleur entier, qui modifie la manière dont l’action est exécutée.

## <a name="understanding-action-filters"></a>Fonctionnement des filtres d’action

L’objectif de ce didacticiel est d’expliquer les filtres d’action. Un filtre d’action est un attribut que vous pouvez appliquer à une action de contrôleur, ou à un contrôleur entier, qui modifie la manière dont l’action est exécutée. L’infrastructure MVC ASP.NET comprend plusieurs filtres d’action :

- OutputCache : ce filtre d’action met en cache la sortie d’une action de contrôleur pendant un laps de temps spécifié.
- HandleError : ce filtre d’action gère les erreurs déclenchées lors de l’exécution d’une action de contrôleur.
- Authorize : ce filtre d’action vous permet de restreindre l’accès à un utilisateur ou à un rôle particulier.

Vous pouvez également créer vos propres filtres d’action personnalisés. Par exemple, vous souhaiterez peut-être créer un filtre d’action personnalisé afin d’implémenter un système d’authentification personnalisé. Vous pouvez également créer un filtre d’action qui modifie les données d’affichage retournées par une action de contrôleur.

Dans ce didacticiel, vous allez apprendre à créer un filtre d’action à partir de zéro. Nous créons un filtre d’action de journal qui journalise différentes étapes du traitement d’une action dans la fenêtre sortie de Visual Studio.

### <a name="using-an-action-filter"></a>Utilisation d’un filtre d’action

Un filtre d’action est un attribut. Vous pouvez appliquer la plupart des filtres d’action à une action de contrôleur individuel ou à un contrôleur entier.

Par exemple, le contrôleur de données de la liste 1 expose une action nommée `Index()` qui retourne l’heure actuelle. Cette action est décorée avec le filtre d’action `OutputCache`. Ce filtre entraîne la mise en cache de la valeur retournée par l’action pendant 10 secondes.

**Liste 1 – `Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Si vous appelez de manière répétée l’action `Index()` en entrant l’URL/Data/Index dans la barre d’adresses de votre navigateur et en appuyant plusieurs fois sur le bouton actualiser, vous verrez la même durée pendant 10 secondes. La sortie de l’action `Index()` est mise en cache pendant 10 secondes (voir la figure 1).

[temps mis en cache ![](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Figure 01**: temps mis en cache ([cliquez pour afficher l’image en taille réelle](understanding-action-filters-vb/_static/image3.png))

Dans la liste 1, un filtre d’action unique, le filtre d’action `OutputCache`, est appliqué à la méthode `Index()`. Si vous le souhaitez, vous pouvez appliquer plusieurs filtres d’action à la même action. Par exemple, vous souhaiterez peut-être appliquer à la fois les filtres d’action `OutputCache` et `HandleError` à la même action.

Dans la liste 1, le filtre d’action `OutputCache` est appliqué à l’action `Index()`. Vous pouvez également appliquer cet attribut à la classe `DataController` elle-même. Dans ce cas, le résultat retourné par une action exposée par le contrôleur est mis en cache pendant 10 secondes.

### <a name="the-different-types-of-filters"></a>Les différents types de filtres

L’infrastructure MVC ASP.NET prend en charge quatre types de filtres :

1. Filtres d’autorisation : implémente l’attribut `IAuthorizationFilter`.
2. Filtres d’action : implémente l’attribut `IActionFilter`.
3. Filtres de résultats : implémente l’attribut `IResultFilter`.
4. Filtres d’exception : implémente l’attribut `IExceptionFilter`.

Les filtres sont exécutés dans l’ordre indiqué ci-dessus. Par exemple, les filtres d’autorisation sont toujours exécutés avant que les filtres d’action et les filtres d’exception soient toujours exécutés après chaque autre type de filtre.

Les filtres d’autorisation sont utilisés pour implémenter l’authentification et l’autorisation pour les actions du contrôleur. Par exemple, le filtre authorize est un exemple de filtre d’autorisation.

Les filtres d’action contiennent une logique qui est exécutée avant et après l’exécution d’une action de contrôleur. Vous pouvez utiliser un filtre d’action, par exemple, pour modifier les données d’affichage retournées par une action de contrôleur.

Les filtres de résultats contiennent une logique qui est exécutée avant et après l’exécution d’un résultat de vue. Par exemple, vous souhaiterez peut-être modifier un résultat d’affichage juste avant que la vue ne soit rendue dans le navigateur.

Les filtres d’exceptions sont le dernier type de filtre à exécuter. Vous pouvez utiliser un filtre d’exception pour gérer les erreurs générées par les actions du contrôleur ou les résultats de l’action du contrôleur. Vous pouvez également utiliser des filtres d’exception pour consigner les erreurs.

Chaque type de filtre est exécuté dans un ordre particulier. Si vous souhaitez contrôler l’ordre dans lequel les filtres du même type sont exécutés, vous pouvez définir la propriété d’ordre d’un filtre.

La classe de base pour tous les filtres d’action est la classe `System.Web.Mvc.FilterAttribute`. Si vous souhaitez implémenter un type particulier de filtre, vous devez créer une classe qui hérite de la classe de filtre de base et implémente une ou plusieurs des interfaces IAuthorizationFilter, IActionFilter, IResultFilter ou ExceptionFilter.

### <a name="the-base-actionfilterattribute-class"></a>Classe de base ActionFilterAttribute

Pour faciliter l’implémentation d’un filtre d’action personnalisé, l’infrastructure MVC ASP.NET comprend une classe de `ActionFilterAttribute` de base. Cette classe implémente à la fois les interfaces `IActionFilter` et `IResultFilter` et hérite de la classe `Filter`.

La terminologie ici n’est pas entièrement cohérente. Techniquement, une classe qui hérite de la classe ActionFilterAttribute est à la fois un filtre d’action et un filtre de résultat. Toutefois, dans le sens approximatif, le filtre d’action Word est utilisé pour faire référence à n’importe quel type de filtre dans l’infrastructure MVC ASP.NET.

La classe de base ActionFilterAttribute contient les méthodes suivantes que vous pouvez substituer :

- OnActionExecuting : cette méthode est appelée avant l’exécution d’une action de contrôleur.
- OnActionExecuted : cette méthode est appelée après l’exécution d’une action de contrôleur.
- OnResultExecuting : cette méthode est appelée avant l’exécution d’un résultat d’action de contrôleur.
- OnResultExecuted : cette méthode est appelée après l’exécution d’un résultat d’action de contrôleur.

Dans la section suivante, nous verrons comment vous pouvez implémenter chacune de ces différentes méthodes.

### <a name="creating-a-log-action-filter"></a>Création d’un filtre d’action de journal

Pour illustrer la façon dont vous pouvez créer un filtre d’action personnalisé, nous allons créer un filtre d’action personnalisé qui journalise les étapes de traitement d’une action de contrôleur dans la fenêtre sortie de Visual Studio. Notre `LogActionFilter` est contenu dans la liste 2.

**Liste 2 – `ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

Dans la liste 2, les méthodes `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`et `OnResultExecuted()` appellent toutes la méthode `Log()`. Le nom de la méthode et les données d’itinéraire actuelles sont passés à la méthode `Log()`. La méthode `Log()` écrit un message dans la fenêtre sortie de Visual Studio (voir la figure 2).

[![l’écriture dans la fenêtre sortie de Visual Studio](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Figure 02**: écriture dans la fenêtre sortie de Visual Studio ([cliquez pour afficher l’image en taille réelle](understanding-action-filters-vb/_static/image6.png))

Le contrôleur d’hébergement de la liste 3 montre comment vous pouvez appliquer le filtre d’action de journal à une classe de contrôleur entière. Chaque fois que l’une des actions exposées par le contrôleur d’hébergement est appelée, soit la méthode `Index()` soit la méthode `About()` : les étapes de traitement de l’action sont enregistrées dans la fenêtre sortie de Visual Studio.

**Liste 3 – `Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez introduit les filtres d’action MVC ASP.NET. Vous avez appris les quatre types de filtres : les filtres d’autorisation, les filtres d’action, les filtres de résultats et les filtres d’exception. Vous avez également appris sur la classe de base `ActionFilterAttribute`.

Enfin, vous avez appris à implémenter un filtre d’action simple. Nous avons créé un filtre d’action de journal qui journalise les étapes de traitement d’une action de contrôleur dans la fenêtre sortie de Visual Studio.

> [!div class="step-by-step"]
> [Précédent](asp-net-mvc-routing-overview-vb.md)
> [Suivant](improving-performance-with-output-caching-vb.md)
