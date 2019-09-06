---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Résolution des erreurs HTTP 405 après la publication d’applications API Web | Microsoft Docs
author: rmcmurray
description: Ce didacticiel décrit comment résoudre les erreurs HTTP 405 après la publication d’une application API Web sur un serveur Web de production.
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 6da01ef5cd2faa3b8e76d1b0800e21a5cc1c61da
ms.sourcegitcommit: fe5c7512383a9b0a05d321ff10d3cca1611556f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70386456"
---
# <a name="troubleshooting-http-405-errors-after-publishing-web-api-applications"></a>Résolution des erreurs HTTP 405 après la publication d’applications d’API Web

> Ce didacticiel décrit comment résoudre les erreurs HTTP 405 après la publication d’une application API Web sur un serveur Web de production.
> 
> ## <a name="software-used-in-this-tutorial"></a>Logiciel utilisé dans ce didacticiel
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (version 7 ou ultérieure)
> - [API Web](../../index.md) 

Les applications d’API Web utilisent généralement plusieurs verbes HTTP courants : Recevez, PUBLIEz, mettez en place, SUPPRIMEz et parfois des CORRECTIFs. Cela dit, les développeurs peuvent s’exécuter dans des situations où ces verbes sont implémentés par un autre module IIS sur leur serveur de production, ce qui se traduit par une situation dans laquelle un contrôleur d’API Web qui fonctionne correctement dans Visual Studio ou sur un serveur de développement renverra un Erreur HTTP 405 lorsqu’elle est déployée sur un serveur de production. Heureusement, ce problème est facilement résolu, mais la résolution justifie une explication de la raison pour laquelle le problème se produit.

## <a name="what-causes-http-405-errors"></a>Causes des erreurs HTTP 405

La première étape pour apprendre à résoudre les erreurs HTTP 405 consiste à comprendre ce qu’une erreur HTTP 405 signifie réellement. Le document principal régissant le protocole http est [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), qui définit le code d’état HTTP 405 comme ***méthode non autorisée***et décrit ce code d’État comme une situation &quot;où la méthode spécifiée dans la ligne de demande n’est pas autorisée. pour la ressource identifiée par l’URI de demande.&quot; En d’autres termes, le verbe HTTP n’est pas autorisé pour l’URL spécifique demandée par un client HTTP.

Pour résumer, voici quelques-unes des méthodes HTTP les plus utilisées, telles que définies dans le document RFC 2616, RFC 4918 et RFC 5789 :

| Méthode HTTP | Description |
| --- | --- |
| **TÉLÉCHARGER** | Cette méthode est utilisée pour récupérer des données d’un URI, et il s’agit probablement de la méthode HTTP la plus utilisée. |
| **HEAD** | Cette méthode est très similaire à la méthode d’extraction, à la différence qu’elle n’extrait pas réellement les données de l’URI de la demande. elle récupère simplement l’état HTTP. |
| **POST** | Cette méthode est généralement utilisée pour envoyer de nouvelles données à l’URI. La publication est souvent utilisée pour envoyer des données de formulaire. |
| **PUT** | Cette méthode est généralement utilisée pour envoyer des données brutes à l’URI. PUT est souvent utilisé pour envoyer des données JSON ou XML à des applications API Web. |
| **DELETE** | Cette méthode permet de supprimer des données d’un URI. |
| **OPTIONS** | Cette méthode est généralement utilisée pour récupérer la liste des méthodes HTTP prises en charge pour un URI. |
| **COPIER LE DÉPLACEMENT** | Ces deux méthodes sont utilisées avec WebDAV et leur but est explicite. |
| **MKCOL** | Cette méthode est utilisée avec WebDAV et elle est utilisée pour créer une collection (par exemple, un répertoire) à l’URI spécifié. |
| **PROPFIND PROPPATCH** | Ces deux méthodes sont utilisées avec WebDAV et sont utilisées pour interroger ou définir les propriétés d’un URI. |
| **VERROUILLER LE DÉVERROUILLAGE** | Ces deux méthodes sont utilisées avec WebDAV et sont utilisées pour verrouiller/déverrouiller la ressource identifiée par l’URI de requête lors de la création. |
| **CORRECTIF** | Cette méthode est utilisée pour modifier une ressource HTTP existante. |

Quand l’une de ces méthodes HTTP est configurée pour une utilisation sur le serveur, le serveur répond avec l’état HTTP et d’autres données appropriées pour la demande. (Par exemple, une méthode d’extraction peut recevoir une réponse HTTP 200 ***OK*** , et une méthode put peut recevoir une réponse http 201 ***créée*** .)

Si la méthode HTTP n’est pas configurée pour une utilisation sur le serveur, le serveur répond avec une erreur HTTP 501 ***non implémentée*** .

Toutefois, lorsqu’une méthode HTTP est configurée pour être utilisée sur le serveur, mais qu’elle a été désactivée pour un URI donné, le serveur répond avec une erreur HTTP 405 ***non autorisée*** .

## <a name="example-http-405-error"></a>Exemple d’erreur HTTP 405

L’exemple suivant de requête et de réponse HTTP illustre une situation dans laquelle un client HTTP tente de placer une valeur dans une application API Web sur un serveur Web et le serveur renvoie une erreur HTTP indiquant que la méthode PUT n’est pas autorisée :

Requête HTTP :

[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]

Réponse HTTP :

[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]

Dans cet exemple, le client HTTP a envoyé une requête JSON valide à l’URL pour une application API Web sur un serveur Web, mais le serveur a renvoyé un message d’erreur HTTP 405 qui indique que la méthode PUT n’a pas été autorisée pour l’URL. En revanche, si l’URI de la demande ne correspondait pas à un itinéraire pour l’application API Web, le serveur renvoie une erreur HTTP 404 ***introuvable*** .

## <a name="resolve-http-405-errors"></a>Résoudre les erreurs HTTP 405

Il existe plusieurs raisons pour lesquelles un verbe HTTP spécifique peut ne pas être autorisé, mais il existe un scénario principal qui est la cause principale de cette erreur dans IIS : plusieurs gestionnaires sont définis pour le même verbe/méthode et l’un des gestionnaires bloque le gestionnaire attendu de traitement de la requête. À titre d’explication, IIS traite les gestionnaires de premier à dernier en fonction des entrées du gestionnaire de commandes dans les fichiers applicationHost. config et Web. config, où la première combinaison correspondante de Path, Verb, Resource, etc., sera utilisée pour gérer la requête.

L’exemple suivant est un extrait d’un fichier applicationHost. config pour un serveur IIS qui retournait une erreur HTTP 405 lors de l’utilisation de la méthode PUT pour envoyer des données à une application API Web. Dans cet extrait, plusieurs gestionnaires HTTP sont définis, et chaque gestionnaire a un ensemble différent de méthodes HTTP pour lequel il est configuré : la dernière entrée de la liste est le gestionnaire de contenu statique, qui est le gestionnaire par défaut utilisé une fois que les autres gestionnaires ont eu un chanc e pour examiner la requête :

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

Dans l’exemple ci-dessus, le gestionnaire WebDAV et le gestionnaire d’URL sans extension pour ASP.NET (utilisé pour l’API Web) sont clairement définis pour des listes distinctes de méthodes HTTP. Notez que le gestionnaire DLL ISAPI est configuré pour toutes les méthodes HTTP, bien que cette configuration ne provoque pas nécessairement une erreur. Toutefois, les paramètres de configuration de ce type doivent être pris en compte lors de la résolution des erreurs HTTP 405.

Dans l’exemple ci-dessus, le gestionnaire de DLL ISAPI n’était pas le problème. en fait, le problème n’a pas été défini dans le fichier applicationHost. config du serveur IIS : le problème est dû à une entrée qui a été effectuée dans le fichier Web. config lorsque l’application API Web a été créée dans Visual Studio. L’extrait suivant du fichier Web. config de l’application montre l’emplacement du problème :

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

Dans cet extrait, le gestionnaire d’URL sans extension pour ASP.NET est redéfini pour inclure des méthodes HTTP supplémentaires qui seront utilisées avec l’application API Web. Toutefois, étant donné qu’un ensemble similaire de méthodes HTTP est défini pour le gestionnaire WebDAV, un conflit se produit. Dans ce cas spécifique, le gestionnaire WebDAV est défini et chargé par IIS, même si WebDAV est désactivé pour le site Web qui comprend l’application API Web. Pendant le traitement d’une requête HTTP PUT, IIS appelle le module WebDAV puisqu’il est défini pour le verbe PUT. Lorsque le module WebDAV est appelé, il vérifie sa configuration et constate qu’il est désactivé. il renverra donc une erreur HTTP 405 ***non autorisée*** pour toute demande qui ressemble à une requête WebDAV. Pour résoudre ce problème, vous devez supprimer WebDAV de la liste des modules HTTP pour le site Web où votre application API Web est définie. L’exemple suivant montre ce à quoi cela ressemble :

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Ce scénario se produit souvent après la publication d’une application à partir d’un environnement de développement dans un environnement de production, ce qui se produit parce que la liste des gestionnaires/modules est différente entre vos environnements de développement et de production. Par exemple, si vous utilisez Visual Studio 2012 ou une version ultérieure pour développer une application API Web, IIS Express est le serveur Web par défaut à des fins de test. Ce serveur Web de développement est une version réduite de la fonctionnalité IIS complète fournie dans un produit serveur, et ce serveur Web de développement contient quelques modifications qui ont été ajoutées pour les scénarios de développement. Par exemple, le module WebDAV est souvent installé sur un serveur Web de production qui exécute la version complète d’IIS, bien qu’il ne soit peut-être pas en cours d’utilisation. La version de développement d’IIS, (IIS Express), installe le module WebDAV, mais les entrées du module WebDAV sont intentionnellement commentées, de sorte que le module WebDAV n’est jamais chargé sur IIS Express à moins que vous ne modifiiez spécifiquement votre configuration IIS Express paramètres pour ajouter la fonctionnalité WebDAV à votre installation IIS Express. Par conséquent, votre application Web peut fonctionner correctement sur votre ordinateur de développement, mais vous pouvez rencontrer des erreurs HTTP 405 lorsque vous publiez votre application API Web sur votre serveur Web de production.

## <a name="summary"></a>Récapitulatif

Les erreurs HTTP 405 sont générées lorsqu’une méthode HTTP n’est pas autorisée par un serveur Web pour une URL demandée. Cette condition s’affiche souvent lorsqu’un gestionnaire particulier a été défini pour un verbe spécifique, et que ce gestionnaire remplace le gestionnaire que vous prévoyez de traiter la demande.

Si vous rencontrez une situation où vous recevez un message d’erreur HTTP 501, ce qui signifie que la fonctionnalité spécifique n’a pas été implémentée sur le serveur, cela signifie souvent qu’aucun gestionnaire n’est défini dans vos paramètres IIS qui correspond à la requête HTTP, qui indique probablement qu’un problème n’a pas été installé correctement sur votre système ou a modifié vos paramètres IIS afin qu’aucun gestionnaire ne prenne en charge la méthode HTTP spécifique. Pour résoudre ce problème, vous devez réinstaller toute application qui tente d’utiliser une méthode HTTP pour laquelle elle n’a aucune définition de module ou de gestionnaire correspondante.
