---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Envoi de données de formulaire HTML dans API Web ASP.NET : form-urlencoded Data-ASP.NET 4. x'
author: MikeWasson
description: Cet article explique comment poster des données form-urlencoded sur un contrôleur d’API Web avec ASP.NET 4. x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557602"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Envoi de données de formulaire HTML dans API Web ASP.NET : données form-urlencoded

par [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Partie 1 : données form-urlencoded

Cet article explique comment poster des données form-urlencoded vers un contrôleur d’API Web.

- [Vue d’ensemble des formulaires HTML](#overview_of_html_forms)
- [Envoi de types complexes](#sending_complex_types)
- [Envoi de données de formulaire via AJAX](#sending_form_data_via_ajax)
- [Envoi de types simples](#sending_simple_types)

> [!NOTE]
> [Téléchargez le projet terminé](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Vue d’ensemble des formulaires HTML

Les formulaires HTML utilisent l’extraction ou la publication pour envoyer des données au serveur. L’attribut **Method** de l’élément **Form** fournit la méthode http :

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

La méthode par défaut est obtient. Si le formulaire utilise la fonction obtenir, les données de formulaire sont encodées dans l’URI en tant que chaîne de requête. Si le formulaire utilise la publication, les données du formulaire sont placées dans le corps de la requête. Pour les données publiées, l’attribut **enctype** spécifie le format du corps de la demande :

| enctype | Description |
| --- | --- |
| application/x-www-form-urlencoded | Les données de formulaire sont encodées sous forme de paires nom/valeur, comme dans le cas d’une chaîne de requête URI. Il s’agit du format par défaut pour la publication. |
| multipart/formulaire-données | Les données de formulaire sont encodées sous la forme d’un message MIME à parties multiples. Utilisez ce format si vous téléchargez un fichier sur le serveur. |

La première partie de cet article examine le format x-www-form-urlencoded. La [partie 2](sending-html-form-data-part-2.md) décrit le MIME à parties multiples.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Envoi de types complexes

En général, vous envoyez un type complexe, composé de valeurs extraites de plusieurs contrôles de formulaire. Prenons le modèle suivant qui représente une mise à jour d’État :

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Voici un contrôleur d’API Web qui accepte un objet `Update` par le biais de la publication.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Ce contrôleur utilise le [routage basé sur les actions](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), de sorte que le modèle de routage est &quot;API/{Controller}/{action}/{id}&quot;. Le client va poster les données vers &quot;&quot;/API/updates/Complex.

Nous allons maintenant écrire un formulaire HTML pour que les utilisateurs envoient une mise à jour d’État.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Notez que l’attribut **action** du formulaire est l’URI de notre action de contrôleur. Voici le formulaire avec des valeurs entrées dans :

![](sending-html-form-data-part-1/_static/image1.png)

Quand l’utilisateur clique sur envoyer, le navigateur envoie une requête HTTP semblable à la suivante :

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Notez que le corps de la demande contient les données de formulaire, mises en forme en tant que paires nom/valeur. L’API Web convertit automatiquement les paires nom/valeur en une instance de la classe `Update`.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Envoi de données de formulaire via AJAX

Lorsqu’un utilisateur envoie un formulaire, le navigateur quitte la page actuelle et restitue le corps du message de réponse. C’est OK lorsque la réponse est une page HTML. Toutefois, avec une API Web, le corps de la réponse est généralement vide ou contient des données structurées, telles que JSON. Dans ce cas, il est plus logique d’envoyer les données de formulaire à l’aide d’une requête AJAX, afin que la page puisse traiter la réponse.

Le code suivant montre comment poster des données de formulaire à l’aide de jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

La fonction jQuery **Submit** remplace l’action de formulaire par une nouvelle fonction. Cela remplace le comportement par défaut du bouton Envoyer. La fonction **Serialize** sérialise les données de formulaire dans des paires nom/valeur. Pour envoyer les données de formulaire au serveur, appelez `$.post()`.

Une fois la demande terminée, le gestionnaire de `.success()` ou de `.error()` affiche un message approprié à l’utilisateur.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Envoi de types simples

Dans les sections précédentes, nous avons envoyé un type complexe, que l’API Web désérialisée en une instance d’une classe de modèle. Vous pouvez également envoyer des types simples, tels qu’une chaîne.

> [!NOTE]
> Avant d’envoyer un type simple, encapsulez la valeur dans un type complexe à la place. Cela vous donne les avantages de la validation de modèle côté serveur, et facilite l’extension de votre modèle si nécessaire.

Les étapes de base pour envoyer un type simple sont identiques, mais il existe deux légères différences. Tout d’abord, dans le contrôleur, vous devez décorer le nom du paramètre avec l’attribut **FromBody** .

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Par défaut, l’API Web tente d’obtenir des types simples à partir de l’URI de la demande. L’attribut **FromBody** indique à l’API Web de lire la valeur du corps de la demande.

> [!NOTE]
> L’API Web lit le corps de la réponse au plus une fois, de sorte qu’un seul paramètre d’une action peut provenir du corps de la demande. Si vous devez obtenir plusieurs valeurs à partir du corps de la demande, définissez un type complexe.

Deuxièmement, le client doit envoyer la valeur au format suivant :

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

Plus précisément, la partie nom de la paire nom/valeur doit être vide pour un type simple. Tous les navigateurs ne prennent pas en charge cette fonction pour les formulaires HTML, mais vous créez ce format dans le script comme suit :

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Voici un exemple de formulaire :

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

Et voici le script pour envoyer la valeur du formulaire. La seule différence par rapport au script précédent est l’argument passé dans la fonction de **publication** .

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Vous pouvez utiliser la même approche pour envoyer un tableau de types simples :

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Ressources supplémentaires

[Partie 2 : Téléchargement de fichiers et MIME à parties multiples](sending-html-form-data-part-2.md)
