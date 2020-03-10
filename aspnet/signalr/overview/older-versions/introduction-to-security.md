---
uid: signalr/overview/older-versions/introduction-to-security
title: Présentation de la sécurité de Signalr (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Décrit les problèmes de sécurité que vous devez prendre en compte lors du développement d’une application Signalr.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 34172c0a2a15a7ab0d782704d5831ce236f5c989
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536728"
---
# <a name="introduction-to-signalr-security-signalr-1x"></a>Introduction à la sécurité de SignalR (SignalR 1.x)

de [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cet article décrit les problèmes de sécurité que vous devez prendre en compte lors du développement d’une application Signalr.

## <a name="overview"></a>Présentation

Ce document contient les sections suivantes :

- [Concepts de sécurité de signalr](#concepts)

    - [Authentification et autorisation](#authentication)
    - [Jeton de connexion](#connectiontoken)
    - [Rejoindre des groupes lors de la reconnexion](#rejoingroup)
- [Comment signaler empêche la falsification de requête intersites](#csrf)
- [Recommandations de sécurité signalr](#recommendations)

    - [Protocole SSL (Secure Socket Layers)](#ssl)
    - [N’utilisez pas de groupes comme mécanisme de sécurité](#groupsecurity)
    - [Gestion en toute sécurité des entrées des clients](#input)
    - [Rapprochement d’une modification de l’état de l’utilisateur avec une connexion active](#reconcile)
    - [Fichiers de proxy JavaScript générés automatiquement](#autogen)
    - [Exceptions](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Concepts de sécurité de signalr

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Authentification et autorisation

Signalr est conçu pour être intégré à la structure d’authentification existante pour une application. Il ne fournit aucune fonctionnalité permettant d’authentifier les utilisateurs. Au lieu de cela, vous authentifiez les utilisateurs comme vous le feriez normalement dans votre application, puis vous travaillez avec les résultats de l’authentification dans votre code Signalr. Par exemple, vous pouvez authentifier vos utilisateurs avec l’authentification ASP.NET Forms, puis, dans votre Hub, appliquer les utilisateurs ou les rôles autorisés à appeler une méthode. Dans votre Hub, vous pouvez également transmettre des informations d’authentification, telles que le nom d’utilisateur ou si un utilisateur appartient à un rôle, au client.

Signalr fournit l’attribut [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) pour spécifier les utilisateurs ayant accès à un concentrateur ou à une méthode. Vous appliquez l’attribut Authorize à un concentrateur ou à des méthodes particulières dans un concentrateur. Sans l’attribut Authorize, toutes les méthodes publiques sur le concentrateur sont disponibles pour un client qui est connecté au concentrateur. Pour plus d’informations sur les concentrateurs, consultez [authentification et autorisation pour les concentrateurs signalr](../security/hub-authorization.md).

L’attribut `Authorize` est utilisé uniquement avec les hubs. Pour appliquer des règles d’autorisation lors de l’utilisation d’un `PersistentConnection` vous devez remplacer la méthode `AuthorizeRequest`. Pour plus d’informations sur les connexions persistantes, consultez [authentification et autorisation pour les connexions persistantes signalr](../security/persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Jeton de connexion

Signalr limite le risque d’exécution de commandes malveillantes en validant l’identité de l’expéditeur. Un jeton de connexion, contenant l’ID de connexion et le nom d’utilisateur pour les utilisateurs authentifiés, est transmis entre le client et le serveur pour chaque demande. L’ID de connexion est un identificateur unique qui est généré de manière aléatoire par le serveur lors de la création d’une nouvelle connexion, et il est conservé pendant toute la durée de la connexion. Le nom d’utilisateur est fourni par le mécanisme d’authentification pour l’application Web. Le jeton de connexion est protégé par un chiffrement et une signature numérique.

![](introduction-to-security/_static/image2.png)

Pour chaque demande, le serveur valide le contenu du jeton pour s’assurer que la demande provient de l’utilisateur spécifié. Le nom d’utilisateur doit correspondre à l’ID de connexion. En validant à la fois l’ID de connexion et le nom d’utilisateur, Signalr empêche un utilisateur malveillant d’emprunter facilement l’identité d’un autre utilisateur. Si le serveur ne peut pas valider le jeton de connexion, la demande échoue.

![](introduction-to-security/_static/image4.png)

Étant donné que l’ID de connexion fait partie du processus de vérification, vous ne devez pas révéler l’ID de connexion d’un utilisateur à d’autres utilisateurs ou stocker la valeur sur le client, par exemple dans un cookie.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Rejoindre des groupes lors de la reconnexion

Par défaut, l’application Signalr réaffecte automatiquement un utilisateur aux groupes appropriés lors de la reconnexion après une interruption temporaire, par exemple lorsqu’une connexion est supprimée et rétablie avant l’expiration du délai de connexion. Lors de la reconnexion, le client passe un jeton de groupe qui comprend l’ID de connexion et les groupes affectés. Le jeton de groupe est signé numériquement et chiffré. Le client conserve le même ID de connexion après une reconnexion ; par conséquent, l’ID de connexion transmis à partir du client reconnecté doit correspondre à l’ID de connexion précédent utilisé par le client. Cette vérification empêche un utilisateur malveillant de transmettre des demandes pour joindre des groupes non autorisés lors de la reconnexion.

Toutefois, il est important de noter que le jeton de groupe n’expire pas. Si un utilisateur appartenait à un groupe dans le passé, mais qu’il a été exclu de ce groupe, cet utilisateur peut être en mesure de reproduire un jeton de groupe qui comprend le groupe interdit. Si vous devez gérer en toute sécurité les utilisateurs qui appartiennent à des groupes, vous devez stocker ces données sur le serveur, par exemple dans une base de données. Ensuite, ajoutez une logique à votre application qui vérifie sur le serveur si un utilisateur appartient à un groupe. Pour obtenir un exemple de vérification de l’appartenance à un groupe, consultez [utilisation des groupes](../guide-to-the-api/working-with-groups.md).

La rejointure automatique de groupes s’applique uniquement lorsqu’une connexion est reconnectée après une interruption temporaire. Si un utilisateur se déconnecte en quittant l’application ou le redémarrage de l’application, votre application doit gérer l’ajout de cet utilisateur aux groupes appropriés. Pour plus d’informations, consultez [utilisation des groupes](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Comment signaler empêche la falsification de requête intersites

La falsification de requête intersites (CSRF) est une attaque dans laquelle un site malveillant envoie une demande à un site vulnérable dans lequel l’utilisateur est actuellement connecté. Signalr empêche les CSRF en rendant très peu probable qu’un site malveillant crée une demande valide pour votre application Signalr.

### <a name="description-of-csrf-attack"></a>Description de l’attaque CSRF

Voici un exemple d’attaque CSRF :

1. Un utilisateur se connecte à `www.example.com`à l’aide de l’authentification par formulaire.
2. Le serveur authentifie l’utilisateur. La réponse du serveur comprend un cookie d’authentification.
3. Sans déconnexion, l’utilisateur visite un site Web malveillant. Ce site malveillant contient le formulaire HTML suivant : 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Notez que l’action de formulaire publie sur le site vulnérable, et non sur le site malveillant. Il s’agit de la partie « inter-sites » de CSRF.
4. L’utilisateur clique sur le bouton Envoyer. Le navigateur comprend le cookie d’authentification avec la demande.
5. La demande s’exécute sur le serveur example.com avec le contexte d’authentification de l’utilisateur et peut faire tout ce qu’un utilisateur authentifié est autorisé à faire.

Bien que cet exemple exige que l’utilisateur clique sur le bouton de formulaire, la page malveillante peut tout aussi facilement exécuter un script qui envoie une requête AJAX à votre application Signalr. En outre, l’utilisation de SSL n’empêche pas une attaque CSRF, car le site malveillant peut envoyer une requête « https:// ».

En général, les attaques CSRF sont possibles sur les sites Web qui utilisent des cookies pour l’authentification, car les navigateurs envoient tous les cookies pertinents au site Web de destination. Toutefois, les attaques CSRF ne sont pas limitées à l’exploitation des cookies. Par exemple, l’authentification de base et Digest est également vulnérable. Une fois qu’un utilisateur se connecte avec l’authentification de base ou Digest, le navigateur envoie automatiquement les informations d’identification jusqu’à la fin de la session.

### <a name="csrf-mitigations-taken-by-signalr"></a>Atténuations de CSRF effectuées par Signalr

Signalr effectue les étapes suivantes pour empêcher un site malveillant de créer des demandes valides pour votre application Signalr. Ces étapes sont effectuées par défaut et ne nécessitent aucune action dans votre code.

- **Désactiver les demandes inter-domaines**  
 Par défaut, les demandes inter-domaines sont désactivées dans une application Signalr pour empêcher les utilisateurs d’appeler un point de terminaison Signalr à partir d’un domaine externe. Toutes les demandes provenant d’un domaine externe sont automatiquement considérées comme non valides et sont bloquées. Il est recommandé de conserver ce comportement par défaut ; dans le cas contraire, un site malveillant pourrait inciter les utilisateurs à envoyer des commandes à votre site. Si vous devez utiliser des requêtes inter-domaines, consultez [Comment établir une connexion inter-domaines](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Passer le jeton de connexion dans la chaîne de requête, et non le cookie**  
 Signalr transmet le jeton de connexion en tant que valeur de chaîne de requête, et non en tant que cookie. En ne stockant pas le jeton de connexion en tant que cookie, le jeton de connexion n’est pas transmis par inadvertance par le navigateur lorsque du code malveillant est rencontré. En outre, le jeton de connexion n’est pas conservé au-delà de la connexion actuelle. Par conséquent, un utilisateur malveillant ne peut pas effectuer une demande sous les informations d’identification d’un autre utilisateur.
- **Vérifier le jeton de connexion**  
 Comme décrit dans la section [jeton de connexion](#connectiontoken) , le serveur sait quel ID de connexion est associé à chaque utilisateur authentifié. Le serveur ne traite pas les demandes provenant d’un ID de connexion qui ne correspond pas au nom d’utilisateur. Il est peu probable qu’un utilisateur malveillant puisse deviner une demande valide, car l’utilisateur malveillant doit connaître le nom d’utilisateur et l’ID de connexion actuel généré de manière aléatoire. Cet ID de connexion devient non valide dès que la connexion est terminée. Les utilisateurs anonymes ne doivent pas avoir accès à des informations sensibles.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Recommandations de sécurité signalr

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocole SSL (Secure Socket Layers)

Le protocole SSL utilise le chiffrement pour sécuriser le transport de données entre un client et un serveur. Si votre application Signalr transmet des informations sensibles entre le client et le serveur, utilisez SSL pour le transport. Pour plus d’informations sur la configuration de SSL, consultez Configuration de [SSL sur IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>N’utilisez pas de groupes comme mécanisme de sécurité

Les groupes sont un moyen pratique de collecter des utilisateurs associés, mais ils ne constituent pas un mécanisme sécurisé pour limiter l’accès aux informations sensibles. Cela est particulièrement vrai lorsque les utilisateurs peuvent rejoindre automatiquement des groupes lors d’une reconnexion. Au lieu de cela, envisagez d’ajouter des utilisateurs privilégiés à un rôle et de limiter l’accès à une méthode de concentrateur uniquement aux membres de ce rôle. Pour obtenir un exemple de restriction de l’accès en fonction d’un rôle, consultez la page [authentification et autorisation pour les concentrateurs signalr](../security/hub-authorization.md). Pour obtenir un exemple de vérification de l’accès utilisateur aux groupes lors de la reconnexion, consultez [utilisation des groupes](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Gestion en toute sécurité des entrées des clients

Toutes les entrées des clients destinés à la diffusion vers d’autres clients doivent être codées pour s’assurer qu’un utilisateur malveillant n’envoie pas de script à d’autres utilisateurs. Il est préférable d’encoder les messages sur les clients de réception plutôt que sur le serveur, car votre application Signalr peut avoir de nombreux types de clients différents. Par conséquent, l’encodage HTML fonctionne pour un client Web, mais pas pour d’autres types de clients. Par exemple, une méthode de client Web pour afficher un message de conversation gère en toute sécurité le nom d’utilisateur et le message en appelant la fonction `html()`.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Rapprochement d’une modification de l’état de l’utilisateur avec une connexion active

Si l’état d’authentification d’un utilisateur change alors qu’une connexion active existe, l’utilisateur reçoit un message d’erreur indiquant que l’identité de l’utilisateur ne peut pas changer au cours d’une connexion active Signalr. Dans ce cas, votre application doit se reconnecter au serveur pour s’assurer que l’ID de connexion et le nom d’utilisateur sont coordonnés. Par exemple, si votre application autorise l’utilisateur à se déconnecter alors qu’une connexion active existe, le nom d’utilisateur de la connexion ne correspondra plus au nom transmis pour la requête suivante. Vous pouvez arrêter la connexion avant que l’utilisateur se déconnecte, puis la redémarrer.

Toutefois, il est important de noter que la plupart des applications n’ont pas besoin d’arrêter et de démarrer manuellement la connexion. Si votre application redirige les utilisateurs vers une page distincte après la déconnexion, par exemple le comportement par défaut d’une application Web Forms ou d’une application MVC, ou actualise la page actuelle après la déconnexion, la connexion active est automatiquement déconnectée et ne prend pas nécessite aucune action supplémentaire.

L’exemple suivant montre comment arrêter et démarrer une connexion lorsque l’état de l’utilisateur a changé.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Ou, l’état d’authentification de l’utilisateur peut changer si votre site utilise l’expiration décalée avec l’authentification par formulaire et qu’il n’y a aucune activité pour conserver le cookie d’authentification valide. Dans ce cas, l’utilisateur sera déconnecté et le nom d’utilisateur ne correspondra plus au nom d’utilisateur dans le jeton de connexion. Vous pouvez résoudre ce problème en ajoutant un script qui demande périodiquement une ressource sur le serveur Web afin de conserver le cookie d’authentification valide. L’exemple suivant montre comment demander une ressource toutes les 30 minutes.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Fichiers de proxy JavaScript générés automatiquement

Si vous ne souhaitez pas inclure tous les hubs et méthodes dans le fichier proxy JavaScript pour chaque utilisateur, vous pouvez désactiver la génération automatique du fichier. Vous pouvez choisir cette option si vous avez plusieurs hubs et méthodes, mais que vous ne voulez pas que chaque utilisateur soit informé de toutes les méthodes. Vous désactivez la génération automatique en affectant à **EnableJavaScriptProxies** la **valeur false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Pour plus d’informations sur les fichiers de proxy JavaScript, consultez [le proxy généré et ce qu’il fait pour vous](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Exceptions

Vous devez éviter de passer des objets d’exception aux clients, car les objets peuvent exposer des informations sensibles aux clients. Au lieu de cela, appelez une méthode sur le client qui affiche le message d’erreur approprié.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
