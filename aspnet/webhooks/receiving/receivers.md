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
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="b25a1-103">Récepteurs de webhooks ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b25a1-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="b25a1-104">La réception de webhooks dépend de l’expéditeur.</span><span class="sxs-lookup"><span data-stu-id="b25a1-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="b25a1-105">Il arrive parfois que des étapes supplémentaires inscrivent un webhook pour vérifier que l’abonné est vraiment à l’écoute.</span><span class="sxs-lookup"><span data-stu-id="b25a1-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="b25a1-106">Certains webhooks fournissent un modèle push-to-pull dans lequel la requête HTTP POSTÉRIEURe contient uniquement une référence aux informations d’événement qui seront ensuite récupérées indépendamment.</span><span class="sxs-lookup"><span data-stu-id="b25a1-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="b25a1-107">Le modèle de sécurité varie souvent légèrement.</span><span class="sxs-lookup"><span data-stu-id="b25a1-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="b25a1-108">L’objectif de Microsoft ASP.NET webhooks est de rendre plus simple et plus cohérent pour associer votre API sans perdre beaucoup de temps à savoir comment gérer une variante particulière des webhooks.</span><span class="sxs-lookup"><span data-stu-id="b25a1-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="b25a1-109">Un récepteur webhook est chargé d’accepter et de vérifier les webhooks d’un expéditeur particulier.</span><span class="sxs-lookup"><span data-stu-id="b25a1-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="b25a1-110">Un récepteur de webhook peut prendre en charge un nombre quelconque de webhooks, chacun avec sa propre configuration.</span><span class="sxs-lookup"><span data-stu-id="b25a1-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="b25a1-111">Par exemple, le récepteur webhook GitHub peut accepter des webhooks à partir d’un nombre quelconque de dépôts GitHub.</span><span class="sxs-lookup"><span data-stu-id="b25a1-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="b25a1-112">URI du récepteur de webhook</span><span class="sxs-lookup"><span data-stu-id="b25a1-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="b25a1-113">En installant Microsoft ASP.NET webhooks, vous recevez un contrôleur webhook général qui accepte les requêtes de webhook à partir d’un nombre de services en cours d’achèvement.</span><span class="sxs-lookup"><span data-stu-id="b25a1-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="b25a1-114">Lorsqu’une requête arrive, elle choisit le récepteur approprié que vous avez installé pour gérer un expéditeur de webhook particulier.</span><span class="sxs-lookup"><span data-stu-id="b25a1-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="b25a1-115">L’URI de ce contrôleur est l’URI de webhook que vous inscrivez auprès du service et se présente sous la forme :</span><span class="sxs-lookup"><span data-stu-id="b25a1-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="b25a1-116">Pour des raisons de sécurité, de nombreux récepteurs de webhook requièrent que l’URI soit un URI *https* et, dans certains cas, il doit également contenir un paramètre de requête supplémentaire qui est utilisé pour garantir que seul le tiers prévu peut envoyer des webhooks à l’URI ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b25a1-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="b25a1-117">Le composant `<receiver>` est le nom du récepteur, par exemple `github` ou `slack`.</span><span class="sxs-lookup"><span data-stu-id="b25a1-117">The `<receiver>` component is the name of the receiver, for example `github` or `slack`.</span></span>

<span data-ttu-id="b25a1-118">*{ID}* est un identificateur facultatif qui peut être utilisé pour identifier une configuration de récepteur de webhook particulière.</span><span class="sxs-lookup"><span data-stu-id="b25a1-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="b25a1-119">Cela peut être utilisé pour inscrire N webhooks avec un récepteur particulier.</span><span class="sxs-lookup"><span data-stu-id="b25a1-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="b25a1-120">Par exemple, les trois URI suivants peuvent être utilisés pour s’inscrire à trois webhooks indépendants :</span><span class="sxs-lookup"><span data-stu-id="b25a1-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="b25a1-121">Installation d’un récepteur webhook</span><span class="sxs-lookup"><span data-stu-id="b25a1-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="b25a1-122">Pour recevoir des webhooks à l’aide de Microsoft ASP.NET webhook, vous devez d’abord installer le package NuGet pour le fournisseur webhook ou les fournisseurs à partir desquels vous souhaitez recevoir des webhooks.</span><span class="sxs-lookup"><span data-stu-id="b25a1-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="b25a1-123">Les packages NuGet sont nommés [Microsoft. Aspnet. webhooks. récepteurs. \*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) où la dernière partie indique que le service est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="b25a1-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="b25a1-124">Exemple :</span><span class="sxs-lookup"><span data-stu-id="b25a1-124">For example</span></span>

<span data-ttu-id="b25a1-125">[Microsoft. Aspnet. webhooks. Decepteurs. GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fournit la prise en charge de la réception de webhooks à partir de GitHub et de [Microsoft. Aspnet. webhooks. les destinataires. Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fournit la prise en charge de la réception de webhooks générés par les webhooks ASP.net.</span><span class="sxs-lookup"><span data-stu-id="b25a1-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="b25a1-126">Prêt à l’emploi, vous pouvez trouver la prise en charge de Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, mou, Stripe, Trello et WordPress, mais il est possible de prendre en charge un nombre quelconque d’autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="b25a1-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="b25a1-127">Configuration d’un récepteur webhook</span><span class="sxs-lookup"><span data-stu-id="b25a1-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="b25a1-128">Les récepteurs de webhook sont configurés via le inteface [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) et des implémentations spécifiques de cette interface peuvent être inscrites à l’aide de n’importe quel modèle d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="b25a1-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="b25a1-129">L’implémentation par défaut utilise des paramètres d’application qui peuvent être définis dans le fichier Web. config ou, si vous utilisez Azure Web Apps, peuvent être définis via le [portail Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b25a1-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Paramètres de l’application dans Microsoft Azure](_static/AzureAppSettings.png)

<span data-ttu-id="b25a1-131">Le format des clés de paramètres d’application est le suivant :</span><span class="sxs-lookup"><span data-stu-id="b25a1-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="b25a1-132">La valeur est une liste de valeurs séparées par des virgules correspondant aux valeurs *{ID}* pour lesquelles des webhooks ont été enregistrés, par exemple :</span><span class="sxs-lookup"><span data-stu-id="b25a1-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="b25a1-133">Initialisation d’un récepteur webhook</span><span class="sxs-lookup"><span data-stu-id="b25a1-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="b25a1-134">Les récepteurs de webhooks sont initialisés en les inscrivant, généralement dans la classe statique *WebApiConfig* , par exemple :</span><span class="sxs-lookup"><span data-stu-id="b25a1-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
