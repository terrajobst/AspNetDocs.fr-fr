---
uid: webhooks/receiving/handlers
title: Gestionnaires de webhooks ASP.NET | Microsoft Docs
author: rick-anderson
description: Comment gérer les requêtes dans des webhooks ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637871"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="27d1c-103">Gestionnaires de webhooks ASP.NET</span><span class="sxs-lookup"><span data-stu-id="27d1c-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="27d1c-104">Une fois que les demandes de webhooks ont été validées par un récepteur webhook, elles sont prêtes à être traitées par le code utilisateur.</span><span class="sxs-lookup"><span data-stu-id="27d1c-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="27d1c-105">C’est là qu’interviennent les *gestionnaires* .</span><span class="sxs-lookup"><span data-stu-id="27d1c-105">This is where *handlers* come in.</span></span> <span data-ttu-id="27d1c-106">Les gestionnaires dérivent de l’interface [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) , mais utilisent généralement la classe [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) au lieu de dériver directement de l’interface.</span><span class="sxs-lookup"><span data-stu-id="27d1c-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="27d1c-107">Une demande de webhook peut être traitée par un ou plusieurs gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="27d1c-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="27d1c-108">Les gestionnaires sont appelés dans l’ordre en fonction de leur propriété de *classement* respective, de la plus petite à la plus élevée, où l’ordre est un entier simple (suggéré pour être compris entre 1 et 100) :</span><span class="sxs-lookup"><span data-stu-id="27d1c-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagramme des propriétés d’ordre du gestionnaire de webhook](_static/Handlers.png)

<span data-ttu-id="27d1c-110">Un gestionnaire peut éventuellement définir la propriété *Response* sur le WebHookHandlerContext, ce qui entraînera l’arrêt du traitement et la réponse à renvoyer en tant que réponse http au webhook.</span><span class="sxs-lookup"><span data-stu-id="27d1c-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="27d1c-111">Dans le cas ci-dessus, le gestionnaire C n’est pas appelé, car il a un ordre supérieur à B et B définit la réponse.</span><span class="sxs-lookup"><span data-stu-id="27d1c-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="27d1c-112">La définition de la réponse est généralement uniquement applicable aux webhooks où la réponse peut retransmettre des informations à l’API d’origine.</span><span class="sxs-lookup"><span data-stu-id="27d1c-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="27d1c-113">C’est par exemple le cas avec des webhooks de marge où la réponse est publiée sur le canal d’où provient le webhook.</span><span class="sxs-lookup"><span data-stu-id="27d1c-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="27d1c-114">Les gestionnaires peuvent définir la propriété Receiver s’ils veulent uniquement recevoir des webhooks de ce récepteur particulier.</span><span class="sxs-lookup"><span data-stu-id="27d1c-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="27d1c-115">Si elles ne définissent pas le récepteur, elles sont appelées pour chacune d’elles.</span><span class="sxs-lookup"><span data-stu-id="27d1c-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="27d1c-116">Une autre utilisation courante d’une réponse consiste à utiliser une réponse *410 disparu* pour indiquer que le webhook n’est plus actif et qu’aucune autre demande ne doit être envoyée.</span><span class="sxs-lookup"><span data-stu-id="27d1c-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="27d1c-117">Par défaut, un gestionnaire est appelé par tous les récepteurs de webhook.</span><span class="sxs-lookup"><span data-stu-id="27d1c-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="27d1c-118">Toutefois, si la propriété *Receiver* est définie sur le nom d’un gestionnaire, ce gestionnaire recevra uniquement les demandes webhook de ce récepteur.</span><span class="sxs-lookup"><span data-stu-id="27d1c-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="27d1c-119">Traitement d’un webhook</span><span class="sxs-lookup"><span data-stu-id="27d1c-119">Processing a WebHook</span></span>

<span data-ttu-id="27d1c-120">Lorsqu’un gestionnaire est appelé, il obtient un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenant des informations sur la demande de webhook.</span><span class="sxs-lookup"><span data-stu-id="27d1c-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="27d1c-121">Les données, généralement le corps de la requête HTTP, sont disponibles à partir de la propriété de *données* .</span><span class="sxs-lookup"><span data-stu-id="27d1c-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="27d1c-122">Le type des données est généralement JSON ou des données de formulaire HTML, mais il est possible d’effectuer un cast en un type plus spécifique si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="27d1c-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="27d1c-123">Par exemple, les webhooks personnalisés générés par les webhooks ASP.NET peuvent être convertis en type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) comme suit :</span><span class="sxs-lookup"><span data-stu-id="27d1c-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="27d1c-124">Traitement en file d’attente</span><span class="sxs-lookup"><span data-stu-id="27d1c-124">Queued Processing</span></span>

<span data-ttu-id="27d1c-125">La plupart des expéditeurs webhook renvoient un webhook si aucune réponse n’est générée dans un délai de quelques secondes.</span><span class="sxs-lookup"><span data-stu-id="27d1c-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="27d1c-126">Cela signifie que votre gestionnaire doit terminer le traitement dans ce laps de temps afin de ne pas l’appeler à nouveau.</span><span class="sxs-lookup"><span data-stu-id="27d1c-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="27d1c-127">Si le traitement prend plus de temps ou est mieux géré séparément, le [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) peut être utilisé pour envoyer la demande de webhook à une file d’attente, par exemple la [file d’attente de stockage Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="27d1c-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="27d1c-128">Un plan d’implémentation de [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) est fourni ici :</span><span class="sxs-lookup"><span data-stu-id="27d1c-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
