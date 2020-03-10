---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Configuration de API Web ASP.NET 2-ASP.NET 4. x
author: MikeWasson
description: 'Configurez API Web ASP.NET 2 pour ASP.NET 4. x : configurer les paramètres, ASP.NET 4. x hébergement, auto-hébergement OWIN, services globaux et configuration de précontrôleur.'
ms.author: riande
ms.date: 03/31/2014
ms.custom: seoapril2019
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4f76728fa5e4602e35e1b7cb2d41b2245093cad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557721"
---
# <a name="configuring-aspnet-web-api-2"></a>Configuration de API Web ASP.NET 2

par [Mike Wasson](https://github.com/MikeWasson)

Cette rubrique explique comment configurer API Web ASP.NET.

- [Paramètres de configuration](#settings)
- [Configuration de l’API Web avec l’hébergement ASP.NET](#webhost)
- [Configuration de l’API Web avec l’auto-hébergement OWIN](#selfhost)
- [Services API Web globaux](#services)
- [Configuration par contrôleur](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Paramètres de configuration

Les paramètres de configuration de l’API Web sont définis dans la classe [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) .

| Membre | Description |
| --- | --- |
| **DependencyResolver** | Active l’injection de dépendances pour les contrôleurs. Consultez [utilisation du programme de résolution des dépendances de l’API Web](dependency-injection.md). |
| **Filtres** | Filtres d'action. |
| **Formateurs** | [Formateurs de type de média](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Spécifie si le serveur doit inclure les détails de l’erreur, tels que les messages d’exception et les traces de la pile, dans les messages de réponse HTTP. Consultez [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Initialiseur** | Fonction qui effectue l’initialisation finale de **HttpConfiguration**. |
| **MessageHandlers** | [Gestionnaires de messages http](http-message-handlers.md). |
| **ParameterBindingRules** | Collection de règles pour la liaison de paramètres sur des actions de contrôleur. |
| **Propriétés** | Conteneur de propriétés générique. |
| **Itinéraires** | Collection d’itinéraires. Consultez [routage dans API Web ASP.net](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Services** | Collection de services. Consultez [services](#services). |

## <a name="prerequisites"></a>Conditions préalables requises

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) édition Communauté, Professionnel ou Entreprise.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Configuration de l’API Web avec l’hébergement ASP.NET

Dans une application ASP.NET, configurez l’API Web en appelant [GlobalConfiguration. configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) dans l' **application\_méthode Start** . La méthode **configure** prend un délégué avec un seul paramètre de type **HttpConfiguration**. Procédez à l’ensemble de votre configuration à l’intérieur du délégué.

Voici un exemple d’utilisation d’un délégué anonyme :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

Dans Visual Studio 2017, le modèle de projet « application Web ASP.NET » configure automatiquement le code de configuration, si vous sélectionnez « API Web » dans la boîte de dialogue **nouveau projet ASP.net** .

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Le modèle de projet crée un fichier nommé WebApiConfig.cs dans le dossier de démarrage de l’application\_. Ce fichier de code définit le délégué dans lequel vous devez placer le code de configuration de votre API Web.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Le modèle de projet ajoute également le code qui appelle le délégué à partir de l' **Application\_démarrer**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Configuration de l’API Web avec l’auto-hébergement OWIN

Si vous êtes auto-hébergé avec OWIN, créez une nouvelle instance **HttpConfiguration** . Effectuez une configuration sur cette instance, puis transmettez l’instance à la méthode d’extension **Owin. UseWebApi** .

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Le didacticiel [utiliser OWIN pour l’auto-hébergement API Web ASP.NET 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) montre les étapes complètes.

<a id="services"></a>
## <a name="global-web-api-services"></a>Services API Web globaux

La collection **HttpConfiguration. services** contient un ensemble de services globaux que l’API Web utilise pour effectuer diverses tâches, telles que la sélection de contrôleur et la négociation de contenu.

> [!NOTE]
> La collection de **services** n’est pas un mécanisme à usage général pour la découverte de service ou l’injection de dépendances. Il stocke uniquement les types de service connus de l’infrastructure de l’API Web.

La collection de **services** est initialisée avec un ensemble de services par défaut, et vous pouvez fournir vos propres implémentations personnalisées. Certains services prennent en charge plusieurs instances, tandis que d’autres ne peuvent avoir qu’une seule instance. (Toutefois, vous pouvez également fournir des services au niveau du contrôleur ; consultez [configuration par contrôleur](#percontrollerconfig).

Services à instance unique

| Service | Description |
| --- | --- |
| **IActionValueBinder** | Obtient une liaison pour un paramètre. |
| **IApiExplorer** | Obtient les descriptions des API exposées par l’application. Consultez [création d’une page d’aide pour une API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Obtient une liste des assemblys pour l’application. Consultez [routage et sélection des actions](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Valide un modèle qui est lu à partir du corps de la demande par un formateur de type média. |
| **IContentNegotiator** | Exécute la négociation de contenu. |
| **IDocumentationProvider** | Fournit la documentation pour les API. La valeur par défaut est **null**. Consultez [création d’une page d’aide pour une API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Indique si l’hôte doit mettre en mémoire tampon des corps d’entité de message HTTP. |
| **IHttpActionInvoker** | Appelle une action de contrôleur. Consultez [routage et sélection des actions](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Sélectionne une action de contrôleur. Consultez [routage et sélection des actions](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Active un contrôleur. Consultez [routage et sélection des actions](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Sélectionne un contrôleur. Consultez [routage et sélection des actions](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Fournit une liste des types de contrôleurs d’API Web dans l’application. Consultez [routage et sélection des actions](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Initialise l’infrastructure de traçage. Consultez [traçage dans API Web ASP.net](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Fournit un enregistreur de trace. La valeur par défaut est un enregistreur de suivi « no-op ». Consultez [traçage dans API Web ASP.net](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Fournit un cache de validateurs de modèle. |

Services à instances multiples

|                 Service                 |                                                                                                              Description                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Retourne une liste de filtres pour une action de contrôleur.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Retourne un classeur de modèles pour un type donné.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Fournit des métadonnées pour un modèle.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Fournit un validateur pour un modèle.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Crée un fournisseur de valeurs. Pour plus d’informations, consultez le billet de blog de Mike Stall [comment créer un fournisseur de valeurs personnalisées dans WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Pour ajouter une implémentation personnalisée à un service multi-instance, appelez **Add** ou **Insert** dans la collection **services** :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Pour remplacer un service à instance unique par une implémentation personnalisée, appelez la fonction **Replace** sur la collection **services** :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Configuration par contrôleur

Vous pouvez remplacer les paramètres suivants pour chaque contrôleur :

- Formateurs de type de média
- Règles de liaison de paramètre
- Services

Pour ce faire, définissez un attribut personnalisé qui implémente l’interface **IControllerConfiguration** . Appliquez ensuite l’attribut au contrôleur.

L’exemple suivant remplace les formateurs de type de média par défaut par un formateur personnalisé.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

La méthode **IControllerConfiguration. Initialize** accepte deux paramètres :

- Objet **HttpControllerSettings**
- Objet **HttpControllerDescriptor**

Le **HttpControllerDescriptor** contient une description du contrôleur, que vous pouvez examiner à titre d’information (par exemple, pour faire la distinction entre deux contrôleurs).

Utilisez l’objet **HttpControllerSettings** pour configurer le contrôleur. Cet objet contient le sous-ensemble de paramètres de configuration que vous pouvez remplacer pour chaque contrôleur. Tous les paramètres que vous ne modifiez pas par défaut pour l’objet global **HttpConfiguration** .
