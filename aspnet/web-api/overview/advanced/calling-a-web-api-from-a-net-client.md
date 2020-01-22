---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Appeler une API Web à partir d’un clientC#.net ()-ASP.net 4. x
author: MikeWasson
description: Ce didacticiel montre comment appeler une API Web à partir d’une application .NET 4. x.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 960960d26863cc3f725eee8a6c98844c5d3ce721
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519178"
---
# <a name="call-a-web-api-from-a-net-client-c"></a>Appeler une API Web à partir d’un clientC#.net ()

par [Mike Wasson](https://github.com/MikeWasson) et [Rick Anderson](https://twitter.com/RickAndMSFT)

[Télécharger le projet terminé](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Télécharger les instructions](/aspnet/core/tutorials/#how-to-download-a-sample). 

Ce didacticiel montre comment appeler une API Web à partir d’une application .NET, à l’aide de [System .net. http. httpclient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

Dans ce didacticiel, une application cliente est écrite et utilise l’API Web suivante :

| Action | Méthode HTTP | URI relatif |
| --- | --- | --- |
| Obtenir un produit par ID | GET | /api/products/*id* |
| Créer un produit | PUBLIER | /api/products |
| Mettre à jour un produit | PUT | /api/products/*id* |
| Supprimer un produit | SUPPR | /api/products/*id* |

Pour savoir comment implémenter cette API avec API Web ASP.NET, consultez [création d’une API Web qui prend en charge les opérations CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Pour plus de simplicité, l’application cliente dans ce didacticiel est une application console Windows. **Httpclient** est également pris en charge pour les applications Windows Phone et Windows Store. Pour plus d’informations, consultez [écriture de code client d’API Web pour plusieurs plateformes à l’aide de bibliothèques portables](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Création d'application console

Dans Visual Studio, créez une nouvelle application console Windows nommée **HttpClientSample** et collez le code suivant :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Le code précédent est l’application cliente complète.

`RunAsync` s’exécute et se bloque jusqu’à ce qu’il se termine. La plupart des méthodes **httpclient** sont asynchrones, car elles exécutent des e/s réseau. Toutes les tâches asynchrones sont effectuées dans `RunAsync`. Normalement, une application ne bloque pas le thread principal, mais cette application n’autorise aucune interaction.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Installer les bibliothèques clientes de l’API Web

Utilisez le gestionnaire de package NuGet pour installer le package de bibliothèques clientes de l’API Web.

Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**. Dans la console du gestionnaire de package (PMC), tapez la commande suivante :

`Install-Package Microsoft.AspNet.WebApi.Client`

La commande précédente ajoute les packages NuGet suivants au projet :

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Netwonsoft. JSON (également appelé Json.NET) est une infrastructure JSON très performante pour .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Ajouter une classe de modèle

Examiner la classe `Product` :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Cette classe correspond au modèle de données utilisé par l’API Web. Une application peut utiliser **httpclient** pour lire une instance de `Product` à partir d’une réponse http. L’application n’a pas besoin d’écrire de code de désérialisation.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Créer et initialiser HttpClient

Examinez la propriété **httpclient** statique :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**Httpclient** est conçu pour être instancié une fois et réutilisé tout au long de la vie d’une application. Les conditions suivantes peuvent entraîner des erreurs **SocketException** :

* Création d’une nouvelle instance **httpclient** par demande.
* Serveur soumis à une charge importante.

La création d’une nouvelle instance **httpclient** par demande peut épuiser les sockets disponibles.

Le code suivant initialise l’instance **httpclient** :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Le code précédent :

* Définit l’URI de base pour les requêtes HTTP. Remplacez le numéro de port par le port utilisé dans l’application serveur. L’application ne fonctionne pas, sauf si le port de l’application serveur est utilisé.
* Définit l’en-tête Accept sur « application/JSON ». La définition de cet en-tête indique au serveur d’envoyer des données au format JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Envoyer une demande d’extraction pour récupérer une ressource

Le code suivant envoie une requête d’extraction pour un produit :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

La méthode **GetAsync** envoie la requête http. Lorsque la méthode se termine, elle retourne un **HttpResponseMessage** qui contient la réponse http. Si le code d’État dans la réponse est un code de réussite, le corps de la réponse contient la représentation JSON d’un produit. Appelez **ReadAsAsync** pour désérialiser la charge utile JSON en une instance `Product`. La méthode **ReadAsAsync** est asynchrone, car le corps de la réponse peut être arbitrairement grand.

**Httpclient** ne lève pas d’exception lorsque la réponse http contient un code d’erreur. Au lieu de cela, la propriété **IsSuccessStatusCode** a la **valeur false** si l’État est un code d’erreur. Si vous préférez traiter les codes d’erreur HTTP comme des exceptions, appelez [HttpResponseMessage. EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) sur l’objet Response. `EnsureSuccessStatusCode` lève une exception si le code d’État se trouve en dehors de la plage 200&ndash;299. Notez que **httpclient** peut lever des exceptions pour d’autres raisons &mdash; par exemple, si la demande expire.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Formateurs de type média à désérialiser

Quand **ReadAsAsync** est appelé sans paramètre, il utilise un jeu de *formateurs de médias* par défaut pour lire le corps de la réponse. Les formateurs par défaut prennent en charge les données JSON, XML et de type URL de formulaire.

Au lieu d’utiliser les formateurs par défaut, vous pouvez fournir une liste de formateurs à la méthode **ReadAsAsync** .  L’utilisation d’une liste de formateurs est utile si vous avez un formateur de type de média personnalisé :

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Pour plus d’informations, consultez [formateurs de médias dans API Web ASP.NET 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Envoi d’une demande de publication pour créer une ressource

Le code suivant envoie une demande de publication contenant une `Product` instance au format JSON :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

Méthode **PostAsJsonAsync** :

* Sérialise un objet au format JSON.
* Envoie la charge utile JSON dans une demande de publication.

Si la demande est réussie :

* Elle doit retourner une réponse 201 (créée).
* La réponse doit inclure l’URL des ressources créées dans l’en-tête Location.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Envoi d’une demande PUT pour mettre à jour une ressource

Le code suivant envoie une demande PUT pour mettre à jour un produit :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

La méthode **PutAsJsonAsync** fonctionne comme **PostAsJsonAsync**, à ceci près qu’elle envoie une demande put au lieu de la publication.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Envoi d’une demande DELETE pour supprimer une ressource

Le code suivant envoie une demande DELETE pour supprimer un produit :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Comme par exemple, une demande DELETE n’a pas de corps de demande. Vous n’avez pas besoin de spécifier le format JSON ou XML avec DELETE.

## <a name="test-the-sample"></a>Tester l’exemple

Pour tester l’application cliente :

1. [Téléchargez](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) et exécutez l’application serveur. [Télécharger les instructions](/aspnet/core/#how-to-download-a-sample). Vérifiez que l’application serveur fonctionne. Par exemple, `http://localhost:64195/api/products` doit retourner une liste de produits.
2. Définissez l’URI de base pour les requêtes HTTP. Remplacez le numéro de port par le port utilisé dans l’application serveur.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Exécutez l’application cliente. La sortie suivante est produite :

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
