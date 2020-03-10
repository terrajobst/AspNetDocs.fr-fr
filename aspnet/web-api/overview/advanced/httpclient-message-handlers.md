---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Gestionnaires de messages HttpClient dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Créer des gestionnaires de messages personnalisés pour API Web ASP.NET dans ASP.NET 4. x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 265bd9b2f48ed7d1e955f3c4947d10fd589b3e17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557644"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a>Gestionnaires de messages HttpClient dans API Web ASP.NET

par [Mike Wasson](https://github.com/MikeWasson)

Un *Gestionnaire de messages* est une classe qui reçoit une requête HTTP et retourne une réponse http.

En général, une série de gestionnaires de messages sont chaînés ensemble. Le premier gestionnaire reçoit une requête HTTP, effectue un traitement et transmet la requête au gestionnaire suivant. À un moment donné, la réponse est créée et reprend la chaîne. Ce modèle est appelé gestionnaire de *délégation* .

![](httpclient-message-handlers/_static/image1.png)

Côté client, la classe **httpclient** utilise un gestionnaire de messages pour traiter les demandes. Le gestionnaire par défaut est **HttpClientHandler**, qui envoie la demande sur le réseau et obtient la réponse du serveur. Vous pouvez insérer des gestionnaires de messages personnalisés dans le pipeline client :

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> API Web ASP.NET utilise également des gestionnaires de messages côté serveur. Pour plus d’informations, consultez [gestionnaires de messages http](http-message-handlers.md).

## <a name="custom-message-handlers"></a>Gestionnaires de messages personnalisés

Pour écrire un gestionnaire de messages personnalisé, dérivez de **System .net. http. DelegatingHandler** et substituez la méthode **SendAsync** . Voici la signature de méthode :

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

La méthode prend un **HttpRequestMessage** comme entrée et retourne de manière asynchrone un **HttpResponseMessage**. Une implémentation classique effectue les opérations suivantes :

1. Traiter le message de demande.
2. Appelez `base.SendAsync` pour envoyer la demande au gestionnaire interne.
3. Le gestionnaire interne retourne un message de réponse. (Cette étape est asynchrone.)
4. Traitez la réponse et renvoyez-la à l’appelant.

L’exemple suivant illustre un gestionnaire de messages qui ajoute un en-tête personnalisé à la demande sortante :

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

L’appel à `base.SendAsync` est asynchrone. Si le gestionnaire effectue une opération après cet appel, utilisez le mot clé **await** pour reprendre l’exécution une fois la méthode terminée. L’exemple suivant montre un gestionnaire qui journalise les codes d’erreur. La journalisation elle-même n’est pas très intéressante, mais l’exemple montre comment accéder à la réponse à l’intérieur du gestionnaire.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Ajout de gestionnaires de messages au pipeline client

Pour ajouter des gestionnaires personnalisés à **httpclient**, utilisez la méthode **HttpClientFactory. Create** :

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Les gestionnaires de messages sont appelés dans l’ordre dans lequel vous les transmettez à la méthode **Create** . Étant donné que les gestionnaires sont imbriqués, le message de réponse se déplace dans l’autre direction. Autrement dit, le dernier gestionnaire est le premier à recevoir le message de réponse.
