---
uid: mvc/mvc3
title: ASP.NET MVC
author: rick-anderson
description: (comprend la mise à jour des outils d’avril 2011) ASP.NET MVC 3 est une infrastructure permettant de créer des applications Web évolutives basées sur des normes à l’aide d’un modèle de conception bien établi...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 421a06c89d4dbcb05d4080033813cc6558b7c698
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616409"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC

> *(comprend la mise à jour des outils d’avril 2011)*
> 
> ASP.NET MVC 3 est une infrastructure permettant de créer des applications Web évolutives et basées sur des normes à l’aide de modèles de conception bien établis et de la puissance de ASP.NET et de la .NET Framework.
> 
> Installé côte à côte avec ASP.NET MVC 2, commencez à l’utiliser dès aujourd’hui !
> 
> Téléchargez le [programme d’installation ici](https://go.microsoft.com/fwlink/?LinkID=208140)

## <a name="top-features"></a>Principales fonctionnalités

- Système de génération de modèles automatique intégré extensible via NuGet
- Modèles de projet HTML 5 activés
- Affichages expressifs, y compris le nouveau moteur d’affichage Razor
- Raccordements puissants avec l’injection de dépendances et les filtres d’action globaux
- Prise en charge enrichie de JavaScript avec JavaScript discrète, validation jQuery et liaison JSON
- *Lire la liste complète des fonctionnalités [ci-dessous](#overview)*

## <a name="top-links"></a>Liens principaux

Nouveautés de ASP.NET MVC 3

- Phil Haack : [ASP.NET MVC 3](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx) a été publié
- Scott Hanselman : [ASP.net MvC3, WebMatrix, NuGet, IIS Express et verger commercialisés-la version Web de janvier de Microsoft en contexte](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie : [annonce de la publication de ASP.NET MVC 3, IIS Express, SQL ce 4, infrastructure de batterie de serveurs Web, verger, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Notes de publication pour ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Installation et aide

- Installer ASP.NET MVC 3 à l’aide du [Web Platform Installer (recommandé)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Installer ASP.NET MVC 3 à l’aide du [fichier exécutable du programme d’installation](https://go.microsoft.com/fwlink/?LinkID=208140)
- Installer [ASP.NET MVC 3 pour Visual Studio 11 developer preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Lisez le [didacticiel Intro to ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Obtenir de l’aide et discuter de ASP.NET MVC 3 dans les [Forums](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 Overview

ASP.NET MVC 3 s’appuie sur ASP.NET MVC 1 et 2, en ajoutant de nouvelles fonctionnalités qui simplifient votre code et permettent une extensibilité plus poussée. Cette rubrique fournit une vue d’ensemble des nouvelles fonctionnalités incluses dans cette version, organisées dans les sections suivantes :

- [Génération de modèles automatique avec intégration MvcScaffold](#BM_MvcScaffolding)
- [Modèles de projet HTML 5 activés](#BM_HTML5)
- [Moteur d’affichage Razor](#BM_TheRazorViewEngine)
- [Prise en charge de plusieurs moteurs d’affichage](#BM_Support_for_Multiple_View_Engines)
- [Améliorations du contrôleur](#BM_Controller_Improvements)
- [JavaScript et Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Améliorations de la validation de modèle](#BM_Model_Validation_Improvements)
- [Améliorations de l’injection de dépendances](#BM_Dependency_Injection_Improvements)
- [Autres nouvelles fonctionnalités](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Génération de modèles automatique avec intégration MvcScaffold

Le nouveau système de génération de modèles automatique facilite la récupération et l’utilisation productive si vous êtes entièrement nouveau dans l’infrastructure, et pour automatiser les tâches de développement courantes si vous savez déjà ce que vous êtes en train de faire.

Cela est pris en charge par le nouveau package de *génération de modèles* automatique NuGet appelé **MvcScaffolding**. Le terme « génération de modèles automatique » est utilisé par de nombreuses technologies logicielles pour signifier « générer rapidement une structure de base de vos logiciels que vous pouvez ensuite modifier et personnaliser ». Le package de génération de modèles automatique que nous créons pour ASP.NET MVC est très utile dans plusieurs scénarios :

- **Si vous vous familiarisez avec ASP.NET MVC pour la première fois**, il vous offre un moyen rapide d’obtenir un code de travail utile, que vous pouvez ensuite modifier et adapter en fonction de vos besoins. Elle vous fait gagner du trauma de regarder une page vierge et n’a aucune idée où commencer !
- **Si vous savez bien ASP.NET MVC et que vous explorez maintenant une nouvelle technologie de module complémentaire** , telle qu’un mappeur objet-relationnel, un moteur d’affichage, une bibliothèque de test, etc., parce que le créateur de cette technologie a peut-être également créé un package de génération de modèles automatique pour celui-ci.
- **Si votre travail implique de créer de façon répétée des classes ou des fichiers similaires**, parce que vous pouvez créer des modèles de modèle personnalisés qui génèrent des contextes de test, des scripts de déploiement ou tout autre dont vous avez besoin. Tous les membres de votre équipe peuvent également utiliser vos générateurs de modèles personnalisés.

Les autres fonctionnalités de MvcScaffolding sont les suivantes :

- Prise en C# charge des projets et VB
- Prise en charge des moteurs d’affichage Razor et ASPX
- Prend en charge la génération de modèles automatique dans les zones MVC ASP.NET et l’utilisation des présentations/formes de vue personnalisées
- Vous pouvez facilement personnaliser la sortie en modifiant les modèles T4
- Vous pouvez ajouter de nouvelles échafaudages entièrement nouveaux à l’aide de la logique PowerShell personnalisée et des modèles T4 personnalisés. Celles-ci (ainsi que les paramètres personnalisés que vous leur avez attribués) s’affichent automatiquement dans la liste de saisie semi-automatique de l’onglet de la console.
- Vous pouvez obtenir des packages NuGet contenant des modèles de modèle supplémentaires pour différentes technologies (par exemple, il existe une preuve de concept pour LINQ to SQL maintenant) et les combiner et les mettre en correspondance ensemble

La mise à jour des outils ASP.NET MVC 3 offre une excellente prise en charge de Visual Studio pour ce système de génération de modèles automatique, par exemple :

- La boîte de dialogue Ajouter un contrôleur prend désormais en charge la génération de modèles automatique complète des actions de création, lecture, mise à jour et suppression des contrôleurs et des vues correspondantes. Par défaut, cette structure génère le code d’accès aux données à l’aide d’EF Code First.
- La boîte de dialogue Ajouter un contrôleur prend en charge les *échafaudages extensibles* via des packages NuGet tels que *MvcScaffolding*. Cela permet de brancher des échafaudages personnalisés dans la boîte de dialogue, ce qui vous permet de créer des échafaudages pour d’autres technologies d’accès aux données telles que NHibernate ou même JET avec ODBCDirect si vous le souhaitez.

Pour plus d’informations sur la génération de modèles automatique dans ASP.NET MVC 3, consultez les ressources suivantes :

- Les séries de Steve Sanderson, notamment : 

    1. [Introduction : échafaudage de votre projet ASP.NET MVC 3 avec le package MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Utilisation standard : cas d’utilisation et options classiques](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relations un-à-plusieurs](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Actions de génération de modèles automatique et tests unitaires](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Remplacement des modèles T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Ce billet : création de modèles de modèle personnalisés](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Publication de Scott Hanselman à partir de sa session PDC 2010 [création d’un blog avec Microsoft « Package sans nom de l’amour pour le Web »](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Notes de publication de MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Modèles de projet HTML 5

La boîte de dialogue Nouveau projet comprend une case à cocher Activer les versions HTML 5 des modèles de projet. Ces modèles tirent parti de Modernizr 1,7 pour assurer la prise en charge de la compatibilité pour HTML 5 et CSS 3 dans les navigateurs de niveau supérieur.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Moteur d’affichage Razor

ASP.NET MVC 3 est fourni avec un nouveau moteur d’affichage nommé Razor qui offre les avantages suivants :

- Syntaxe Razor est propre et concis, ce qui nécessite un nombre minimal de frappes de touches.
- Razor est facile à apprendre, en partie parce qu’il est basé sur des langages existants comme C# et Visual Basic.
- Visual Studio intègre IntelliSense et la colorisation de code pour syntaxe Razor.
- Les vues Razor peuvent être testées par unité sans que vous ayez besoin d’exécuter l’application ou de lancer un serveur Web.

Voici quelques-unes des nouvelles fonctionnalités Razor :

- `@model` syntaxe permettant de spécifier le type passé à la vue.
- `@* *@` la syntaxe des commentaires.
- La possibilité de spécifier des valeurs par défaut (par exemple `layoutpage`) une seule fois pour un site entier.
- La méthode `Html.Raw` pour afficher du texte sans l’encoder en HTML.
- Prise en charge du partage de code entre plusieurs vues (\_les fichiers *ViewStart. cshtml* ou *\_ViewStart. vbhtml* ).

Razor comprend également de nouvelles applications auxiliaires HTML, telles que les suivantes :

- `Chart`. Génère le rendu d’un graphique, en offrant les mêmes fonctionnalités que le contrôle Chart dans ASP.NET 4.
- `WebGrid`. Effectue le rendu d’une grille de données, avec la fonctionnalité de pagination et de tri.
- `Crypto`. Utilise des algorithmes de hachage pour créer des mots de passe correctement salés et hachés.
- `WebImage`. Génère le rendu d’une image.
- `WebMail`. Envoie un e-mail.

Pour plus d’informations sur Razor, consultez les ressources suivantes :

- [Billet de blog de Scott Guthrie présentation de Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Billet de blog de Scott Guthrie présentation du mot clé @model](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Billet de blog de Scott Guthrie présentation des dispositions Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Aide-mémoire de l’API Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Notes de publication de MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Prise en charge de plusieurs moteurs d’affichage

La boîte de dialogue **Ajouter une vue** dans ASP.NET MVC 3 vous permet de choisir le moteur d’affichage que vous souhaitez utiliser et la boîte de dialogue **nouveau projet** vous permet de spécifier le moteur d’affichage par défaut d’un projet. Vous pouvez choisir le moteur d’affichage de Web Forms (ASPX), Razor ou un moteur d’affichage Open source comme [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/)ou [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Améliorations du contrôleur

### <a name="global-action-filters"></a>Filtres d’action globaux

Parfois, vous souhaitez exécuter la logique avant l’exécution d’une méthode d’action ou après l’exécution d’une méthode d’action. Pour prendre cela en charge, ASP.NET MVC 2 a fourni des filtres d’action. Les filtres d’action sont des attributs personnalisés qui fournissent un moyen déclaratif d’ajouter un comportement de pré-action et après action à des méthodes d’action de contrôleur spécifiques. Toutefois, dans certains cas, vous souhaiterez peut-être spécifier un comportement pré-action ou après action qui s’applique à toutes les méthodes d’action. MVC 3 vous permet de spécifier des filtres globaux en les ajoutant à la collection `GlobalFilters`. Pour plus d’informations sur les filtres d’action globaux, consultez les ressources suivantes :

- [Blog de Scott Guthrie sur la version préliminaire de MVC 3](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtrage dans ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nouvelle propriété « ViewBag »

Les contrôleurs MVC 2 prennent en charge une propriété `ViewData` qui vous permet de passer des données à un modèle de vue à l’aide d’une API de dictionnaire à liaison tardive. Dans MVC 3, vous pouvez également utiliser une syntaxe assez simple avec la propriété `ViewBag` pour accomplir le même objectif. Par exemple, au lieu d’écrire des `ViewData["Message"]="text"`, vous pouvez écrire des `ViewBag.Message="text"`. Vous n’avez pas besoin de définir des classes fortement typées pour utiliser la propriété `ViewBag`. Étant donné qu’il s’agit d’une propriété dynamique, vous pouvez à la place obtenir ou définir des propriétés et les résoudre de manière dynamique au moment de l’exécution. En interne, les propriétés de `ViewBag` sont stockées sous forme de paires nom/valeur dans le dictionnaire `ViewData`. (Remarque : dans la plupart des versions préliminaires de MVC 3, la propriété `ViewBag` était nommée la propriété `ViewModel`.)

### <a name="new-actionresult-types"></a>Nouveaux types « ActionResult »

Les types de `ActionResult` suivants et les méthodes d’assistance correspondantes sont nouveaux ou améliorés dans MVC 3 :

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Retourne un code d’état HTTP 404 au client.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Retourne une redirection temporaire (code d’état HTTP 302) ou une redirection permanente (code d’état HTTP 301), en fonction d’un paramètre booléen. Conjointement à cette modification, la classe [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) a désormais trois méthodes pour effectuer des redirections permanentes : `RedirectPermanent`, `RedirectToRoutePermanent`et `RedirectToActionPermanent`. Ces méthodes retournent une instance de `RedirectResult` avec la propriété `Permanent` définie sur `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Retourne un code d’état HTTP spécifié par l’utilisateur.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Améliorations apportées à JavaScript et Ajax

Par défaut, Ajax et les applications auxiliaires de validation dans MVC 3 utilisent une approche JavaScript discrète. JavaScript discret évite d’injecter du code JavaScript inline dans du code HTML. Cela rend votre code HTML plus petit et moins encombré, et facilite l’échange ou la personnalisation des bibliothèques JavaScript. Les applications auxiliaires de validation dans MVC 3 utilisent également le plug-in `jQueryValidate` par défaut. Si vous souhaitez un comportement MVC 2, vous pouvez désactiver le JavaScript discrète à l’aide d’un paramètre de fichier *Web. config* . Pour plus d’informations sur les améliorations apportées à JavaScript et Ajax, consultez les ressources suivantes :

- [Présentation de base du JavaScript discret sur le site de Wikipédia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [La publication JavaScript discrète de Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Publication de la validation JavaScript discrète de Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Création d’une application MVC 3 avec Razor et JavaScript discret](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (didacticiel sur le site ASP.net)
- [Notes de publication de MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Validation côté client activée par défaut

Dans les versions antérieures de MVC, vous devez appeler explicitement la méthode `Html.EnableClientValidation` à partir d’une vue afin d’activer la validation côté client. Dans MVC 3, ce n’est plus nécessaire, car la validation côté client est activée par défaut. (Vous pouvez désactiver cette option à l’aide d’un paramètre dans le fichier *Web. config* .)

Pour que la validation côté client fonctionne, vous devez toujours référencer les bibliothèques de validation jQuery et jQuery appropriées dans votre site. Vous pouvez héberger ces bibliothèques sur votre propre serveur ou les référencer à partir d’un réseau de distribution de contenu (CDN) comme CDN de Microsoft ou Google.

### <a name="remote-validator"></a>Validateur distant

ASP.NET MVC 3 prend en charge la nouvelle classe [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) qui vous permet de tirer parti de la prise en charge du validateur distant du plug-in de validation jQuery. Cela permet à la bibliothèque de validation côté client d’appeler automatiquement une méthode personnalisée que vous définissez sur le serveur afin d’effectuer une logique de validation qui ne peut être effectuée que côté serveur.

Dans l’exemple suivant, l’attribut `Remote` spécifie que la validation client appellera une action nommée `UserNameAvailable` sur la classe `UsersController` afin de valider le champ `UserName`.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

L’exemple suivant montre le contrôleur correspondant.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Pour plus d’informations sur l’utilisation de l’attribut `Remote`, consultez [Comment : implémenter la validation à distance dans ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) dans MSDN Library.

### <a name="json-binding-support"></a>Prise en charge de la liaison JSON

ASP.NET MVC 3 comprend une prise en charge intégrée de la liaison JSON qui permet aux méthodes d’action de recevoir des données encodées JSON et de les lier à des paramètres de méthode d’action. Cette fonctionnalité est utile dans les scénarios impliquant des modèles client et la liaison de données. (Les modèles clients vous permettent de mettre en forme et d’afficher un élément de données unique ou un ensemble d’éléments de données à l’aide de modèles qui s’exécutent sur le client.) MVC 3 vous permet de connecter facilement des modèles clients avec des méthodes d’action sur le serveur qui envoie et reçoit des données JSON. Pour plus d’informations sur la prise en charge de la liaison JSON, consultez la section relative aux **améliorations JavaScript et Ajax** du billet de blog de la version d' [évaluation MVC 3 de Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Améliorations de la validation de modèle

### <a name="dataannotations-metadata-attributes"></a>Attributs de métadonnées « DataAnnotations »

ASP.NET MVC 3 prend en charge les attributs de métadonnées `DataAnnotations` tels que les `DisplayAttribute`.

### <a name="validationattribute-class"></a>Classe « ValidationAttribute »

La classe `ValidationAttribute` a été améliorée dans le .NET Framework 4 pour prendre en charge une nouvelle surcharge `IsValid` qui fournit plus d’informations sur le contexte de validation actuel, telles que l’objet en cours de validation. Cela permet des scénarios plus riches dans lesquels vous pouvez valider la valeur actuelle en fonction d’une autre propriété du modèle. Par exemple, le nouvel attribut `CompareAttribute` vous permet de comparer les valeurs de deux propriétés d’un modèle. Dans l’exemple suivant, la propriété `ComparePassword` doit correspondre au champ `Password` afin d’être valide.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfaces de validation

L’interface [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) vous permet d’effectuer une validation au niveau du modèle, et vous permet de fournir des messages d’erreur de validation spécifiques à l’état du modèle global, ou entre deux propriétés dans le modèle. MVC 3 récupère désormais les erreurs de l’interface `IValidatableObject` lors de la liaison de modèle, et signale ou met automatiquement en surbrillance les champs affectés dans une vue à l’aide des applications auxiliaires de formulaire HTML intégrées.

L’interface [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) permet à ASP.NET MVC de détecter au moment de l’exécution si un validateur prend en charge la validation du client. Cette interface a été conçue de manière à pouvoir être intégrée à une variété de frameworks de validation.

Pour plus d’informations sur les interfaces de validation, consultez la section améliorations de la **validation de modèle** dans le billet de blog sur la version d' [évaluation MVC 3 de Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Toutefois, Notez que la référence à « IValidateObject » dans le blog doit être « IValidatableObject ».)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Améliorations de l’injection de dépendances

ASP.NET MVC 3 offre une meilleure prise en charge de l’application de l’injection de dépendances (DI) et de l’intégration avec l’injection de dépendances ou les conteneurs d’inversion de contrôle (IOC). La prise en charge de DI a été ajoutée dans les domaines suivants :

- Contrôleurs (enregistrement et injection de fabriques de contrôleurs, injection de contrôleurs).
- Vues (enregistrement et injection de moteurs d’affichage, injection de dépendances dans les pages de vue).
- Filtres d’action (localisation et injection de filtres).
- Classeurs de modèles (inscription et injection).
- Fournisseurs de validation de modèle (inscription et injection).
- Fournisseurs de métadonnées de modèle (inscription et injection).
- Fournisseurs de valeurs (inscription et injection).

MVC 3 prend en charge la bibliothèque [common service Locator](https://github.com/unitycontainer/commonservicelocator) et tout conteneur di qui prend en charge l’interface `IServiceLocator` de cette bibliothèque. Il prend également en charge une nouvelle interface `IDependencyResolver` qui facilite l’intégration des frameworks DI.

Pour plus d’informations sur DI dans MVC 3, consultez les ressources suivantes :

- [Série de billets de blog de Brad Wilson sur l’emplacement du service](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Notes de publication de MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Autres nouvelles fonctionnalités

### <a name="nuget-integration"></a>Intégration NuGet

ASP.NET MVC 3 installe et active automatiquement NuGet dans le cadre de sa configuration. NuGet est un gestionnaire de package open source gratuit qui facilite la recherche, l’installation et l’utilisation des bibliothèques et des outils .NET dans vos projets. Elle fonctionne avec tous les types de projets Visual Studio (y compris ASP.NET Web Forms et ASP.NET MVC).

NuGet permet aux développeurs qui gèrent des projets open source (par exemple, des projets tels que MOQ, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks et ELMAH) d’empaqueter leurs bibliothèques et de les inscrire dans une galerie en ligne. Il est alors facile pour les développeurs .NET qui souhaitent utiliser l’une de ces bibliothèques de rechercher le package et de l’installer dans des projets sur lesquels ils travaillent.

Avec la mise à jour des outils ASP.NET 3, les modèles de projet incluent les bibliothèques JavaScript préinstallées des packages NuGet, afin qu’ils puissent être mis à jour via NuGet. Entity Framework Code First est également préinstallé en tant que package NuGet.

Pour plus d'informations sur NuGet, consultez la [documentation NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Mise en cache de sortie de pages partielles

ASP.NET MVC prend en charge la mise en cache de sortie des réponses de page complètes depuis la version 1. MVC 3 prend également en charge la mise en cache de sortie de pages partielles, ce qui vous permet de mettre facilement en cache des régions ou des fragments d’une réponse. Pour plus d’informations sur la mise en cache, consultez la section relative à la **mise en cache partielle** de la sortie de page du billet de [blog de Scott Guthrie sur la version Release candidate de MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) et la section **mise en cache de sortie des actions enfants** des [notes de publication de MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Contrôle granulaire sur la validation des demandes

ASP.NET MVC dispose d’une validation de requête intégrée qui permet de se protéger automatiquement contre les attaques par injection XSS et HTML. Toutefois, vous souhaiterez parfois désactiver explicitement la validation de la demande, par exemple si vous souhaitez permettre aux utilisateurs de poster du contenu HTML (par exemple, dans des entrées de blog ou du contenu CMS). Vous pouvez maintenant ajouter un attribut [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) aux modèles ou afficher les modèles pour désactiver la validation de la demande pour chaque propriété lors de la liaison de modèle. Pour plus d’informations sur la validation des demandes, consultez les ressources suivantes :

- La section **JavaScript et validation discrètes** du billet de [blog de Scott Guthrie sur la version Release candidate de MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Notes de publication de MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Boîte de dialogue « Nouveau projet » extensible

Dans ASP.NET MVC 3, vous pouvez ajouter des modèles de projet, des moteurs d’affichage et des infrastructures de projet de test unitaire à la boîte de dialogue **nouveau projet** .

### <a name="template-scaffolding-improvements"></a>Améliorations de la génération de modèles automatique de modèle

Les modèles de génération de modèles automatique ASP.NET MVC 3 permettent d’identifier plus facilement les propriétés de clé primaire sur les modèles et de les gérer de manière appropriée que dans les versions antérieures de MVC. (Par exemple, les modèles de génération de modèles automatique s’assurent à présent que la clé primaire n’est pas échafaudée en tant que champ de formulaire modifiable.)

Par défaut, les échafaudages de création et de modification utilisent désormais le programme d’assistance `Html.EditorFor` au lieu de l’application auxiliaire `Html.TextBoxFor`. Cela améliore la prise en charge des métadonnées sur le modèle sous forme d’attributs d’annotation de données lorsque la boîte de dialogue **Ajouter une vue** génère une vue.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nouvelles surcharges pour « html. LabelFor » et « html. LabelForModel »

De nouvelles surcharges de méthode ont été ajoutées pour les méthodes d’assistance `LabelFor` et `LabelForModel`. Les nouvelles surcharges vous permettent de spécifier ou de substituer le texte de l’étiquette.

### <a name="sessionless-controller-support"></a>Prise en charge des contrôleurs de session

Dans ASP.NET MVC 3, vous pouvez indiquer si vous souhaitez qu’une classe de contrôleur utilise l’état de session, et si tel est le cas, si l’état de session doit être en lecture/écriture ou en lecture seule. Pour plus d’informations sur la prise en charge des contrôleurs de session, consultez [notes de publication MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nouvelle classe « AdditionalMetadataAttribute »

Vous pouvez utiliser l’attribut [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) pour remplir le dictionnaire `ModelMetadata.AdditionalValues` pour une propriété de modèle. Par exemple, si un modèle de vue a une propriété qui doit être affichée uniquement pour un administrateur, vous pouvez annoter cette propriété comme indiqué dans l’exemple suivant :

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Ces métadonnées sont mises à la disposition de tout modèle d’affichage ou d’éditeur lorsqu’un modèle de vue de produit est rendu. C’est à vous de pouvoir interpréter les informations de métadonnées.

### <a name="accountcontroller-improvements"></a>Améliorations apportées à AccountController

Le AccountController dans le modèle de projet Internet a été beaucoup amélioré.

### <a name="new-intranet-project-template"></a>Nouveau modèle de projet intranet

Un nouveau modèle de projet intranet est inclus, ce qui active l’authentification Windows et supprime AccountController.
