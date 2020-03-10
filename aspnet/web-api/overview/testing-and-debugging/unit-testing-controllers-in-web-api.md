---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Contrôleurs de test unitaire dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Cette rubrique décrit des techniques spécifiques pour les contrôleurs de test unitaire dans Web API 2. Avant de lire cette rubrique, vous souhaiterez peut-être lire l’unité du didacticiel...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: cdb1700537021e276669de1a9e0330a62659746c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554991"
---
# <a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Tests unitaires des contrôleurs dans ASP.NET Web API 2

par [Mike Wasson](https://github.com/MikeWasson)

> Cette rubrique décrit des techniques spécifiques pour les contrôleurs de test unitaire dans Web API 2. Avant de lire cette rubrique, vous souhaiterez peut-être lire le didacticiel sur les [tests unitaires API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md), qui montre comment ajouter un projet de test unitaire à votre solution.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2
> - [MOQ](https://github.com/Moq) 4.5.30

> [!NOTE]
> J’ai utilisé MOQ, mais la même idée s’applique à n’importe quel Framework fictif. MOQ 4.5.30 (et versions ultérieures) prend en charge Visual Studio 2017, Roslyn et .NET 4,5 et versions ultérieures.

Un modèle courant de tests unitaires est &quot;les&quot;arrange-Act-Assert :

- Réorganisation : configurez les composants requis pour l’exécution du test.
- Act : effectuez le test.
- Assertion : Vérifiez que le test a réussi.

Dans l’étape de réorganisation, vous utilisez souvent des objets fictifs ou stub. Qui réduit le nombre de dépendances, le test est donc axé sur le test d’une seule chose.

Voici quelques éléments que vous devez tester par unité dans vos contrôleurs d’API Web :

- L’action retourne le type de réponse correct.
- Les paramètres non valides renvoient la réponse d’erreur correcte.
- L’action appelle la méthode correcte sur le référentiel ou la couche de service.
- Si la réponse comprend un modèle de domaine, vérifiez le type de modèle.

Voici quelques-uns des éléments généraux à tester, mais les spécificités dépendent de l’implémentation de votre contrôleur. En particulier, il est important de savoir si vos actions de contrôleur retournent **HttpResponseMessage** ou **IHttpActionResult**. Pour plus d’informations sur ces types de résultats, consultez [résultats des actions dans l’API Web 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Test des actions qui retournent HttpResponseMessage

Voici un exemple de contrôleur dont les actions retournent **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Notez que le contrôleur utilise l’injection de dépendances pour injecter une `IProductRepository`. Cela rend le contrôleur plus testable, car vous pouvez injecter un référentiel fictif. Le test unitaire suivant vérifie que la méthode `Get` écrit un `Product` dans le corps de la réponse. Supposons que `repository` est un simulacre `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Il est important de définir la **demande** et la **configuration** sur le contrôleur. Dans le cas contraire, le test échoue avec **ArgumentNullException** ou **InvalidOperationException**.

## <a name="testing-link-generation"></a>Test de la génération de liens

La méthode `Post` appelle **UrlHelper. Link** pour créer des liens dans la réponse. Cela nécessite un peu plus d’installation dans le test unitaire :

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

La classe **UrlHelper** a besoin de l’URL de la requête et des données d’itinéraire, de sorte que le test doit définir des valeurs pour celles-ci. Une autre option est un simulacre ou un stub **UrlHelper**. Avec cette approche, vous remplacez la valeur par défaut de [ApiController. URL](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) par une version factice ou stub qui retourne une valeur fixe.

Nous allons réécrire le test à l’aide de l’infrastructure [MOQ](https://github.com/Moq) . Installez le package NuGet `Moq` dans le projet de test.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

Dans cette version, vous n’avez pas besoin de configurer de données d’itinéraire, car le simulacre **UrlHelper** retourne une chaîne constante.

## <a name="testing-actions-that-return-ihttpactionresult"></a>Test des actions qui retournent IHttpActionResult

Dans l’API Web 2, une action de contrôleur peut retourner **IHttpActionResult**, ce qui est analogue à **ACTIONRESULT** dans ASP.NET MVC. L’interface **IHttpActionResult** définit un modèle de commande pour la création de réponses http. Au lieu de créer la réponse directement, le contrôleur retourne un **IHttpActionResult**. Plus tard, le pipeline appelle **IHttpActionResult** pour créer la réponse. Cette approche facilite l’écriture de tests unitaires, car vous pouvez ignorer un grand nombre de configurations nécessaires pour **HttpResponseMessage**.

Voici un exemple de contrôleur dont les actions retournent **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Cet exemple montre des modèles courants à l’aide de **IHttpActionResult**. Voyons comment effectuer des tests unitaires.

### <a name="action-returns-200-ok-with-a-response-body"></a>L’action renvoie 200 (OK) avec un corps de réponse

La méthode `Get` appelle `Ok(product)` si le produit est trouvé. Dans le test unitaire, assurez-vous que le type de retour est **OkNegotiatedContentResult** et que le produit retourné a l’ID correct.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Notez que le test unitaire n’exécute pas le résultat de l’action. Vous pouvez supposer que le résultat de l’action crée la réponse HTTP correctement. (C’est la raison pour laquelle l’infrastructure de l’API Web a ses propres tests unitaires !)

### <a name="action-returns-404-not-found"></a>L’action renvoie 404 (introuvable)

La méthode `Get` appelle `NotFound()` si le produit est introuvable. Dans ce cas, le test unitaire vérifie uniquement si le type de retour est **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>L’action renvoie 200 (OK) sans corps de réponse

La méthode `Delete` appelle `Ok()` pour retourner une réponse HTTP 200 vide. Comme dans l’exemple précédent, le test unitaire vérifie le type de retour, dans ce cas **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>L’action renvoie 201 (créé) avec un en-tête d’emplacement

La méthode `Post` appelle `CreatedAtRoute` pour retourner une réponse HTTP 201 avec un URI dans l’en-tête Location. Dans le test unitaire, vérifiez que l’action définit les valeurs de routage correctes.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>L’action renvoie un autre 2XX avec un corps de réponse

La méthode `Put` appelle `Content` pour retourner une réponse HTTP 202 (acceptée) avec un corps de réponse. Ce cas de figure est semblable au retour de 200 (OK), mais le test unitaire doit également vérifier le code d’État.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Simulation d’Entity Framework lors du test unitaire API Web ASP.NET 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Écriture de tests pour un service API Web ASP.net](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (billet de blog de youssef moussaoui).
- [Débogage de API Web ASP.NET avec le débogueur de routage](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
