---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Gestion des erreurs globales dans API Web ASP.NET 2-ASP.NET 4. x
author: davidmatson
description: Vue d’ensemble de la gestion des erreurs globales dans API Web ASP.NET 2 pour ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 94f2d6d31d0b37f9bb0077e6258c70a2dfb1918d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457737"
---
# <a name="global-error-handling-in-aspnet-web-api-2"></a>Gestion des erreurs globales dans API Web ASP.NET 2

par [David Matson](https://github.com/davidmatson), [Rick Anderson](https://twitter.com/RickAndMSFT)

Cette rubrique fournit une vue d’ensemble de la gestion des erreurs globales dans API Web ASP.NET 2 pour ASP.NET 4. x. À l’heure actuelle, il n’existe pas de moyen simple d’enregistrer ou de gérer les erreurs dans l’API Web. Certaines exceptions non gérées peuvent être traitées via des [filtres d’exception](exception-handling.md), mais il existe plusieurs cas où les filtres d’exception ne peuvent pas être gérés. Par exemple :

1. Les exceptions lancées à partir des constructeurs de contrôleur.
2. Les exceptions lancées à partir des gestionnaires de messages.
3. Les exceptions lancées pendant le routage.
4. Exceptions levées lors de la sérialisation du contenu de la réponse.

Nous souhaitons fournir une méthode simple et cohérente pour consigner et gérer (dans la mesure du possible) ces exceptions. 

Il existe deux cas principaux pour la gestion des exceptions, le cas où nous pouvons envoyer une réponse d’erreur et le cas où tout ce que nous pouvons faire est de consigner l’exception. C’est le cas, par exemple, lorsqu’une exception est levée au milieu du contenu de la réponse de streaming. dans ce cas, il est trop tard pour envoyer un nouveau message de réponse dans la mesure où le code d’État, les en-têtes et le contenu partiel ont déjà été dépassés sur le câble, donc nous abandonnons simplement la connexion. Même si l’exception ne peut pas être gérée pour produire un nouveau message de réponse, nous prenons toujours en charge l’enregistrement de l’exception. Dans les cas où nous pouvons détecter une erreur, nous pouvons retourner une réponse d’erreur appropriée, comme illustré ci-dessous :

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Options existantes

En plus des [filtres d’exception](exception-handling.md), les [gestionnaires de messages](../advanced/http-message-handlers.md) peuvent être utilisés aujourd’hui pour observer toutes les réponses de niveau 500, mais le fait d’agir sur ces réponses est difficile, car ils n’ont pas de contexte sur l’erreur d’origine. Les gestionnaires de messages présentent également les mêmes limitations que les filtres d’exception en ce qui concerne les cas qu’ils peuvent gérer. Alors que l’API Web dispose d’une infrastructure de suivi qui capture des conditions d’erreur, l’infrastructure de suivi est à des fins de diagnostic et n’est pas conçue ou adaptée pour s’exécuter dans des environnements de production. La gestion globale des exceptions et la journalisation doivent être des services qui peuvent s’exécuter pendant la production et être connectés à des solutions de surveillance existantes (par exemple, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Vue d'ensemble de la solution

 Nous fournissons deux nouveaux services remplaçables par l’utilisateur, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) et IExceptionHandler, pour consigner et gérer les exceptions non gérées. Les services sont très similaires, avec deux différences principales :

1. Nous prenons en charge l’enregistrement de plusieurs enregistreurs d’exceptions, mais un seul gestionnaire d’exceptions.
2. Les enregistreurs d’exception sont toujours appelés, même si nous sommes sur le paragraphe d’abandonner la connexion. Les gestionnaires d’exceptions sont appelés uniquement lorsque vous pouvez toujours choisir le message de réponse à envoyer.

Les deux services fournissent un accès à un contexte d’exception contenant les informations pertinentes à partir du point où l’exception a été détectée, notamment [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), l’exception levée et la source de l’exception (détails ci-dessous).

### <a name="design-principles"></a>Principes de conception

1. **Aucune modification avec rupture** Étant donné que cette fonctionnalité est ajoutée dans une version mineure, une contrainte importante affectant la solution est qu’il n’y a aucune modification avec rupture, que ce soit pour taper des contrats ou un comportement. Cette contrainte élimine le nettoyage que nous aimerions faire en termes de blocs catch existants qui transforment les exceptions en réponses 500. Ce nettoyage supplémentaire est un point que nous pourrions envisager pour une version majeure ultérieure. Si cela est important pour vous, veuillez voter sur [API Web ASP.net user Voice](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Maintien de la cohérence avec les constructions d’API Web** Le pipeline de filtre de l’API Web est un excellent moyen de gérer les problèmes transversaux avec la flexibilité de l’application de la logique au niveau de l’étendue spécifique au contrôleur ou à l’action. Les filtres, y compris les filtres d’exception, ont toujours des contextes d’action et de contrôleur, même lorsqu’ils sont inscrits au niveau de la portée globale. Ce contrat a un sens pour les filtres, mais cela signifie que les filtres d’exception, même ceux dont la portée est limitée, ne sont pas adaptés à certains cas de gestion des exceptions, tels que les exceptions des gestionnaires de messages, où aucune action ou aucun contexte de contrôleur n’existe. Si vous souhaitez utiliser la portée flexible offerte par les filtres pour la gestion des exceptions, nous avons encore besoin de filtres d’exception. Toutefois, si nous devons gérer une exception en dehors d’un contexte de contrôleur, nous avons également besoin d’une construction distincte pour la gestion globale des erreurs complète (sans le contexte du contrôleur et les contraintes de contexte de l’action).

### <a name="when-to-use"></a>Quand l’utiliser

- Les enregistreurs d’exception sont la solution qui permet de voir toutes les exceptions non gérées interceptées par l’API Web.
- Les gestionnaires d’exceptions sont la solution permettant de personnaliser toutes les réponses possibles aux exceptions non gérées interceptées par l’API Web.
- Les filtres d’exception constituent la solution la plus simple pour traiter les exceptions non gérées de sous-ensemble liées à une action ou à un contrôleur spécifique.

### <a name="service-details"></a>Détails du service

 Les interfaces de service du journal et du gestionnaire d’exceptions sont des méthodes Async simples qui prennent les contextes respectifs : 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Nous fournissons également des classes de base pour ces deux interfaces. La substitution de la méthode de base (Sync ou Async) est tout ce qui est nécessaire pour la journalisation ou le descripteur aux moments recommandés. Pour la journalisation, la classe de base `ExceptionLogger` garantit que la méthode de journalisation principale est appelée une seule fois pour chaque exception (même si elle propage ultérieurement la pile des appels et est de nouveau interceptée). La classe de base `ExceptionHandler` appelle la méthode de gestion Core uniquement pour les exceptions situées en haut de la pile des appels, en ignorant les blocs catch imbriqués hérités. (Les versions simplifiées de ces classes de base figurent dans l’annexe ci-dessous.) `IExceptionLogger` et `IExceptionHandler` recevoir des informations sur l’exception par le biais d’un `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Quand le Framework appelle un enregistreur d’exceptions ou un gestionnaire d’exceptions, il fournit toujours une `Exception` et un `Request`. À l’exception des tests unitaires, il fournira toujours une `RequestContext`. Il fournit rarement un `ControllerContext` et `ActionContext` (uniquement lors d’un appel à partir du bloc catch pour les filtres d’exception). Il fournira très rarement un `Response`(uniquement dans certains cas IIS lorsqu’il tente d’écrire la réponse). Notez que certaines de ces propriétés peuvent être `null` il revient au consommateur de vérifier `null` avant d’accéder aux membres de la classe d’exception.`CatchBlock` est une chaîne indiquant le bloc catch qui a vu l’exception. Les chaînes de bloc catch sont les suivantes :

- HttpServer (méthode SendAsync)
- HttpControllerDispatcher (méthode SendAsync)
- HttpBatchHandler (méthode SendAsync)
- IExceptionFilter (traitement par ApiController du pipeline de filtre d’exception dans ExecuteAsync)
- Hôte OWIN :

    - HttpMessageHandlerAdapter. BufferResponseContentAsync (pour la sortie de la mise en mémoire tampon)
    - HttpMessageHandlerAdapter. CopyResponseContentAsync (pour la sortie de diffusion en continu)
- Hôte Web :

    - HttpControllerHandler. WriteBufferedResponseContentAsync (pour la sortie de la mise en mémoire tampon)
    - HttpControllerHandler. WriteStreamedResponseContentAsync (pour la sortie de diffusion en continu)
    - HttpControllerHandler. WriteErrorResponseContentAsync (pour les échecs de récupération d’erreur en mode de sortie mis en mémoire tampon)

La liste des chaînes de bloc catch est également disponible via les propriétés ReadOnly statiques. (La chaîne de bloc catch principale se trouve sur le ExceptionCatchBlocks statique ; le reste apparaît sur une classe statique pour OWIN et l’hôte Web).`IsTopLevelCatchBlock` est utile pour suivre le modèle recommandé de gestion des exceptions uniquement en haut de la pile des appels. Au lieu de transformer les exceptions en réponses 500 partout où un bloc CATCH imbriqué se produit, un gestionnaire d’exceptions peut permettre la propagation des exceptions jusqu’à ce qu’elles soient sur le point d’être visibles par l’hôte.

En plus de la `ExceptionContext`, un enregistreur d’événements obtient une plus grande partie des informations via le `ExceptionLoggerContext`complet :

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

La deuxième propriété, `CanBeHandled`, permet à un enregistreur d’événements d’identifier une exception qui ne peut pas être gérée. Lorsque la connexion va être abandonnée et qu’aucun nouveau message de réponse ne peut être envoyé, les enregistreurs d’événements sont appelés, mais le gestionnaire n’est ***pas*** appelé, et les journaux peuvent identifier ce scénario à partir de cette propriété.

En plus de la `ExceptionContext`, un gestionnaire obtient une propriété supplémentaire qu’il peut définir sur le `ExceptionHandlerContext` complet pour gérer l’exception :

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Un gestionnaire d’exceptions indique qu’il a traité une exception en affectant à la propriété `Result` un résultat d’action (par exemple, [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)ou un résultat personnalisé). Si la propriété `Result` a la valeur null, l’exception n’est pas gérée et l’exception d’origine est à nouveau levée.

Pour les exceptions en haut de la pile des appels, nous avons effectué une étape supplémentaire pour garantir que la réponse est appropriée pour les appelants d’API. Si l’exception se propage jusqu’à l’hôte, l’appelant voit l’écran jaune de décès ou une autre réponse fournie par l’hôte, qui est généralement HTML et qui, en général, n’est pas une réponse d’erreur d’API appropriée. Dans ce cas, le résultat démarre une valeur non null, et uniquement si un gestionnaire d’exceptions personnalisé le redéfinit explicitement sur `null` (non géré), l’exception est propagée à l’hôte. Dans ce cas, la définition de `Result` sur `null` peut être utile pour deux scénarios :

1. API Web hébergée par OWIN avec l’intergiciel (middleware) de gestion des exceptions personnalisées inscrit avant/à l’extérieur de l’API Web.
2. Débogage local via un navigateur, où l’écran jaune de décès est en fait une réponse utile pour une exception non gérée.

Pour les enregistreurs d’exceptions et les gestionnaires d’exceptions, nous n’avons rien à récupérer si l’enregistreur d’événements ou le gestionnaire lui-même lève une exception. (À l’exception de la propagation de l’exception, laissez vos commentaires en bas de cette page si vous avez une meilleure approche.) Le contrat pour les enregistreurs d’événements d’exception et les gestionnaires est qu’ils ne doivent pas laisser les exceptions se propager à leurs appelants ; dans le cas contraire, l’exception se propage simplement, souvent jusqu’à ce que l’hôte génère une erreur HTML (comme ASP. Écran jaune de NET) renvoyé au client (qui n’est généralement pas l’option préférée pour les appelants d’API qui attendent JSON ou XML).

## <a name="examples"></a>Exemples

### <a name="tracing-exception-logger"></a>Journal des exceptions de suivi

L’enregistreur d’exceptions ci-dessous envoie des données d’exception aux sources de suivi configurées (y compris la fenêtre sortie de débogage dans Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Gestionnaire d’exceptions de message d’erreur personnalisé

Les éléments suivants génèrent une réponse d’erreur personnalisée aux clients, y compris une adresse de messagerie pour contacter le support technique.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Inscription des filtres d’exception

Si vous utilisez le modèle de projet « application Web ASP.NET MVC 4 » pour créer votre projet, placez le code de configuration de votre API Web à l’intérieur de la classe `WebApiConfig`, dans le dossier *application/_Start* :

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Annexe : détails de la classe de base

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
