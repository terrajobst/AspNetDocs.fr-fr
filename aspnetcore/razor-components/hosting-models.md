---
title: Composants de Razor modèles d’hébergement
author: guardrex
description: Comprendre les Blazor côté client et côté serveur ASP.NET Core Razor composants, modèles d’hébergement.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/hosting-models
ms.openlocfilehash: d1e0c472d7d10eeb4cef0da735cf703c98dd1645
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042436"
---
# <a name="razor-components-hosting-models"></a>Composants de Razor modèles d’hébergement

Par [Daniel Roth](https://github.com/danroth27)

Composants de Razor est une infrastructure web conçue pour s’exécuter côté client dans le navigateur sur un runtime .NET basé sur WebAssembly (*Blazor*) ou côté serveur dans ASP.NET Core (*ASP.NET Core Razor composants*). Quel que soit les modèles d’hébergement modèle, l’application et le composant *restent les mêmes*. Cet article décrit les modèles d’hébergement disponibles.

## <a name="client-side-hosting-model"></a>Modèle d’hébergement côté client

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Le modèle d’hébergement principal pour Blazor est en cours d’exécution côté client dans le navigateur. Dans ce modèle, l’application Blazor, ses dépendances et le runtime .NET sont téléchargés sur le navigateur. L’application est exécutée directement sur le thread d’interface utilisateur du navigateur. Toutes les mises à jour de l’interface utilisateur et la gestion des événements se produisent dans le même processus. Les ressources de l’application peuvent être déployés en tant que fichiers statiques à l’aide de tout type de serveur est par défaut (consultez [hôte et déployer](xref:host-and-deploy/razor-components/index)).

![Blazor côté client : L’application Blazor s’exécute sur un thread d’interface utilisateur dans le navigateur.](hosting-models/_static/client-side.png)

Pour créer une application de Blazor en utilisant le modèle d’hébergement côté client, utilisez le **Blazor** ou **Blazor (ASP.NET Core hébergées)** modèles de projet (`blazor` ou `blazorhosted` modèle lors de l’utilisation de la [dotnet nouvelle](/dotnet/core/tools/dotnet-new) à une invite de commande). Les éléments inclus *blazor.webassembly.js* descripteurs de script :

* Téléchargement du runtime .NET, l’application et ses dépendances.
* Initialisation du runtime pour exécuter l’application.

Le modèle d’hébergement côté client offre les avantages suivants. Côté client Blazor :

* N’a aucune dépendance de côté serveur .NET.
* A une interface utilisateur interactive riche.
* Entièrement tire parti des fonctionnalités et les ressources client.
* Déchargements fonctionnent à partir du serveur au client.
* Prend en charge les scénarios hors connexion.

Il existe des inconvénients à l’hébergement du côté client. Côté client Blazor :

* Restreint l’application aux fonctionnalités du navigateur.
* Nécessite le matériel capable de client et logiciels (par exemple, WebAssembly de prise en charge).
* A une plus grande taille de téléchargement et l’application plue le temps de chargement.
* A moins de maturité runtime .NET et les outils de prise en charge (par exemple, les limitations de prise en charge .NET Standard et le débogage).

Visual Studio inclut la **Blazor (ASP.NET Core hébergée)** modèle de projet pour la création d’une application Blazor qui s’exécute sur WebAssembly et qui est hébergée sur un serveur d’ASP.NET Core. L’application ASP.NET Core sert l’application Blazor aux clients, mais il est un processus distinct. L’application de Blazor côté client peut interagir avec le serveur sur le réseau à l’aide d’appels d’API Web ou les connexions SignalR.

> [!IMPORTANT]
> Si une application de Blazor côté client est pris en charge par une application ASP.NET Core hébergée comme une sous-application IIS, désactivez le Gestionnaire de Module ASP.NET Core hérité. Définir le chemin de base d’application dans l’application Blazor *index.html* fichier à l’alias IIS utilisé lors de la configuration de la sous-application dans IIS.
>
> Pour plus d’informations, consultez [chemin de base d’application](xref:host-and-deploy/razor-components/index#app-base-path).

## <a name="server-side-hosting-model"></a>Modèle d’hébergement côté serveur

Dans le modèle d’hébergement ASP.NET Core Razor composants côté serveur, l’application est exécutée sur le serveur à partir d’une application ASP.NET Core. Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés par le biais d’une connexion SignalR.

![ASP.NET Core Razor composants côté serveur : Le navigateur interagit avec l’application (hébergée à l’intérieur d’une application ASP.NET Core) sur le serveur via une connexion SignalR.](hosting-models/_static/server-side.png)

Pour créer une application de composants de Razor en utilisant le modèle d’hébergement côté serveur, utilisez le **Blazor (côté serveur dans ASP.NET Core)** modèle (`blazorserver` lors de l’utilisation [dotnet nouvelle](/dotnet/core/tools/dotnet-new) à une invite de commandes). Une application ASP.NET Core héberge l’application côté serveur de composants de Razor et configure le point de terminaison SignalR où les clients se connectent. L’application fait référence à l’application ASP.NET Core `Startup` classe à ajouter :

* Services de composants de Razor côté serveur.
* L’application pour le pipeline de traitement des requêtes.

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

Le *blazor.server.js* script&dagger; établit la connexion cliente. Il est responsable de l’application pour rendre persistantes et restaurer l’état de l’application en fonction des besoins (par exemple, en cas d’une connexion réseau perdues).

Le modèle d’hébergement côté serveur offre les avantages suivants :

* Vous permet d’écrire votre application entière avec .NET et C# à l’aide du modèle du composant.
* Fournit un aperçu interactif rich et évite les actualisations de la page inutiles.
* A une taille d’application considérablement inférieure à une application de Blazor côté client et se charge plus rapidement.
* Logique de composant permettre tirer pleinement parti des fonctionnalités de serveur, notamment l’utilisation des API compatibles de .NET Core.
* S’exécute sur .NET Core sur le serveur, .NET existantes des outils, telles que le débogage, fonctionne comme prévu.
* Fonctionne avec les clients légers (par exemple, les navigateurs qui ne prennent pas en charge le WebAssembly et ressources limité des appareils).

Il existe des inconvénients à l’hébergement du côté serveur :

* A une latence plus élevée : Chaque interaction utilisateur implique un tronçon réseau.
* N’offre aucune prise en charge hors connexion : Si la connexion du client échoue, l’application cesse de fonctionner.
* A réduit l’évolutivité : Le serveur doit gérer plusieurs connexions de client et gérer l’état du client.
* Nécessite un serveur d’ASP.NET Core pour servir de l’application. Déploiement sans serveur (par exemple, à partir d’un CDN) n’est pas possible.

&dagger;Le *blazor.server.js* script est publié dans le chemin d’accès suivant : *bin / {déboguer | Mise en production} / {FRAMEWORK cible} /publish/ {nom de l’APPLICATION}. Dist/application/_framework*.
