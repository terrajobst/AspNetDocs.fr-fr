---
uid: web-api/overview/error-handling/exception-handling
title: Gestion des exceptions dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.custom: seoapril2019
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: dbdbab6aefec840e2fec9e9cd33f3d124093750e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622317"
---
# <a name="exception-handling-in-aspnet-web-api"></a>Gestion des exceptions dans API Web ASP.NET

par [Mike Wasson](https://github.com/MikeWasson)

Cet article décrit la gestion des erreurs et des exceptions dans API Web ASP.NET.

- [HttpResponseException](#httpresponserexception)
- [Filtres d’exception](#exception_filters)
- [Inscription des filtres d’exception](#registering_exception_filters)
- [Erreur http](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Que se passe-t-il si un contrôleur d’API Web lève une exception non interceptée ? Par défaut, la plupart des exceptions sont converties en réponse HTTP avec le code d’état 500, erreur de serveur interne.

Le type **HttpResponseException** est un cas spécial. Cette exception retourne le code d’état HTTP que vous spécifiez dans le constructeur d’exception. Par exemple, la méthode suivante retourne 404, introuvable, si le paramètre *ID* n’est pas valide.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Pour mieux contrôler la réponse, vous pouvez également construire l’intégralité du message de réponse et l’inclure dans le **HttpResponseException :** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtres d’exception

Vous pouvez personnaliser la façon dont l’API Web gère les exceptions en écrivant un *filtre d’exception*. Un filtre d’exception est exécuté lorsqu’une méthode de contrôleur lève une exception non gérée qui n’est *pas* une exception **HttpResponseException** . Le type **HttpResponseException** est un cas particulier, car il est conçu spécifiquement pour retourner une réponse http.

Les filtres d’exceptions implémentent l’interface **System. Web. http. filters. IExceptionFilter** . La façon la plus simple d’écrire un filtre d’exception consiste à dériver de la classe **System. Web. http. filters. ExceptionFilterAttribute** et à substituer la méthode **OnException** .

> [!NOTE]
> Les filtres d’exception dans API Web ASP.NET sont similaires à ceux de ASP.NET MVC. Toutefois, ils sont déclarés dans un espace de noms et une fonction séparés séparément. En particulier, la classe **HandleErrorAttribute** utilisée dans MVC ne gère pas les exceptions levées par les contrôleurs d’API Web.

Voici un filtre qui convertit les exceptions **NotImplementedException** en code d’état http 501, non implémenté :

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

La propriété **Response** de l’objet **HttpActionExecutedContext** contient le message de réponse HTTP qui sera envoyé au client.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Inscription des filtres d’exception

Plusieurs méthodes pour enregistrer un filtre d’exception d’API web sont possibles :

- Par action
- Par contrôleur
- Globalement

Pour appliquer le filtre à une action spécifique, ajoutez le filtre en tant qu’attribut à l’action :

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Pour appliquer le filtre à toutes les actions sur un contrôleur, ajoutez le filtre en tant qu’attribut à la classe de contrôleur :

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Pour appliquer le filtre globalement à tous les contrôleurs d’API Web, ajoutez une instance du filtre à la collection **GlobalConfiguration. Configuration. filters** . Les filtres d’exception de cette collection s’appliquent à n’importe quelle action de contrôleur d’API web.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Si vous utilisez le modèle de projet « application Web ASP.NET MVC 4 » pour créer votre projet, placez le code de configuration de votre API Web à l’intérieur de la classe `WebApiConfig`, qui se trouve dans le dossier de démarrage de l’application\_:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>Erreur http

L’objet **erreur http** offre un moyen cohérent de retourner des informations d’erreur dans le corps de la réponse. L’exemple suivant montre comment renvoyer le code d’état HTTP 404 (introuvable) avec un **erreur http** dans le corps de la réponse.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** est une méthode d’extension définie dans la classe **System .net. http. HttpRequestMessageExtensions** . En interne, **CreateErrorResponse** crée une instance **erreur http** , puis crée un **HttpResponseMessage** qui contient le **erreur http**.

Dans cet exemple, si la méthode réussit, elle retourne le produit dans la réponse HTTP. Toutefois, si le produit demandé est introuvable, la réponse HTTP contient un **erreur http** dans le corps de la demande. La réponse peut se présenter comme suit :

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Notez que **erreur http** a été SÉRIALISÉ en JSON dans cet exemple. L’un des avantages de l’utilisation de **erreur http** est qu’elle passe par le même processus de [négociation de contenu](../formats-and-model-binding/content-negotiation.md) et de sérialisation que tout autre modèle fortement typé.

### <a name="httperror-and-model-validation"></a>Erreur http et validation de modèle

Pour la validation de modèle, vous pouvez passer l’état du modèle à **CreateErrorResponse**pour inclure les erreurs de validation dans la réponse :

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

Cet exemple peut retourner la réponse suivante :

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Pour plus d’informations sur la validation de modèle, consultez [validation de modèle dans API Web ASP.net](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Utilisation de erreur http avec HttpResponseException

Les exemples précédents renvoient un message **HttpResponseMessage** à partir de l’action du contrôleur, mais vous pouvez également utiliser **HttpResponseException** pour retourner un **erreur http**. Cela vous permet de retourner un modèle fortement typé dans le cas d’une réussite normale, tout en retournant **erreur http** en cas d’erreur :

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
