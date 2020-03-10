---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: Nouveautés de ASP.NET 4,5 et Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans ASP.NET 4,5. Elle décrit également les améliorations apportées au développement Web...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 32fbf7c25b00f3f0796c4c3fdd38ca2a86c89199
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526676"
---
# <a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Nouveautés d’ASP.NET 4.5 et de Visual Studio 2012

> Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans ASP.NET 4,5. Elle décrit également les améliorations apportées au développement Web dans Visual Studio 2012. Ce document a été publié à l’origine le 29 février 2012.

- [ASP.NET Core du runtime et de l’infrastructure](#_Toc318097372)

    - [Lecture et écriture asynchrones de requêtes et de réponses HTTP](#_Toc318097373)
    - [Améliorations apportées à la gestion HttpRequest](#_Toc318097374)
    - [Vidage asynchrone d’une réponse](#_Toc318097375)
    - [Prise en charge des modules et gestionnaires asynchrones basés sur des *tâches*et *await*](#_Toc318097376)
    - [Modules HTTP asynchrones](#_Toc318097377)
    - [Gestionnaires HTTP asynchrones](#_Toc318097378)
    - [Nouvelles fonctionnalités de validation des demandes ASP.NET](#_Toc318097379)
    - [Validation de requête différée (« paresseuse »)](#_Toc318097380)
    - [Prise en charge des demandes non validées](#_Toc318097381)
    - [Bibliothèque AntiXSS](#_Toc318097382)
    - [Prise en charge du protocole WebSockets](#_Toc318097383)
    - [Bundles et minimisation](#_Toc318097384)
    - [Améliorations des performances pour l’hébergement Web](#_Toc_perf)

        - [Facteurs de performance clés](#_Toc_perf_1)
        - [Conditions requises pour les nouvelles fonctionnalités de performances](#_Toc_perf_2)
        - [Partage d’assemblys communs](#_Toc_perf_3)
        - [Utilisation de la compilation JIT multicœur pour un démarrage plus rapide](#_Toc_perf_4)
        - [Paramétrage de garbage collection pour optimiser la mémoire](#_Toc_perf_5)
        - [Prérécupération pour les applications Web](#_Toc_perf_6)
- [ASP.NET Web Forms](#_Toc318097385)

    - [Contrôles de données fortement typées](#_Toc318097386)
    - [Liaison de données](#_Toc318097387)

        - [Sélection de données](#_Toc318097388)
        - [Fournisseurs de valeurs](#_Toc318097389)
        - [Filtrage par valeurs d’un contrôle](#_Toc318097390)
    - [Expressions de liaison de données encodées en HTML](#_Toc318097391)
    - [Validation discrète](#_Toc318097392)
    - [Mises à jour HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [Pages Web ASP.NET 2](#_Toc318097395)
- [Version Release candidate de Visual Studio 2012](#_Toc318097396)

    - [Partage de projet entre Visual Studio 2010 et Visual Studio 2012 release candidate (compatibilité de projet)](#project-compatibility)
    - [Modifications de configuration dans les modèles de site Web ASP.NET 4,5](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Prise en charge native dans IIS 7 pour le routage ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Éditeur HTML](#_Toc318097397)

        - [Tâches intelligentes](#_Toc318097398)
        - [WAI-prise en charge d’ARIA](#_Toc318097399)
        - [Nouveaux extraits HTML5](#_Toc318097400)
        - [Extraire vers le contrôle utilisateur](#_Toc318097401)
        - [IntelliSense pour le code pépites dans les attributs](#_Toc318097402)
        - [Changement de nom automatique de la balise correspondante lorsque vous renommez une balise d’ouverture ou de fermeture](#_Toc318097403)
        - [Génération du gestionnaire d’événements](#_Toc318097404)
        - [Retrait intelligent](#_Toc318097405)
        - [Réduire automatiquement la saisie semi-automatique des instructions](#_Toc318097406)
    - [Éditeur JavaScript](#_Toc318097407)

        - [Mode plan du code](#_Toc318097408)
        - [Accolades correspondantes](#_Toc318097409)
        - [Atteindre la définition](#_Toc318097410)
        - [Support ECMAScript5](#_Toc318097411)
        - [IntelliSense DOM](#_Toc318097412)
        - [VSDOC des surcharges de signature](#_Toc318097413)
        - [Références implicites](#_Toc318097414)
    - [Éditeur CSS](#_Toc318097415)

        - [Réduire automatiquement la saisie semi-automatique des instructions](#_Toc318097416)
        - [Mise en retrait hiérarchique.](#_Toc318097417)
        - [Prise en charge des piratages CSS](#_Toc318097418)
        - [Schémas spécifiques au fournisseur (-moz-,-WebKit)](#_Toc318097419)
        - [Prise en charge des commentaires et des marques de commentaire](#_Toc318097420)
        - [Sélecteur de couleurs](#_Toc318097421)
        - [Extraits de code](#_Toc318097422)
        - [Régions personnalisées](#_Toc318097423)
    - [Inspecteur de page](#_Toc318097424)
    - [Publication](#_Toc318097425)

        - [Profils de publication](#_Toc318097426)
        - [Précompilation et fusion ASP.NET](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [AVERTISSEMENT](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET Core du runtime et de l’infrastructure

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Lecture et écriture asynchrones de requêtes et de réponses HTTP

ASP.NET 4 a introduit la possibilité de lire une entité de requête HTTP en tant que flux à l’aide de la méthode *HttpRequest. GetBufferlessInputStream* . Cette méthode a fourni un accès en continu à l’entité de requête. Toutefois, elle s’est exécutée de façon synchrone, ce qui a bloqué un thread pendant la durée d’une demande.

ASP.NET 4,5 prend en charge la capacité de lire des flux de façon asynchrone sur une entité de requête HTTP, et la possibilité de vider de manière asynchrone. ASP.NET 4,5 vous donne également la possibilité de double-mettre en mémoire tampon une entité de requête HTTP, ce qui permet une intégration plus facile avec les gestionnaires HTTP en aval tels que les gestionnaires de page. aspx et les contrôleurs MVC ASP.NET.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Améliorations apportées à la gestion HttpRequest

La référence de flux retournée par ASP.NET 4,5 de *HttpRequest. GetBufferlessInputStream* prend en charge les méthodes de lecture synchrones et asynchrones. L’objet de *flux* retourné par *GetBufferlessInputStream* implémente désormais les méthodes BeginRead et EndRead. Les méthodes de *flux* asynchrones vous permettent de lire de façon asynchrone l’entité de requête dans des segments, tandis que ASP.net libère le thread actuel entre chaque itération d’une boucle de lecture asynchrone.

ASP.NET 4,5 a également ajouté une méthode auxiliaire pour la lecture de l’entité de requête dans un mode mis en mémoire tampon : *HttpRequest. GetBufferedInputStream*. Cette nouvelle surcharge fonctionne comme *GetBufferlessInputStream*, prenant en charge les lectures synchrones et asynchrones. Toutefois, à mesure qu’il lit, *GetBufferedInputStream* copie également les octets d’entité dans des mémoires tampons internes ASP.net afin que les modules et gestionnaires en aval puissent toujours accéder à l’entité de requête. Par exemple, si un code en amont dans le pipeline a déjà lu l’entité de requête à l’aide de *GetBufferedInputStream*, vous pouvez toujours utiliser *HttpRequest. Form* ou *HttpRequest. Files*. Cela vous permet d’effectuer un traitement asynchrone sur une requête (par exemple, en continuant le chargement d’un fichier volumineux vers une base de données), mais d’exécuter des pages. aspx et des contrôleurs ASP.NET MVC par la suite.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Vidage asynchrone d’une réponse

L’envoi de réponses à un client HTTP peut prendre beaucoup de temps lorsque le client est éloigné ou dispose d’une connexion à faible bande passante. Normalement, ASP.NET met en mémoire tampon les octets de réponse au fur et à mesure qu’ils sont créés par une application. ASP.NET effectue ensuite une opération d’envoi unique des tampons accumulés à la fin du traitement de la requête.

Si la réponse mise en mémoire tampon est volumineuse (par exemple, en diffusant un fichier volumineux sur un client), vous devez appeler de manière périodique *HttpResponse. Flush* pour envoyer la sortie mise en mémoire tampon au client et conserver l’utilisation de la mémoire sous le contrôle. Toutefois, étant donné que *flush* est un appel synchrone, l’appel de *flush* utilise toujours un thread pour la durée des requêtes potentiellement longues.

ASP.NET 4,5 ajoute la prise en charge de l’exécution asynchrone des vidages à l’aide des méthodes *BeginFlush* et *EndFlush* de la classe *HttpResponse* . À l’aide de ces méthodes, vous pouvez créer des modules asynchrones et des gestionnaires asynchrones qui envoient de façon incrémentielle des données à un client sans lier les threads du système d’exploitation. Entre les appels *BeginFlush* et *EndFlush* , ASP.net libère le thread actuel. Cela réduit considérablement le nombre total de threads actifs nécessaires pour prendre en charge les téléchargements HTTP à long terme.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Prise en charge des modules et gestionnaires asynchrones basés sur des *tâches* et *await*

Le .NET Framework 4 a introduit un concept de programmation asynchrone appelé *tâche*. Les tâches sont représentées par le type de *tâche* et les types associés dans l’espace de noms *System. Threading. Tasks* . Le .NET Framework 4,5 s’appuie sur cela avec les améliorations du compilateur qui facilitent l’utilisation des objets de *tâche* . Dans le .NET Framework 4,5, les compilateurs prennent en charge deux nouveaux mots clés : *await* et *Async*. Le mot clé *await* est un raccourci de syntaxe pour indiquer qu’un morceau de code doit attendre de façon asynchrone un autre morceau de code. Le mot clé *Async* représente un indicateur que vous pouvez utiliser pour marquer des méthodes en tant que méthodes asynchrones basées sur des tâches.

La combinaison d' *await*, *Async*et de l’objet de *tâche* facilite grandement l’écriture de code asynchrone dans .net 4,5. ASP.NET 4,5 prend en charge ces simplifications avec de nouvelles API qui vous permettent d’écrire des modules HTTP asynchrones et des gestionnaires HTTP asynchrones à l’aide des nouvelles améliorations du compilateur.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Modules HTTP asynchrones

Supposons que vous souhaitez effectuer un travail asynchrone dans une méthode qui retourne un objet de *tâche* . L’exemple de code suivant définit une méthode asynchrone qui effectue un appel asynchrone pour télécharger la page d’hébergement Microsoft. Notez l’utilisation du mot clé *Async* dans la signature de méthode et l’appel *await* à *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

C’est tout ce que vous devez écrire : le .NET Framework gère automatiquement le déroulement de la pile des appels en attendant la fin du téléchargement, ainsi que la restauration automatique de la pile des appels une fois le téléchargement terminé.

Supposons à présent que vous souhaitiez utiliser cette méthode asynchrone dans un module HTTP asynchrone ASP.NET. ASP.NET 4,5 comprend une méthode d’assistance (*EventHandlerTaskAsyncHelper*) et un nouveau type délégué (*TaskEventHandler*) que vous pouvez utiliser pour intégrer des méthodes asynchrones basées sur des tâches à l’ancien modèle de programmation asynchrone exposé par le pipeline HTTP ASP.net. Cet exemple montre comment :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Gestionnaires HTTP asynchrones

L’approche traditionnelle consistant à écrire des gestionnaires asynchrones dans ASP.NET consiste à implémenter l’interface *IHttpAsyncHandler* . ASP.NET 4,5 introduit le type de base asynchrone *HttpTaskAsyncHandler* à partir duquel vous pouvez dériver, ce qui facilite grandement l’écriture de gestionnaires asynchrones.

Le type *HttpTaskAsyncHandler* est abstrait et vous oblige à substituer la méthode *ProcessRequestAsync* . En interne, ASP.NET s’occupe de l’intégration de la signature de retour (un objet de *tâche* ) de *ProcessRequestAsync* avec l’ancien modèle de programmation asynchrone utilisé par le pipeline ASP.net.

L’exemple suivant montre comment vous pouvez utiliser *Task* et *await* dans le cadre de l’implémentation d’un gestionnaire HTTP asynchrone :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nouvelles fonctionnalités de validation des demandes ASP.NET

Par défaut, ASP.NET effectue la validation des demandes. il examine les requêtes pour rechercher un balisage ou un script dans les champs, les en-têtes, les cookies, etc. Si un est détecté, ASP.NET lève une exception. Cela fait office de première ligne de défense contre les attaques potentielles de script entre sites.

ASP.NET 4,5 permet de lire facilement des données de requête non validées de manière sélective. ASP.NET 4,5 intègre également la bibliothèque AntiXSS populaire, qui était auparavant une bibliothèque externe.

Les développeurs sont souvent invités à pouvoir désactiver de manière sélective la validation des demandes pour leurs applications. Par exemple, si votre application est un logiciel de forum, vous pouvez autoriser les utilisateurs à envoyer des billets de forum et des commentaires au format HTML, tout en veillant à ce que la validation des demandes vérifie tout le reste.

ASP.NET 4,5 introduit deux fonctionnalités qui facilitent l’utilisation sélective d’une entrée non validée : la validation de demande différée (« paresseuse ») et l’accès aux données de requête non validées.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Validation de requête différée (« paresseuse »)

Dans ASP.NET 4,5, par défaut, toutes les données de la demande sont soumises à la validation de la demande. Toutefois, vous pouvez configurer l’application pour différer la validation de la demande jusqu’à ce que vous ayez réellement accès aux données de la demande. (Cette opération est parfois appelée validation différée des demandes, selon des termes tels que le chargement différé pour certains scénarios de données.) Vous pouvez configurer l’application pour qu’elle utilise la validation différée dans le fichier Web. config en affectant à l’attribut *RequestValidationMode* la valeur 4,5 dans l’élément *httpRUntime* , comme dans l’exemple suivant :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Lorsque le mode de validation de la demande est défini sur 4,5, la validation de la demande est déclenchée uniquement pour une valeur de requête spécifique et uniquement lorsque votre code accède à cette valeur. Par exemple, si votre code obtient la valeur de Request. Form [« Forum\_publication »], la validation de la demande est appelée uniquement pour cet élément dans la collection de formulaires. Aucun des autres éléments de la collection de *formulaires* n’est validé. Dans les versions précédentes de ASP.NET, la validation de la demande a été déclenchée pour l’ensemble de la collection de demandes lors de l’accès à un élément de la collection. Le nouveau comportement permet aux différents composants de l’application d’examiner différents éléments de données de requête sans déclencher la validation de demande sur d’autres éléments.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Prise en charge des demandes non validées

La validation de demande différée seule ne résout pas le problème de la validation de demande de manière sélective. L’appel à request. Form ["Forum\_publication"] déclenche toujours la validation de la demande pour cette valeur de requête spécifique. Toutefois, vous souhaiterez peut-être accéder à ce champ sans déclencher la validation, car vous souhaitez autoriser le balisage dans ce champ.

Pour ce faire, ASP.NET 4,5 prend désormais en charge l’accès non validé aux données de demande. ASP.NET 4,5 comprend une nouvelle propriété de collection non *validée* dans la classe *HttpRequest* . Cette collection permet d’accéder à toutes les valeurs courantes des données de demande, comme *Form*, *QueryString*, *cookies*et *URL*.

À l’aide de l’exemple de forum, pour être en mesure de lire des données de requête non validées, vous devez d’abord configurer l’application pour qu’elle utilise le nouveau mode de validation de requête :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Vous pouvez ensuite utiliser la propriété *HttpRequest. unvalidated* pour lire la valeur de formulaire non validée :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]

> [!WARNING]
> Sécurité : *Utilisez des données de requête non validées avec précaution.* ASP.NET 4,5 a ajouté les collections et les propriétés de requête non validées pour vous permettre d’accéder plus facilement à des données de requête non validées très spécifiques. Toutefois, vous devez toujours effectuer une validation personnalisée sur les données de demande brutes pour vous assurer que du texte dangereux n’est pas rendu aux utilisateurs.

<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Bibliothèque AntiXSS

En raison de la popularité de Microsoft AntiXSS Library, ASP.NET 4,5 intègre désormais les routines d’encodage de base de la version 4,0 de cette bibliothèque.

Les routines d’encodage sont implémentées par le type *AntiXssEncoder* dans le nouvel espace de noms *System. Web. Security. AntiXss* . Vous pouvez utiliser le type *AntiXssEncoder* directement en appelant l’une des méthodes d’encodage statiques implémentées dans le type. Toutefois, l’approche la plus simple pour utiliser les nouvelles routines Anti-XSS consiste à configurer une application ASP.NET pour utiliser la classe *AntiXssEncoder* par défaut. Pour ce faire, ajoutez l’attribut suivant au fichier Web. config :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Lorsque l’attribut *encoderType* est défini pour utiliser le type *AntiXssEncoder* , tout l’encodage de sortie dans ASP.NET utilise automatiquement les nouvelles routines d’encodage.

Voici les parties de la bibliothèque AntiXSS externe qui ont été incorporées dans ASP.NET 4,5 :

- *HtmlEncode*, *HtmlFormUrlEncode*et *HtmlAttributeEncode*
- *XmlAttributeEncode* et *XmlEncode*
- *UrlEncode* et *UrlPathEncode* (nouveau)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Prise en charge du protocole WebSockets

Le protocole WebSockets est un protocole réseau basé sur des normes qui définit comment établir des communications bidirectionnelles sécurisées et en temps réel entre un client et un serveur via HTTP. Microsoft a travaillé avec les organismes de normalisation de l’IETF et du W3C pour vous aider à définir le protocole. Le protocole WebSocket est pris en charge par n’importe quel client (pas seulement pour les navigateurs), avec Microsoft investissant des ressources importantes prenant en charge le protocole WebSocket sur les systèmes d’exploitation clients et mobiles.

Le protocole WebSocket simplifie grandement la création de transferts de données à long terme entre un client et un serveur. Par exemple, l’écriture d’une application de conversation est beaucoup plus facile, car vous pouvez établir une connexion à long terme entre un client et un serveur. Vous n’avez pas besoin de recourir à des solutions de contournement, telles que l’interrogation périodique ou l’interrogation longue HTTP pour simuler le comportement d’un Socket.

ASP.NET 4,5 et IIS 8 incluent la prise en charge de WebSockets de bas niveau, permettant aux développeurs ASP.NET d’utiliser des API gérées pour la lecture et l’écriture asynchrones de données de type chaîne et binaire sur un objet WebSocket. Pour ASP.NET 4,5, il existe un nouvel espace de noms *System. Web. WebSockets* qui contient des types pour l’utilisation du protocole WebSocket.

Un client de navigateur établit une connexion WebSocket en créant un objet *WebSocket* DOM qui pointe vers une URL dans une application ASP.net, comme dans l’exemple suivant :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Vous pouvez créer des points de terminaison WebSocket dans ASP.NET à l’aide de n’importe quel type de module ou de gestionnaire. Dans l’exemple précédent, un fichier. ashx a été utilisé, car les fichiers. ashx sont un moyen rapide de créer un gestionnaire.

D’après le protocole WebSockets, une application ASP.NET accepte la demande WebSockets d’un client en indiquant que la demande doit être mise à niveau à partir d’une requête HTTP d’extraction vers une requête WebSocket. Voici un exemple :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

La méthode *AcceptWebSocketRequest* accepte un délégué de fonction, car ASP.net déroule la requête HTTP actuelle, puis transfère le contrôle au délégué de fonction. Conceptuellement, cette approche est similaire à la façon dont vous utilisez *System. Threading. thread*, où vous définissez un délégué de démarrage de thread dans lequel le travail en arrière-plan est effectué.

Une fois que ASP.NET et le client ont terminé avec succès un protocole de transfert WebSocket, ASP.NET appelle votre délégué et l’application WebSockets commence à s’exécuter. L’exemple de code suivant montre une application Echo simple qui utilise la prise en charge intégrée de WebSockets dans ASP.NET :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

La prise en charge dans .NET 4,5 pour le mot clé *await* et les opérations asynchrones basées sur les tâches est un choix naturel pour l’écriture d’applications WebSocket. L’exemple de code montre qu’une requête WebSocket s’exécute complètement de manière asynchrone dans ASP.NET. L’application attend de façon asynchrone qu’un message soit envoyé par un client en appelant *await Socket. ReceiveAsync*. De même, vous pouvez envoyer un message asynchrone à un client en appelant le *Socket await. SendAsync*.

Dans le navigateur, une application reçoit des messages WebSocket via une fonction *onMessage* . Pour envoyer un message à partir d’un navigateur, vous appelez la méthode *Send* du type DOM *WebSocket* , comme illustré dans cet exemple :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

À l’avenir, nous pourrions publier des mises à jour de cette fonctionnalité qui éliminent une partie du codage de bas niveau requis dans cette version pour les applications WebSocket.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Bundles et minimisation

Le regroupement vous permet de combiner des fichiers JavaScript et CSS individuels dans un bundle qui peut être traité comme un fichier unique. La minimisation condense les fichiers JavaScript et CSS en supprimant les espaces et les autres caractères qui ne sont pas requis. Ces fonctionnalités fonctionnent avec Web Forms, ASP.NET MVC et Web pages.

Les offres groupées sont créées à l’aide de la classe Bundle ou de l’une de ses classes enfants, ScriptBundle et StyleBundle. Après la configuration d’une instance d’un bundle, le bundle est mis à la disposition des demandes entrantes en l’ajoutant simplement à une instance BundleCollection globale. Dans les modèles par défaut, la configuration de regroupement est effectuée dans un fichier baBundleConfig. Cette configuration par défaut crée des regroupements pour tous les principaux scripts et fichiers CSS utilisés par les modèles.

Les offres groupées sont référencées dans les vues à l’aide d’une des deux méthodes d’assistance possibles. Pour prendre en charge le rendu d’un balisage différent pour un regroupement en mode de débogage et en mode de mise en sortie, les classes ScriptBundle et StyleBundle ont la méthode d’assistance, Render. En mode débogage, Render génère le balisage pour chaque ressource du bundle. En mode de mise en sortie, Render génère un seul élément de balisage pour le bundle entier. Le basculement entre le mode debug et le mode release peut être effectué en modifiant l’attribut debug de l’élément compilation dans Web. config, comme indiqué ci-dessous :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

En outre, l’activation ou la désactivation de l’optimisation peut être définie directement via la propriété BundleTable. EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Quand des fichiers sont regroupés, ils sont d’abord triés par ordre alphabétique (comme ils s’affichent dans **Explorateur de solutions**). Ils sont ensuite organisés afin que les bibliothèques connues et leurs extensions personnalisées (telles que jQuery, MooTools et dojo) soient chargées en premier. Par exemple, l’ordre final pour le regroupement du dossier scripts, comme indiqué ci-dessus, est le suivant :

1. jQuery-1.6.2. js
2. jQuery-UI. js
3. jQuery. Tools. js
4. a. js

Les fichiers CSS sont également triés par ordre alphabétique, puis réorganisés de sorte que Reset. CSS et Normalize. CSS viennent avant tout autre fichier. Le tri final du regroupement du dossier de styles indiqué ci-dessus est le suivant :

1. Reset. CSS
2. contenu. CSS
3. Forms. CSS
4. Globals. CSS
5. menu. CSS
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Améliorations des performances pour l’hébergement Web

Les .NET Framework 4,5 et Windows 8 introduisent des fonctionnalités qui peuvent vous aider à améliorer les performances des charges de travail de serveur Web. Cela comprend une réduction (jusqu’à 35%) à la fois dans le temps de démarrage et dans l’encombrement mémoire des sites d’hébergement Web qui utilisent ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Facteurs de performance clés

Idéalement, tous les sites Web doivent être actifs et en mémoire pour garantir une réponse rapide à la requête suivante, à chaque fois qu’elle est fournie. Les facteurs qui peuvent affecter la réactivité du site sont les suivants :

- Temps nécessaire au redémarrage d’un site après le recyclage d’un pool d’applications. Il s’agit du temps nécessaire pour lancer un processus de serveur Web pour le site lorsque les assemblys de site ne sont plus en mémoire. (Les assemblys de la plateforme sont toujours en mémoire, car ils sont utilisés par d’autres sites.) Cette situation est appelée « site froid, démarrage de l’infrastructure chaude » ou simplement « démarrage de site froid ».
- Quantité de mémoire occupée par le site. Les termes sont « consommation de mémoire par site » ou « plage de travail non partagée ».

Les nouvelles améliorations en matière de performances sont axées sur ces deux facteurs.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Conditions requises pour les nouvelles fonctionnalités de performances

La configuration requise pour les nouvelles fonctionnalités peut être divisée en catégories :

- Améliorations qui s’exécutent sur le .NET Framework 4.
- Les améliorations qui nécessitent le .NET Framework 4,5 mais peuvent s’exécuter sur n’importe quelle version de Windows.
- Améliorations disponibles uniquement avec .NET Framework 4,5 s’exécutant sur Windows 8.

Les performances augmentent avec chaque niveau d’amélioration que vous pouvez activer.

Certaines des améliorations apportées à l' .NET Framework 4,5 tirent parti des fonctionnalités de performances plus larges qui s’appliquent également à d’autres scénarios.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Partage d’assemblys communs

**Exigence**: Kit de développement logiciel (SDK) .NET Framework 4 et Visual Studio 11 developer preview

Les différents sites sur un serveur utilisent souvent les mêmes assemblys d’assistance (par exemple, les assemblys d’un starter kit ou d’un exemple d’application). Chaque site possède sa propre copie de ces assemblys dans son répertoire bin. Bien que le code de l’objet pour les assemblys soit identique, il s’agit d’assemblys physiquement distincts, de sorte que chaque assembly doit être lu séparément lors du démarrage du site froid et conservé séparément dans la mémoire.

La nouvelle fonctionnalité d’internation résout cette inefficacité et réduit les besoins en RAM et le temps de chargement. L’interning permet à Windows de conserver une seule copie de chaque assembly dans le système de fichiers, et les assemblys individuels des dossiers bin du site sont remplacés par des liens symboliques vers la copie unique. Si un site individuel a besoin d’une version distincte de l’assembly, le lien symbolique est remplacé par la nouvelle version de l’assembly et seul ce site est affecté.

Le partage d’assemblys à l’aide de liens symboliques requiert un nouvel outil nommé ASPNET\_intern. exe, qui vous permet de créer et de gérer le magasin d’assemblys internés. Il est fourni dans le cadre du kit de développement logiciel (SDK) Visual Studio 11 developer preview. (Toutefois, il fonctionne sur un système sur lequel seul le .NET Framework 4 est installé, en supposant que vous avez installé la dernière [mise à jour](https://support.microsoft.com/kb/2468871).)

Pour vous assurer que tous les assemblys éligibles ont été mis en pool, exécutez ASPNET\_intern. exe régulièrement (par exemple, une fois par semaine en tant que tâche planifiée). Une utilisation classique est la suivante :

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Pour afficher toutes les options, exécutez l’outil sans arguments.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Utilisation de la compilation JIT multicœur pour un démarrage plus rapide

**Exigence**: .NET Framework 4,5

Pour le démarrage d’un site froid, non seulement les assemblys doivent être lus à partir du disque, mais le site doit être compilé juste-à-temps. Pour un site complexe, cela peut entraîner des retards importants. Une nouvelle technique à usage général dans la .NET Framework 4,5 réduit ces délais en répartissant la compilation JIT entre les cœurs de processeur disponibles. Il le fait autant et aussi tôt que possible en utilisant les informations collectées lors des lancements précédents du site. Cette fonctionnalité est implémentée par la méthode [System. Runtime. ProfileOptimization. StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) .

La compilation JIT à l’aide de plusieurs cœurs est activée par défaut dans ASP.NET. vous n’avez donc pas besoin de faire quoi que ce soit pour tirer parti de cette fonctionnalité. Si vous souhaitez désactiver cette fonctionnalité, définissez le paramètre suivant dans le fichier Web. config :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Paramétrage de garbage collection pour optimiser la mémoire

**Exigence**: .NET Framework 4,5

Une fois qu’un site est en cours d’exécution, son utilisation du tas de garbage collector (GC) peut être un facteur important dans sa consommation de mémoire. Comme tout garbage collector, le .NET Framework GC fait des compromis entre le temps processeur (fréquence et signification des collections) et la consommation de mémoire (espace supplémentaire utilisé pour les nouveaux objets, libérés ou libres). Pour les versions précédentes, nous avons fourni des conseils sur la façon de configurer le GC pour obtenir le bon équilibre (par exemple, consultez Configuration de l' [hébergement partagé ASP.NET 2.0/3.5](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Pour la .NET Framework 4,5, au lieu de plusieurs paramètres autonomes, un paramètre de configuration défini par la charge de travail est disponible et permet d’activer tous les paramètres GC précédemment recommandés, ainsi que le nouveau paramétrage qui offre des performances supplémentaires pour chaque site. plage de travail.

Pour activer le réglage de la mémoire GC, ajoutez le paramètre suivant au fichier Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Si vous êtes familiarisé avec les instructions précédentes relatives aux modifications apportées à Aspnet. config, Notez que ce paramètre remplace les anciens paramètres. par exemple, il n’est pas nécessaire de définir gcServer, gcConcurrent, etc. Vous n’avez pas besoin de supprimer les anciens paramètres.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Prérécupération pour les applications Web

**Exigence**: .NET Framework 4,5 s’exécutant sur Windows 8

Pour plusieurs versions, Windows a inclus une technologie appelée [prérécupération](http://en.wikipedia.org/wiki/Prefetcher) qui réduit le coût de lecture de disque du démarrage de l’application. Étant donné que le démarrage à froid est principalement un problème pour les applications clientes, cette technologie n’a pas été incluse dans Windows Server, qui inclut uniquement les composants essentiels à un serveur. La prérécupération est désormais disponible dans la dernière version de Windows Server, où elle peut optimiser le lancement de sites Web individuels.

Pour Windows Server, la prérécupération n’est pas activée par défaut. Pour activer et configurer le prérécupération pour l’hébergement Web à haute densité, exécutez l’ensemble de commandes suivant sur la ligne de commande :

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Ensuite, pour intégrer le prérécupération à des applications ASP.NET, ajoutez le code suivant au fichier Web. config :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Forms

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Contrôles de données fortement typées

Dans ASP.NET 4,5, Web Forms apporte des améliorations à l’utilisation des données. La première amélioration est l’un des contrôles de données fortement typés. Pour les contrôles Web Forms dans les versions précédentes de ASP.NET, vous affichez une valeur liée aux données à l’aide de *eval* et d’une expression de liaison de données :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Pour la liaison de données bidirectionnelle, vous utilisez *Bind*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

Au moment de l’exécution, ces appels utilisent la réflexion pour lire la valeur du membre spécifié, puis affichent le résultat dans le balisage. Cette approche facilite la liaison de données à des données arbitraires et informes.

Toutefois, les expressions de liaison de données comme celle-ci ne prennent pas en charge les fonctionnalités telles que IntelliSense pour les noms de membres, la navigation (par exemple, atteindre la définition) ou la vérification au moment de la compilation pour ces noms.

Pour résoudre ce problème, ASP.NET 4,5 ajoute la possibilité de déclarer le type de données des données auxquelles un contrôle est lié. Pour ce faire, vous utilisez la nouvelle propriété *ItemType* . Lorsque vous définissez cette propriété, deux nouvelles variables typées sont disponibles dans l’étendue des expressions de liaison de données : *Item* et *BindItem*. Étant donné que les variables sont fortement typées, vous bénéficiez de tous les avantages de l’expérience de développement Visual Studio.

Pour les expressions de liaison de données bidirectionnelle, utilisez la variable *BindItem* :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]

La plupart des contrôles de l’infrastructure de Web Forms ASP.NET qui prennent en charge la liaison de données ont été mis à jour pour prendre en charge la propriété *ItemType* .

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Liaison de modèle

La liaison de modèle étend la liaison de données dans les contrôles de Web Forms ASP.NET pour utiliser l’accès aux données centré sur le code. Il intègre des concepts issus du contrôle *ObjectDataSource* et de la liaison de modèle dans ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Sélection de données

Pour configurer un contrôle de données afin d’utiliser la liaison de modèle pour sélectionner des données, vous affectez à la propriété *SelectMethod* du contrôle le nom d’une méthode dans le code de la page. Le contrôle de données appelle la méthode au moment approprié dans le cycle de vie de la page et lie automatiquement les données retournées. Il n’est pas nécessaire d’appeler explicitement la méthode *DataBind* .

Dans l’exemple suivant, le contrôle *GridView* est configuré pour utiliser une méthode nommée *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Vous créez la méthode *GetCategories* dans le code de la page. Pour une opération Select simple, la méthode n’a besoin d’aucun paramètre et doit retourner un objet *IEnumerable* ou *IQueryable* . Si la nouvelle *propriété ItemType* est définie (ce qui active les expressions de liaison de données fortement typées, comme expliqué dans les [contrôles de données fortement typés](#_Toc318097386) précédemment), les versions génériques de ces interfaces doivent être retournées : *IEnumerable&lt;T&gt;* ou *IQueryable&lt;t&gt;* , avec le paramètre *t* correspondant au type de la propriété *ItemType* (par exemple, *IQueryable&lt;Category&gt;* ).

L’exemple suivant montre le code pour une méthode *GetCategories* . Cet exemple utilise le modèle de Code First Entity Framework avec l’exemple de base de données Northwind. Le code vérifie que la requête retourne les détails des produits associés pour chaque catégorie par le biais de la méthode *include* . (Cela permet de s’assurer que l’élément *TemplateField* dans le balisage affiche le nombre de produits dans chaque catégorie sans nécessiter une [sélection n + 1](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Lorsque la page s’exécute, le contrôle *GridView* appelle automatiquement la méthode *GetCategories* et restitue les données retournées à l’aide des champs configurés :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Étant donné que la méthode Select retourne un objet *IQueryable* , le contrôle *GridView* peut poursuivre la manipulation de la requête avant de l’exécuter. Par exemple, le contrôle *GridView* peut ajouter des expressions de requête pour le tri et la pagination à l’objet *IQueryable* retourné avant son exécution, afin que ces opérations soient effectuées par le fournisseur LINQ sous-jacent. Dans ce cas, Entity Framework garantit que ces opérations sont exécutées dans la base de données.

L’exemple suivant montre le contrôle *GridView* modifié pour autoriser le tri et la pagination :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Désormais, lorsque la page s’exécute, le contrôle peut s’assurer que seule la page de données actuelle est affichée et qu’elle est triée par la colonne sélectionnée :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Pour filtrer les données retournées, les paramètres doivent être ajoutés à la méthode Select. Ces paramètres sont remplis par la liaison de modèle au moment de l’exécution, et vous pouvez les utiliser pour modifier la requête avant de retourner les données.

Par exemple, supposons que vous souhaitez permettre aux utilisateurs de filtrer les produits en entrant un mot clé dans la chaîne de requête. Vous pouvez ajouter un paramètre à la méthode et mettre à jour le code pour utiliser la valeur du paramètre :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Ce code comprend une expression *Where* si une valeur est fournie pour le *mot clé* , puis retourne les résultats de la requête.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Fournisseurs de valeurs

L’exemple précédent n’était pas spécifique sur l’emplacement d’origine de la valeur du paramètre de *mot clé* . Pour indiquer ces informations, vous pouvez utiliser un attribut de paramètre. Pour cet exemple, vous pouvez utiliser la classe *QueryStringAttribute* qui se trouve dans l’espace de noms *System. Web. ModelBinding* :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Cela indique à la liaison de modèle d’essayer de lier une valeur de la chaîne de requête au paramètre de *mot clé* au moment de l’exécution. (Cela peut impliquer la conversion de type, bien qu’il ne soit pas dans ce cas.) Si une valeur ne peut pas être fournie et que le type n’accepte pas les valeurs NULL, une exception est levée.

Les sources de valeurs pour ces méthodes sont appelées fournisseurs de valeurs, et les attributs de paramètres qui indiquent le fournisseur de valeur à utiliser sont appelés attributs de fournisseur de valeur. Web Forms inclura les fournisseurs de valeurs et les attributs correspondants pour toutes les sources typiques d’entrée utilisateur dans une application Web Forms, telles que la chaîne de requête, les cookies, les valeurs de formulaire, les contrôles, l’état d’affichage, l’état de session et les propriétés de profil. Vous pouvez également écrire des fournisseurs de valeurs personnalisés.

Par défaut, le nom du paramètre est utilisé comme clé pour rechercher une valeur dans la collection de fournisseurs de valeurs. Dans l’exemple, le code recherche une valeur de chaîne de requête nommée mot clé (par exemple, ~/default.aspx ? Keyword = chef). Vous pouvez spécifier une clé personnalisée en la transmettant en tant qu’argument à l’attribut de paramètre. Par exemple, pour utiliser la valeur de la variable de chaîne de requête nommée q, vous pouvez procéder de la façon suivante :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Si cette méthode est dans le code de la page, les utilisateurs peuvent filtrer les résultats en passant un mot clé à l’aide de la chaîne de requête :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

La liaison de modèle effectue de nombreuses tâches que vous devriez sinon coder manuellement : en lisant la valeur, en vérifiant une valeur null, en tentant de la convertir en type approprié, en vérifiant si la conversion a réussi, et enfin en utilisant la valeur de la demande. La liaison de modèle génère beaucoup moins de code et permet de réutiliser les fonctionnalités dans toute votre application.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtrage par valeurs d’un contrôle

Supposons que vous souhaitiez étendre l’exemple pour permettre à l’utilisateur de choisir une valeur de filtre dans une liste déroulante. Ajoutez la liste déroulante suivante au balisage et configurez-la pour qu’elle récupère ses données d’une autre méthode à l’aide de la propriété *SelectMethod* :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

En général, vous ajoutez également un élément *EmptyDataTemplate* au contrôle *GridView* afin que le contrôle affiche un message si aucun produit correspondant n’est trouvé :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Dans le code de la page, ajoutez la nouvelle méthode Select pour la liste déroulante :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Enfin, mettez à jour la méthode Select *GetProducts* pour prendre un nouveau paramètre qui contient l’ID de la catégorie sélectionnée dans la liste déroulante :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Désormais, lorsque la page s’exécute, les utilisateurs peuvent sélectionner une catégorie dans la liste déroulante, et le contrôle *GridView* est automatiquement relié pour afficher les données filtrées. Cela est possible parce que la liaison de modèle suit les valeurs des paramètres pour les méthodes Select et détecte si une valeur de paramètre a changé après une publication (postback). Si c’est le cas, la liaison de modèle force le contrôle de données associé à se relier aux données.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Expressions de liaison de données encodées en HTML

Vous pouvez désormais encoder en HTML le résultat des expressions de liaison de données. Ajoutez un signe deux-points ( :) à la fin du préfixe &lt;% # qui marque l’expression de liaison de données :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Validation discrète

Vous pouvez maintenant configurer les contrôles de validateur intégrés pour utiliser un JavaScript discret pour la logique de validation côté client. Cela réduit considérablement la quantité de JavaScript rendue inline dans le balisage de la page et réduit la taille globale de la page. Vous pouvez configurer un JavaScript discret pour les contrôles de validateur de l’une des manières suivantes :

- Globalement en ajoutant le paramètre suivant à l’élément *&lt;appSettings&gt;* dans le fichier Web. config : 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Globalement en affectant à la propriété statique *System. Web. UI. ValidationSettings. UnobtrusiveValidationMode* la valeur *UnobtrusiveValidationMode. WebForms* (généralement dans l' *application\_méthode Start* dans le fichier global. asax).
- Individuellement pour une page en affectant à la nouvelle propriété *UnobtrusiveValidationMode* de la classe *page* la valeur *UnobtrusiveValidationMode. WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Mises à jour HTML5

Des améliorations ont été apportées à Web Forms contrôles serveur pour tirer parti des nouvelles fonctionnalités de HTML5 :

- La propriété *TextMode* du contrôle *TextBox* a été mise à jour pour prendre en charge les nouveaux types d’entrées HTML5 comme *email*, *DateTime*, etc.
- Le contrôle *FileUpload* prend désormais en charge plusieurs chargements de fichiers à partir de navigateurs qui prennent en charge cette fonctionnalité HTML5.
- Les contrôles Validator prennent désormais en charge la validation des éléments d’entrée HTML5.
- Les nouveaux éléments HTML5 qui ont des attributs qui représentent une URL prennent désormais en charge runat = "Server". Par conséquent, vous pouvez utiliser des conventions ASP.NET dans les chemins d’URL, comme l’opérateur ~ pour représenter la racine de l’application (par exemple, &lt;vidéo runat = "Server" src = "~/myVideo.wmv"/&gt;).
- Le contrôle *UpdatePanel* a été résolu pour prendre en charge la publication des champs d’entrée HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta est désormais inclus dans Visual Studio 11 bêta. ASP.NET MVC est une infrastructure qui vous aide à développer des applications Web hautement testables et faciles à gérer en tirant parti du modèle MVC (Model-View-Controller). ASP.NET MVC 4 facilite la création d’applications pour le Web mobile et comprend API Web ASP.NET, qui vous aide à créer des services HTTP capables d’atteindre n’importe quel appareil. Pour plus d’informations, consultez les [notes de publication de ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>Pages web ASP.NET 2

Les nouvelles fonctionnalités sont les suivantes :

- Modèles de site nouveaux et mis à jour.
- Ajout de la validation côté serveur et côté client à l’aide du programme d’assistance de *validation* .
- La possibilité d’inscrire des scripts à l’aide d’un gestionnaire de ressources.
- Activation des connexions à partir de Facebook et d’autres sites à l’aide d’OAuth et OpenID.
- Ajout de mappages à l’aide du programme d’assistance *Maps* .
- Exécution des applications Web pages côte à côte.
- Rendu des pages pour les périphériques mobiles.

Pour plus d’informations sur ces fonctionnalités et des exemples de code de page entière, consultez [les principales fonctionnalités de Web pages 2 bêta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 bêta

Cette section fournit des informations sur les améliorations apportées au développement Web dans Visual Web Developer 11 Beta et Visual Studio 2012 release candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Partage de projet entre Visual Studio 2010 et Visual Studio 2012 release candidate (compatibilité de projet)

Jusqu’à la version Release candidate de Visual Studio 2012, l’ouverture d’un projet existant dans une version plus récente de Visual Studio a lancé l’Assistant Conversion. Cela obligeait une mise à niveau du contenu (ressources) d’un projet et d’une solution aux nouveaux formats qui n’étaient pas à compatibilité descendante. Par conséquent, après la conversion, vous n’avez pas pu ouvrir le projet dans la version antérieure de Visual Studio.

De nombreux clients nous ont dit qu’il ne s’agissait pas de la bonne approche. Dans Visual Studio 11 Beta, nous prenons désormais en charge le partage de projets et de solutions avec Visual Studio 2010 SP1. Cela signifie que si vous ouvrez un projet 2010 dans Visual Studio 2012 release candidate, vous pourrez toujours ouvrir le projet dans Visual Studio 2010 SP1.

> [!NOTE]
> Certains types de projets ne peuvent pas être partagés entre Visual Studio 2010 SP1 et Visual Studio 2012 release candidate. Celles-ci incluent des projets plus anciens (tels que les projets ASP.NET MVC 2) ou des projets à des fins spéciales (tels que des projets d’installation).

Quand vous ouvrez un projet Web Visual Studio 2010 SP1 pour la première fois dans Visual Studio 11 Beta, les propriétés suivantes sont ajoutées au fichier projet :

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags, UpgradeBackupLocation et OldToolsVersion sont utilisés par le processus qui met à niveau le fichier projet. Ils n’ont aucun impact sur l’utilisation du projet dans Visual Studio 2010.

VisualStudioVersion est une nouvelle propriété utilisée par MSBuild 4,5 qui indique la version de Visual Studio pour le projet actuel. Étant donné que cette propriété n’existait pas dans MSBuild 4,0 (la version de MSBuild utilisée par Visual Studio 2010 SP1), nous injectons une valeur par défaut dans le fichier projet.

La propriété VSToolsPath est utilisée pour déterminer le fichier. targets correct à importer à partir du chemin d’accès représenté par le paramètre MSBuildExtensionsPath32.

Des modifications sont également apportées aux éléments d’importation. Ces modifications sont nécessaires pour prendre en charge la compatibilité entre les deux versions de Visual Studio.

> [!NOTE]
> Si un projet est partagé entre Visual Studio 2010 SP1 et Visual Studio 11 bêta sur deux ordinateurs différents, et si le projet comprend une base de données locale dans le dossier de données de l’application\_, vous devez vous assurer que la version de SQL Server utilisée par la base de données est installée sur les deux ordinateurs.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Modifications de configuration dans les modèles de site Web ASP.NET 4,5

Les modifications suivantes ont été apportées au fichier *Web. config* par défaut pour le site créé à l’aide de modèles de site Web dans Visual Studio 2012 release candidate :

- Dans l’élément `<httpRuntime>`, l’attribut `encoderType` est maintenant défini par défaut pour utiliser les types AntiXSS qui ont été ajoutés à ASP.NET. Pour plus d’informations, consultez [AntiXSS Library](#_Toc318097382).
- De même, dans l’élément `<httpRuntime>`, l’attribut `requestValidationMode` est défini sur « 4,5 ». Cela signifie que, par défaut, la validation de la demande est configurée pour utiliser la validation différée (« Lazy »). Pour plus d’informations, consultez [nouvelles fonctionnalités de validation des demandes ASP.net](#_Toc318097379).
- L’élément `<modules>` de la section `<system.webServer>` ne contient pas d’attribut `runAllManagedModulesForAllRequests`. (Sa valeur par défaut est false.) Cela signifie que si vous utilisez une version d’IIS 7 qui n’a pas été mise à jour vers SP1, vous risquez de rencontrer des problèmes de routage dans un nouveau site. Pour plus d’informations, consultez [prise en charge native dans IIS 7 pour le routage ASP.net](#Native_Support_In_IIS7_For_ASPNET_Routine).

Ces modifications n’affectent pas les applications existantes. Toutefois, ils peuvent représenter une différence de comportement entre les sites Web existants et les nouveaux sites Web que vous créez pour ASP.NET 4,5 à l’aide des nouveaux modèles.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Prise en charge native dans IIS 7 pour le routage ASP.NET

Ce n’est pas une modification de ASP.NET en tant que tel, mais une modification des modèles pour les nouveaux projets de site Web qui peuvent vous affecter si vous utilisez une version d’IIS 7 pour laquelle la mise à jour SP1 n’a pas été appliquée.

Dans ASP.NET, vous pouvez ajouter le paramètre de configuration suivant aux applications afin de prendre en charge le routage :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Quand **runAllManagedModulesForAllRequests** a la valeur true, une URL telle que `http://mysite/myapp/home` passe à ASP.net, même s’il n’y a pas de *. aspx*, *. Mvc*ou une extension similaire sur l’URL.

Une mise à jour apportée à IIS 7 rend le paramètre **runAllManagedModulesForAllRequests** inutile et prend en charge le routage ASP.net en mode natif. (Pour plus d’informations sur la mise à jour, consultez l’article Support Microsoft [une mise à jour est disponible qui permet à certains gestionnaires iis 7,0 ou iis 7,5 de gérer les demandes dont les URL ne se terminent pas par un point](https://support.microsoft.com/kb/980368).)

Si votre site Web s’exécute sur IIS 7 et si IIS a été mis à jour, vous n’avez pas besoin de définir **runAllManagedModulesForAllRequests** sur true. En fait, l’affectation de la valeur true n’est pas recommandée, car elle ajoute une surcharge de traitement inutile à la demande. Lorsque ce paramètre est défini sur true, toutes les demandes, y compris celles pour les fichiers *. htm*, *. jpg*et les autres fichiers statiques, passent par le pipeline de requête ASP.net.

Si vous créez un nouveau site Web ASP.NET 4,5 à l’aide des modèles fournis dans Visual Studio 2012 RC, la configuration du site Web n’inclut pas le paramètre **runAllManagedModulesForAllRequests** . Cela signifie que, par défaut, le paramètre a la valeur false.

Si vous exécutez ensuite le site Web sur Windows 7 sans SP1, IIS 7 n’inclut pas la mise à jour requise. Par conséquent, le routage ne fonctionnera pas et vous verrez des erreurs. Si vous rencontrez un problème où le routage ne fonctionne pas, vous pouvez effectuer les opérations suivantes :

- Mettez à jour Windows 7 vers SP1, qui ajoutera la mise à jour à IIS 7.
- Installez la mise à jour décrite dans l’article Support Microsoft répertorié précédemment.
- Affectez à **runAllManagedModulesForAllRequests** la valeur true dans le fichier Web. config de ce site Web. Notez que cette opération ajoute une surcharge aux demandes.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Éditeur HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Tâches intelligentes

Dans Mode Création, les propriétés complexes des contrôles serveur ont souvent des boîtes de dialogue et des assistants associés pour faciliter leur définition. Par exemple, vous pouvez utiliser une boîte de dialogue spéciale pour ajouter une source de données à un contrôle *Repeater* ou ajouter des colonnes à un contrôle *GridView* .

Toutefois, ce type d’aide sur l’interface utilisateur pour les propriétés complexes n’est pas disponible en mode Source. Par conséquent, Visual Studio 11 introduit des tâches intelligentes pour le mode Source. Les tâches intelligentes sont des raccourcis de contexte pour les fonctionnalités couramment C# utilisées dans les éditeurs et Visual Basic.

Pour les contrôles de Web Forms ASP.NET, les tâches intelligentes apparaissent sur les balises de serveur comme un petit glyphe lorsque le point d’insertion se trouve à l’intérieur de l’élément :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Le Tâche guidée se développe lorsque vous cliquez sur le glyphe ou que vous appuyez sur CTRL +. (point), tout comme dans les éditeurs de code. Il affiche ensuite des raccourcis similaires aux tâches intelligentes dans Mode Création.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Par exemple, le Tâche guidée de l’illustration précédente montre les options des tâches GridView. Si vous choisissez Modifier les colonnes, la boîte de dialogue suivante s’affiche :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Le remplissage de la boîte de dialogue définit les mêmes propriétés que celles que vous pouvez définir dans Mode Création. Lorsque vous cliquez sur OK, le balisage du contrôle est mis à jour avec les nouveaux paramètres :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>WAI-prise en charge d’ARIA

L’écriture de sites Web accessibles devient de plus en plus important. La [norme d’accessibilité WAI-ARIA](http://www.w3.org/WAI/intro/aria) définit la manière dont les développeurs doivent écrire des sites Web accessibles. Cette norme est désormais entièrement prise en charge dans Visual Studio.

Par exemple, l’attribut *role* a désormais une fonctionnalité IntelliSense complète :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

La norme WAI-ARIA introduit également des attributs qui ont le préfixe *Aria-* qui vous permettent d’ajouter une sémantique à un document HTML5. Visual Studio prend également entièrement en charge ces attributs *Aria* :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nouveaux extraits HTML5

Pour accélérer et faciliter l’écriture du balisage HTML5 couramment utilisé, Visual Studio comprend un certain nombre d’extraits. Voici un exemple d’extrait vidéo :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Pour appeler l’extrait de code, appuyez deux fois sur la touche Tab lorsque l’élément est sélectionné dans IntelliSense :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Cela génère un extrait de code que vous pouvez personnaliser.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Extraire vers le contrôle utilisateur

Dans les pages Web volumineuses, il peut être judicieux de déplacer des éléments individuels dans les contrôles utilisateur. Cette forme de refactorisation peut contribuer à améliorer la lisibilité de la page et peut simplifier la structure de la page.

Pour faciliter cette opération, lorsque vous modifiez Web Forms pages en mode Source, vous pouvez maintenant sélectionner du texte dans une page, cliquer dessus avec le bouton droit, puis choisir extraire vers le contrôle utilisateur :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense pour le code pépites dans les attributs

Visual Studio a toujours fourni IntelliSense pour le code pépites côté serveur dans n’importe quelle page ou n’importe quel contrôle. Désormais, Visual Studio intègre IntelliSense pour le code pépites dans les attributs HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Cela facilite la création d’expressions de liaison de données :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Changement de nom automatique de la balise correspondante lorsque vous renommez une balise d’ouverture ou de fermeture

Si vous renommez un élément HTML (par exemple, si vous modifiez une balise *div* en une balise d' *en-tête* ), la balise d’ouverture ou de fermeture correspondante change également en temps réel.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Cela permet d’éviter l’erreur où vous oubliez de modifier une balise de fermeture ou de modifier une balise de fermeture incorrecte.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Génération du gestionnaire d’événements

Visual Studio propose désormais des fonctionnalités en mode source pour vous aider à écrire des gestionnaires d’événements et à les lier manuellement. Si vous modifiez un nom d’événement en mode Source, IntelliSense affiche &lt;créer un nouvel événement&gt;, qui crée un gestionnaire d’événements dans le code de la page qui a la signature appropriée :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Par défaut, le gestionnaire d’événements utilise l’ID du contrôle pour le nom de la méthode de gestion des événements :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Le gestionnaire d’événements résultant doit ressembler à ceci (dans ce C#cas, dans) :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Retrait intelligent

Lorsque vous appuyez sur entrée alors qu’il se trouve dans un élément HTML vide, l’éditeur place le point d’insertion au bon endroit :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Si vous appuyez sur entrée à cet emplacement, la balise de fermeture est déplacée vers le dessous et mise en retrait pour correspondre à la balise d’ouverture. Le point d’insertion est également mis en retrait :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Réduire automatiquement la saisie semi-automatique des instructions

La liste IntelliSense dans Visual Studio filtre maintenant en fonction de ce que vous tapez pour afficher uniquement les options pertinentes :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense filtre également en fonction de la casse des mots individuels dans la liste IntelliSense. Par exemple, si vous tapez « DL », DL et ASP : DataList s’affichent :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Cette fonctionnalité permet d’obtenir plus rapidement la saisie semi-automatique des instructions pour les éléments connus.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>éditeur de code JavaScript

L’éditeur JavaScript dans la version Release candidate de Visual Studio 2012 est entièrement nouveau et il améliore considérablement l’expérience de l’utilisation de JavaScript dans Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Plan du code

Les régions en mode plan sont maintenant créées automatiquement pour toutes les fonctions, ce qui vous permet de réduire les parties du fichier qui ne sont pas pertinentes pour votre focus actuel.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Correspondance d’accolade

Lorsque vous placez le point d’insertion sur une accolade ouvrante ou fermante, l’éditeur met en surbrillance celui qui correspond.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Atteindre la définition

La commande atteindre la définition vous permet d’accéder à la source d’une fonction ou d’une variable.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Support ECMAScript5

L’éditeur prend en charge la nouvelle syntaxe et les nouvelles API dans ECMAScript5, la dernière version de la norme qui décrit le langage JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>IntelliSense DOM

IntelliSense pour les API DOM a été amélioré, avec la prise en charge de nombreuses nouvelles API HTML5, notamment *querySelector*, le stockage DOM, la messagerie entre documents et *Canvas*. DOM IntelliSense est maintenant piloté par un simple fichier JavaScript, plutôt que par une définition de bibliothèque de types native. Cela facilite l’extension ou le remplacement.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>VSDOC des surcharges de signature

Des commentaires IntelliSense détaillés peuvent maintenant être déclarés pour des surcharges distinctes de fonctions JavaScript à l’aide de la nouvelle *&lt;signature&gt;* élément, comme illustré dans cet exemple :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Références implicites

Vous pouvez maintenant ajouter des fichiers JavaScript à une liste centrale qui sera implicitement incluse dans la liste des fichiers référencés par un fichier ou un bloc JavaScript donné, ce qui signifie que vous obtiendrez IntelliSense pour son contenu. Par exemple, vous pouvez ajouter des fichiers jQuery à la liste de fichiers centrale, et vous obtiendrez IntelliSense pour les fonctions jQuery dans n’importe quel bloc de fichier JavaScript, que vous l’ayez référencé explicitement (à l’aide de///&lt;référence/&gt;) ou non.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>éditeur CSS

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Réduire automatiquement la saisie semi-automatique des instructions

La liste IntelliSense pour CSS filtre désormais en fonction des propriétés CSS et des valeurs prises en charge par le schéma sélectionné.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense prend également en charge les recherches de cas de titre :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Mise en retrait hiérarchique

L’éditeur CSS utilise la mise en retrait pour afficher des règles hiérarchiques, ce qui vous donne une vue d’ensemble de la façon dont les règles en cascade sont organisées logiquement. Dans l’exemple suivant, le #list un sélecteur est un enfant en cascade de List et est par conséquent mis en retrait.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

L’exemple suivant montre un héritage plus complexe :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

La mise en retrait d’une règle est déterminée par ses règles parentes. La mise en retrait hiérarchique est activée par défaut, mais vous pouvez la désactiver dans la boîte de dialogue Options (outils, options dans la barre de menus) :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Prise en charge des piratages CSS

L’analyse de centaines de fichiers CSS réalistes montre que les pirates CSS sont très courants et que Visual Studio prend désormais en charge les plus couramment utilisés. Cette prise en charge comprend IntelliSense et la validation des attaques de la propriété Star (\*) et du trait de soulignement (\_) :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Les attaques par sélecteur standard sont également prises en charge afin que la mise en retrait hiérarchique soit conservée même lorsqu’elles sont appliquées. Un pirate de sélecteur classique utilisé pour cibler Internet Explorer 7 consiste à ajouter un sélecteur avec *\*: first-child + html*. L’utilisation de cette règle conservera la mise en retrait hiérarchique :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Schémas spécifiques au fournisseur (-moz-,-WebKit)

CSS3 introduit de nombreuses propriétés qui ont été implémentées par différents navigateurs à des moments différents. Cela obligeait auparavant les développeurs à coder pour des navigateurs spécifiques à l’aide de la syntaxe spécifique au fournisseur. Ces propriétés spécifiques au navigateur sont désormais incluses dans IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Prise en charge des commentaires et des marques de commentaire

Vous pouvez maintenant commenter et supprimer les commentaires des règles CSS en utilisant les mêmes raccourcis que ceux que vous utilisez dans l’éditeur de code (Ctrl + K, C pour commenter et Ctrl + K, sans commenter).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Sélecteur de couleurs

Dans les versions précédentes de Visual Studio, IntelliSense pour les attributs liés aux couleurs était constitué d’une liste déroulante de valeurs de couleur nommées. Cette liste a été remplacée par un sélecteur de couleurs complet.

Lorsque vous entrez une valeur de couleur, le sélecteur de couleurs s’affiche automatiquement et affiche la liste des couleurs précédemment utilisées, suivies d’une palette de couleurs par défaut. Vous pouvez sélectionner une couleur à l’aide de la souris ou du clavier.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

La liste peut être développée dans un sélecteur de couleurs complet. Le sélecteur vous permet de contrôler le canal alpha en convertissant automatiquement toute couleur en RVBA lorsque vous déplacez le curseur d’opacité :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Extraits de code

Les extraits de code dans l’éditeur CSS facilitent et accélèrent la création de styles multinavigateurs. De nombreuses propriétés CSS3 qui requièrent des paramètres spécifiques au navigateur ont été restaurées dans des extraits de code.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Les extraits de code CSS prennent en charge des scénarios avancés (comme les requêtes de média CSS3) en tapant l’arobase (@), qui affiche la liste IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Lorsque vous sélectionnez @media valeur et appuyez sur la touche Tab, l’éditeur CSS insère l’extrait de code suivant :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Comme avec les extraits de code, vous pouvez créer vos propres extraits CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Régions personnalisées

Les régions de code nommées, qui sont déjà disponibles dans l’éditeur de code, sont désormais disponibles pour la modification CSS. Cela vous permet de regrouper facilement des blocs de style associés.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Quand une région est réduite, elle affiche le nom de la région :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Inspecteur de page

Inspecteur de page est un outil qui rend une page Web (HTML, Web Forms, ASP.NET MVC ou pages Web) dans l’IDE de Visual Studio et vous permet d’examiner à la fois le code source et le résultat obtenu. Pour les pages ASP.NET, Inspecteur de page vous permet de déterminer le code côté serveur qui a produit le balisage HTML rendu dans le navigateur.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Pour plus d’informations sur Inspecteur de page, consultez les didacticiels suivants :

- Utilisation de Inspecteur de page dans [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Utilisation de Inspecteur de page dans [ASP.NET Web Forms](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publication

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Profils de publication

Dans Visual Studio 2010, la publication d’informations pour les projets d’application Web n’est pas stockée dans le contrôle de version et n’est pas conçue pour le partage avec d’autres utilisateurs. Dans la version Release candidate de Visual Studio 2012, le format du profil de publication a été modifié. Il a été créé en tant qu’artefact d’équipe et il est désormais facile de tirer parti des builds basées sur MSBuild. Les informations de configuration de build se trouvent dans la boîte de dialogue publier afin que vous puissiez facilement basculer les configurations de build avant la publication.

Les profils de publication sont stockés dans le dossier PublishProfiles. L’emplacement du dossier dépend du langage de programmation que vous utilisez :

- C#: Properties\PublishProfiles
- Visual Basic : mes Project\PublishProfiles

Chaque profil est un fichier MSBuild. Pendant la publication, ce fichier est importé dans le fichier MSBuild du projet. Dans Visual Studio 2010, si vous souhaitez apporter des modifications au processus de publication ou de package, vous devez placer vos personnalisations dans un fichier nommé **ProjectName**. WPP. targets. Cela est toujours pris en charge, mais vous pouvez désormais placer vos personnalisations dans le profil de publication lui-même. De cette façon, les personnalisations sont utilisées uniquement pour ce profil.

Vous pouvez maintenant également tirer parti des profils de publication de MSBuild. Pour ce faire, utilisez la commande suivante lorsque vous générez le projet :

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

La valeur Project. csproj est le chemin d’accès du projet, et ProfileName est le nom du profil à publier. Au lieu de transmettre le nom de profil pour la propriété *PublishProfile* , vous pouvez également transmettre le chemin d’accès complet au profil de publication.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>Précompilation et fusion ASP.NET

Pour les projets d’application Web, la version Release candidate de Visual Studio 2012 ajoute une option dans la page Propriétés du package/publication Web qui vous permet de précompiler et de fusionner le contenu de votre site lorsque vous publiez ou Empaquetez le projet. Pour afficher ces options, cliquez avec le bouton droit sur le projet dans Explorateur de solutions, choisissez Propriétés, puis choisissez la page de propriétés Package/Publication Web. L’illustration suivante montre l’option précompiler cette application avant la publication.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Quand cette option est sélectionnée, Visual Studio précompile l’application chaque fois que vous publiez ou Empaquetez l’application Web. Si vous souhaitez contrôler la façon dont le site est précompilé ou la manière dont les assemblys sont fusionnés, cliquez sur le bouton avancé pour configurer ces options.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Le serveur Web par défaut pour le test des projets Web dans Visual Studio est désormais IIS Express. Le Serveur Visual Studio Development est toujours une option pour le serveur Web local pendant le développement, mais IIS Express est désormais le serveur recommandé. L’utilisation de IIS Express dans Visual Studio 11 bêta est très similaire à son utilisation dans Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Exclusion de responsabilité

Ce document est une version préliminaire et peut être modifié substantiellement avant le lancement de la mise en production commerciale finale du logiciel qu’il décrit.

Les informations contenues dans ce document correspondent à la connaissance que Microsoft Corporation possède des problèmes abordés à la date de la publication. Microsoft devant répondre à des conditions de marché qui évoluent, ce document ne doit pas être considéré comme un engagement de sa part, et Microsoft ne peut pas garantir l’exactitude des informations présentées à la date de la publication.

Ce livre blanc est fourni à titre d'information uniquement. MICROSOFT NE FOURNIT AUCUNE GARANTIE, EXPRESSE, IMPLICITE OU LÉGALE, QUANT AUX INFORMATIONS CONTENUES DANS CE DOCUMENT.

L'utilisateur est tenu d'observer la réglementation relative aux droits d'auteur applicable dans son pays. Aucune partie de ce document ne peut être reproduite, stockée ou introduite dans un système de restitution, ou transmise à quelque fin ou par quelque moyen que ce soit (électronique, mécanique, photocopie, enregistrement ou autre) sans la permission expresse et écrite de Microsoft Corporation.

Microsoft peut détenir des brevets, avoir déposé des demandes d'enregistrement de brevets ou être titulaire de marques, droits d'auteur ou autres droits de propriété intellectuelle portant sur tout ou partie des éléments qui font l'objet du présent document. Sauf stipulation expresse contraire d'un contrat de licence écrit de Microsoft, la fourniture de ce document n'a pas pour effet de vous concéder une licence sur ces brevets, marques, droits d'auteur ou autres droits de propriété intellectuelle.

Sauf mention contraire, les exemples de sociétés, d’organisations, de produits, de noms de domaine, d’adresses de messagerie, de logos, de personnes, de lieux et d’événements mentionnés dans le présent document sont fictifs et ne sont pas associés à une société, une organisation, un produit, un nom de domaine, une adresse de messagerie réels une adresse, un logo, une personne, un lieu ou un événement est intentionnel ou doit être déduit.

© 2012 Microsoft Corporation. Tous droits réservés.

Microsoft et Windows sont soit des marques déposées de Microsoft Corporation, soit des marques de Microsoft Corporation aux États-Unis d'Amérique et/ou dans d'autres pays.

Les noms des sociétés et des produits mentionnés dans le présent document peuvent être des marques de leurs propriétaires respectifs.
