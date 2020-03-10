---
uid: signalr/overview/older-versions/troubleshooting
title: Résolution des problèmes signaler (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Cet article décrit les problèmes courants liés au développement d’applications Signalr.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 3dbf091ac2daa4da477b405852bb4d1316584fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579603"
---
# <a name="signalr-troubleshooting-signalr-1x"></a>Résolution des problèmes de SignalR (SignalR 1.x)

de [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce document décrit les problèmes courants de dépannage avec Signalr.

Ce document contient les sections suivantes.

- [L’appel de méthodes entre le client et le serveur échoue silencieusement](#connection)
- [Autres problèmes de connexion](#other)
- [Erreurs de compilation et côté serveur](#server)
- [Problèmes liés à Visual Studio](#vs)
- [Problèmes de Internet Information Services](#iis)
- [Problèmes liés à Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>L’appel de méthodes entre le client et le serveur échoue silencieusement

Cette section décrit les causes possibles de l’échec d’un appel de méthode entre le client et le serveur sans un message d’erreur explicite. Dans une application Signalr, le serveur n’a pas d’informations sur les méthodes implémentées par le client. Lorsque le serveur appelle une méthode cliente, le nom de la méthode et les données de paramètre sont envoyés au client, et la méthode est exécutée uniquement si elle existe dans le format spécifié par le serveur. Si aucune méthode correspondante n’est trouvée sur le client, rien ne se passe et aucun message d’erreur n’est déclenché sur le serveur.

Pour examiner plus en détail les méthodes clientes qui ne sont pas appelées, vous pouvez activer la journalisation avant d’appeler la méthode Start sur le concentrateur pour voir les appels provenant du serveur. Pour activer la journalisation dans une application JavaScript, consultez [Comment activer la journalisation côté client (version du client JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Pour activer la journalisation dans une application cliente .NET, consultez [Comment activer la journalisation côté client (version du client .net)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Méthode mal orthographiée, signature de méthode incorrecte ou nom de concentrateur incorrect

Si le nom ou la signature d’une méthode appelée ne correspond pas exactement à une méthode appropriée sur le client, l’appel échoue. Vérifiez que le nom de la méthode appelée par le serveur correspond au nom de la méthode sur le client. En outre, Signalr crée le proxy Hub à l’aide de méthodes de casse mixte, comme le fait la fonction de JavaScript, donc une méthode appelée `SendMessage` sur le serveur est appelée `sendMessage` dans le proxy client. Si vous utilisez l’attribut `HubName` dans votre code côté serveur, vérifiez que le nom utilisé correspond au nom utilisé pour créer le concentrateur sur le client. Si vous n’utilisez pas l’attribut `HubName`, vérifiez que le nom du concentrateur dans un client JavaScript est en casse mixte, par exemple chatHub au lieu de ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nom de méthode dupliqué sur le client

Vérifiez que vous n’avez pas de méthode dupliquée sur le client qui diffère uniquement par la casse. Si votre application cliente a une méthode appelée `sendMessage`, vérifiez que la méthode n’est pas également appelée `SendMessage`.

### <a name="missing-json-parser-on-the-client"></a>Analyseur JSON manquant sur le client

Signalr requiert qu’un analyseur JSON soit présent pour sérialiser les appels entre le serveur et le client. Si votre client ne dispose pas d’un analyseur JSON intégré (tel qu’Internet Explorer 7), vous devez en inclure un dans votre application. Vous pouvez télécharger l’analyseur JSON [ici](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Combinaison du Hub et de la syntaxe PersistentConnection

Signalr utilise deux modèles de communication : hubs et PersistentConnections. La syntaxe d’appel de ces deux modèles de communication est différente dans le code client. Si vous avez ajouté un Hub dans le code de votre serveur, vérifiez que l’ensemble de votre code client utilise la syntaxe de concentrateur appropriée.

**Code client JavaScript qui crée un PersistentConnection dans un client JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Code client JavaScript qui crée un proxy Hub dans un client JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C#code serveur qui mappe un itinéraire à un PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**C#code serveur qui mappe un itinéraire à un concentrateur ou à plusieurs concentrateurs si vous avez plusieurs applications**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Connexion démarrée avant l’ajout d’abonnements

Si la connexion du concentrateur est démarrée avant que les méthodes qui peuvent être appelées à partir du serveur soient ajoutées au proxy, les messages ne sont pas reçus. Le code JavaScript suivant ne démarre pas correctement le Hub :

**Code incorrect du client JavaScript qui n’autorise pas la réception des messages hubs**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Au lieu de cela, ajoutez les abonnements de méthode avant d’appeler Start :

**Code client JavaScript qui ajoute correctement des abonnements à un Hub**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Nom de méthode manquant sur le proxy Hub

Vérifiez que la méthode définie sur le serveur est abonnée sur le client. Même si le serveur définit la méthode, il doit toujours être ajouté au proxy client. Les méthodes peuvent être ajoutées au proxy client comme suit (Notez que la méthode est ajoutée au `client` membre du concentrateur, et non pas directement au hub) :

**Code client JavaScript qui ajoute des méthodes à un proxy Hub**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Les méthodes de concentrateur ou de concentrateur ne sont pas déclarées comme public

Pour être visible sur le client, les méthodes et l’implémentation du concentrateur doivent être déclarées comme `public`.

### <a name="accessing-hub-from-a-different-application"></a>Accès à Hub à partir d’une autre application

Les concentrateurs signalr sont accessibles uniquement par le biais d’applications qui implémentent des clients Signalr. Signalr ne peut pas interagir avec d’autres bibliothèques de communication (comme les services Web SOAP ou WCF). Si aucun client Signalr n’est disponible pour votre plateforme cible, vous ne pouvez pas accéder directement au point de terminaison du serveur.

### <a name="manually-serializing-data"></a>Sérialisation manuelle des données

Signalr utilisera automatiquement JSON pour sérialiser les paramètres de votre méthode. vous n’avez pas besoin de le faire vous-même.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Méthode de concentrateur distant non exécutée sur le client dans la fonction OnDisconnected

Ce comportement est inhérent à la conception. Lorsque `OnDisconnected` est appelé, le concentrateur a déjà entré l’État `Disconnected`, qui n’autorise pas l’appel d’autres méthodes de concentrateur.

**C#code serveur qui exécute correctement le code dans l’événement OnDisconnected**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Limite de connexion atteinte

Lors de l’utilisation de la version complète d’IIS sur un système d’exploitation client tel que Windows 7, une limite de 10 connexions est imposée. Si vous utilisez un système d’exploitation client, utilisez IIS Express à la place pour éviter cette limite.

### <a name="cross-domain-connection-not-set-up-properly"></a>La connexion inter-domaines n’est pas configurée correctement

Si une connexion inter-domaines (une connexion pour laquelle l’URL Signalr n’est pas dans le même domaine que la page d’hébergement) n’est pas configurée correctement, la connexion peut échouer sans message d’erreur. Pour plus d’informations sur l’activation de la communication entre domaines, consultez [Comment établir une connexion inter-domaines](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>La connexion utilisant NTLM (Active Directory) ne fonctionne pas dans le client .NET

Une connexion dans une application cliente .NET qui utilise la sécurité du domaine peut échouer si la connexion n’est pas correctement configurée. Pour utiliser Signalr dans un environnement de domaine, définissez la propriété de connexion requise comme suit :

**C#code client qui implémente les informations d’identification de connexion**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Autres problèmes de connexion

Cette section décrit les causes et les solutions pour des symptômes ou des messages d’erreur spécifiques qui se produisent pendant une connexion.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Erreur « le début doit être appelé avant que les données ne puissent être envoyées »

Cette erreur se produit généralement si le code fait référence à des objets Signalr avant le démarrage de la connexion. Le wireup pour les gestionnaires et le similaire qui appellera des méthodes définies sur le serveur doit être ajouté une fois la connexion terminée. Notez que l’appel à `Start` est asynchrone, donc le code qui suit l’appel peut être exécuté avant de se terminer. La meilleure façon d’ajouter des gestionnaires après qu’une connexion a démarré complètement consiste à les placer dans une fonction de rappel transmise en tant que paramètre à la méthode Start :

**Code client JavaScript qui ajoute correctement des gestionnaires d’événements qui font référence à des objets Signalr**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Cette erreur se produit également si une connexion s’arrête alors que les objets Signalr sont toujours référencés.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>erreur « 301 déplacé définitivement » ou « 302 déplacé temporairement »

Cette erreur peut se produire si le projet contient un dossier appelé Signalr, qui interfère avec le proxy créé automatiquement. Pour éviter cette erreur, n’utilisez pas de dossier nommé `SignalR` dans votre application, ou désactivez la génération de proxy automatique. Pour plus d’informations [, consultez le proxy généré et ce qu’il fait pour vous](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) .

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>erreur « 403 interdit » dans .NET ou le client Silverlight

Cette erreur peut se produire dans les environnements inter-domaines où la communication entre domaines n’est pas correctement activée. Pour plus d’informations sur l’activation de la communication entre domaines, consultez [Comment établir une connexion inter-domaines](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Pour établir une connexion inter-domaines dans un client Silverlight, consultez [connexions inter-domaines à partir de clients Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>erreur « 404 introuvable »

Ce problème peut être dû à plusieurs causes. Vérifiez les éléments suivants :

- La **référence d’adresse du proxy Hub n’est pas correctement mise en forme :** Cette erreur se produit généralement si la référence à l’adresse proxy Hub générée n’est pas mise en forme correctement. Vérifiez que la référence à l’adresse de concentrateur est correctement effectuée. Pour plus d’informations, consultez [Guide pratique pour référencer le proxy généré dynamiquement](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) .
- **Ajout d’itinéraires à l’application avant l’ajout de l’itinéraire du Hub :** Si votre application utilise d’autres itinéraires, vérifiez que le premier itinéraire ajouté est l’appel à `MapHubs`.

### <a name="500-internal-server-error"></a>« Erreur interne du serveur 500 »

Il s’agit d’une erreur très générique qui peut avoir un grand nombre de causes. Les détails de l’erreur doivent apparaître dans le journal des événements du serveur ou être trouvés par le biais du débogage du serveur. Vous pouvez obtenir des informations plus détaillées sur l’erreur en activant les erreurs détaillées sur le serveur. Pour plus d’informations, consultez [Comment gérer les erreurs dans la classe de concentrateur](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>Erreur « TypeError : &lt;hubType&gt; n’est pas défini »

Cette erreur se produit si l’appel à `MapHubs` n’est pas effectué correctement. Pour plus d’informations [, consultez Comment inscrire l’itinéraire signalr et configurer les options signalr](../guide-to-the-api/hubs-api-guide-server.md#route) .

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException n’était pas géré par le code utilisateur

Vérifiez que les paramètres que vous envoyez à vos méthodes n’incluent pas les types non sérialisables (tels que les handles de fichiers ou les connexions de base de données). Si vous devez utiliser des membres sur un objet côté serveur que vous ne souhaitez pas envoyer au client (pour des raisons de sécurité ou pour des raisons de sérialisation), utilisez l’attribut `JSONIgnore`.

### <a name="protocol-error-unknown-transport-error"></a>Erreur « erreur de protocole : transport inconnu »

Cette erreur peut se produire si le client ne prend pas en charge les transports utilisés par Signalr. Pour plus d’informations sur les navigateurs qui peuvent être utilisés avec Signalr [, consultez transports et secours](../getting-started/introduction-to-signalr.md#transports) .

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>« La génération du proxy du Hub JavaScript a été désactivée ».

Cette erreur se produit si `DisableJavaScriptProxies` est définie alors qu’elle inclut également une référence au proxy généré dynamiquement sur `signalr/hubs`. Pour plus d’informations sur la création manuelle du proxy, consultez [le proxy généré et ce qu’il fait pour vous](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>Erreur « l’ID de connexion est dans un format incorrect » ou « l’identité de l’utilisateur ne peut pas changer au cours d’une connexion active Signalr ».

Cette erreur peut se produire si l’authentification est utilisée et si le client est déconnecté avant que la connexion ne soit arrêtée. La solution consiste à arrêter la connexion Signalr avant de déconnecter le client.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>«Erreur non interceptée : Signalr : jQuery introuvable. Vérifiez que jQuery est référencé avant le fichier Signalr. js».

Le client JavaScript Signalr requiert jQuery pour s’exécuter. Vérifiez que votre référence à jQuery est correcte, que le chemin d’accès utilisé est valide et que la référence à jQuery est antérieure à la référence à Signalr.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>Erreur « Impossible de lire la propriété «&lt;de la propriété&gt;» d’une erreur non définie

Cette erreur est due au fait que jQuery ou le proxy hubs est référencé correctement. Vérifiez que votre référence à jQuery et au proxy hubs est correcte, que le chemin d’accès utilisé est valide et que la référence à jQuery est antérieure à la référence au proxy hubs. La référence par défaut au proxy hubs doit ressembler à ce qui suit :

**Code HTML côté client qui fait correctement référence au proxy hubs**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Erreur « RuntimeBinderException n’est pas gérée par le code utilisateur »

Cette erreur peut se produire lorsque la surcharge incorrecte de `Hub.On` est utilisée. Si la méthode a une valeur de retour, le type de retour doit être spécifié en tant que paramètre de type générique :

**Méthode définie sur le client (sans proxy généré)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>L’ID de connexion est incohérent ou des interruptions de connexion entre les chargements de page

Ce comportement est inhérent à la conception. Étant donné que l’objet Hub est hébergé dans l’objet page, le concentrateur est détruit lors de l’actualisation de la page. Une application à plusieurs pages doit gérer l’association entre les utilisateurs et les ID de connexion afin qu’ils soient cohérents entre les chargements de pages. Les ID de connexion peuvent être stockés sur le serveur dans un objet `ConcurrentDictionary` ou dans une base de données.

### <a name="value-cannot-be-null-error"></a>Erreur « la valeur ne peut pas être null »

Les méthodes côté serveur avec des paramètres facultatifs ne sont pas prises en charge actuellement ; Si le paramètre facultatif est omis, la méthode échoue. Pour plus d’informations, consultez [paramètres facultatifs](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>« Firefox ne peut pas établir de connexion au serveur à &lt;adresse&gt;» dans Firebug

Ce message d’erreur peut être consulté dans le Firebug si la négociation du transport WebSocket échoue et qu’un autre transport est utilisé à la place. Ce comportement est inhérent à la conception.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Erreur « le certificat distant n’est pas valide selon la procédure de validation » dans l’application cliente .NET

Si votre serveur requiert des certificats clients personnalisés, vous pouvez ajouter un X509Certificate à la connexion avant que la demande ne soit effectuée. Ajoutez le certificat à la connexion à l’aide de `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>La connexion est abandonnée après l’expiration du délai d’authentification

Ce comportement est inhérent à la conception. Les informations d’identification d’authentification ne peuvent pas être modifiées tant qu’une connexion est active ; pour actualiser les informations d’identification, la connexion doit être arrêtée et redémarrée.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected est appelé deux fois lors de l’utilisation de jQuery mobile

la fonction de `initializePage` de jQuery mobile force la réexécution des scripts dans chaque page, créant ainsi une deuxième connexion. Les solutions à ce problème sont les suivantes :

- Incluez la référence à jQuery mobile avant votre fichier JavaScript.
- Désactivez la fonction `initializePage` en définissant `$.mobile.autoInitializePage = false`.
- Attendez la fin de l’initialisation de la page avant de commencer la connexion.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Les messages sont retardés dans les applications Silverlight à l’aide des événements envoyés par le serveur

Les messages sont retardés lors de l’utilisation des événements envoyés par le serveur sur Silverlight. Pour forcer l’utilisation à la place de l’interrogation longue, utilisez les éléments suivants lors du démarrage de la connexion :

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>« Autorisation refusée » à l’aide du protocole de trame perpétuelle

Il s’agit d’un problème connu, décrit [ici](https://github.com/SignalR/SignalR/issues/1963). Ce symptôme peut être observé à l’aide de la dernière bibliothèque JQuery. la solution de contournement consiste à rétrograder votre application à JQuery 1.8.2.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Erreurs de compilation et côté serveur

 La section suivante contient des solutions possibles pour les erreurs d’exécution côté serveur et du compilateur. 

### <a name="reference-to-hub-instance-is-null"></a>La référence à l’instance de concentrateur est null

Étant donné qu’une instance de concentrateur est créée pour chaque connexion, vous ne pouvez pas créer vous-même une instance d’un Hub dans votre code. Pour appeler des méthodes sur un concentrateur en dehors du concentrateur lui-même, consultez [How to Call client Methods and Manage groups from the Hub Class](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) pour obtenir une référence au contexte Hub.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext. Current. session a la valeur null

Ce comportement est inhérent à la conception. Signalr ne prend pas en charge l’état de session ASP.NET, car l’activation de l’état de session interrompt la messagerie duplex.

### <a name="no-suitable-method-to-override"></a>Aucune méthode appropriée à substituer

Vous pouvez voir cette erreur si vous utilisez le code de la documentation ou des blogs plus anciens. Vérifiez que vous ne faites pas référence aux noms des méthodes qui ont été modifiées ou déconseillées (comme `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions. WebSocketServerUrl a la valeur null

Ce comportement est inhérent à la conception. Ce membre est déconseillé et ne doit pas être utilisé.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>« Un itinéraire nommé’signalr. hubs’figure déjà dans la collection d’itinéraires ».

Cette erreur se produit si `MapHubs` est appelé deux fois par votre application. Certains exemples d’applications appellent `MapHubs` directement dans le fichier d’application global ; d’autres effectuent l’appel dans une classe wrapper. Assurez-vous que votre application n’effectue pas les deux.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problèmes liés à Visual Studio

Cette section décrit les problèmes rencontrés dans Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Le nœud documents de script n’apparaît pas dans Explorateur de solutions

Certains de nos didacticiels vous dirigent vers le nœud « documents de script » dans Explorateur de solutions lors du débogage. Ce nœud est généré par le débogueur JavaScript et s’affiche uniquement pendant le débogage des clients de navigateur dans Internet Explorer ; le nœud n’apparaît pas si chrome ou Firefox sont utilisés. Le débogueur JavaScript ne s’exécutera pas non plus si un autre débogueur client est en cours d’exécution, tel que le débogueur Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>Signalr ne fonctionne pas sur Visual Studio 2008 ou version antérieure

Ce comportement est inhérent à la conception. Signalr requiert .NET Framework 4 ou version ultérieure ; Cela nécessite que les applications Signalr soient développées dans Visual Studio 2010 ou une version ultérieure.

<a id="iis"></a>

## <a name="iis-issues"></a>Problèmes liés à IIS

Cette section contient des problèmes avec Internet Information Services.

### <a name="web-site-crashes-after-maphubs-call"></a>Blocage du site Web après l’appel de MapHubs

Ce problème a été résolu dans la dernière version de Signalr. Vérifiez que vous utilisez la dernière version publiée de Signalr en mettant à jour votre installation à l’aide de NuGet.

<a id="azure"></a>

## <a name="azure-issues"></a>Problèmes liés à Azure

Cette section contient des problèmes avec Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>Les messages ne sont pas reçus via le fond de panier Azure après modification des noms de rubriques

Les rubriques utilisées par le fond de panier Azure ne sont pas destinées à être configurées par l’utilisateur.
