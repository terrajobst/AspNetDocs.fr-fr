---
uid: web-api/samples-list
title: Liste d’exemples d’API Web-ASP.NET 4. x
author: rick-anderson
description: Liste d’exemples de API Web ASP.NET pour ASP.NET 4. x
ms.author: riande
ms.date: 09/18/2012
ms.custom: seoapril2019
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 24368fc80e7fc3c840f1f1f9accf13fa15a06c1b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598461"
---
# <a name="web-api-samples-list"></a>Liste d’exemples d’API web

## <a name="httpclient-samples"></a>Exemples HttpClient

**Exemple Bing Translate** | [source vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/BingTranslateSample)

Montre comment appeler le [service Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) à l’aide de la classe **httpclient** . L’API du service Microsoft Translator requiert un jeton OAuth, que l’application obtient en envoyant une demande au serveur de jeton Azure pour chaque demande au service de traduction. Le résultat du serveur de jetons est introduit dans la demande envoyée au service de traduction. Avant d’exécuter cet exemple, vous devez obtenir une [clé d’application à partir de](https://msdn.microsoft.com/library/hh454950.aspx) la place de marché Azure et renseigner les informations dans l’exemple de classe AccessTokenMessageHandler.

**Exemple Google Maps** | [Description détaillée](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [source vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/GoogleMapsSample)

Utilise **httpclient** pour télécharger une carte de Redmond, WA à partir de l' [API Google Maps](https://developers.google.com/maps/), l’enregistre dans un fichier local et ouvre l’afficheur d’images par défaut.

**Exemple de client Twitter** | [Description détaillée](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [vs 2012 source](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/TwitterSample)

Montre comment écrire un client Twitter simple à l’aide de **httpclient**. L’exemple utilise un **HttpMessageHandler** pour insérer les informations d’authentification OAuth dans le **HttpRequestMessage**sortant. Le résultat de Twitter est lu à l’aide de JSON.NET. Avant d’exécuter cet exemple, vous devez obtenir une [clé d’application à partir de Twitter](https://dev.twitter.com/)et renseigner les informations dans l’exemple de classe OAuthMessageHandler.

**Exemple de Banque mondiale** | [description détaillée](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) [2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net40) | la source | source vs [2012 source](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net45)

Montre comment récupérer des données à partir du site de données de la Banque mondiale, à l’aide de JSON.NET pour analyser le résultat.

## <a name="web-api-samples"></a>Exemples d’API Web

**Prise en main avec API Web ASP.net** [source | vs 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Montre comment créer une API Web de base qui prend en charge les requêtes HTTP d’obtention. Contient le code source du didacticiel de [votre première API Web ASP.net](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**Scénarios JavaScript API Web ASP.net : commentaires** | [source vs 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Montre comment utiliser API Web ASP.NET pour créer des API Web qui prennent en charge des clients de navigateur et peuvent être facilement appelées à l’aide de jQuery.

**Gestionnaire de contacts** | [source vs 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Cet exemple utilise API Web ASP.NET pour créer une application de gestionnaire de contacts simple. L’application se compose d’une API Web de gestionnaire de contacts qui est utilisée par une application ASP.NET MVC et une application Windows Phone pour afficher et gérer une liste de contacts.

**Exemple de traitement par lot** | une [Description détaillée](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [source vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/BatchSample)

Montre comment implémenter le traitement par lots HTTP dans ASP.NET. Le traitement par lot consiste à placer plusieurs requêtes HTTP au sein d’un seul corps d’entité MIME à parties multiples, qui est ensuite envoyé au serveur en tant que HTTP. Les demandes sont traitées individuellement, et les réponses sont placées dans un autre corps d’entité MIME à parties multiples, qui est retourné au client.

**Exemple de contrôleur de contenu** | [description détaillée](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) des | [vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net40) source | [vs 2012 source](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net45)

Montre comment lire et écrire des entités de demande et de réponse de façon asynchrone à l’aide de flux. L’exemple de contrôleur comporte deux actions : une action PUT qui lit le corps d’entité de la requête de façon asynchrone et le stocke dans un fichier local, ainsi qu’une action d’extraction qui retourne le contenu du fichier local.

Exemple de programme de **résolution d’assembly personnalisé** | [source vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomAssemblyResolverSample)

Montre comment modifier des API Web ASP.NET pour prendre en charge la découverte de contrôleurs à partir d’un assembly de bibliothèque chargé dynamiquement. L’exemple implémente un **IAssembliesResolver** personnalisé qui appelle l’implémentation par défaut, puis ajoute l’assembly de bibliothèque aux résultats par défaut.

**Exemple de formateur de type de média personnalisé** | [Description détaillée](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [source vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomMediaTypeFormatterSample)

Montre comment créer un formateur de type de média personnalisé à l’aide de la classe de base **BufferedMediaTypeFormatter** . Cette classe de base est destinée aux formateurs qui utilisent principalement des opérations de lecture et d’écriture synchrones. En plus d’afficher le formateur de type de média, l’exemple montre comment l’associer en l’inscrivant dans le cadre du **HttpConfiguration** pour votre application. Notez qu’il est également possible d’utiliser directement la classe de base **MediaTypeFormatter** , pour les formateurs qui utilisent principalement des opérations de lecture et d’écriture asynchrones.

**Exemple de liaison de paramètre personnalisé** | [Description détaillée](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [source vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomParameterBinding)

Montre comment personnaliser le processus de liaison de paramètre, qui est le processus qui détermine la façon dont les informations d’une requête sont liées aux paramètres d’action. Dans cet exemple, le contrôleur d’hébergement a quatre actions :

1. BindPrincipal montre comment lier un paramètre IPrincipal à partir d’un principal générique personnalisé, et non à partir d’un message HTTP.
2. BindCustomComplexTypeFromUriOrBody montre comment lier un paramètre de type complexe, qui peut provenir du corps du message ou de l’URI de la demande d’un message HTTP HTTP.
3. BindCustomComplexTypeFromUriWithRenamedProperty montre comment lier un paramètre de type complexe à une propriété renommée qui provient de l’URI de demande d’un message HTTP.
4. PostMultipleParametersFromBody montre comment lier plusieurs paramètres du corps pour un message de publication ;

**Exemple de chargement de fichier** | [Description détaillée](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [source vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/FileUploadSample)

Montre comment charger des fichiers sur un **ApiController** à l’aide du téléchargement de fichiers multiparts MIME et comment configurer des notifications de progression avec **httpclient** à l’aide de **ProgressNotificationHandler**. Le contrôleur lit le contenu d’un fichier HTML téléchargé de manière asynchrone et écrit une ou plusieurs parties du corps dans un fichier local. La réponse contient des informations sur le ou les fichiers téléchargés.

**Exemple de chargement de fichier dans le magasin d’objets BLOB Azure** | [Description détaillée](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [source vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/AzureBlobsFileUploadSample)

Cet exemple est similaire à l’exemple de chargement de fichier, mais au lieu d’enregistrer les fichiers téléchargés sur le disque local, il télécharge de façon asynchrone les fichiers dans le [magasin d’objets BLOB Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) à l’aide du [Kit de développement logiciel (SDK) Windows Azure pour .net](https://www.windowsazure.com/develop/net/). Il fournit également un mécanisme permettant de répertorier les objets BLOB actuellement présents dans un [conteneur de stockage d’objets BLOB Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Vous pouvez essayer l’exemple en cours d’exécution sur l' **émulateur de stockage Azure** fourni avec le kit de développement logiciel (SDK) Azure. Si vous avez un [compte de stockage Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), vous pouvez également exécuter sur le service de stockage réel.

**Exemple de pipeline de gestionnaire de messages Http** | [Description détaillée](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [source vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/HttpMessageHandlerPipelineSample)

Montre comment associer des instances **HttpMessageHandler** à la fois sur le client (**httpclient**) et sur le serveur (API Web ASP.net). Dans l’exemple, le même gestionnaire est utilisé à la fois sur le client et sur le serveur. Bien qu’il soit rare que le même gestionnaire s’exécute aux deux emplacements, le modèle objet est le même côté client et côté serveur.

**Exemple de chargement JSON** | [source vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/JsonUploadSample)

Montre comment charger et télécharger JSON vers et à partir d’un **ApiController**. L’exemple utilise un **ApiController** minimal et y accède à l’aide de **httpclient**.

**Exemple** d’application web hybride | [Description détaillée](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [source vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MashupSample)

Montre comment accéder de façon asynchrone à plusieurs sites distants à partir d’une action **ApiController** . Chaque fois que l’action est atteinte, les demandes sont exécutées de façon asynchrone, afin qu’aucun thread ne soit bloqué.

**Exemple de suivi** de la mémoire | [Description détaillée](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [source vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MemoryTracingSample)

Cet exemple de projet crée un package NuGet qui installe un writer de trace en mémoire personnalisé dans des applications API Web ASP.NET.

**Exemple MongoDB** | [Description détaillée](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [vs 2012 source](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MongoSample)

Montre comment utiliser MongoDB comme magasin persistant pour un **ApiController**, à l’aide d’un modèle de référentiel.

**Exemple de processeur de corps de réponse** | [source vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ResponseEntityProcessorSample)

Montre comment copier une entité de réponse (autrement dit, un corps de réponse HTTP) dans un fichier local avant qu’elle ne soit transmise au client et effectuer un traitement supplémentaire sur ce fichier de façon asynchrone. L’exemple implémente un **HttpMessageHandler** qui encapsule l’entité de réponse à l’aide d’une qui s’écrit dans la sortie en tant que normale et dans un fichier local.

**Exemple de chargement de XDocument** | [Description détaillée](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [source vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/UploadXDocumentSample)

Montre comment charger un XDocument sur un **ApiController** à l’aide de **PushStreamContent** et **httpclient**.

**Exemple de Validation** | [source vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ValidationSample)

Montre comment vous pouvez utiliser des attributs de validation sur vos modèles dans ASP.NET WebAPI pour valider le contenu de la requête HTTP. Montre comment marquer des propriétés en fonction des besoins, comment utiliser des attributs de validation personnalisés et définis par l’infrastructure pour annoter votre modèle, et comment retourner des réponses d’erreur pour les États de modèle non valides.

**Exemple de formulaire Web** | [Description détaillée](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [source vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/WebFormSample)

Affiche un ApiController ajouté à un projet Web Forms.

**[Exemple RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs est une application de suivi des bogues simple qui montre comment utiliser API Web ASP.NET et la nouvelle bibliothèque cliente HTTP pour créer un système piloté par hypermédia. L’exemple comprend des implémentations de client et de serveur, à l’aide de API Web ASP.NET. Le serveur utilise un formateur Razor personnalisé pour générer des représentations de ressources. L’exemple fournit également un serveur node. js pour illustrer les avantages résultant de l’utilisation d’une conception hypermédia pour découpler les clients et les serveurs.
