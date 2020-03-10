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
# <a name="aspnet-webhooks-handlers"></a>Gestionnaires de webhooks ASP.NET

Une fois que les demandes de webhooks ont été validées par un récepteur webhook, elles sont prêtes à être traitées par le code utilisateur. C’est là qu’interviennent les *gestionnaires* . Les gestionnaires dérivent de l’interface [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) , mais utilisent généralement la classe [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) au lieu de dériver directement de l’interface.

Une demande de webhook peut être traitée par un ou plusieurs gestionnaires. Les gestionnaires sont appelés dans l’ordre en fonction de leur propriété de *classement* respective, de la plus petite à la plus élevée, où l’ordre est un entier simple (suggéré pour être compris entre 1 et 100) :

![Diagramme des propriétés d’ordre du gestionnaire de webhook](_static/Handlers.png)

Un gestionnaire peut éventuellement définir la propriété *Response* sur le WebHookHandlerContext, ce qui entraînera l’arrêt du traitement et la réponse à renvoyer en tant que réponse http au webhook. Dans le cas ci-dessus, le gestionnaire C n’est pas appelé, car il a un ordre supérieur à B et B définit la réponse.

La définition de la réponse est généralement uniquement applicable aux webhooks où la réponse peut retransmettre des informations à l’API d’origine. C’est par exemple le cas avec des webhooks de marge où la réponse est publiée sur le canal d’où provient le webhook. Les gestionnaires peuvent définir la propriété Receiver s’ils veulent uniquement recevoir des webhooks de ce récepteur particulier. Si elles ne définissent pas le récepteur, elles sont appelées pour chacune d’elles.

Une autre utilisation courante d’une réponse consiste à utiliser une réponse *410 disparu* pour indiquer que le webhook n’est plus actif et qu’aucune autre demande ne doit être envoyée.

Par défaut, un gestionnaire est appelé par tous les récepteurs de webhook. Toutefois, si la propriété *Receiver* est définie sur le nom d’un gestionnaire, ce gestionnaire recevra uniquement les demandes webhook de ce récepteur.

## <a name="processing-a-webhook"></a>Traitement d’un webhook

Lorsqu’un gestionnaire est appelé, il obtient un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenant des informations sur la demande de webhook. Les données, généralement le corps de la requête HTTP, sont disponibles à partir de la propriété de *données* .

Le type des données est généralement JSON ou des données de formulaire HTML, mais il est possible d’effectuer un cast en un type plus spécifique si vous le souhaitez. Par exemple, les webhooks personnalisés générés par les webhooks ASP.NET peuvent être convertis en type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) comme suit :

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

  ## <a name="queued-processing"></a>Traitement en file d’attente

La plupart des expéditeurs webhook renvoient un webhook si aucune réponse n’est générée dans un délai de quelques secondes. Cela signifie que votre gestionnaire doit terminer le traitement dans ce laps de temps afin de ne pas l’appeler à nouveau.

Si le traitement prend plus de temps ou est mieux géré séparément, le [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) peut être utilisé pour envoyer la demande de webhook à une file d’attente, par exemple la [file d’attente de stockage Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Un plan d’implémentation de [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) est fourni ici :

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
