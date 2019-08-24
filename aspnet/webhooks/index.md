---
uid: webhooks/index
title: Vue d’ensemble des webhooks ASP.NET | Microsoft Docs
author: rick-anderson
description: Présentation des webhooks ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: aa65a20e1af16d58533e37fafc77ac246e0fe327
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000728"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="4f413-103">Vue d’ensemble des webhooks ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4f413-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="4f413-104">Webhook est un modèle HTTP léger qui fournit un modèle Pub/Sub simple pour le câblage des API Web et des services SaaS.</span><span class="sxs-lookup"><span data-stu-id="4f413-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="4f413-105">Lorsqu’un événement se produit dans un service, une notification est envoyée sous la forme d’une requête HTTP POSTALe aux abonnés inscrits.</span><span class="sxs-lookup"><span data-stu-id="4f413-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="4f413-106">La demande de publication contient des informations sur l’événement, ce qui permet au récepteur d’agir en conséquence.</span><span class="sxs-lookup"><span data-stu-id="4f413-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="4f413-107">En raison de leur simplicité, les webhooks sont déjà exposés par un grand nombre de services, notamment [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), les [marges](http://www.slack.com), les [rayures](http://www.stripe.com), les [Trello](http://www.trello.com/)et de nombreux libérer.</span><span class="sxs-lookup"><span data-stu-id="4f413-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="4f413-108">Par exemple, un webhook peut indiquer qu’un fichier a été modifié dans [Dropbox](http://dropbox.com/)ou qu’une modification du code a été validée dans GitHub, ou qu’un paiement a été initié dans [PayPal](http://www.paypal.com/)ou qu’une carte a été créée dans [Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="4f413-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="4f413-109">Les possibilités sont infinies!</span><span class="sxs-lookup"><span data-stu-id="4f413-109">The possibilities are endless!</span></span>

<span data-ttu-id="4f413-110">Microsoft ASP.NET webhooks facilite l’envoi et la réception de webhooks dans le cadre de votre application ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="4f413-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="4f413-111">Du côté de la réception, il fournit un modèle commun pour la réception et le traitement de webhooks à partir d’un nombre quelconque de fournisseurs de webhook.</span><span class="sxs-lookup"><span data-stu-id="4f413-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="4f413-112">Elle est prête à l’emploi avec la prise en charge de [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [mou](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) et [Zendesk](https://www.zendesk.com/) , mais il est facile d’ajouter la prise en charge pour plus.</span><span class="sxs-lookup"><span data-stu-id="4f413-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="4f413-113">Côté envoi, il prend en charge la gestion et le stockage des abonnements, ainsi que l’envoi de notifications d’événements à l’ensemble approprié d’abonnés.</span><span class="sxs-lookup"><span data-stu-id="4f413-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="4f413-114">Cela vous permet de définir votre propre ensemble d’événements auxquels les abonnés peuvent s’abonner et de les notifier lorsque des événements se produisent.</span><span class="sxs-lookup"><span data-stu-id="4f413-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="4f413-115">Les deux parties peuvent être utilisées ensemble ou indépendamment selon votre scénario.</span><span class="sxs-lookup"><span data-stu-id="4f413-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="4f413-116">Si vous devez uniquement recevoir des webhooks à partir d’autres services, vous pouvez utiliser uniquement la partie destinataire. Si vous souhaitez uniquement exposer des webhooks pour que d’autres utilisateurs puissent les utiliser, vous pouvez le faire uniquement.</span><span class="sxs-lookup"><span data-stu-id="4f413-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="4f413-117">Le code cible API Web ASP.NET 2 et ASP.NET MVC 5 et est disponible en tant que [OSS sur GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="4f413-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="4f413-118">Vue d’ensemble des webhooks</span><span class="sxs-lookup"><span data-stu-id="4f413-118">WebHooks Overview</span></span>

<span data-ttu-id="4f413-119">Webhook est un modèle qui signifie qu’il varie en fonction de son utilisation du service au service, mais l’idée de base est la même.</span><span class="sxs-lookup"><span data-stu-id="4f413-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="4f413-120">Vous pouvez considérer les webhooks comme un modèle Pub/Sub simple dans lequel un utilisateur peut s’abonner à des événements qui se produisent ailleurs.</span><span class="sxs-lookup"><span data-stu-id="4f413-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="4f413-121">Les notifications d’événements sont propagées en tant que requêtes HTTP POSTALes contenant des informations sur l’événement lui-même.</span><span class="sxs-lookup"><span data-stu-id="4f413-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="4f413-122">En général, la requête HTTP HTTP contient un objet JSON ou des données de formulaire HTML déterminés par l’expéditeur webhook, y compris des informations sur l’événement qui provoque le déclenchement du webhook.</span><span class="sxs-lookup"><span data-stu-id="4f413-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="4f413-123">Par exemple, un corps de requête POSTALe de webhook de [GitHub](http://www.github.com/) ressemble à ceci suite à l’ouverture d’un nouveau problème dans un référentiel particulier:</span><span class="sxs-lookup"><span data-stu-id="4f413-123">For example, a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="4f413-124">Pour vous assurer que le webhook provient bien de l’expéditeur prévu, la demande de publication est sécurisée d’une certaine façon, puis vérifiée par le destinataire.</span><span class="sxs-lookup"><span data-stu-id="4f413-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="4f413-125">Par exemple, le [Webhook GitHub](https://developer.github.com/webhooks/) comprend un en-tête http *X-Hub-signature* avec un hachage du corps de la demande qui est vérifié par l’implémentation du récepteur, de sorte que vous n’avez pas à vous en soucier.</span><span class="sxs-lookup"><span data-stu-id="4f413-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="4f413-126">Le flot de webhook ressemble généralement à ceci:</span><span class="sxs-lookup"><span data-stu-id="4f413-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="4f413-127">L’expéditeur de webhook expose les événements auxquels un client peut s’abonner.</span><span class="sxs-lookup"><span data-stu-id="4f413-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="4f413-128">Les événements décrivent les modifications observables du système, par exemple, qu’un nouvel élément de données a été inséré, qu’un processus est terminé ou autre chose.</span><span class="sxs-lookup"><span data-stu-id="4f413-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="4f413-129">Le récepteur de webhook s’abonne en inscrivant un webhook constitué de quatre éléments:</span><span class="sxs-lookup"><span data-stu-id="4f413-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="4f413-130">URI pour lequel la notification d’événement doit être publiée sous la forme d’une requête HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="4f413-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="4f413-131">Ensemble de filtres décrivant les événements particuliers pour lesquels le webhook doit être déclenché;</span><span class="sxs-lookup"><span data-stu-id="4f413-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="4f413-132">Clé secrète utilisée pour signer la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="4f413-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="4f413-133">Données supplémentaires à inclure dans la requête HTTP après.</span><span class="sxs-lookup"><span data-stu-id="4f413-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="4f413-134">Il peut s’agir, par exemple, de champs ou de propriétés d’en-tête HTTP supplémentaires inclus dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="4f413-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="4f413-135">Une fois qu’un événement se produit, les inscriptions du webhook correspondantes sont trouvées et les requêtes HTTP POSTALes sont envoyées.</span><span class="sxs-lookup"><span data-stu-id="4f413-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="4f413-136">En règle générale, la génération des requêtes HTTP POSTALes est retentée plusieurs fois si, pour une raison quelconque, le destinataire ne répond pas ou si la requête HTTP Après génère une réponse d’erreur.</span><span class="sxs-lookup"><span data-stu-id="4f413-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="4f413-137">Pipeline de traitement des webhooks</span><span class="sxs-lookup"><span data-stu-id="4f413-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="4f413-138">Le pipeline de traitement du webhook Microsoft ASP.NET pour les webhooks entrants se présente comme suit:</span><span class="sxs-lookup"><span data-stu-id="4f413-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Pipeline de traitement des webhooks ASP.NET](_static/WebHookReceivers.png)

<span data-ttu-id="4f413-140">Les deux concepts clés ici sont les *récepteurs* et les *gestionnaires*:</span><span class="sxs-lookup"><span data-stu-id="4f413-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="4f413-141">Les destinataires sont responsables de la gestion de la version particulière du webhook d’un expéditeur donné et de l’application de vérifications de sécurité pour s’assurer que la requête de webhook provient bien de l’expéditeur prévu.</span><span class="sxs-lookup"><span data-stu-id="4f413-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="4f413-142">Les *gestionnaires* sont généralement là où le code utilisateur exécute le traitement du webhook particulier.</span><span class="sxs-lookup"><span data-stu-id="4f413-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="4f413-143">Dans les nœuds suivants, ces concepts sont décrits plus en détail.</span><span class="sxs-lookup"><span data-stu-id="4f413-143">In the following nodes these concepts are described in more details.</span></span>
