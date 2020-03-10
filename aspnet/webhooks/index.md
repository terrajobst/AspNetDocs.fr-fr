---
uid: webhooks/index
title: Vue d’ensemble des webhooks ASP.NET | Microsoft Docs
author: rick-anderson
description: Présentation des webhooks ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 1e21c92e950893c0ff87c63f03f4710a158441fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637290"
---
# <a name="aspnet-webhooks-overview"></a>Vue d’ensemble des webhooks ASP.NET

Webhook est un modèle HTTP léger qui fournit un modèle Pub/Sub simple pour le câblage des API Web et des services SaaS. Lorsqu’un événement se produit dans un service, une notification est envoyée sous la forme d’une requête HTTP POSTALe aux abonnés inscrits. La demande de publication contient des informations sur l’événement, ce qui permet au récepteur d’agir en conséquence.

En raison de leur simplicité, les webhooks sont déjà exposés par un grand nombre de services, notamment [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), les [marges](http://www.slack.com), les [rayures](http://www.stripe.com), les [Trello](http://www.trello.com/)et bien d’autres encore. Par exemple, un webhook peut indiquer qu’un fichier a été modifié dans [Dropbox](http://dropbox.com/)ou qu’une modification du code a été validée dans GitHub, ou qu’un paiement a été initié dans [PayPal](http://www.paypal.com/)ou qu’une carte a été créée dans [Trello](http://www.trello.com/). Les possibilités sont infinies !

Microsoft ASP.NET webhooks facilite l’envoi et la réception de webhooks dans le cadre de votre application ASP.NET :

* Du côté de la réception, il fournit un modèle commun pour la réception et le traitement de webhooks à partir d’un nombre quelconque de fournisseurs de webhook. Il est prêt à l’emploi avec la prise en charge de [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [mou](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) et [Zendesk](https://www.zendesk.com/) , mais il est facile d’ajouter la prise en charge pour plus.

* Côté envoi, il prend en charge la gestion et le stockage des abonnements, ainsi que l’envoi de notifications d’événements à l’ensemble approprié d’abonnés. Cela vous permet de définir votre propre ensemble d’événements auxquels les abonnés peuvent s’abonner et de les notifier lorsque des événements se produisent.

Les deux parties peuvent être utilisées ensemble ou indépendamment selon votre scénario. Si vous devez uniquement recevoir des webhooks à partir d’autres services, vous pouvez utiliser uniquement la partie destinataire. Si vous souhaitez uniquement exposer des webhooks pour que d’autres utilisateurs puissent les utiliser, vous pouvez le faire uniquement.

Le code cible API Web ASP.NET 2 et ASP.NET MVC 5 et est disponible en tant que [OSS sur GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Vue d’ensemble des webhooks

Webhook est un modèle qui signifie qu’il varie en fonction de son utilisation du service au service, mais l’idée de base est la même. Vous pouvez considérer les webhooks comme un modèle Pub/Sub simple dans lequel un utilisateur peut s’abonner à des événements qui se produisent ailleurs. Les notifications d’événements sont propagées en tant que requêtes HTTP POSTALes contenant des informations sur l’événement lui-même.

En général, la requête HTTP HTTP contient un objet JSON ou des données de formulaire HTML déterminés par l’expéditeur webhook, y compris des informations sur l’événement qui provoque le déclenchement du webhook. Par exemple, un corps de requête POSTALe de webhook de [GitHub](https://www.github.com/) ressemble à ceci suite à l’ouverture d’un nouveau problème dans un référentiel particulier :

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

Pour vous assurer que le webhook provient bien de l’expéditeur prévu, la demande de publication est sécurisée d’une certaine façon, puis vérifiée par le destinataire. Par exemple, le [Webhook GitHub](https://developer.github.com/webhooks/) comprend un en-tête http *X-Hub-signature* avec un hachage du corps de la demande qui est vérifié par l’implémentation du récepteur, de sorte que vous n’avez pas à vous en soucier.

Le flot de webhook ressemble généralement à ceci :

* L’expéditeur de webhook expose les événements auxquels un client peut s’abonner. Les événements décrivent les modifications observables du système, par exemple, qu’un nouvel élément de données a été inséré, qu’un processus est terminé ou autre chose.

* Le récepteur de webhook s’abonne en inscrivant un webhook constitué de quatre éléments :

     1. URI pour lequel la notification d’événement doit être publiée sous la forme d’une requête HTTP POST.

     2. Ensemble de filtres décrivant les événements particuliers pour lesquels le webhook doit être déclenché ;

     3. Clé secrète utilisée pour signer la requête HTTP.

     4. Données supplémentaires à inclure dans la requête HTTP après. Il peut s’agir, par exemple, de champs ou de propriétés d’en-tête HTTP supplémentaires inclus dans le corps de la requête HTTP.

* Une fois qu’un événement se produit, les inscriptions du webhook correspondantes sont trouvées et les requêtes HTTP POSTALes sont envoyées. En règle générale, la génération des requêtes HTTP POSTALes est retentée plusieurs fois si, pour une raison quelconque, le destinataire ne répond pas ou si la requête HTTP Après génère une réponse d’erreur.

## <a name="webhooks-processing-pipeline"></a>Pipeline de traitement des webhooks

Le pipeline de traitement du webhook Microsoft ASP.NET pour les webhooks entrants se présente comme suit :

![Pipeline de traitement des webhooks ASP.NET](_static/WebHookReceivers.png)

Les deux concepts clés ici sont les *récepteurs* et les *gestionnaires*:

* Les *destinataires* sont responsables de la gestion de la version particulière du webhook d’un expéditeur donné et de l’application de vérifications de sécurité pour s’assurer que la requête de webhook provient bien de l’expéditeur prévu.

* Les *gestionnaires* sont généralement là où le code utilisateur exécute le traitement du webhook particulier.

Dans les nœuds suivants, ces concepts sont décrits plus en détail.
