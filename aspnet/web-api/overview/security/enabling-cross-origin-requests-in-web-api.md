---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Activation des demandes Cross-Origin dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Montre comment prendre en charge le partage des ressources Cross-Origin (CORS) dans API Web ASP.NET.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d3016d98fa6c3a55359c6dab0737407b29925f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555705"
---
# <a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a>Activer les demandes Cross-Origin dans API Web ASP.NET 2

par [Mike Wasson](https://github.com/MikeWasson)

> La sécurité des navigateurs empêche une page web d’adresser des demandes AJAX à un autre domaine. Cette restriction est appelée *stratégie de même origine* et empêche un site malveillant de lire des données sensibles à partir d’un autre site. Toutefois, vous souhaitez parfois laisser d’autres sites appeler votre API web.
>
> Le [partage des ressources Cross-Origin](http://www.w3.org/TR/cors/) (cors) est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine. À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes cross-origin lors du refus d’autres. CORS est plus sûr et plus flexible que les techniques antérieures telles que [JSONP](http://en.wikipedia.org/wiki/JSONP). Ce didacticiel montre comment activer CORS dans votre application API Web.
>
> ## <a name="software-used-in-the-tutorial"></a>Logiciel utilisé dans le didacticiel
>
> - [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2,2

## <a name="introduction"></a>Introduction

Ce didacticiel illustre la prise en charge de CORS dans API Web ASP.NET. Nous allons commencer par créer deux projets ASP.NET : l’un nommé « WebClient », qui héberge un contrôleur d’API Web et l’autre appelé « WebClient », qui appelle WebService. Étant donné que les deux applications sont hébergées dans des domaines différents, une requête AJAX de WebClient à WebService est une demande Cross-Origin.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Que veut dire « même origine » ?

Deux URL ont la même origine si elles ont des schémas, des hôtes et des ports identiques. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Ces deux URL ont le même origine :

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Ces URL ont des origines différentes aux deux précédentes :

- `http://example.net`-domaine différent
- `http://example.com:9000/foo.html` : port différent
- `https://example.com/foo.html` : schéma différent
- `http://www.example.com/foo.html`-sous-domaine différent

> [!NOTE]
> Internet Explorer ne prend pas en compte le port lors de la comparaison des origines.

## <a name="create-the-webservice-project"></a>Créer le projet WebService

> [!NOTE]
> Cette section suppose que vous savez déjà comment créer des projets d’API Web. Si ce n’est pas le cas, consultez [prise en main avec API Web ASP.net](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

1. Démarrez Visual Studio et créez un projet d' **application Web ASP.net (.NET Framework)** .
2. Dans la boîte de dialogue **nouvelle application Web ASP.net** , sélectionnez le modèle de projet **vide** . Sous **Ajouter des dossiers et des références principales pour**, activez la case à cocher **API Web** .

   ![Boîte de dialogue Nouveau projet ASP.NET dans Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. Ajoutez un contrôleur d’API Web nommé `TestController` avec le code suivant :

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. Vous pouvez exécuter l’application localement ou la déployer sur Azure. (Pour les captures d’écran de ce didacticiel, l’application se déploie sur Azure App Service Web Apps.) Pour vérifier que l’API Web fonctionne, accédez à `http://hostname/api/test/`, où *hostname* est le domaine dans lequel vous avez déployé l’application. Vous devez voir le texte de la réponse, &quot;obtenir : message de test&quot;.

   ![Navigateur Web indiquant le message de test](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a>Créer le projet WebClient

1. Créez un autre projet d' **application Web ASP.net (.NET Framework)** et sélectionnez le modèle de projet **MVC** . Si vous le souhaitez, sélectionnez **modifier l’authentification** > **aucune authentification**. Vous n’avez pas besoin d’authentification pour ce didacticiel.

   ![Modèle MVC dans la boîte de dialogue Nouveau projet ASP.NET dans Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. Dans **Explorateur de solutions**, ouvrez le fichier *views/orig/index. cshtml*. Remplacez le code de ce fichier par ce qui suit :

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   Pour la variable *ServiceUrl* , utilisez l’URI de l’application WebService.

3. Exécutez l’application WebClient localement ou publiez-la sur un autre site Web.

Quand vous cliquez sur le bouton « essayer », une requête AJAX est soumise à l’application service Web à l’aide de la méthode HTTP indiquée dans la zone déroulante (obtenir, poster ou PUT). Cela vous permet d’examiner différentes demandes Cross-Origin. Actuellement, l’application WebService ne prend pas en charge CORS. par conséquent, si vous cliquez sur le bouton, vous obtenez une erreur.

![Erreur « essayer » dans le navigateur](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Si vous regardez le trafic HTTP dans un outil tel que [Fiddler](https://www.telerik.com/fiddler), vous verrez que le navigateur envoie la requête d’extraction et que la demande est réussie, mais l’appel Ajax retourne une erreur. Il est important de comprendre que la stratégie de même origine n’empêche pas le navigateur d' *Envoyer* la demande. Au lieu de cela, il empêche l’application de voir la *réponse*.

![Débogueur Web Fiddler avec les requêtes Web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a>Activez CORS

Activons à présent CORS dans l’application WebService. Tout d’abord, ajoutez le package NuGet CORS. Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Dans la fenêtre de la console du gestionnaire de package, tapez la commande suivante :

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Cette commande installe le package le plus récent et met à jour toutes les dépendances, y compris les bibliothèques principales de l’API Web. Utilisez l’indicateur `-Version` pour cibler une version spécifique. Le package CORS requiert l’API Web 2,0 ou une version ultérieure.

Ouvrez le fichier d' *application\_Start/WebApiConfig. cs*. Ajoutez le code suivant à la méthode **WebApiConfig. Register** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Ensuite, ajoutez l’attribut **[EnableCors]** à la classe `TestController` :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Pour le paramètre *Origins* , utilisez l’URI où vous avez déployé l’application WebClient. Cela permet d’effectuer des requêtes Cross-Origin à partir de WebClient, tout en désautorisant toutes les autres requêtes inter-domaines. Plus tard, je décrirai les paramètres pour **[EnableCors]** plus en détail.

N’incluez pas de barre oblique à la fin de l’URL des *origines* .

Redéployez l’application WebService mise à jour. Vous n’avez pas besoin de mettre à jour WebClient. La requête AJAX de WebClient doit maintenant s’effectuer correctement. Les méthodes d’extraction, de placement et de publication sont toutes autorisées.

![Navigateur Web indiquant que le message de test a réussi](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a>Fonctionnement de CORS

Cette section décrit ce qui se produit dans une demande CORS, au niveau des messages HTTP. Il est important de comprendre le fonctionnement de CORS, afin que vous puissiez configurer l’attribut **[EnableCors]** correctement et résoudre les problèmes si les choses ne fonctionnent pas comme prévu.

La spécification CORS introduit plusieurs nouveaux en-têtes HTTP qui permettent les demandes cross-origin. Si un navigateur prend en charge CORS, il définit automatiquement ces en-têtes pour les demandes Cross-Origin. vous n’avez rien à faire de spécial dans votre code JavaScript.

Voici un exemple de demande Cross-Origin. L’en-tête « Origin » indique le domaine du site qui effectue la demande.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Si le serveur autorise la demande, il définit l’en-tête Access-Control-allow-Origin. La valeur de cet en-tête correspond à l’en-tête d’origine ou à la valeur de caractère générique «\*», ce qui signifie que toute origine est autorisée.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Si la réponse n’inclut pas l’en-tête Access-Control-allow-Origin, la requête AJAX échoue. Plus précisément, le navigateur n’autorise pas la demande. Même si le serveur retourne une réponse correcte, le navigateur ne met pas la réponse à la disposition de l’application cliente.

**Demandes préliminaires**

Pour certaines demandes CORS, le navigateur envoie une demande supplémentaire, appelée « demande préliminaire », avant d’envoyer la demande réelle pour la ressource.

Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :

- La méthode de demande est obtenir, tête ou publication, *et*
- L’application ne définit pas d’en-têtes de requête autres que accepter, Accept-Language, Content-Language, Content-type ou Last-Event-ID, *et*
- L’en-tête `Content-Type` (si défini) est une des opérations suivantes :

    - application/x-www-form-urlencoded
    - multipart/formulaire-données
    - texte/brut

La règle relative aux en-têtes de demande s’applique aux en-têtes définis par l’application en appelant **setRequestHeader** sur l’objet **XMLHttpRequest** . (La spécification CORS appelle ces « en-têtes de demande d’auteur ».) La règle ne s’applique pas aux en-têtes que le *navigateur* peut définir, tels que user-agent, Host ou Content-Length.

Voici un exemple de demande préliminaire :

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

La requête de pré-vol utilise la méthode HTTP OPTIONS. Il comprend deux en-têtes spéciaux :

- Access-Control-Request-Method : méthode HTTP qui sera utilisée pour la demande réelle.
- Access-Control-request-headers : liste des en-têtes de requête définis par l' *application* sur la demande réelle. (Là encore, cela n’inclut pas les en-têtes définis par le navigateur.)

Voici un exemple de réponse, en supposant que le serveur autorise la requête :

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

La réponse inclut un en-tête `Access-Control-Allow-Methods` qui répertorie les méthodes autorisées et éventuellement un en-tête `Access-Control-Allow-Headers`, qui répertorie les en-têtes autorisés. Si la demande préliminaire réussit, le navigateur envoie la demande réelle, comme décrit précédemment.

Les outils couramment utilisés pour tester des points de terminaison avec des demandes d’OPTIONS préliminaires (par exemple, [Fiddler](https://www.telerik.com/fiddler) et [poster](https://www.getpostman.com/)) n’envoient pas les en-têtes d’options requis par défaut. Vérifiez que les en-têtes `Access-Control-Request-Method` et `Access-Control-Request-Headers` sont envoyés avec la demande et que les en-têtes d’OPTIONS atteignent l’application via IIS.

Pour configurer IIS afin de permettre à une application ASP.NET de recevoir et de gérer les demandes d’OPTION, ajoutez la configuration suivante au fichier *Web. config* de l’application dans la section `<system.webServer><handlers>` :

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

La suppression de `OPTIONSVerbHandler` empêche IIS de gérer les demandes d’OPTIONS. Le remplacement de `ExtensionlessUrlHandler-Integrated-4.0` permet aux demandes d’OPTIONS d’atteindre l’application, car l’inscription du module par défaut autorise uniquement les demandes d’extraction, de début, de publication et de débogage avec des URL sans extension.

## <a name="scope-rules-for-enablecors"></a>Règles d’étendue pour [EnableCors]

Vous pouvez activer CORS par action, par contrôleur ou globalement pour tous les contrôleurs d’API Web dans votre application.

**Par action**

Pour activer CORS pour une seule action, définissez l’attribut **[EnableCors]** sur la méthode d’action. L’exemple suivant active CORS pour la méthode `GetItem` uniquement.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Par contrôleur**

Si vous définissez **[EnableCors]** sur la classe Controller, elle s’applique à toutes les actions sur le contrôleur. Pour désactiver CORS pour une action, ajoutez l’attribut **[DisableCors]** à l’action. L’exemple suivant active CORS pour chaque méthode, à l’exception de `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Globalement**

Pour activer CORS pour tous les contrôleurs d’API Web dans votre application, transmettez une instance **EnableCorsAttribute** à la méthode **EnableCors** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Si vous définissez l’attribut sur plusieurs étendues, l’ordre de priorité est le suivant :

1. Action
2. Contrôleur
3. Global

## <a name="set-the-allowed-origins"></a>Définir les origines autorisées

Le paramètre *Origins* de l’attribut **[EnableCors]** spécifie les origines autorisées à accéder à la ressource. La valeur est une liste séparée par des virgules des origines autorisées.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Vous pouvez également utiliser la valeur de caractère générique «\*» pour autoriser les demandes émanant d’origines.

Réfléchissez bien avant d’autoriser des demandes à partir de n’importe quelle origine. Cela signifie que tout site Web peut effectuer des appels AJAX à votre API Web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a>Définir les méthodes HTTP autorisées

Le paramètre *Methods* de l’attribut **[EnableCors]** spécifie les méthodes http autorisées à accéder à la ressource. Pour autoriser toutes les méthodes, utilisez la valeur de caractère générique «\*». L’exemple suivant autorise uniquement les demandes d’extraction et de publication.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a>Définir les en-têtes de demande autorisés

Cet article a décrit plus haut comment une demande préliminaire peut inclure un en-tête Access-Control-request-headers, qui répertorie les en-têtes HTTP définis par l’application (appelé « en-têtes de demande d’auteur »). Le paramètre *en-têtes* de l’attribut **[EnableCors]** spécifie les en-têtes de demande d’auteur autorisés. Pour autoriser tous les en-têtes, définissez les *en-têtes* sur «\*». Pour obtenir les en-têtes spécifiques à la liste verte, définissez les *en-têtes* sur une liste séparée par des virgules des en-têtes autorisés :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Toutefois, les navigateurs ne sont pas entièrement cohérents dans la façon dont ils définissent les en-têtes Access-Control-Request-Header. Par exemple, chrome contient actuellement « Origin ». FireFox n’inclut pas les en-têtes standard tels que « accepter », même lorsque l’application les définit dans un script.

Si vous définissez *les en-têtes* sur une valeur autre que «\*», vous devez inclure au moins « Accept », « Content-type » et « Origin », ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.

## <a name="set-the-allowed-response-headers"></a>Définir les en-têtes de réponse autorisés

Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application. Les en-têtes de réponse qui sont disponibles par défaut sont :

- Cache-Control
- Content-Language
- Content-Type
- Expires
- Dernière modification
- Bali

La spécification CORS appelle ces [en-têtes de réponse simples](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Pour mettre d’autres en-têtes à la disposition de l’application, définissez le paramètre *exposedHeaders* de **[EnableCors]** .

Dans l’exemple suivant, la méthode `Get` du contrôleur définit un en-tête personnalisé nommé « X-Custom-Header ». Par défaut, le navigateur n’expose pas cet en-tête dans une demande Cross-Origin. Pour rendre l’en-tête disponible, incluez « X-Custom-header » dans *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a>Transmettre des informations d’identification dans les demandes Cross-Origin

Les informations d’identification nécessitent un traitement particulier dans une demande CORS. Par défaut, le navigateur n’envoie pas d’informations d’identification avec une demande Cross-Origin. Les informations d’identification incluent les cookies, ainsi que des schémas d’authentification HTTP. Pour envoyer des informations d’identification avec une demande Cross-Origin, le client doit définir **XMLHttpRequest. withCredentials** sur true.

Utilisation directe de **XMLHttpRequest** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

Dans jQuery :

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

En outre, le serveur doit autoriser les informations d’identification. Pour autoriser les informations d’identification Cross-Origin dans l’API Web, définissez la propriété **SupportsCredentials** sur true dans l’attribut **[EnableCors]** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Si cette propriété a la valeur true, la réponse HTTP inclut un en-tête Access-Control-allow-Credential. Cet en-tête indique au navigateur que le serveur autorise les informations d’identification pour une demande Cross-Origin.

Si le navigateur envoie des informations d’identification, mais que la réponse n’inclut pas d’en-tête Access-Control-allow-Credential valide, le navigateur n’expose pas la réponse à l’application et la requête AJAX échoue.

Veillez à affecter à **SupportsCredentials** la valeur true, car cela signifie qu’un site Web situé dans un autre domaine peut envoyer les informations d’identification d’un utilisateur connecté à votre API Web pour le compte de l’utilisateur, sans que l’utilisateur ne soit conscient. La spécification CORS indique également que la définition d' *origines* pour &quot;\*&quot; n’est pas valide si **SupportsCredentials** a la valeur true.

## <a name="custom-cors-policy-providers"></a>Fournisseurs de stratégie CORS personnalisés

L’attribut **[EnableCors]** implémente l’interface **ICorsPolicyProvider** . Vous pouvez fournir votre propre implémentation en créant une classe qui dérive de **attribute** et implémente **ICorsPolicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Vous pouvez maintenant appliquer l’attribut à tous les emplacements que vous placez dans **[EnableCors]** .

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Par exemple, un fournisseur de stratégie CORS personnalisé peut lire les paramètres à partir d’un fichier de configuration.

En guise d’alternative à l’utilisation d’attributs, vous pouvez inscrire un objet **ICorsPolicyProviderFactory** qui crée des objets **ICorsPolicyProvider** .

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Pour définir le **ICorsPolicyProviderFactory**, appelez la méthode d’extension **SetCorsPolicyProviderFactory** au démarrage, comme suit :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a>Prise en charge des navigateurs

Le package CORS de l’API Web est une technologie côté serveur. Le navigateur de l’utilisateur doit également prendre en charge CORS. Heureusement, les versions actuelles de tous les principaux navigateurs incluent la [prise en charge de cors](http://caniuse.com/cors).
