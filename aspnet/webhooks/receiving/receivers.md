---
uid: webhooks/receiving/receivers
title: Récepteurs de webhooks ASP.NET | Microsoft Docs
author: rick-anderson
description: Récepteurs de webhooks ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: d771a588b23abcd7b1b33e694af17b219683fc48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574192"
---
# <a name="aspnet-webhooks-receivers"></a>Récepteurs de webhooks ASP.NET

La réception de webhooks dépend de l’expéditeur. Il arrive parfois que des étapes supplémentaires inscrivent un webhook pour vérifier que l’abonné est vraiment à l’écoute. Certains webhooks fournissent un modèle push-to-pull dans lequel la requête HTTP POSTÉRIEURe contient uniquement une référence aux informations d’événement qui seront ensuite récupérées indépendamment. Le modèle de sécurité varie souvent légèrement.

L’objectif de Microsoft ASP.NET webhooks est de rendre plus simple et plus cohérent pour associer votre API sans perdre beaucoup de temps à savoir comment gérer une variante particulière des webhooks.

Un récepteur webhook est chargé d’accepter et de vérifier les webhooks d’un expéditeur particulier. Un récepteur de webhook peut prendre en charge un nombre quelconque de webhooks, chacun avec sa propre configuration. Par exemple, le récepteur webhook GitHub peut accepter des webhooks à partir d’un nombre quelconque de dépôts GitHub.

## <a name="webhook-receiver-uris"></a>URI du récepteur de webhook

En installant Microsoft ASP.NET webhooks, vous recevez un contrôleur webhook général qui accepte les requêtes de webhook à partir d’un nombre de services en cours d’achèvement. Lorsqu’une requête arrive, elle choisit le récepteur approprié que vous avez installé pour gérer un expéditeur de webhook particulier.

L’URI de ce contrôleur est l’URI de webhook que vous inscrivez auprès du service et se présente sous la forme :

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Pour des raisons de sécurité, de nombreux récepteurs de webhook requièrent que l’URI soit un URI *https* et, dans certains cas, il doit également contenir un paramètre de requête supplémentaire qui est utilisé pour garantir que seul le tiers prévu peut envoyer des webhooks à l’URI ci-dessus.

Le composant `<receiver>` est le nom du récepteur, par exemple `github` ou `slack`.

*{ID}* est un identificateur facultatif qui peut être utilisé pour identifier une configuration de récepteur de webhook particulière. Cela peut être utilisé pour inscrire N webhooks avec un récepteur particulier. Par exemple, les trois URI suivants peuvent être utilisés pour s’inscrire à trois webhooks indépendants :

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Installation d’un récepteur webhook

Pour recevoir des webhooks à l’aide de Microsoft ASP.NET webhook, vous devez d’abord installer le package NuGet pour le fournisseur webhook ou les fournisseurs à partir desquels vous souhaitez recevoir des webhooks. Les packages NuGet sont nommés [Microsoft. Aspnet. webhooks. récepteurs. *](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) où la dernière partie indique que le service est pris en charge. Exemple :

[Microsoft. Aspnet. webhooks. Decepteurs. GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fournit la prise en charge de la réception de webhooks à partir de GitHub et de [Microsoft. Aspnet. webhooks. les destinataires. Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fournit la prise en charge de la réception de webhooks générés par les webhooks ASP.net.

Prêt à l’emploi, vous pouvez trouver la prise en charge de Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, mou, Stripe, Trello et WordPress, mais il est possible de prendre en charge un nombre quelconque d’autres fournisseurs.

## <a name="configuring-a-webhook-receiver"></a>Configuration d’un récepteur webhook

Les récepteurs de webhook sont configurés via le inteface [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) et des implémentations spécifiques de cette interface peuvent être inscrites à l’aide de n’importe quel modèle d’injection de dépendances. L’implémentation par défaut utilise des paramètres d’application qui peuvent être définis dans le fichier Web. config ou, si vous utilisez Azure Web Apps, peuvent être définis via le [portail Azure](https://portal.azure.com/).

![Paramètres de l’application dans Microsoft Azure](_static/AzureAppSettings.png)

Le format des clés de paramètres d’application est le suivant :

```
MS_WebHookReceiverSecret_<receiver>
```

La valeur est une liste de valeurs séparées par des virgules correspondant aux valeurs *{ID}* pour lesquelles des webhooks ont été enregistrés, par exemple :

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Initialisation d’un récepteur webhook

Les récepteurs de webhooks sont initialisés en les inscrivant, généralement dans la classe statique *WebApiConfig* , par exemple :

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
