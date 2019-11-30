---
uid: visual-studio/overview/2013/release-notes
title: Notes de publication de ASP.NET et Web Tools pour Visual Studio 2013 | Microsoft Docs
author: microsoft
description: Ce document décrit la version de ASP.NET et Web Tools pour Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: d8af9c8e7ee1316a5eac90c5959d07c628154e09
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600439"
---
# <a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET et Web Tools pour Visual Studio 2013 - Notes de publication

par [Microsoft](https://github.com/microsoft)

> Ce document décrit la version de ASP.NET et Web Tools pour Visual Studio 2013.

## <a name="contents"></a>Sommaire

- [Notes d’installation](#TOC1)
- [Documentation](#TOC2)
- [Configuration logicielle requise](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nouvelles fonctionnalités de ASP.NET et Web Tools pour Visual Studio 2013

- [Un ASP.NET](#TOC6)
- [Nouvelle expérience de projet Web](#newproj)
- [Génération de modèles automatique ASP.NET](#scaffold)
- [Lien du navigateur](#browser-link)
- [Améliorations de l’éditeur Web de Visual Studio](#web-editor)
- [Azure App Service Web Apps prise en charge dans Visual Studio](#waws)
- [Améliorations de la publication Web](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET Web Forms](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [API Web ASP.NET 2](#TOC11)
- [Signaleur ASP.NET](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Composants Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [Interruption de l’application ASP.NET](#TOC15)
- [Problèmes connus et modifications avec rupture](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Notes d'installation

Les ASP.NET et Web Tools pour les Visual Studio 2013 sont regroupées dans le programme d’installation principal et peuvent être téléchargées [ici](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Documentation

Des didacticiels et d’autres informations sur les ASP.NET et Web Tools pour Visual Studio 2013 sont disponibles sur le [site Web ASP.net](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Configuration logicielle requise

ASP.NET et Web Tools nécessite Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nouvelles fonctionnalités de ASP.NET et Web Tools pour Visual Studio 2013

Les sections suivantes décrivent les fonctionnalités qui ont été introduites dans la version.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Un ASP.NET

Avec la publication de Visual Studio 2013, nous avons entrepris un pas à pas pour unifier l’expérience de l’utilisation des technologies ASP.NET, afin que vous puissiez facilement mélanger et correspondre à ceux que vous souhaitez. Par exemple, vous pouvez démarrer un projet à l’aide de MVC et facilement ajouter Web Forms pages au projet ultérieurement, ou générer des modèles d’API Web dans un projet Web Forms. L’un des ASP.NET consiste à vous aider en tant que développeur à faire les choses que vous aimez dans ASP.NET. Quelle que soit la technologie que vous choisissez, vous avez la certitude que vous créez sur l’infrastructure sous-jacente approuvée d’un ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nouvelle expérience de projet Web

Nous avons amélioré l’expérience de création de projets Web dans Visual Studio 2013. Dans la boîte de dialogue **nouveau projet Web ASP.net** , vous pouvez sélectionner le type de projet que vous souhaitez, configurer une combinaison de technologies (Web Forms, MVC, API Web), configurer les options d’authentification et ajouter un projet de test unitaire.

![Nouveau projet ASP.NET](release-notes/_static/image1.png)

La nouvelle boîte de dialogue vous permet de modifier les options d’authentification par défaut pour la plupart des modèles. Par exemple, lorsque vous créez un projet Web Forms ASP.NET, vous pouvez sélectionner l’une des options suivantes :

- Aucune authentification
- Comptes d’utilisateur individuels (appartenance ASP.NET ou connexion du fournisseur social)
- Comptes organisationnels (Active Directory dans une application Internet)
- Authentification Windows (Active Directory dans une application intranet)

![Options d’authentification](release-notes/_static/image2.png)

Pour plus d’informations sur le nouveau processus de création de projets Web, consultez [création de projets web ASP.net dans Visual Studio 2013](creating-web-projects-in-visual-studio.md). Pour plus d’informations sur les nouvelles options d’authentification, consultez [ASP.net Identity](#TOC8) plus loin dans ce document.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>Génération de modèles automatique ASP.NET

La génération de modèles automatique ASP.NET est une infrastructure de génération de code pour les applications Web ASP.NET. Il facilite l’ajout de code réutilisable à votre projet qui interagit avec un modèle de données.

Dans les versions précédentes de Visual Studio, la génération de modèles automatique était limitée aux projets ASP.NET MVC. Avec Visual Studio 2013, vous pouvez désormais utiliser la génération de modèles automatique pour tous les projets ASP.NET, y compris les Web Forms. Visual Studio 2013 ne prend pas actuellement en charge la génération de pages pour un projet Web Forms, mais vous pouvez toujours utiliser la génération de modèles automatique avec Web Forms en ajoutant des dépendances MVC au projet. La prise en charge de la génération de pages pour Web Forms sera ajoutée dans une prochaine mise à jour.

Lors de l’utilisation de la génération de modèles automatique, nous garantissons que toutes les dépendances requises sont installées dans le projet. Par exemple, si vous démarrez avec un projet ASP.NET Web Forms et que vous utilisez ensuite la génération de modèles automatique pour ajouter un contrôleur d’API Web, les références et packages NuGet requis sont ajoutés automatiquement à votre projet.

Pour ajouter la génération de modèles automatique MVC à un projet Web Forms, ajoutez un **nouvel élément de génération de modèles** automatique et sélectionnez **dépendances MVC 5** dans la fenêtre de dialogue. Il existe deux options pour la génération de modèles automatique MVC ; Minimale et complète. Si vous sélectionnez minimal, seuls les packages NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet. Si vous sélectionnez l’option complète, les dépendances minimales sont ajoutées, ainsi que les fichiers de contenu requis pour un projet MVC.

La prise en charge de la génération de modèles automatique des contrôleurs asynchrones utilise les nouvelles fonctionnalités Async de Entity Framework 6.

Pour plus d’informations et des didacticiels, consultez [vue d’ensemble](aspnet-scaffolding-overview.md)de la génération de modèles automatique ASP.net.

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Lien du navigateur – canal Signalr entre le navigateur et Visual Studio

La nouvelle fonctionnalité de [lien de navigateur](using-browser-link.md) vous permet de connecter plusieurs navigateurs à Visual Studio et de les actualiser en cliquant sur un bouton dans la barre d’outils. Vous pouvez connecter plusieurs navigateurs à votre site de développement, y compris les émulateurs mobiles, puis cliquer sur Actualiser pour actualiser tous les navigateurs en même temps. Le lien du navigateur expose également une API pour permettre aux développeurs d’écrire des extensions de lien de navigateur.

![](release-notes/_static/image3.png)

En permettant aux développeurs de tirer parti de l’API de lien de navigateur, il devient possible de créer des scénarios très avancés qui franchissent les limites entre Visual Studio et tout navigateur connecté. Web Essentials tire parti de l’API pour créer une expérience intégrée entre Visual Studio et les outils de développement du navigateur, en contrôlant à distance les émulateurs mobiles et beaucoup plus.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Améliorations de l’éditeur Web de Visual Studio

Visual Studio 2013 comprend un nouvel éditeur HTML pour les fichiers Razor et les fichiers HTML dans les applications Web. Le nouvel éditeur HTML fournit un seul schéma unifié basé sur HTML5. Il dispose d’une fonction d’accolade automatique, de l’interface utilisateur jQuery et de l’attribut AngularJS IntelliSense, du regroupement IntelliSense d’attribut, de l’ID et du nom de classe IntelliSense, ainsi que d’autres améliorations, notamment des performances, une mise en forme et des SmartTags.

La capture d’écran suivante illustre l’utilisation de l’attribut de démarrage IntelliSense dans l’éditeur HTML.

![IntelliSense dans l’éditeur HTML](release-notes/_static/image4.png)

Visual Studio 2013 est également fourni avec les éditeurs CoffeeScript et moins intégrés. L’éditeur le moins intègre toutes les fonctionnalités intéressantes de l’éditeur CSS et a des IntelliSense spécifiques pour les variables et les Mixins sur tous les documents de la chaîne de @import.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Azure App Service Web Apps prise en charge dans Visual Studio

Dans Visual Studio 2013 avec le kit de développement logiciel (SDK) Azure pour .NET 2,2, vous pouvez utiliser **Explorateur de serveurs** pour interagir directement avec vos applications Web distantes. Vous pouvez vous connecter à votre compte Azure, créer des applications Web, configurer des applications, afficher des journaux en temps réel, et bien plus encore. Disponible prochainement après la publication du kit de développement logiciel (SDK) 2,2, vous pourrez exécuter en mode débogage à distance dans Azure. La plupart des nouvelles fonctionnalités de Azure App Service Web Apps fonctionnent également dans Visual Studio 2012 quand vous installez la version actuelle du kit de développement logiciel (SDK) Azure pour .NET.

Pour plus d'informations, voir les ressources suivantes :

- [Créer une application Web ASP.NET dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Résoudre les problèmes d’une application web dans Azure App Service avec Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Améliorations de la publication Web

Visual Studio 2013 comprend des fonctionnalités de publication Web nouvelles et améliorées. Voici quelques-unes d’entre elles :

- [Automatisez facilement le chiffrement du fichier Web. config](https://go.microsoft.com/fwlink/?LinkId=325529). (Ce lien et les deux points suivants de la documentation sur MSDN qui peuvent ne pas être disponibles jusqu’à la fin de la journée, le 10/17.)
- Automatisez facilement la mise [hors connexion d’une application pendant le déploiement](https://go.microsoft.com/fwlink/?LinkId=325530).
- Configurez Web Deploy pour [utiliser la somme de contrôle du fichier au lieu de la date de dernière modification](https://go.microsoft.com/fwlink/?LinkId=325531) pour déterminer quels fichiers doivent être copiés sur le serveur.
- Publiez rapidement des fichiers sélectionnés individuels (y compris Web. config) quand vous utilisez les méthodes de publication de système de fichiers ou FTP, ainsi que les Web Deploy.

Pour plus d’informations sur le déploiement Web ASP.NET, consultez [le site ASP.net](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2,7 comprend un ensemble complet de nouvelles fonctionnalités qui sont décrites en détail dans les [notes de publication de nuget 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Cette version de NuGet supprime également la nécessité de fournir un consentement explicite pour la fonctionnalité de restauration de package de NuGet afin de télécharger des packages. Le consentement (et la case à cocher associée dans la boîte de dialogue des préférences de NuGet) est désormais accordé en installant NuGet. Désormais, la restauration de package fonctionne simplement par défaut.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Forms

### <a name="one-aspnet"></a>Un ASP.NET

Les modèles de projet Web Forms s’intègrent en toute transparence à la nouvelle expérience ASP.NET. Vous pouvez ajouter la prise en charge de MVC et de l’API Web à votre projet Web Forms, et vous pouvez configurer l’authentification à l’aide de l’Assistant Création d’un projet ASP.NET. Pour plus d’informations, consultez [création de projets Web ASP.net dans Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Les modèles de projet Web Forms prennent en charge la nouvelle infrastructure de ASP.NET Identity. En outre, les modèles prennent désormais en charge la création d’un projet Web Forms intranet. Pour plus d’informations, consultez [méthodes d’authentification](creating-web-projects-in-visual-studio.md#auth) dans **création de projets Web ASP.net dans Visual Studio 2013**.

### <a name="bootstrap"></a>Démarrage

Les modèles de Web Forms utilisent [bootstrap](http://twitter.github.io/bootstrap/) pour fournir une apparence élégante et réactive que vous pouvez facilement personnaliser. Pour plus d’informations, consultez [bootstrap dans les modèles de projet web Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Un ASP.NET

Les modèles de projet Web MVC s’intègrent en toute transparence à la nouvelle expérience ASP.NET. Vous pouvez personnaliser votre projet MVC et configurer l’authentification à l’aide de l’Assistant Création d’un projet ASP.NET. Vous trouverez un didacticiel d’introduction à ASP.NET MVC 5 sur [prise en main avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Pour plus d’informations sur la mise à niveau de projets MVC 4 vers MVC 5, consultez Guide pratique [pour mettre à niveau un projet ASP.NET MVC 4 et API Web vers ASP.NET MVC 5 et API Web 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Les modèles de projet MVC ont été mis à jour pour utiliser ASP.NET Identity pour l’authentification et la gestion des identités. Vous trouverez un didacticiel sur l’authentification Facebook et Google et la nouvelle API d’appartenance dans la [création d’une application ASP.NET MVC 5 avec Facebook et Google OAuth2 et](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) l’authentification OpenID, ainsi que la [création d’une application MVC ASP.net avec auth et SQL DB et le déploiement sur Azure App service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Démarrage

Le modèle de projet MVC a été mis à jour pour utiliser [bootstrap](http://getbootstrap.com/) pour fournir une apparence élégante et réactive que vous pouvez facilement personnaliser. Pour plus d’informations, consultez [bootstrap dans les modèles de projet web Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtres d’authentification

Les filtres d’authentification sont un nouveau type de filtre dans ASP.NET MVC qui s’exécute avant les filtres d’autorisation dans le pipeline MVC ASP.NET, et vous permet de spécifier la logique d’authentification par action, par contrôleur ou globalement pour tous les contrôleurs. Les filtres d’authentification traitent les informations d’identification dans la demande et fournissent un principal correspondant. Les filtres d’authentification peuvent également ajouter des problèmes d’authentification en réponse à des demandes non autorisées.

### <a name="filter-overrides"></a>Remplacements de filtre

Vous pouvez maintenant remplacer les filtres qui s’appliquent à une méthode d’action ou à un contrôleur donné en spécifiant un filtre de remplacement. Les filtres de remplacement spécifient un ensemble de types de filtres qui ne doivent pas être exécutés pour une étendue donnée (action ou contrôleur). Cela vous permet de configurer des filtres qui s’appliquent globalement, mais d’exclure certains filtres globaux de s’appliquer à des actions ou à des contrôleurs spécifiques.

### <a name="attribute-routing"></a>Routage par attributs

ASP.NET MVC prend désormais en charge le routage d’attributs, grâce à une contribution de Tim McCall, l’auteur de [http://attributerouting.net](http://attributerouting.net). Avec le routage d’attributs, vous pouvez spécifier vos itinéraires en annotant vos actions et vos contrôleurs.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>API web ASP.NET 2

### <a name="attribute-routing"></a>Routage par attributs

API Web ASP.NET prend désormais en charge le routage d’attributs, grâce à une contribution de Tim McCall, l’auteur de [http://attributerouting.net](http://attributerouting.net). Avec le routage d’attributs, vous pouvez spécifier vos itinéraires d’API Web en annotant vos actions et vos contrôleurs comme suit :

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Le routage des attributs vous donne davantage de contrôle sur les URI de votre API Web. Par exemple, vous pouvez facilement définir une hiérarchie de ressources à l’aide d’un contrôleur d’API unique :

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Le routage d’attributs fournit également une syntaxe pratique pour spécifier des paramètres facultatifs, des valeurs par défaut et des contraintes d’itinéraire :

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Pour plus d’informations sur le routage d’attributs, consultez [routage d’attributs dans l’API Web 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2,0

L’API Web et les modèles de projet d’application à page unique prennent désormais en charge l’autorisation à l’aide d’OAuth 2,0. OAuth 2,0 est une infrastructure permettant d’autoriser l’accès client aux ressources protégées. Il fonctionne pour un large éventail de clients, notamment des navigateurs et des appareils mobiles.

La prise en charge d’OAuth 2,0 est basée sur un nouveau middleware de sécurité fourni par les composants Microsoft OWIN pour l’authentification du porteur et l’implémentation du rôle de serveur d’autorisation. Les clients peuvent également être autorisés à l’aide d’un serveur d’autorisation d’organisation, tel que Azure Active Directory ou ADFS dans Windows Server 2012 R2.

### <a name="odata-improvements"></a>Améliorations d’OData

**Prise en charge de $select, $expand, $batch et $value**

API Web ASP.NET OData offre désormais une prise en charge complète des $select, $expand et $value. Vous pouvez également utiliser $batch pour le traitement par lot des demandes et le traitement des ensembles de modifications.

Les options $select et $expand vous permettent de modifier la forme des données retournées à partir d’un point de terminaison OData. Pour plus d’informations, consultez Présentation de la [prise en charge des $SELECT et des $Expand dans l’API Web OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Extensibilité améliorée**

Les formateurs OData sont désormais extensibles. Vous pouvez ajouter des métadonnées d’entrée Atom, prendre en charge les entrées de flux nommé et de lien multimédia, ajouter des annotations d’instance et personnaliser la manière dont les liens sont générés.

**Prise en charge sans type**

Vous pouvez maintenant créer des services OData sans avoir besoin de définir des types CLR pour vos types d’entité. Au lieu de cela, vos contrôleurs OData peuvent accepter ou retourner des instances de **IEdmObject**, qui sont les formateurs OData Serialize/désérialiser.

**Réutiliser un modèle existant**

Si vous disposez déjà d’un modèle EDM (Entity Data Model) existant, vous pouvez maintenant le réutiliser directement, au lieu d’en créer un nouveau. Par exemple, si vous utilisez Entity Framework, vous pouvez utiliser le modèle EDM généré par EF.

### <a name="request-batching"></a>Traitement par lot des demandes

Le traitement par lot des demandes combine plusieurs opérations en une seule requête HTTP Après, afin de réduire le trafic réseau et de fournir une interface utilisateur plus lisse et moins bavarde. API Web ASP.NET prend désormais en charge plusieurs stratégies pour le traitement par lot des demandes :

- Utilisez le point de terminaison $batch d’un service OData.
- Empaquetez plusieurs demandes dans une seule requête MIME à plusieurs parties.
- Utilisez un format de traitement par lot personnalisé.

Pour activer le traitement par lot des demandes, ajoutez simplement un itinéraire avec un gestionnaire de traitement par lot à votre configuration d’API Web :

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Vous pouvez également contrôler si les demandes sont exécutées de manière séquentielle ou dans n’importe quel ordre.

### <a name="portable-aspnet-web-api-client"></a>Client API Web ASP.NET portable

Vous pouvez maintenant utiliser le client API Web ASP.NET pour créer des bibliothèques de classes portables qui fonctionnent dans vos applications Windows Store et Windows Phone 8. Vous pouvez également créer des formateurs portables qui peuvent être partagés entre le client et le serveur.

### <a name="improved-testability"></a>Amélioration de la testabilité

L’API Web 2 facilite grandement le test unitaire de vos contrôleurs d’API. Il vous suffit d’instancier votre contrôleur d’API avec votre message et votre configuration de demande, puis d’appeler la méthode d’action que vous souhaitez tester. Il est également facile de simuler la classe **UrlHelper** , pour les méthodes d’action qui effectuent la génération de liens.

### <a name="ihttpactionresult"></a>IHttpActionResult

Vous pouvez désormais implémenter IHttpActionResult pour encapsuler le résultat de vos méthodes d’action d’API Web. Un IHttpActionResult retourné à partir d’une méthode d’action de l’API Web est exécuté par le Runtime API Web ASP.NET pour produire le message de réponse résultant. Une IHttpActionResult peut être retournée à partir de n’importe quelle action de l’API Web pour simplifier le test unitaire de l’implémentation de votre API Web. Pour des raisons pratiques, un certain nombre d’implémentations de IHttpActionResult sont fournies par le biais de, notamment les résultats pour retourner des codes d’État spécifiques, du contenu mis en forme ou des réponses négociées par le contenu.

### <a name="httprequestcontext"></a>HttpRequestContext

Le nouveau **HttpRequestContext** suit tout État lié à la demande, mais n’est pas immédiatement disponible à partir de la demande. Par exemple, vous pouvez utiliser **HttpRequestContext** pour obtenir des données d’itinéraire, le principal associé à la demande, le certificat client, **UrlHelper** et la racine du chemin d’accès virtuel. Vous pouvez facilement créer un **HttpRequestContext** à des fins de test unitaire.

Étant donné que le principal de la demande est transmis avec la requête au lieu de s’appuyer sur **thread. CurrentPrincipal**, le principal est désormais disponible pendant toute la durée de vie de la requête, alors qu’il se trouve dans le pipeline de l’API Web.

### <a name="cors"></a>CORS

Grâce à une autre contribution importante de Brock Allen, ASP.NET prend désormais entièrement en charge le partage des demandes Cross Origin (CORS).

La sécurité du navigateur empêche une page Web d’effectuer des demandes AJAX à un autre domaine. [Cors](http://www.w3.org/TR/cors/) est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine. À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes Cross-Origin tout en rejetant d’autres.

L’API Web 2 prend désormais en charge CORS, y compris la gestion automatique des demandes préliminaires. Pour plus d’informations, consultez la page [activation des demandes Cross-Origin dans API Web ASP.net](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtres d’authentification

Les filtres d’authentification sont un nouveau type de filtre dans les API Web ASP.NET qui s’exécutent avant les filtres d’autorisation dans le pipeline API Web ASP.NET et vous permettent de spécifier la logique d’authentification par action, par contrôleur ou globalement pour tous les contrôleurs. Les filtres d’authentification traitent les informations d’identification dans la demande et fournissent un principal correspondant. Les filtres d’authentification peuvent également ajouter des problèmes d’authentification en réponse à des demandes non autorisées.

### <a name="filter-overrides"></a>Remplacements de filtre

Vous pouvez maintenant remplacer les filtres qui s’appliquent à une méthode d’action ou à un contrôleur donné, en spécifiant un filtre de remplacement. Les filtres de remplacement spécifient un ensemble de types de filtres qui ne doivent pas s’exécuter pour une étendue donnée (action ou contrôleur). Cela vous permet d’ajouter des filtres globaux, mais de les exclure des actions ou des contrôleurs spécifiques.

### <a name="owin-integration"></a>Intégration de OWIN

API Web ASP.NET prend désormais entièrement en charge OWIN et peut être exécuté sur n’importe quel hôte compatible OWIN. Un **HostAuthenticationFilter** qui fournit l’intégration au système d’authentification OWIN est également inclus.

Avec l’intégration de OWIN, vous pouvez auto-héberger des API Web dans votre propre processus avec d’autres intergiciels OWIN, tels que Signalr. Pour plus d’informations, consultez [utiliser OWIN pour l’auto-hébergement API Web ASP.net](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET Signalr 2,0

Les sections suivantes décrivent les fonctionnalités de Signalr 2,0.

- [Basé sur OWIN](#builtonowin)
- [MapHubs et MapConnection sont maintenant MapSignalR](#MapSignalR)
- [Prise en charge inter-domaines](#crossdomain)
- [prise en charge d’iOS et d’Android via MonoTouch et MonoDroid](#mobile)
- [Client .NET portable](#portable)
- [Nouveau package auto-hôte](#selfhost)
- [Prise en charge des serveurs à compatibilité descendante](#backwardcompat)
- [Suppression de la prise en charge des serveurs pour .NET 4,0](#remove40)
- [Envoi d’un message à une liste de clients et de groupes](#messagelist)
- [Envoi d’un message à un utilisateur spécifique](#sendtouser)
- [Meilleure prise en charge de la gestion des erreurs](#errorhandling)
- [Tests unitaires plus faciles des hubs](#unittesting)
- [Gestion des erreurs JavaScript](#javascripterror)

Pour obtenir un exemple de la façon de mettre à niveau un projet 1. x existant vers Signalr 2,0, consultez [mise à niveau d’un projet signalr 1. x](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Basé sur OWIN

Signalr 2,0 est entièrement construit sur [OWIN (l’interface Web ouverte pour .net)](http://owin.org/). Cette modification rend le processus d’installation de Signalr bien plus cohérent entre les applications Signalr hébergées sur le Web et auto-hébergés, mais il a également nécessité un certain nombre de modifications d’API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs et MapConnection sont maintenant MapSignalR

Pour la compatibilité avec les normes OWIN, ces méthodes ont été renommées en `MapSignalR`. `MapSignalR` appelée sans paramètres mappera tous les hubs (comme le fait `MapHubs` dans la version 1. x); pour mapper des objets **PersistentConnection** individuels, spécifiez le type de connexion comme paramètre de type et l’extension d’URL pour la connexion comme premier argument.

La méthode `MapSignalR` est appelée dans une classe de démarrage Owin. Visual Studio 2013 contient un nouveau modèle pour une classe de démarrage Owin ; pour utiliser ce modèle, procédez comme suit :

1. Cliquez avec le bouton droit sur le projet
2. Sélectionner **Ajouter**, **nouvel élément...**
3. Sélectionnez **Owin Startup Class**. Nommez la nouvelle classe **Startup.cs**.

Dans une **application Web,** la classe de démarrage Owin contenant la méthode `MapSignalR` est ensuite ajoutée au processus de démarrage de Owin à l’aide d’une entrée dans le nœud Paramètres de l’application du fichier Web. config, comme indiqué ci-dessous.

Dans une **application auto-hébergée**, la classe Startup est passée comme paramètre de type de la méthode `WebApp.Start`.

**Mappage des hubs et des connexions dans Signalr 1. x (à partir du fichier d’application global dans une application Web) :** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Mappage des hubs et des connexions dans Signalr 2,0 (à partir d’un fichier de classe de démarrage Owin) :** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

Dans une **application auto-hébergée**, la classe Startup est passée comme paramètre de type pour la méthode `WebApp.Start`, comme indiqué ci-dessous.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Prise en charge inter-domaines

Dans Signalr 1. x, les demandes inter-domaines étaient contrôlées par un seul indicateur EnableCrossDomain. Cet indicateur contrôlait à la fois les demandes JSONP et CORS. Pour une plus grande flexibilité, toute la prise en charge de CORS a été supprimée du composant serveur de Signalr (les clients JavaScript continuent d’utiliser CORS normalement s’il est détecté que le navigateur le prend en charge) et le nouveau middleware OWIN a été mis à disposition pour prendre en charge ces scénarios.

Dans Signalr 2,0, si JSONP est requis sur le client (pour prendre en charge les requêtes inter-domaines dans les navigateurs plus anciens), vous devez l’activer explicitement en définissant `EnableJSONP` sur l’objet `HubConfiguration` à `true`, comme indiqué ci-dessous. JSONP est désactivé par défaut, car il est moins sécurisé que CORS.

Pour ajouter le nouveau middleware CORS dans Signalr 2,0, ajoutez la bibliothèque `Microsoft.Owin.Cors` à votre projet, puis appelez `UseCors` avant votre intergiciel Signalr, comme indiqué dans la section ci-dessous.

**Ajout de Microsoft. Owin. cors à votre projet**: pour installer cette bibliothèque, exécutez la commande suivante dans la console du gestionnaire de package :

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Cette commande ajoute la version 2.0.0 du package à votre projet.

**Appel de UseCors**

Les extraits de code suivants montrent comment implémenter des connexions inter-domaines dans Signalr 1. x et 2,0.

**Implémentation de requêtes inter-domaines dans Signalr 1. x (à partir du fichier d’application global)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implémentation de requêtes inter-domaines dans Signalr 2,0 (à C# partir d’un fichier de code)**

Le code suivant montre comment activer CORS ou JSONP dans un projet Signalr 2,0. Cet exemple de code utilise `Map` et `RunSignalR` au lieu de `MapSignalR`, de sorte que l’intergiciel (middleware) CORS s’exécute uniquement pour les demandes Signalr qui requièrent la prise en charge de CORS (plutôt que pour tout le trafic au niveau du chemin spécifié dans `MapSignalR`.) `Map` peut également être utilisé pour tout autre intergiciel qui doit s’exécuter pour un préfixe d’URL spécifique, plutôt que pour l'

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>prise en charge d’iOS et d’Android via MonoTouch et MonoDroid

Une prise en charge a été ajoutée pour les clients iOS et Android qui utilisent des composants monotactiles et MonoDroid à partir de la [bibliothèque Xamarin](https://xamarin.com/). Pour plus d’informations sur la façon de les utiliser, consultez [utilisation des composants Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Ces composants sont disponibles dans le [magasin Xamarin](https://store.xamarin.com/) quand la version RTW de signalr est disponible.

<a id="portable"></a># # # Client .NET portable

Pour faciliter le développement multiplateforme, les clients Silverlight, WinRT et Windows Phone ont été remplacés par un seul client mobile .NET qui prend en charge les plateformes suivantes :

- NET 4,5
- Silverlight 5
- WinRT (.NET pour les applications du Windows Store)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nouveau package auto-hôte

Il existe désormais un package NuGet qui facilite la prise en main de l’auto-hébergement Signalr (les applications Signalr hébergées dans un processus ou une autre application, au lieu d’être hébergées sur un serveur Web). Pour mettre à niveau un projet auto-hôte généré avec Signalr 1. x, supprimez le package Microsoft. AspNet. Signalr. Owin, puis ajoutez le package Microsoft. AspNet. Signalr. SelfHost. Pour plus d’informations sur la prise en main du package auto-hébergé, consultez [Didacticiel : signalr auto-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Prise en charge des serveurs à compatibilité descendante

Dans les versions précédentes de Signalr, les versions du package Signalr utilisées dans le client et le serveur devaient être identiques. Pour prendre en charge les applications clientes lourdes qui seraient difficiles à mettre à jour, Signalr 2,0 prend désormais en charge l’utilisation d’une version de serveur plus récente avec un client plus ancien. **Remarque : Signalr 2,0 ne prend pas en charge les serveurs créés avec des versions antérieures avec des clients plus récents.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Suppression de la prise en charge des serveurs pour .NET 4,0

Signalr 2,0 a supprimé la prise en charge de l’interopérabilité des serveurs avec .NET 4,0. .NET 4,5 doit être utilisé avec les serveurs Signalr 2,0. Il existe toujours un client .NET 4,0 pour Signalr 2,0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Envoi d’un message à une liste de clients et de groupes

Dans Signalr 2,0, il est possible d’envoyer un message à l’aide d’une liste d’ID de client et de groupe. Les extraits de code suivants montrent comment procéder.

**Envoi d’un message à une liste de clients et de groupes à l’aide de PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Envoi d’un message à une liste de clients et de groupes à l’aide de hubs**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Envoi d’un message à un utilisateur spécifique

Cette fonctionnalité permet aux utilisateurs de spécifier l’ID de l’utilisateur sur la base d’un IRequest via une nouvelle interface IUserIdProvider :

**Interface IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Par défaut, il y aura une implémentation qui utilise le IPrincipal.Identity.Name de l’utilisateur comme nom d’utilisateur.

Dans hubs, vous pouvez envoyer des messages à ces utilisateurs via une nouvelle API :

**Utilisation de l’API clients. User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Meilleure prise en charge de la gestion des erreurs

Les utilisateurs peuvent désormais lever des **HubException** à partir de n’importe quel appel de concentrateur. Le constructeur du **HubException** peut accepter un message de type chaîne et un objet des données d’erreur supplémentaires. Signalr effectue automatiquement la sérialisation de l’exception et l’envoie au client où elle sera utilisée pour rejeter/faire échouer l’appel de la méthode de concentrateur.

Le paramètre **afficher les exceptions détaillées du concentrateur** n’a aucun impact sur **HubException** renvoyé au client ou non ; Il est toujours envoyé.

**Code côté serveur illustrant l’envoi d’un HubException au client**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Code client JavaScript montrant comment répondre à un HubException envoyé à partir du serveur**

[!code-html[Main](release-notes/samples/sample16.html)]

**Code client .NET montrant comment répondre à un HubException envoyé à partir du serveur**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Tests unitaires plus faciles des hubs

Signalr 2,0 comprend une interface appelée `IHubCallerConnectionContext` sur des hubs qui facilite la création d’appels côté client fictifs. Les extraits de code suivants illustrent l’utilisation de cette interface avec des harnais de test populaires [xUnit.net](https://github.com/xunit/xunit) et [MOQ](https://code.google.com/p/moq/).

**Signaler des tests unitaires avec xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Signaler des tests unitaires avec MOQ**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Gestion des erreurs JavaScript

Dans Signalr 2,0, tous les rappels de gestion des erreurs JavaScript retournent des objets d’erreur JavaScript au lieu de chaînes brutes. Cela permet à Signalr de transmettre des informations plus riches à vos gestionnaires d’erreurs. Vous pouvez récupérer l’exception interne à partir de la propriété `source` de l’erreur.

**Code client JavaScript qui gère l’exception Start. Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Nouveau système d’appartenance ASP.NET

ASP.NET Identity est le nouveau système d’appartenance pour les applications ASP.NET. ASP.NET Identity facilite l’intégration des données de profil spécifiques à l’utilisateur avec les données d’application. ASP.NET Identity vous permet également de choisir le modèle de persistance pour les profils utilisateur dans votre application. Vous pouvez stocker les données dans une base de données SQL Server ou dans un autre magasin de données, y compris des magasins de données NoSQL tels que des tables de stockage Azure. Pour plus d’informations, consultez [comptes d’utilisateur individuels](creating-web-projects-in-visual-studio.md#indauth) dans **création de projets Web ASP.net dans Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Authentification basée sur les revendications

ASP.NET prend désormais en charge l’authentification basée sur les revendications, où l’identité de l’utilisateur est représentée sous la forme d’un ensemble de revendications à partir d’un émetteur approuvé. Les utilisateurs peuvent être authentifiés à l’aide d’un nom d’utilisateur et d’un mot de passe stockés dans une base de données d’application, ou à l’aide de fournisseurs d’identité sociale (par exemple : comptes Microsoft, Facebook, Google, Twitter) ou à l’aide de comptes professionnels via Azure Active Directory ou Services ADFS (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Intégration à Azure Active Directory et Windows Server Active Directory

Vous pouvez maintenant créer des projets ASP.NET qui utilisent Azure Active Directory ou Windows Server Active Directory (AD) pour l’authentification. Pour plus d’informations, consultez [comptes organisationnels](creating-web-projects-in-visual-studio.md#orgauth) pour la **création de projets Web ASP.net dans Visual Studio 2013**.

### <a name="owin-integration"></a>Intégration de OWIN

L’authentification ASP.NET est désormais basée sur un intergiciel (middleware) OWIN qui peut être utilisé sur n’importe quel hôte OWIN. Pour plus d’informations sur OWIN, consultez la section [composants Microsoft OWIN](#TOC7) suivants.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Composants Microsoft OWIN

[Open Web interface for .net](http://owin.org/) (OWIN) définit une abstraction entre les serveurs Web .net et les applications Web. OWIN découple l’application Web du serveur, ce qui rend les applications Web indépendantes des hôtes. Par exemple, vous pouvez héberger une application Web basée sur OWIN dans IIS ou auto-héberger dans un processus personnalisé.

Les modifications introduites dans les composants Microsoft OWIN (également connu sous le nom de projet Katana) incluent les nouveaux composants serveur et hôte, les nouvelles bibliothèques d’assistance et l’intergiciel et un intergiciel (middleware) d’authentification.

Pour plus d’informations sur OWIN et Katana, consultez [Nouveautés dans OWIN et Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Remarque : les applications [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) ne peuvent pas s’exécuter en mode classique IIS ; ils doivent être exécutés en mode intégré.**

**Remarque : les applications [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) doivent être exécutées avec une confiance totale.**

### <a name="new-servers-and-hosts"></a>Nouveaux serveurs et hôtes

Avec cette version, de nouveaux composants ont été ajoutés pour permettre les scénarios d’auto-hébergement. Ces composants incluent les packages NuGet suivants :

- **Microsoft. Owin. Host. HttpListener**. Fournit un serveur OWIN qui utilise **HttpListener** pour écouter les requêtes http et les diriger dans le pipeline OWIN.
- **Microsoft. Owin. Hosting** fournit une bibliothèque pour les développeurs qui souhaitent auto-héberger un pipeline Owin dans un processus personnalisé, tel qu’une application console ou un service Windows.
- **OwinHost**. Fournit un exécutable autonome qui encapsule `Microsoft.Owin.Hosting` et vous permet d’auto-héberger un pipeline OWIN sans avoir à écrire une application hôte personnalisée.

En outre, le package de `Microsoft.Owin.Host.SystemWeb` permet à présent à l’intergiciel (middleware) de fournir des indications au serveur **SystemWeb** , indiquant que l’intergiciel (middleware) doit être appelé au cours d’une étape de pipeline ASP.net spécifique. Cette fonctionnalité est particulièrement utile pour l’intergiciel (middleware) d’authentification, qui doit s’exécuter tôt dans le pipeline ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Bibliothèques d’assistance et intergiciel

Bien que vous puissiez écrire des composants OWIN en utilisant uniquement les définitions de fonction et de type de la spécification OWIN, le nouveau package `Microsoft.Owin` fournit un ensemble plus convivial d’abstractions. Ce package combine plusieurs packages précédents (par exemple, `Owin.Extensions`, `Owin.Types`) dans un modèle d’objet unique et bien structuré qui peut ensuite être facilement utilisé par d’autres composants OWIN. En fait, la majorité des composants Microsoft OWIN utilisent désormais ce package.

> [!NOTE]
> Les applications [OWIN](http://www.owin.org) ne peuvent pas s’exécuter en mode classique IIS ; ils doivent être exécutés en mode intégré.

> [!NOTE]
> Les applications [OWIN](http://www.owin.org) doivent être exécutées en confiance totale.

Cette version comprend également le package Microsoft. Owin. Diagnostics, qui comprend un intergiciel pour valider une application OWIN en cours d’exécution, ainsi qu’un intergiciel (middleware) de page d’erreur pour examiner les défaillances.

### <a name="authentication-components"></a>Composants d’authentification

Les composants d’authentification suivants sont disponibles.

- **Microsoft. Owin. Security. ActiveDirectory**. Active l’authentification à l’aide des services d’annuaire sur site ou basés sur le Cloud.
- **Microsoft. Owin. Security. Cookies** permet l’authentification à l’aide de cookies. Ce package a été précédemment nommé `Microsoft.Owin.Security.Forms`.
- **Microsoft. Owin. Security. Facebook** permet l’authentification à l’aide du service basé sur OAuth de Facebook.
- **Microsoft. Owin. Security. Google** permet l’authentification à l’aide du service OpenID de Google.
- **Microsoft. Owin. Security. JWT** permet l’authentification à l’aide de jetons JWT.
- **Microsoft. Owin. Security. MicrosoftAccount** permet l’authentification à l’aide de comptes Microsoft.
- **Microsoft. Owin. Security. OAuth**. Fournit un serveur d’autorisation OAuth, ainsi qu’un intergiciel (middleware) pour l’authentification des jetons de porteur.
- **Microsoft. Owin. Security. Twitter** active l’authentification à l’aide du service OAuth de Twitter.

Cette version inclut également le package `Microsoft.Owin.Cors`, qui contient des intergiciels (middleware) pour le traitement des requêtes HTTP Cross-Origin.

> [!NOTE]
> La prise en charge de la signature JWT a été supprimée dans la version finale de Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Pour obtenir la liste des nouvelles fonctionnalités et d’autres modifications apportées à Entity Framework 6, consultez [Entity Framework l’historique des versions](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 comprend les nouvelles fonctionnalités suivantes :

- Prise en charge de la modification de tabulation. Auparavant, la commande **mettre le document en forme** , mise en retrait automatique et mise en forme automatique dans Visual Studio ne fonctionnait pas correctement avec l’option conserver les **tabulations** . Cette modification corrige la mise en forme de Visual Studio pour le code Razor pour la mise en forme des tabulations.
- Prise en charge des règles de réécriture d’URL lors de la génération de liens.
- Suppression de l’attribut transparent de sécurité.
  > [!NOTE]
  > Il s’agit d’une modification avec rupture, qui rend Razor 3 incompatible avec MVC4 et les versions antérieures, tandis que Razor 2 est incompatible avec MVC5 ou les assemblys compilés sur MVC5.

Les problèmes associés à Razor 3 résolus dans Visual Studio 2013 à partir de versions préliminaires sont accessibles [ici](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Interruption de l’application ASP.NET

L’application ASP.NET suspend est une fonctionnalité révolutionnaire du .NET Framework 4.5.1 qui modifie radicalement l’expérience utilisateur et le modèle économique pour héberger un grand nombre de sites ASP.NET sur une seule machine. Pour plus d’informations, consultez [application ASP.net suspend-hébergement Web .net partagé réactif](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et modifications avec rupture

Cette section décrit les problèmes connus et les modifications importantes apportées au ASP.NET et Web Tools pour Visual Studio 2013.

### <a name="nuget"></a>NuGet

- La restauration d’un [nouveau package ne fonctionne pas sur mono quand vous utilisez le fichier sln](https://nuget.codeplex.com/workitem/3596) . elle sera corrigée dans un prochain téléchargement de NuGet. exe et la mise à jour du [package NuGet. CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) .
- La restauration d’un [nouveau package ne fonctionne pas avec les projets WiX](https://nuget.codeplex.com/workitem/3598) . elle sera corrigée dans un prochain téléchargement de NuGet. exe et la mise à jour du [package NuGet. CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) .
- [La restauration automatique des packages ne fonctionne pas pour les projets dans un dossier de solution](https://nuget.codeplex.com/workitem/3625) , elle sera corrigée dans NuGet 2,8.

### <a name="aspnet-web-api"></a>API Web ASP.NET

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` ne retourne pas `IQueryable<T>` toujours, car nous avons ajouté la prise en charge de `$select` et `$expand`.

    Nos exemples précédents pour `ODataQueryOptions<T>` convertissent toujours la valeur de retour de `ApplyTo` en `IQueryable<T>`. Cela fonctionnait plus tôt car les options de requête que nous avons prises en charge précédemment (`$filter`, `$orderby`, `$skip`, `$top`) ne modifient pas la forme de la requête. Maintenant que nous prenons en charge `$select` et `$expand` la valeur de retour de `ApplyTo` n’est pas `IQueryable<T>` Always.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Si vous utilisez l’exemple de code de la version antérieure, il continuera à fonctionner si le client n’envoie pas `$select` et `$expand`. Toutefois, si vous souhaitez prendre en charge `$select` et `$expand` vous devez modifier ce code.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request. URL ou RequestContext. URL a la valeur null pendant une requête de lot**

    Dans un scénario de traitement par lot, **UrlHelper** a la valeur NULL quand il est accessible à partir de **Request. URL** ou de **RequestContext. URL**.

    Ce problème est actuellement suivi ici : [BatchRequestContext. URL a la valeur null pour la demande de traitement par lot](http://aspnetwebstack.codeplex.com/workitem/1301).

    La solution de contournement de ce problème consiste à créer une nouvelle instance de **UrlHelper**, comme dans l’exemple suivant :

    **Création d’une nouvelle instance de UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Si vous utilisez MVC5 et OrgAuth, si vous avez des vues qui effectuent la validation AntiForgerToken, vous pouvez rencontrer l’erreur suivante lorsque vous publiez des données dans la vue :

    **Erreur**:

    *Erreur de serveur dans l’application'/'.*

    <em>Une revendication de type'<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'ou'<https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>'n’était pas présente sur le ClaimsIdentity fourni. Pour activer la prise en charge des jetons anti-contrefaçon avec l’authentification basée sur les revendications, vérifiez que le fournisseur de revendications configuré fournit ces deux revendications sur les instances ClaimsIdentity qu’il génère. Si le fournisseur de revendications configuré utilise à la place un type de revendication différent comme identificateur unique, il peut être configuré en définissant la propriété statique AntiForgeryConfig. UniqueClaimTypeIdentifier.</em>

    **Solution de contournement** :

    Ajoutez la ligne suivante dans global. asax pour la corriger :

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Ce problème sera résolu dans la prochaine version.
2. Après la mise à niveau d’une application MVC4 vers MVC5, générez la solution et lancez-la. L’erreur suivante doit s’afficher :

    Un System. Web. webpages. Razor. Configuration. HostSection ne peut pas être casté en [B] System. Web. webpages. Razor. Configuration. HostSection. Le type A provient de’System. Web. webpages. Razor, version = 2.0.0.0, culture = neutral, PublicKeyToken = 31bf3856ad364e35 'dans le contexte’default’à l’emplacement’C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35 \ System. Web. webpages. Razor. dll'. Le type B provient de’System. Web. webpages. Razor, version = 3.0.0.0, culture = neutral, PublicKeyToken = 31bf3856ad364e35 'dans le contexte’default’à l’emplacement’C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01 \ System. Web. webpages. Razor. dll'.

    Pour corriger l’erreur ci-dessus, ouvrez *tous* les fichiers Web. config (y compris ceux qui se trouvent dans le dossier views) dans votre projet et procédez comme suit :

   1. Mettez à jour toutes les occurrences de la version « 4.0.0.0 » de « System. Web. Mvc » en « 5.0.0.0 ».
   2. Mettez à jour toutes les occurrences de la version « 2.0.0.0 » de « System. Web. Helpers », &quot;System. Web. webpages&quot; et &quot;System. Web. webpages. Razor&quot; sur « 3.0.0.0 »

      Par exemple, une fois que vous avez effectué les modifications ci-dessus, les liaisons d’assembly doivent ressembler à ceci :

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Pour plus d’informations sur la mise à niveau de projets MVC 4 vers MVC 5, consultez Guide pratique [pour mettre à niveau un projet ASP.NET MVC 4 et API Web vers ASP.NET MVC 5 et API Web 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Lors de l’utilisation de la validation côté client avec la validation jQuery discrète, le message de validation est parfois incorrect pour un élément input HTML de type = 'Number'. L’erreur de validation pour une valeur obligatoire (« le champ d’expiration est requis ») s’affiche lorsqu’un nombre non valide est entré au lieu du message correct indiquant qu’un nombre valide est requis.

    Ce problème se trouve généralement dans le code de génération de modèles automatique d’un modèle avec une propriété de type entier sur les vues créer et modifier.

    Pour contourner ce problème, modifiez le programme d’assistance de l’éditeur à partir de :

    `@Html.EditorFor(person => person.Age)`

    À :

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 ne prend plus en charge la confiance partielle. Les projets qui sont liés aux binaires MVC ou WebAPI doivent supprimer l’attribut [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) et l’attribut [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) . La suppression de ces attributs éliminera les erreurs de compilation, telles que les suivantes.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Notez que, en tant qu’effet secondaire, vous ne pouvez pas utiliser les assemblys 4,0 et 5,0 dans la même application. Vous devez les mettre à jour sur 5,0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Le modèle SPA avec autorisation Facebook peut provoquer une instabilité dans IE alors que le site Web est hébergé dans la zone Intranet

Le modèle SPA fournit une connexion externe avec Facebook. Lorsque le projet créé avec le modèle s’exécute localement, la connexion peut entraîner le blocage d’IE.

Solution :

1. Héberger le site Web dans la zone Internet ; ni

2. Testez le scénario dans un navigateur autre qu’Internet Explorer.

### <a name="web-forms-scaffolding"></a>Génération de modèles automatique Web Forms

La génération de modèles automatique Web Forms a été supprimée de VS2013 et sera disponible dans une prochaine mise à jour de Visual Studio. Toutefois, vous pouvez toujours utiliser la génération de modèles automatique dans un projet Web Forms en ajoutant des dépendances MVC et en générant la génération de modèles automatique pour MVC. Votre projet contiendra une combinaison de Web Forms et MVC.

Pour ajouter MVC à votre projet Web Forms, ajoutez un nouvel élément de génération de modèles automatique et sélectionnez **dépendances MVC 5**. Sélectionnez minimale ou complète selon que vous avez besoin de tous les fichiers de contenu, tels que les scripts. Ajoutez ensuite un élément de génération de modèles automatique pour MVC, qui créera des vues et un contrôleur dans votre projet.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>Génération de modèles automatique MVC et API Web-erreur HTTP 404, introuvable

Si une erreur se produit lors de l’ajout d’un élément généré à l’échafaudage à un projet, il est possible que votre projet reste dans un état incohérent. Certaines des modifications apportées à la génération de modèles automatique seront annulées, mais d’autres modifications, telles que les packages NuGet installés, ne seront pas annulées. Si les modifications de configuration de routage sont annulées, les utilisateurs reçoivent une erreur HTTP 404 lors de la navigation vers des éléments de génération de modèles automatique.

Solution de contournement :

- Pour corriger cette erreur pour MVC, ajoutez un nouvel élément de génération de modèles automatique et sélectionnez dépendances MVC 5 (minimale ou complète). Ce processus ajoutera toutes les modifications requises à votre projet.
- Pour corriger cette erreur pour l’API Web :

  1. Ajoutez la classe WebApiConfig à votre projet.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Configurez WebApiConfig. Register dans l’application\_méthode Start dans global. asax comme suit :

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
