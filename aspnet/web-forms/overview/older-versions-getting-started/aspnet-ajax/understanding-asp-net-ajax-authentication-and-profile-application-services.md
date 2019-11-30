---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Fonctionnement de l’authentification et du profil ASP.NET AJAX Services d’application | Microsoft Docs
author: scottcate
description: Le service d’authentification permet aux utilisateurs de fournir des informations d’identification pour recevoir un cookie d’authentification, et est le service de passerelle pour autoriser un utilisateur personnalisé...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: cab9acb1ffd75cca87f6c575a6abdd000235828e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74635678"
---
# <a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Présentation de l’authentification et des services d’application de profil d’ASP.NET AJAX

par [Scott Cate](https://github.com/scottcate)

[Télécharger PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Le service d’authentification permet aux utilisateurs de fournir des informations d’identification pour recevoir un cookie d’authentification, et est le service de passerelle permettant d’autoriser les profils utilisateur personnalisés fournis par ASP.NET. L’utilisation du service d’authentification ASP.NET AJAX est compatible avec l’authentification par formulaire ASP.NET standard. ainsi, les applications qui utilisent actuellement l’authentification par formulaire (par exemple, avec le contrôle de connexion) ne seront pas interrompues par la mise à niveau vers le service d’authentification AJAX.

## <a name="introduction"></a>Introduction

Dans le cadre de la .NET Framework 3,5, Microsoft propose une mise à niveau d’environnement dimensionnable. non seulement un nouvel environnement de développement est disponible, mais les nouvelles fonctionnalités LINQ (Language-Integrated Query) et d’autres améliorations du langage sont à venir. En outre, certaines fonctionnalités familières d’autres ensembles d’outils, notamment les extensions ASP.NET AJAX, sont incluses en tant que membres de première classe de la bibliothèque de classes de base .NET Framework. Ces extensions permettent de nombreuses nouvelles fonctionnalités clientes riches, y compris le rendu partiel des pages sans nécessiter une actualisation complète de la page, la possibilité d’accéder aux services Web via le script client (y compris l’API de profilage ASP.NET) et une API côté client étendue. conçu pour mettre en miroir un grand nombre des schémas de contrôle vus dans le jeu de contrôles côté serveur ASP.NET.

Ce livre blanc aborde l’implémentation et l’utilisation des services de profilage et d’authentification par formulaire ASP.NET tels qu’ils sont exposés par le Microsoft ASP.NET Extensions AJAX ExtensionsThe AJAX rendent très facile l’authentification par formulaire, telle qu’elle (et le service de profilage) est exposé via un script de proxy de service Web. Les extensions AJAX prennent également en charge l’authentification personnalisée via la classe AuthenticationServiceManager.

Ce livre blanc est basé sur la version bêta 2 de Visual Studio 2008 et le .NET Framework 3,5. Ce livre blanc suppose également que vous utilisez Visual Studio 2008 bêta 2, et non Visual Web Developer Express, et vous fournira des procédures pas à pas en fonction de l’interface utilisateur de Visual Studio. Certains exemples de code peuvent utiliser des modèles de projet non disponibles dans Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Profils et authentification*

Les profils de Microsoft ASP.NET et les services d’authentification sont fournis par le système d’authentification ASP.NET Forms, et sont des composants standard de ASP.NET. Les extensions ASP.NET AJAX fournissent un accès de script à ces services via des proxys de script, via un modèle assez simple sous l’espace de noms sys. services de la bibliothèque AJAX cliente.

Le service d’authentification permet aux utilisateurs de fournir des informations d’identification pour recevoir un cookie d’authentification, et est le service de passerelle permettant d’autoriser les profils utilisateur personnalisés fournis par ASP.NET. L’utilisation du service d’authentification ASP.NET AJAX est compatible avec l’authentification par formulaire ASP.NET standard. ainsi, les applications qui utilisent actuellement l’authentification par formulaire (par exemple, avec le contrôle de connexion) ne seront pas interrompues par la mise à niveau vers le service d’authentification AJAX.

Le service de profil permet l’intégration et le stockage automatiques des données utilisateur en fonction de l’appartenance fournie par le service d’authentification. Les données stockées sont spécifiées par le fichier Web. config, et les différents fournisseurs de services de profilage gèrent la gestion des données. Comme pour le service d’authentification, le service de profil AJAX est compatible avec le service de profil ASP.NET standard, de sorte que les pages qui incorporent actuellement des fonctionnalités du service de profil ASP.NET ne doivent pas être interrompues en incluant la prise en charge d’AJAX.

L’intégration de l’authentification ASP.NET et des services de profilage eux-mêmes dans une application est en dehors de la portée de ce livre blanc. Pour plus d’informations sur cette rubrique, consultez l’article de référence sur la bibliothèque MSDN Managing Users by Membership at [https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET comprend également un utilitaire permettant de configurer automatiquement l’appartenance à un SQL Server, qui est le fournisseur de services d’authentification par défaut pour l’appartenance à ASP.NET. Pour plus d’informations, consultez l’article ASP.NET SQL Server Registration Tool (ASPNET\_RegSql. exe) sur [https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Utilisation du service d’authentification ASP.NET AJAX*

Le service d’authentification ASP.NET AJAX doit être activé dans le fichier Web. config :

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Le service d’authentification requiert l’activation de l’authentification par formulaire ASP.NET et nécessite l’activation des cookies sur le navigateur client (un script ne peut pas activer une session sans cookie, car les sessions sans cookies nécessitent des paramètres d’URL).

Une fois que le service d’authentification AJAX est activé et configuré, le script client peut immédiatement tirer parti de l’objet sys. services. AuthenticationService. En premier lieu, le script client souhaite tirer parti de la méthode `login` et de la propriété `isLoggedIn`. Il existe plusieurs propriétés permettant de fournir des valeurs par défaut pour la méthode de connexion, qui peut accepter un grand nombre de paramètres.

*Membres sys. services. AuthenticationService*

*méthode de connexion :*

La méthode Login () commence une demande d’authentification des informations d’identification de l’utilisateur. Cette méthode est asynchrone et ne bloque pas l’exécution.

*Paramètres*

| **Nom du paramètre** | **C.-à-d** |
| --- | --- |
| userName | Requis. Nom d’utilisateur à authentifier. |
| mot de passe du . | Facultatif (valeur par défaut : null). Mot de passe de l'utilisateur. |
| isPersistent | Facultatif (false par défaut). Indique si le cookie d’authentification de l’utilisateur doit être rendu persistant entre les sessions. Si la valeur est false, l’utilisateur se déconnecte lorsque le navigateur est fermé ou que la session expire. |
| redirectUrl | Facultatif (valeur par défaut : null). URL à laquelle rediriger le navigateur en cas d’authentification réussie. Si ce paramètre a la valeur null ou est une chaîne vide, aucune redirection ne se produit. |
| customInfo | Facultatif (valeur par défaut : null). Ce paramètre est actuellement inutilisé et est réservé à une utilisation ultérieure. |
| loginCompletedCallback | Facultatif (valeur par défaut : null). Fonction à appeler lorsque la connexion s’est terminée avec succès. S’il est spécifié, ce paramètre remplace la propriété defaultLoginCompleted. |
| failedCallback | Facultatif (valeur par défaut : null). Fonction à appeler lorsque la connexion a échoué. S’il est spécifié, ce paramètre remplace la propriété defaultFailedCallback. |
| userContext | Facultatif (valeur par défaut : null). Données de contexte d’utilisateur personnalisées qui doivent être passées aux fonctions de rappel. |

*Valeur de retour :*

Cette fonction n’inclut pas de valeur de retour. Toutefois, un certain nombre de comportements sont inclus à la fin d’un appel à cette fonction :

- La page actuelle est soit actualisée, soit modifiée si le paramètre `redirectUrl` n’était ni null ni une chaîne vide.
- Toutefois, si le paramètre a la valeur null ou est une chaîne vide, le paramètre `loginCompletedCallback` ou la propriété `defaultLoginCompletedCallback` est appelé.
- Si l’appel au service Web échoue, le paramètre `failedCallback` de `defaultFailedCallback` propriété est appelé.

*méthode logout :*

La méthode Logout () supprime le cookie des informations d’identification et déconnecte l’utilisateur actuel de l’application Web.

*Paramètres*

| **Nom du paramètre** | **C.-à-d** |
| --- | --- |
| redirectUrl | Facultatif (valeur par défaut : null). URL à laquelle rediriger le navigateur en cas d’authentification réussie. Si ce paramètre a la valeur null ou est une chaîne vide, aucune redirection ne se produit. |
| logoutCompletedCallback | Facultatif (valeur par défaut : null). Fonction à appeler lorsque la déconnexion s’est terminée avec succès. S’il est spécifié, ce paramètre remplace la propriété defaultLogoutCompleted. |
| failedCallback | Facultatif (valeur par défaut : null). Fonction à appeler lorsque la connexion a échoué. S’il est spécifié, ce paramètre remplace la propriété defaultFailedCallback. |
| userContext | Facultatif (valeur par défaut : null). Données de contexte d’utilisateur personnalisées qui doivent être passées aux fonctions de rappel. |

*Valeur de retour :*

Cette fonction n’inclut pas de valeur de retour. Toutefois, un certain nombre de comportements sont inclus à la fin d’un appel à cette fonction :

- La page actuelle est soit actualisée, soit modifiée si le paramètre `redirectUrl` n’était ni null ni une chaîne vide.
- Toutefois, si le paramètre a la valeur null ou est une chaîne vide, le paramètre `logoutCompletedCallback` ou la propriété `defaultLogoutCompletedCallback` est appelé.
- Si l’appel au service Web échoue, le paramètre `failedCallback` de `defaultFailedCallback` propriété est appelé.

*defaultFailedCallback, propriété (obtient, Set) :*

Cette propriété spécifie une fonction qui doit être appelée si l’échec de la communication avec le service Web se produit. Elle doit recevoir un délégué (ou une référence de fonction).

La référence de fonction spécifiée par cette propriété doit avoir la signature suivante :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Paramètres*

| **Nom du paramètre** | **C.-à-d** |
| --- | --- |
| erreur | Spécifie les informations sur l’erreur. |
| userContext | Spécifie les informations de contexte utilisateur fournies lors de l’appel de la fonction de connexion ou de déconnexion. |
| methodName | Nom de la méthode appelante. |

*propriété defaultLoginCompletedCallback (obtient, Set) :*

Cette propriété spécifie une fonction qui doit être appelée lorsque l’appel du service Web de connexion est terminé. Elle doit recevoir un délégué (ou une référence de fonction).

La référence de fonction spécifiée par cette propriété doit avoir la signature suivante :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Paramètres*

| **Nom du paramètre** | **C.-à-d** |
| --- | --- |
| validCredentials | Spécifie si l’utilisateur a fourni des informations d’identification valides. `true` si l’utilisateur s’est connecté avec succès ; sinon `false`. |
| userContext | Spécifie les informations de contexte utilisateur fournies lors de l’appel de la fonction de connexion. |
| methodName | Nom de la méthode appelante. |

*propriété defaultLogoutCompletedCallback (obtient, Set) :*

Cette propriété spécifie une fonction qui doit être appelée lorsque l’appel du service Web de déconnexion est terminé. Elle doit recevoir un délégué (ou une référence de fonction).

La référence de fonction spécifiée par cette propriété doit avoir la signature suivante :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Paramètres*

| **Nom du paramètre** | **C.-à-d** |
| --- | --- |
| result | Ce paramètre sera toujours `null`; elle est réservée à une utilisation ultérieure. |
| userContext | Spécifie les informations de contexte utilisateur fournies lors de l’appel de la fonction de connexion. |
| methodName | Nom de la méthode appelante. |

*propriété isLoggedIn (obtient) :*

Cette propriété obtient l’état d’authentification actuel de l’utilisateur. elle est définie par l’objet ScriptManager lors d’une demande de page.

Cette propriété retourne `true` si l’utilisateur est actuellement connecté ; Sinon, elle retourne `false`.

*propriété Path (obtient, Set) :*

Cette propriété détermine par programmation l’emplacement du service Web d’authentification. Il peut être utilisé pour remplacer le fournisseur d’authentification par défaut, ainsi qu’un ensemble de façon déclarative dans la propriété Path du nœud enfant AuthenticationService du contrôle ScriptManager (pour plus d’informations, consultez Utilisation d’un fournisseur de services d’authentification personnalisé. rubrique ci-dessous).

Notez que l’emplacement du service d’authentification par défaut ne change pas. Toutefois, ASP.NET AJAX vous permet de spécifier l’emplacement d’un service Web qui fournit la même interface de classe que le proxy du service d’authentification ASP.NET AJAX.

Notez également que cette propriété ne doit pas être définie sur une valeur qui dirige la demande de script à partir du site actuel. Étant donné que l’application actuelle ne reçoit pas les informations d’identification d’authentification, elle serait inutile. en outre, la technologie sous-jacente à AJAX ne doit pas poster de demandes intersites et peut générer une exception de sécurité dans le navigateur client.

Cette propriété est un objet `String` représentant le chemin d’accès au service Web d’authentification.

*propriété Timeout (obtient, Set) :*

Cette propriété détermine la durée d’attente du service d’authentification avant que la demande de connexion ait échoué. Si le délai d’attente expire pendant qu’il attend qu’un appel se termine, le rappel d’échec de la demande est appelé, et l’appel ne se termine pas.

Cette propriété est un objet `Number` représentant le nombre de millisecondes d’attente des résultats du service d’authentification.

*Exemple de code : connexion au service d’authentification*

Le balisage suivant est un exemple de page ASP.NET avec un simple appel de script aux méthodes login et logout de la classe AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Accès aux données de profilage ASP.NET via AJAX

Le service de profilage ASP.NET est également exposé par le biais des extensions ASP.NET AJAX. Étant donné que le service de profilage ASP.NET fournit une API riche et granulaire permettant de stocker et de récupérer des données utilisateur, il peut s’agir d’un excellent outil de productivité.

Le service de profil doit être activé dans le fichier Web. config ; elle n’est pas par défaut. Pour ce faire, assurez-vous que l’élément enfant `profileService` a activé = true dans Web. config, et que vous avez spécifié les propriétés qui peuvent être lues ou écrites comme suit :

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Le service de profil doit également être configuré. Bien que la configuration du service de profilage ne se situe pas dans le cadre de ce livre blanc, il est utile de noter que les groupes tels que définis dans les paramètres de configuration de profil sont accessibles en tant que sous-propriétés du nom du groupe. Par exemple, avec la section de profil suivante spécifiée :

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Le script client peut accéder aux propriétés nom, adresse. ligne1, adresse. ligne2, adresse. City, adresse. State, Address. zip et BackgroundColor en tant que propriétés du champ Properties de la classe ProfileService.

Une fois le service de profilage AJAX configuré, il est immédiatement disponible dans les pages ; Toutefois, il doit être chargé une fois avant d’être utilisé.

*Membres sys. services. ProfileService*

*champ de propriétés :*

Le champ propriétés expose toutes les données de profil configurées en tant que propriétés enfants qui peuvent être référencées par la Convention de nom de point-opérateur. Les propriétés qui sont des enfants de groupes de propriétés sont appelées GroupName. NomPropriété. Dans l’exemple de configuration de profil présenté ci-dessus, vous pouvez utiliser l’identificateur suivant pour connaître l’état de l’utilisateur :

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*méthode Load :*

Charge une liste sélectionnée ou toutes les propriétés à partir du serveur.

*Paramètres*

| **Nom du paramètre** | **C.-à-d** |
| --- | --- |
| propertyNames | Facultatif (valeur par défaut : null). Propriétés à charger à partir du serveur. |
| loadCompletedCallback | Facultatif (valeur par défaut : null). Fonction à appeler lorsque le chargement est terminé. |
| failedCallback | Facultatif (valeur par défaut : null). Fonction à appeler si une erreur se produit. |
| userContext | Facultatif (valeur par défaut : null). Informations de contexte à passer à la fonction de rappel. |

La fonction Load n’a pas de valeur de retour. Si l’appel s’est terminé avec succès, il appellera le paramètre `loadCompletedCallback` ou la propriété `defaultLoadCompletedCallback`. Si l’appel a échoué ou que le délai d’attente a expiré, le paramètre `failedCallback` ou la propriété `defaultFailedCallback` sera appelé.

Si le paramètre `propertyNames` n’est pas fourni, toutes les propriétés configurées en lecture sont récupérées à partir du serveur.

*méthode Save :*

La méthode Save () enregistre la liste de propriétés spécifiée (ou toutes les propriétés) dans le profil ASP.NET de l’utilisateur.

*Paramètres*

| **Nom du paramètre** | **C.-à-d** |
| --- | --- |
| propertyNames | Facultatif (valeur par défaut : null). Propriétés à enregistrer sur le serveur. |
| saveCompletedCallback | Facultatif (valeur par défaut : null). Fonction à appeler lorsque l’enregistrement est terminé. |
| failedCallback | Facultatif (valeur par défaut : null). Fonction à appeler si une erreur se produit. |
| userContext | Facultatif (valeur par défaut : null). Informations de contexte à passer à la fonction de rappel. |

La fonction d’enregistrement n’a pas de valeur de retour. Si l’appel se termine correctement, il appellera le paramètre `saveCompletedCallback` ou la propriété `defaultSaveCompletedCallback`. Si l’appel a échoué ou que le délai d’attente a expiré, la propriété `failedCallback` ou `defaultFailedCallback` sera appelée.

Si le paramètre `propertyNames` a la valeur null, toutes les propriétés de profil sont envoyées au serveur et le serveur détermine les propriétés qui peuvent être enregistrées et celles qui ne le sont pas.

*defaultFailedCallback, propriété (obtient, Set) :*

Cette propriété spécifie une fonction qui doit être appelée si l’échec de la communication avec le service Web se produit. Elle doit recevoir un délégué (ou une référence de fonction).

La référence de fonction spécifiée par cette propriété doit avoir la signature suivante :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Paramètres*

| **Nom du paramètre** | **C.-à-d** |
| --- | --- |
| Erreur du | Spécifie les informations sur l’erreur. |
| userContext | Spécifie les informations de contexte utilisateur fournies lorsque la fonction de chargement ou d’enregistrement a été appelée. |
| methodName | Nom de la méthode appelante. |

*propriété defaultSaveCompleted (obtient, Set) :*

Cette propriété spécifie une fonction qui doit être appelée à la fin de l’enregistrement des données de profil de l’utilisateur. Elle doit recevoir un délégué (ou une référence de fonction).

La référence de fonction spécifiée par cette propriété doit avoir la signature suivante :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Paramètres*

| **Nom du paramètre** | **C.-à-d** |
| --- | --- |
| numPropsSaved | Spécifie le nombre de propriétés qui ont été enregistrées. |
| userContext | Spécifie les informations de contexte utilisateur fournies lorsque la fonction de chargement ou d’enregistrement a été appelée. |
| methodName | Nom de la méthode appelante. |

*propriété defaultLoadCompleted (obtient, Set) :*

Cette propriété spécifie une fonction qui doit être appelée à la fin du chargement des données de profil de l’utilisateur. Elle doit recevoir un délégué (ou une référence de fonction).

La référence de fonction spécifiée par cette propriété doit avoir la signature suivante :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Paramètres*

| **Nom du paramètre** | **C.-à-d** |
| --- | --- |
| numPropsLoaded | Spécifie le nombre de propriétés chargées. |
| userContext | Spécifie les informations de contexte utilisateur fournies lorsque la fonction de chargement ou d’enregistrement a été appelée. |
| methodName | Nom de la méthode appelante. |

*propriété Path (obtient, Set) :*

Cette propriété détermine par programmation l’emplacement du service Web de profil. Il peut être utilisé pour remplacer le fournisseur de services de profil par défaut, ainsi qu’un ensemble de façon déclarative dans la propriété Path du nœud enfant ProfileService du contrôle ScriptManager.

Notez que l’emplacement du service de profil par défaut ne change pas. Toutefois, ASP.NET AJAX vous permet de spécifier l’emplacement d’un service Web qui fournit la même interface de classe que le proxy du service d’authentification ASP.NET AJAX.

Notez également que cette propriété ne doit pas être définie sur une valeur qui dirige la demande de script à partir du site actuel. La technologie sous-jacente à AJAX ne doit pas poster de requêtes entre sites et peut générer une exception de sécurité dans le navigateur client.

Cette propriété est un objet `String` représentant le chemin d’accès au service Web de profil.

*propriété Timeout (obtient, Set) :*

Cette propriété détermine la durée d’attente du service de profil avant de supposer que la demande de chargement ou d’enregistrement a échoué. Si le délai d’attente expire pendant qu’il attend qu’un appel se termine, le rappel d’échec de la demande est appelé, et l’appel ne se termine pas.

Cette propriété est un objet `Number` représentant le nombre de millisecondes d’attente des résultats du service de profil.

*Exemple de code : chargement des données de profil au chargement de la page*

Le code suivant vérifie si un utilisateur est authentifié et, le cas échéant, charge la couleur d’arrière-plan préférée de l’utilisateur en tant que la page.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Utilisation d’un fournisseur de services d’authentification personnalisé*

Les extensions ASP.NET AJAX vous permettent de créer un fournisseur de services d’authentification de script personnalisé en exposant vos fonctionnalités par le biais d’un service Web personnalisé. Pour être utilisé, votre service Web doit exposer deux méthodes, `Login` et `Logout`; et ces méthodes doivent être spécifiées avec les mêmes signatures de méthode que le service Web d’authentification ASP.NET AJAX par défaut.

Une fois que vous avez créé le service Web personnalisé, vous devez spécifier son chemin d’accès, soit de manière déclarative sur votre page, par programme dans le code, soit via le script client.

*Pour définir le chemin d’accès de façon déclarative :*

Pour définir le chemin d’accès de façon déclarative, incluez l’enfant AuthenticationService de l’objet ScriptManager sur votre page ASP.NET :

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Pour définir le chemin d’accès dans le code :*

Pour définir le chemin d’accès par programme, spécifiez le chemin d’accès via l’instance de votre gestionnaire de script :

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Pour définir le chemin d’accès dans le script :*

Pour définir le chemin d’accès par programme dans le script, utilisez la propriété `path` de la classe AuthenticationService :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Exemple de service Web pour l’authentification personnalisée*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Récapitulatif

Les services ASP.NET, en particulier les services de profilage, d’appartenance et d’authentification, sont facilement exposés à JavaScript sur le navigateur client. Cela permet aux développeurs d’intégrer leur code côté client avec le mécanisme d’authentification en toute transparence, sans dépendre de contrôles tels que des UpdatePanel pour faire le gros du problème. Les données de profil peuvent également être protégées du client, en utilisant les paramètres de configuration Web. aucune donnée n’est disponible par défaut, et les développeurs doivent s’abonner à des propriétés de profil.

En outre, en créant des implémentations de service Web simplifiées avec des signatures de méthode équivalentes, les développeurs peuvent créer des fournisseurs de scripts personnalisés pour ces services ASP.NET intrinsèques. La prise en charge de ces techniques simplifie le développement d’applications clientes riches, tout en offrant aux développeurs un large éventail de flexibilité pour répondre à des besoins spécifiques.

## <a name="bio"></a>*Équivalence*

Scott Cate travaille avec les technologies Web de Microsoft depuis 1997 et est le Président de myKB.com ([www.myKB.com](http://www.myKB.com)), où il se spécialise dans l’écriture d’applications ASP.net axées sur les solutions logicielles de la base de connaissances. Scott peut être contacté par e-mail à [scott.cate@myKB.com](mailto:scott.cate@myKB.com) ou son blog à l’adresse [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Précédent](understanding-asp-net-ajax-updatepanel-triggers.md)
> [Suivant](understanding-asp-net-ajax-localization.md)
