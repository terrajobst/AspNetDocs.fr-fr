---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Vue d’ensemble du projet Katana | Microsoft Docs
author: howarddierking
description: L’infrastructure ASP.NET a été utilisée depuis plus de dix ans et la plateforme a permis le développement de sites et de services Web en nombre. En tant que application Web...
ms.author: riande
ms.date: 08/30/2013
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 1f28db822930cdfd2ebf4cf9bb27d173f4aa4201
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617235"
---
# <a name="an-overview-of-project-katana"></a>Vue d’ensemble du projet Katana

par [Howard Dierking](https://github.com/howarddierking)

> L’infrastructure ASP.NET a été utilisée depuis plus de dix ans et la plateforme a permis le développement de sites et de services Web en nombre. Au fur et à mesure de l’évolution des stratégies de développement d’applications Web, l’infrastructure a pu évoluer à l’étape avec des technologies telles que ASP.NET MVC et API Web ASP.NET. Au fur et à mesure que le développement d’applications Web prend la prochaine étape dans le monde de cloud computing, Project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) fournit l’ensemble sous-jacent de composants aux applications ASP.net, ce qui leur permet d’être flexibles, portables, légers et de fournir de meilleures performances, en d’autres termes, Project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) Cloud optimise vos applications ASP.net.

## <a name="why-katana--why-now"></a>Pourquoi Katana – pourquoi maintenant ?

 Qu’il s’agisse d’une infrastructure de développement ou d’un produit d’utilisateur final, il est important de comprendre les motivations sous-jacentes pour la création du produit, et une partie de qui comprend la connaissance du produit pour lequel le produit a été créé. ASP.NET a été créé à l’origine avec deux clients à l’esprit.   
  
**Le premier groupe de clients était les développeurs ASP classiques.** À l’époque, ASP était l’une des principales technologies permettant de créer des applications et des sites Web dynamiques et orientés données en intertissage avec le balisage et le script côté serveur. Script côté serveur fourni par le runtime ASP avec un ensemble d’objets qui a abstrait les aspects essentiels du protocole HTTP et du serveur Web sous-jacents et a fourni un accès à des services supplémentaires tels que la gestion de l’état des sessions et des applications, le cache, etc. Bien qu’elles soient puissantes, les applications ASP classiques deviennent un défi à gérer, car leur taille et leur complexité ont augmenté. Cela était en grande partie dû à l’absence de structure dans les environnements de script couplés à la duplication de code résultant de l’entrelacement de code et de balisage. Pour tirer parti des avantages de l’ASP classique tout en répondant à certains de ses défis, ASP.NET a profité de l’organisation de code fournie par les langages orientés objet du .NET Framework tout en préservant le modèle de programmation côté serveur. auxquels les développeurs ASP classiques étaient habitués.

**Le deuxième groupe de clients cibles pour ASP.NET était Windows Business application Developers.** Contrairement aux développeurs ASP classiques, qui étaient habitués à écrire du balisage HTML et le code pour générer davantage de balisage HTML, les développeurs WinForms (comme les développeurs VB6 antérieurs) étaient habitués à une expérience au moment de la conception qui comprenait un canevas et un ensemble complet d’utilisateurs contrôles d’interface. La première version de ASP.NET, également appelée « Web Forms », a fourni une expérience de conception similaire avec un modèle d’événement côté serveur pour les composants d’interface utilisateur et un ensemble de fonctionnalités d’infrastructure (comme ViewState) pour créer une expérience de développeur transparente entre la programmation côté client et côté serveur. Web Forms clairement HID la nature sans état du Web dans un modèle d’événement avec état qui était familier aux développeurs WinForms.

### <a name="challenges-raised-by-the-historical-model"></a>Défis soulevés par le modèle d’historique

**Le résultat net était un modèle de programmation de développeur et de Runtime riche en fonctionnalités mature.** Toutefois, avec cette fonctionnalité, la richesse a été confrontée à deux défis notables. Tout d’abord, l’infrastructure était **monolithique**, avec des unités de fonctionnalité logiquement couplées étroitement dans le même assembly System. Web. dll (par exemple, les principaux objets http avec l’infrastructure Web Forms). Deuxièmement, ASP.NET a été inclus dans le cadre de la .NET Framework plus grande, ce qui signifiait que le **temps entre les versions était de l’ordre des années.** Il est donc difficile pour ASP.NET de suivre le rythme de toutes les modifications qui se produisent dans le développement Web en constante évolution. Enfin, System. Web. dll lui-même a été couplé de différentes manières à une option d’hébergement Web spécifique : Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Étapes évolutionnaires : ASP.NET MVC et API Web ASP.NET

Et de nombreuses modifications ont été apportées au développement Web ! Les applications Web étaient de plus en plus développées en tant que série de petits composants ciblés plutôt que de grands frameworks. Le nombre de composants ainsi que la fréquence à laquelle ils ont été libérés augmentaient à un rythme encore plus rapide. Il était clair que le fait de suivre le rythme avec le Web nécessiterait des frameworks plus petits, découplés et plus ciblés, plutôt que plus grands et plus riches en fonctionnalités. par conséquent, l' **équipe ASP.net a pris plusieurs mesures évolutives pour permettre à ASP.net d’être une famille de composants Web enfichables plutôt qu’un Framework unique**.

L’une des premières modifications était l’augmentation de la popularité du modèle de conception MVC (Model-View-Controller) bien connu grâce aux infrastructures de développement Web telles que Ruby on rails. Ce style de création d’applications Web permettait au développeur de mieux contrôler le balisage de son application tout en conservant la séparation des balises et de la logique métier, qui était l’un des points de vente initiaux pour ASP.NET. Pour répondre à la demande de ce type de développement d’applications Web, Microsoft a eu l’occasion de mieux se positionner dans le futur en **développant ASP.NET MVC hors bande** (sans l’inclure dans le .NET Framework). ASP.NET MVC a été publié en tant que téléchargement indépendant. L’équipe d’ingénierie a ainsi la possibilité de fournir des mises à jour bien plus souvent qu’auparavant.

Une autre évolution importante du développement d’applications Web était l’évolution des pages Web dynamiques générées par le serveur jusqu’au balisage initial statique avec des sections dynamiques de la page générées à partir de scripts côté client communiquant **avec les API Web principales via des demandes Ajax**. Ce changement architectural permettait de faire passer les API Web et le développement de l’infrastructure API Web ASP.NET. Comme dans le cas de ASP.NET MVC, la version de API Web ASP.NET a fourni une autre possibilité d’évolution du ASP.NET en tant que Framework plus modulaire. L’équipe d’ingénierie a tiré parti de l’opportunité et a **créé API Web ASP.net de telle sorte qu’elle n’avait aucune dépendance vis-à-vis des types Framework de base trouvés dans System. Web. dll**. Cela a permis deux choses : tout d’abord, cela signifiait que API Web ASP.NET pouvait évoluer d’une manière entièrement autonome (et qu’elle pouvait continuer à itérer rapidement, car elle est fournie via NuGet). Deuxièmement, étant donné qu’il n’y avait pas de dépendances externes avec System. Web. dll et, par conséquent, aucune dépendance à IIS, API Web ASP.NET incluait la possibilité de s’exécuter dans un hôte personnalisé (par exemple, une application console, un service Windows, etc.)

### <a name="the-future-a-nimble-framework"></a>Avenir : un Framework grande rapidité

En découplant les composants d’infrastructure les uns des autres, puis en les publiant sur NuGet, les frameworks pourraient maintenant **itérer plus de manière indépendante et plus rapidement**. En outre, la puissance et la flexibilité de la fonctionnalité d’auto-hébergement de l’API Web s’est avérée très intéressante pour les développeurs qui souhaitaient un **petit hôte léger** pour leurs services. Il s’est avéré si attrayant, en fait que les autres Framework souhaitaient également cette fonctionnalité, et cela présentait un nouveau défi dans la mesure où chaque infrastructure s’exécutait dans son propre processus hôte sur sa propre adresse de base et devait être gérée (démarrée, arrêtée, etc.) de manière indépendante. Une application Web moderne prend généralement en charge les services de fichiers statiques, la génération de pages dynamiques, l’API Web et les notifications push et en temps réel plus récentes. S’attendre à ce que chacun de ces services soit exécuté et géré indépendamment n’était tout simplement pas réaliste.

Ce qui était nécessaire était une abstraction d’hébergement unique qui permettrait à un développeur de composer une application à partir de différents composants et infrastructures, puis d’exécuter cette application sur un hôte de prise en charge.

## <a name="the-open-web-interface-for-net-owin"></a>Interface Web ouverte pour .NET (OWIN)

 Inspirées par les avantages atteints par le [rack](http://rack.github.io/) dans la communauté Ruby, plusieurs membres de la communauté .net ont établi la création d’une abstraction entre les serveurs Web et les composants de l’infrastructure. Deux objectifs de conception pour l’abstraction OWIN étaient qu’elle était simple et qu’elle prenait le moins de dépendances possibles sur d’autres types d’infrastructure. Ces deux objectifs permettent de garantir :

- Les nouveaux composants peuvent être plus facilement développés et consommés.
- Les applications peuvent être plus facilement portées entre les ordinateurs hôtes et les plateformes/systèmes d’exploitation.

L’abstraction obtenue se compose de deux éléments principaux. Le premier est le dictionnaire d’environnement. Cette structure de données est chargée de stocker l’ensemble de l’État nécessaire au traitement d’une requête et d’une réponse HTTP, ainsi que tout état de serveur approprié. Le dictionnaire d’environnement est défini comme suit :

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Un serveur Web compatible OWIN est chargé de remplir le dictionnaire d’environnement avec des données telles que les flux de corps et les collections d’en-têtes pour une requête et une réponse HTTP. Il incombe ensuite à l’application ou aux composants de l’infrastructure de remplir ou de mettre à jour le dictionnaire avec des valeurs supplémentaires et d’écrire dans le flux du corps de la réponse.

En plus de spécifier le type du dictionnaire d’environnement, la spécification OWIN définit une liste de paires clé-valeur de clé du dictionnaire. Par exemple, le tableau suivant présente les clés de dictionnaire requises pour une requête HTTP :

| Nom de clé | Description de la valeur |
| --- | --- |
| `"owin.RequestBody"` | Flux avec le corps de la demande, le cas échéant. Stream. null peut être utilisé comme espace réservé s’il n’y a pas de corps de requête. Consultez le corps de la [requête](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>` d’en-têtes de demande. Consultez [en-têtes](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | `string` contenant la méthode de requête HTTP de la demande (par exemple, `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | `string` contenant le chemin d’accès de la requête. Le chemin d’accès doit être relatif à la « racine » du délégué d’application. consultez [chemins d’accès](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | `string` contenant la partie du chemin d’accès de la demande correspondant à la « racine » du délégué d’application ; consultez [chemins d’accès](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | `string` contenant le nom et la version du protocole (par exemple, `"HTTP/1.0"` ou `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | `string` contenant le composant de chaîne de requête de l’URI de la requête HTTP, sans le «  ? » de début (par exemple, `"foo=bar&baz=quux"`). La valeur peut être une chaîne vide. |
| `"owin.RequestScheme"` | `string` contenant le modèle d’URI utilisé pour la demande (par exemple, `"http"`, `"https"`); consultez [schéma d’URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

Le deuxième élément clé de OWIN est le délégué d’application. Il s’agit d’une signature de fonction qui sert d’interface principale entre tous les composants d’une application OWIN. La définition du délégué d’application est la suivante :

`Func<IDictionary<string, object>, Task>;`

Le délégué d’application est alors simplement une implémentation du type délégué Func où la fonction accepte le dictionnaire d’environnement comme entrée et retourne une tâche. Cette conception a plusieurs implications pour les développeurs :

- Il existe un très petit nombre de dépendances de type requises pour écrire des composants OWIN. Cela augmente considérablement l’accessibilité de OWIN aux développeurs.
- La conception asynchrone permet à l’abstraction d’être efficace avec sa gestion des ressources informatiques, en particulier dans les opérations nécessitant de nombreuses e/s.
- Étant donné que le délégué d’application est une unité d’exécution atomique et que le dictionnaire d’environnement est traité en tant que paramètre sur le délégué, les composants OWIN peuvent être facilement chaînés ensemble pour créer des pipelines de traitement HTTP complexes.

Du point de vue de l’implémentation, OWIN est une spécification ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Son objectif n’est pas d’être le prochain Framework Web, mais plutôt une spécification sur la manière dont les infrastructures Web et les serveurs Web interagissent.

Si vous avez examiné [OWIN](http://owin.org/) ou [Katana](https://github.com/aspnet/AspNetKatana/wiki), vous avez peut-être également remarqué le [package NuGet OWIN](http://nuget.org/packages/Owin) et OWIN. dll. Cette bibliothèque contient une seule interface, [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), qui formalise et codifie la séquence de démarrage décrite dans la [section 4](http://owin.org/html/owin.html#4-application-startup) de la spécification OWIN. Bien qu’il ne soit pas nécessaire pour créer des serveurs OWIN, l’interface [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) fournit un point de référence concret, et elle est utilisée par les composants de projet Katana.

## <a name="project-katana"></a>Projet Katana

Tandis que la spécification [OWIN](http://owin.org/html/owin.html) et *OWIN. dll* sont tous les deux des efforts de la communauté et de la Communauté, le projet [Katana](https://github.com/aspnet/AspNetKatana/wiki) représente l’ensemble des composants OWIN qui, tout en restant Open source, sont créés et publiés par Microsoft. Ces composants incluent à la fois des composants d’infrastructure, tels que des hôtes et des serveurs, ainsi que des composants fonctionnels, tels que des composants d’authentification et des liaisons à des frameworks tels que [signalr](../../../signalr/index.md) et [API Web ASP.net](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Le projet a les trois objectifs de haut niveau suivants : 

- **Portable** : les composants doivent pouvoir être facilement remplacés par les nouveaux composants dès qu’ils sont disponibles. Cela comprend tous les types de composants, de l’infrastructure au serveur et à l’hôte. L’implication de cet objectif est que les frameworks tiers peuvent s’exécuter en toute transparence sur des serveurs Microsoft, alors que les infrastructures Microsoft peuvent potentiellement s’exécuter sur des serveurs et des hôtes tiers.
- **Modulaire/flexible**: contrairement à de nombreuses infrastructures qui incluent une myriade de fonctionnalités activées par défaut, les composants de projet Katana doivent être petits et concentrés, donnant ainsi le contrôle au développeur d’applications de déterminer les composants à utiliser dans son application.
- **Léger/performant/évolutif** : en séparant la notion traditionnelle d’une infrastructure en un ensemble de petits composants axés qui sont ajoutés explicitement par le développeur de l’application, une application Katana résultante peut consommer moins de ressources informatiques et, par conséquent, gérer davantage de charge, qu’avec d’autres types de serveurs et d’infrastructures. À mesure que les exigences de l’application demandent davantage de fonctionnalités à partir de l’infrastructure sous-jacente, elles peuvent être ajoutées au pipeline OWIN, mais cela doit être une décision explicite de la part du développeur de l’application. En outre, la substitution des composants de niveau inférieur signifie qu’à mesure qu’ils deviennent disponibles, de nouveaux serveurs hautes performances peuvent être introduits en toute transparence pour améliorer les performances des applications OWIN sans rompre ces applications.

## <a name="getting-started-with-katana-components"></a>Prise en main avec les composants Katana

Lorsqu’il a été introduit pour la première fois, l’un des aspects de l’infrastructure [node. js](http://nodejs.org/) qui a immédiatement attiré l’attention des personnes était la simplicité avec laquelle il était possible de créer et d’exécuter un serveur Web. Si les objectifs de Katana étaient encadrés par [node. js](http://nodejs.org/), vous pouvez les résumer en disant à Katana d’apporter un grand nombre des avantages de [node. js](http://nodejs.org/) (et de frameworks comme celui-ci) sans obliger le développeur à lever tout ce qu’il sait sur le développement d’applications Web ASP.net. Pour que cette instruction contienne la valeur true, la prise en main du projet Katana doit être aussi simple que la nature de [node. js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Création de « Hello World ! »

L’une des différences notables entre le développement JavaScript et .NET est la présence (ou l’absence) d’un compilateur. Par conséquent, le point de départ d’un serveur Katana simple est un projet Visual Studio. Toutefois, nous pouvons commencer avec les types de projet les plus minimes : l’application Web ASP.NET vide.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Ensuite, nous allons installer le package NuGet [Microsoft. Owin. Host. SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) dans le projet. Ce package fournit un serveur OWIN qui s’exécute dans le pipeline de demande ASP.NET. Il se trouve sur la [galerie NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) et peut être installé à l’aide de la boîte de dialogue du gestionnaire de package Visual Studio ou de la console du gestionnaire de package, avec la commande suivante :

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

L’installation du package `Microsoft.Owin.Host.SystemWeb` installe quelques packages supplémentaires en tant que dépendances. L’une de ces dépendances est `Microsoft.Owin`, une bibliothèque qui fournit plusieurs types et méthodes d’assistance pour le développement d’applications OWIN. Nous pouvons utiliser ces types pour écrire rapidement le serveur « Hello World » suivant.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Ce serveur Web très simple peut maintenant être exécuté à l’aide de la commande **F5** de Visual Studio et prend entièrement en charge le débogage.

## <a name="switching-hosts"></a>Basculement des ordinateurs hôtes

Par défaut, l’exemple « Hello World » précédent s’exécute dans le pipeline de requête ASP.NET, qui utilise System. Web dans le contexte d’IIS. Cela peut lui-même ajouter de la valeur exceptionnelle, car cela nous permet de tirer parti de la flexibilité et de la forme d’un pipeline OWIN avec les fonctionnalités de gestion et la maturité globale d’IIS. Toutefois, il peut arriver que les avantages fournis par IIS ne soient pas requis et que le désir soit pour un hôte plus petit et plus léger. Qu’est-ce qui est nécessaire, puis, pour exécuter notre simple serveur Web en dehors d’IIS et de System. Web ?

Pour illustrer l’objectif de la portabilité, le passage d’un hôte de serveur Web à un hôte de ligne de commande nécessite simplement l’ajout du nouveau serveur et des dépendances de l’hôte au dossier de sortie du projet, puis le démarrage de l’hôte. Dans cet exemple, nous allons héberger notre serveur Web dans un hôte Katana appelé `OwinHost.exe` et utilisera le serveur Katana HttpListener. Comme pour les autres composants Katana, ceux-ci sont acquis à partir de NuGet à l’aide de la commande suivante :

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

À partir de la ligne de commande, nous pouvons ensuite accéder au dossier racine du projet et exécuter simplement le `OwinHost.exe` (qui a été installé dans le dossier Tools de son package NuGet respectif). Par défaut, `OwinHost.exe` est configuré pour rechercher le serveur HttpListener et aucune configuration supplémentaire n’est nécessaire. Si vous naviguez dans un navigateur Web vers `http://localhost:5000/`, l’application s’exécute maintenant via la console.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Architecture Katana

 L’architecture du composant Katana divise une application en quatre couches logiques, comme illustré ci-dessous : *hôte, serveur, intergiciel* et *application*. L’architecture du composant est factorisée de manière à ce que les implémentations de ces couches puissent être facilement remplacées, dans de nombreux cas, sans nécessiter de recompilation de l’application.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Hôte

 L’hôte est responsable des tâches suivantes :

- Gestion du processus sous-jacent.
- Orchestration du flux de travail entraînant la sélection d’un serveur et la construction d’un pipeline OWIN par le biais duquel les demandes seront gérées.

  À l’heure actuelle, il existe 3 options d’hébergement principales pour les applications basées sur Katana :  
  
**IIS/ASP. net**: à l’aide des types HttpModule et HttpHandler standard, les pipelines OWIN peuvent s’exécuter sur IIS dans le cadre d’un workflow de demande ASP.net. La prise en charge de l’hébergement ASP.NET est activée en installant le package NuGet Microsoft. AspNet. Host. SystemWeb dans un projet d’application Web. En outre, étant donné qu’IIS agit à la fois comme un ordinateur hôte et un serveur, la distinction entre le serveur OWIN et l’hôte est contourée dans ce package NuGet, ce qui signifie que si vous utilisez l’hôte SystemWeb, un développeur ne peut pas substituer une autre implémentation de serveur.  
  
**Hôte personnalisé**: la suite de composants Katana offre à un développeur la possibilité d’héberger des applications dans son propre processus personnalisé, qu’il s’agisse d’une application console, d’un service Windows, etc. Cette fonctionnalité ressemble à la fonctionnalité d’auto-hébergement fournie par l’API Web. L’exemple suivant montre un hôte personnalisé de code d’API Web :  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

La configuration de l’auto-hébergement pour une application Katana est similaire :

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

L’une des différences notables entre l’API Web et les exemples d’auto-hébergement Katana est que le code de configuration de l’API Web est manquant dans l’exemple d’auto-hôte Katana. Pour permettre la portabilité et la composabilité, Katana sépare le code qui démarre le serveur du code qui configure le pipeline de traitement des requêtes. Le code qui configure l’API Web est contenu dans le démarrage de la classe, qui est également spécifié comme paramètre de type dans WebApplication. Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

La classe Startup sera abordée plus en détail plus loin dans cet article. Toutefois, le code requis pour démarrer un processus d’auto-hébergement Katana est très similaire à celui que vous utilisez actuellement dans API Web ASP.NET les applications auto-hébergées.

**OwinHost. exe**: bien qu’une partie souhaite écrire un processus personnalisé pour exécuter des applications Web Katana, beaucoup préfèrent simplement lancer un exécutable prédéfini qui peut démarrer un serveur et exécuter son application. Pour ce scénario, la suite de composants Katana comprend `OwinHost.exe`. Lorsqu’il est exécuté à partir du répertoire racine d’un projet, ce fichier exécutable démarre un serveur (il utilise par défaut le serveur HttpListener) et utilise des conventions pour rechercher et exécuter la classe de démarrage de l’utilisateur. Pour un contrôle plus granulaire, l’exécutable fournit un certain nombre de paramètres de ligne de commande supplémentaires.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Serveur

 Alors que l’hôte est responsable du démarrage et de la maintenance du processus dans lequel l’application s’exécute, la responsabilité du serveur est d’ouvrir un socket réseau, d’écouter les demandes et de les envoyer via le pipeline des composants OWIN spécifiés par l’utilisateur (comme vous peut-être déjà remarqué, ce pipeline est spécifié dans la classe Startup du développeur de l’application). Actuellement, le projet Katana comprend deux implémentations de serveur : 

- **Microsoft. Owin. Host. SystemWeb**: comme mentionné précédemment, IIS de concert avec le pipeline ASP.net agit à la fois comme un hôte et un serveur. Par conséquent, lorsque vous choisissez cette option d’hébergement, IIS gère les préoccupations au niveau de l’hôte, telles que l’activation de processus et écoute les requêtes HTTP. Pour les applications Web ASP.NET, il envoie ensuite les demandes dans le pipeline ASP.NET. L’hôte Katana SystemWeb inscrit un HttpModule ASP.NET et un gestionnaire HttpHandler pour intercepter les requêtes lorsqu’elles transitent par le pipeline HTTP et les envoient via le pipeline OWIN spécifié par l’utilisateur.
- **Microsoft. Owin. Host. HttpListener**: comme son nom l’indique, ce serveur Katana utilise la classe HttpListener de l' .NET Framework pour ouvrir un socket et envoyer des demandes dans un pipeline Owin spécifié par le développeur. Il s’agit actuellement de la sélection de serveur par défaut pour l’API d’auto-hébergement Katana et OwinHost. exe.

## <a name="middlewareframework"></a>Intergiciel/infrastructure

 Comme mentionné précédemment, lorsque le serveur accepte une demande émanant d’un client, il est chargé de le transmettre via un pipeline de composants OWIN, qui sont spécifiés par le code de démarrage du développeur. Ces composants de pipeline sont appelés intergiciel (middleware).  
 À un niveau de base, un composant middleware OWIN doit simplement implémenter le délégué d’application OWIN pour pouvoir être appelé.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Toutefois, afin de simplifier le développement et la composition des composants de l’intergiciel (middleware), Katana prend en charge un certain nombre de conventions et de types d’assistance pour les composants de l’intergiciel (middleware). La plus courante est la classe `OwinMiddleware`. Un composant d’intergiciel (middleware) personnalisé créé à l’aide de cette classe doit ressembler à ce qui suit : 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Cette classe dérive de `OwinMiddleware`, implémente un constructeur qui accepte une instance de l’intergiciel (middleware) suivant dans le pipeline comme l’un de ses arguments, puis la passe au constructeur de base. Les arguments supplémentaires utilisés pour configurer l’intergiciel (middleware) sont également déclarés en tant que paramètres de constructeur après le paramètre d’intergiciel (middleware) suivant.   
  
Lors de l’exécution, l’intergiciel (middleware) est exécuté via la méthode `Invoke` substituée. Cette méthode accepte un seul argument de type `OwinContext`. Cet objet de contexte est fourni par le `Microsoft.Owin` package NuGet décrit plus haut et fournit un accès fortement typé à la demande, à la réponse et au dictionnaire d’environnement, ainsi que quelques types d’assistance supplémentaires.   
  
La classe middleware peut être facilement ajoutée au pipeline OWIN dans le code de démarrage de l’application, comme suit :   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Étant donné que l’infrastructure Katana génère simplement un pipeline de composants d’intergiciel (middleware) OWIN et que les composants doivent simplement prendre en charge le délégué d’application pour participer au pipeline, les composants d’intergiciel (middleware) peuvent aller de la complexité des enregistreurs simples aux infrastructures entières telles que ASP.NET, l’API Web ou [signalr](../../../signalr/index.md). Par exemple, l’ajout de API Web ASP.NET au pipeline OWIN précédent nécessite l’ajout du code de démarrage suivant :

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

L’infrastructure Katana crée le pipeline des composants de l’intergiciel (middleware) en fonction de l’ordre dans lequel ils ont été ajoutés à l’objet IAppBuilder dans la méthode de configuration. Dans notre exemple, LoggerMiddleware peut gérer toutes les demandes qui passent par le pipeline, quelle que soit la façon dont ces demandes sont traitées au final. Cela permet de puissants scénarios dans lesquels un composant middleware (par exemple, un composant d’authentification) peut traiter les demandes pour un pipeline qui comprend plusieurs composants et frameworks (par exemple, API Web ASP.NET, Signalr et un serveur de fichiers statique).
 
## <a name="applications"></a>Applications

Comme l’illustrent les exemples précédents, OWIN et le projet Katana ne doivent pas être considérés comme un nouveau modèle de programmation d’applications, mais plutôt comme une abstraction pour découpler les modèles et les infrastructures de programmation d’application de l’infrastructure de serveur et d’hébergement. Par exemple, lors de la création d’applications d’API Web, l’infrastructure de développement continue à utiliser l’infrastructure API Web ASP.NET, que l’application s’exécute ou non dans un pipeline OWIN à l’aide de composants du projet Katana. Le seul endroit où le code lié à OWIN sera visible par le développeur de l’application sera le code de démarrage de l’application, où le développeur compose le pipeline OWIN. Dans le code de démarrage, le développeur inscrit une série d’instructions UseXx, généralement une pour chaque composant middleware qui traitera les demandes entrantes. Cette expérience aura le même effet que l’inscription des modules HTTP dans le monde actuel de System. Web. En règle générale, un intergiciel (middleware) d’infrastructure plus grand, tel que API Web ASP.NET ou [signalr](../../../signalr/index.md) , sera enregistré à la fin du pipeline. Les composants de l’intergiciel (middleware) de coupe transversale, tels que ceux destinés à l’authentification ou à la mise en cache, sont généralement inscrits au début du pipeline afin qu’ils traitent les demandes de tous les frameworks et composants inscrits plus tard dans le pipeline. Cette séparation des composants de l’intergiciel entre eux et des composants d’infrastructure sous-jacents permet aux composants d’évoluer à des vitesses différentes tout en veillant à ce que l’ensemble du système reste stable.

## <a name="components--nuget-packages"></a>Composants – packages NuGet

Comme de nombreuses bibliothèques et infrastructures actuelles, les composants du projet Katana sont fournis sous la forme d’un ensemble de packages NuGet. Pour la version à venir 2,0, le graphique de dépendance du package Katana se présente comme suit. (Cliquez sur l’image pour l’agrandir.)

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Presque tous les packages du projet Katana dépendent, directement ou indirectement, du package Owin. Vous vous souvenez peut-être qu’il s’agit du package qui contient l’interface IAppBuilder, qui fournit une implémentation concrète de la séquence de démarrage de l’application décrite dans la section 4 de la spécification OWIN. En outre, un grand nombre des packages dépendent de Microsoft. Owin, qui fournit un ensemble de types d’assistance pour l’utilisation des requêtes et des réponses HTTP. Le reste du package peut être classé comme des packages d’infrastructure d’hébergement (serveurs ou hôtes) ou un intergiciel (middleware). Les packages et les dépendances qui sont externes au projet Katana sont affichés en orange.

L’infrastructure d’hébergement pour Katana 2,0 comprend les serveurs SystemWeb et HttpListener, le package OwinHost pour l’exécution d’applications OWIN à l’aide de OwinHost. exe et le package Microsoft. Owin. Hosting pour les applications OWIN auto-hébergées dans un hôte personnalisé (par exemple application console, service Windows, etc.)

Pour Katana 2,0, les composants de l’intergiciel (middleware) sont principalement axés sur la fourniture de différents moyens d’authentification. Un composant middleware supplémentaire est fourni pour les Diagnostics, ce qui permet la prise en charge d’une page de démarrage et d’erreur. Comme OWIN s’étend à l’abstraction d’hébergement de facto, l’écosystème des composants de l’intergiciel (middleware), tous deux développés par Microsoft et des tiers, augmente également en nombre.

## <a name="conclusion"></a>Conclusion

 Dès le début, l’objectif du projet Katana n’a pas été de créer et donc de forcer les développeurs à apprendre encore une autre infrastructure Web. Au lieu de cela, l’objectif est de créer une abstraction pour permettre aux développeurs d’applications Web .NET de choisir plus que ce qui était auparavant possible. En divisant les couches logiques d’une pile d’applications Web standard en un ensemble de composants remplaçables, le projet Katana permet aux composants de la pile de s’améliorer à n’importe quel taux pour ces composants. En générant tous les composants autour de l’abstraction OWIN simple, Katana permet aux infrastructures et aux applications basées sur elles d’être portables sur un large éventail de serveurs et d’hôtes différents. En plaçant le développeur dans le contrôle de la pile, Katana s’assure que le développeur est le meilleur choix quant à la façon dont la pile Web est légère ou plus riche en fonctionnalités.  

## <a name="for-more-information-about-katana"></a>Pour plus d’informations sur Katana

- Le projet Katana sur GitHub : [https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/).
- Vidéo : [le projet Katana-OWIN pour ASP.net](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), par Howard Dierking.

## <a name="acknowledgements"></a>Remerciements

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT) ) Rick est un auteur de programmation senior pour Microsoft axé sur Azure et MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (Twitter [@shanselman](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (Twitter [@jongalloway](https://twitter.com/jongalloway) )
