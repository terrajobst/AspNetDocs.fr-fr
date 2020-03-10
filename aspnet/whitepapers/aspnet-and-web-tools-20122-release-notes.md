---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: Notes de publication de ASP.NET et Web Tools 2012,2 | Microsoft Docs
author: rick-anderson
description: Notes de publication pour ASP.NET et Web Tools 2012,2.
ms.author: riande
ms.date: 02/14/2013
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: a4ea1d7c146309e1d5e8be944d496e9fd87bca3e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523778"
---
# <a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET et Web Tools 2012.2 - Notes de publication

> Ce document décrit la version de ASP.NET et Web Tools 2012,2. Il s’agit d’une mise à jour des outils Web Visual Studio et de ASP.NET.

- [Notes d’installation](#_Installation)
- [Documentation](#_Documentation)
- [Support](#_Support)
- [Configuration logicielle requise](#_Software_Requirements)
- [Nouvelles fonctionnalités de ASP.NET et Web Tools 2012,2](#_New_Features_in)

    - [Outillage](#_Tooling)
    - [Publication Web](#_Web_Publishing)
    - [Modèles MVC ASP.NET](#_Templates)
    - [API Web ASP.NET](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [URL conviviales ASP.NET](#_ASP.NET_Friendly_URLs)
- [Problèmes connus et modifications avec rupture](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Notes d'installation

ASP.NET et Web Tools 2012,2 pour Visual Studio 2012 peuvent être installés à l’aide de [Web Platform Installer](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Il s’agit d’une mise à jour de Visual Studio 2012 ou Visual Studio Express 2012 pour le Web, ce qui est nécessaire. Si vous n’avez pas installé Visual Studio, Visual Studio Express 2012 pour le Web sera installé.

Vous pouvez également installer ASP.NET et Web Tools 2012,2 manuellement. Vous devez avoir installé Visual Studio 2012 ou Visual Studio Express 2012 pour le Web. Utilisez ensuite les instructions suivantes : 

1. Téléchargez le programme d’installation [ASP.net et Web frameworks 2012,2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) à partir du centre de téléchargement.
2. Quand vous y êtes invité, cliquez sur Exécuter. Vous pouvez également enregistrer le fichier pour l’exécuter plus tard.
3. Vérifiez la version de Visual Studio que vous allez mettre à jour. Pour ce faire, vous pouvez lancer le Visual Studio que vous souhaitez mettre à jour. Cliquez ensuite sur l’élément de menu aide.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Si vous voyez l’élément de menu &quot;sur Microsoft Visual Studio 2012 pour le&quot; Web, téléchargez [web Outils de développement 2012,2-Visual Studio Express 2012 pour le Web](https://go.microsoft.com/fwlink/?LinkID=282228). Sinon, téléchargez [le outils de développement Web 2012,2-Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Quand vous y êtes invité, cliquez sur Exécuter. Vous pouvez également enregistrer le fichier pour l’exécuter plus tard.

> [!NOTE]
> ASP.NET et Web Tools version 2012,2 n’inclut pas SQL Server Data Tools. SQL Server et les bases de données SQL Windows Azure offrent un ensemble plus riche d’outils de base de données, notamment le développement hors connexion, la comparaison de schémas et les capacités de déploiement de base de données améliorées. Pour plus d’informations ou pour installer SQL Server Data Tools visitez [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Documentation

Des didacticiels et d’autres informations sur ASP.NET et Web Tools 2012,2 sont disponibles sur le site Web ASP.NET (https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Assistance

ASP.NET et Web Tools 2012,2 est officiellement publié et pris en charge. Vous pouvez utiliser votre canal de support normal. Vous pouvez également poser des questions sur les Forums ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), où les membres de la communauté ASP.net sont souvent en mesure de fournir un support informel.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Configuration logicielle

La ASP.NET et Web Tools 2012,2 nécessite Visual Studio 2012 ou Visual Studio Express 2012 pour le Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nouvelles fonctionnalités de ASP.NET et Web Tools 2012,2

Cette section décrit les fonctionnalités qui ont été introduites dans la version 2012,2 de ASP.NET et Web Tools.

<a id="_Tooling"></a>
### <a name="tooling"></a>Outillage

- Inspecteur de page 

    - Prend en charge le mappage de sélection JavaScript qui permet à Inspecteur de page de mapper des éléments qui ont été ajoutés dynamiquement à la page vers le code JavaScript correspondant.
    - La possibilité de voir les mises à jour CSS en temps réel.
    - Pour plus d’informations, consultez [synchronisation automatique de CSS et mappage de sélection JavaScript dans inspecteur de page](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Éditeur 

    - Prenez en charge la mise en évidence de la syntaxe pour CoffeeScript, angulaires, guidon et JsRender.
    - L’éditeur HTML fournit IntelliSense pour les liaisons Knockout.
    - MOINS de modifications et de prise en charge du compilateur pour activer la génération CSS dynamique à l’aide de moins.
    - Collez le code JSON en tant que classe .NET. À l’aide de cette commande de collage spéciale pour C# coller JSON dans un fichier de code ou VB.net, Visual Studio génère automatiquement des classes .net déduites du JSON.
- La prise en charge de l’émulateur mobile ajoute des hooks d’extensibilité afin que les émulateurs tiers puissent être installés en tant que VSIX. Les émulateurs installés s’affichent dans la liste déroulante F5, afin que les développeurs puissent afficher un aperçu de leurs sites Web sur divers appareils mobiles. Pour plus d’informations sur cette fonctionnalité, consultez l’entrée de blog de Scott Hanselman sur [la nouvelle intégration BrowserStack avec Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>publication Web

- Les projets de site Web ont désormais la même expérience de publication que les projets d’application Web, notamment la publication sur les sites Web Windows Azure.
- Publication &#8211; sélective pour un ou plusieurs fichiers vous pouvez effectuer les actions suivantes (après la publication sur un point de terminaison de Web Deploy) : 

    - Publier les fichiers sélectionnés.
    - Consultez la différence entre un fichier local et un fichier distant.
    - Mettez à jour le fichier local avec le fichier distant ou mettez à jour le fichier distant avec le fichier local.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Modèles MVC ASP.NET

- Le nouveau modèle d’application Facebook facilite l’écriture d’applications de canevas Facebook. En quelques étapes simples, vous pouvez créer une application Facebook qui obtient les données d’un utilisateur connecté et s’intègre à ses amis. Le modèle comprend une nouvelle librairie qui se charge de tous les raccordements qu’implique la création d’une application Facebook, notamment l’authentification, les autorisations, l’accès aux données Facebook, etc. Pour plus d’informations sur l’utilisation du modèle d’application Facebook, consultez [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921).
- Un nouveau modèle MVC d’application à une page permet aux développeurs de créer des applications Web côté client interactives à l’aide des codes HTML 5, CSS 3, ainsi que des bibliothèques JavaScript prisées Knockout et jQuery, sur l’API Web ASP.NET. Le modèle comprend une application de liste « TODO » qui illustre les pratiques courantes de création d’une application JavaScript HTML5 qui utilise une API de serveur RESTful. Pour plus d’informations, consultez [https://www.asp.net/single-page-application](../single-page-application/index.md).
- Vous pouvez maintenant créer une extension VSIX qui ajoute de nouveaux modèles à la boîte de dialogue Nouveau projet ASP.NET MVC. Découvrez comment : [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Les modèles &#8211; de projet MVC du package FixedDisplayModes ont été mis à jour pour inclure le nouveau package NuGet « FixedDisplayModes », qui contient une solution de contournement pour un bogue dans MVC 4. Pour plus d’informations sur le correctif contenu dans le package, reportez-vous à ce billet de blog ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) de l’équipe Mvc.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>API web ASP.NET

API Web ASP.NET a été améliorée avec plusieurs nouvelles fonctionnalités :

- API Web ASP.NET OData
- Suivi des API Web ASP.NET
- API Web ASP.NET la page d’aide

#### <a name="aspnet-web-api-odata"></a>API Web ASP.NET OData

API Web ASP.NET OData vous offre la flexibilité dont vous avez besoin pour créer des points de terminaison OData avec une logique métier enrichie sur n’importe quelle source de données. Avec API Web ASP.NET OData, vous contrôlez la quantité de sémantique OData que vous souhaitez exposer. API Web ASP.NET OData est inclus avec les modèles de projet ASP.NET MVC 4 et est également disponible à partir de NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

API Web ASP.NET OData prend actuellement en charge les fonctionnalités suivantes :

- Activez la sémantique de requête OData en appliquant l’attribut [interrogeable].
- Validez facilement les requêtes OData et restreignez l’ensemble des options de requête, des opérateurs et des fonctions pris en charge.
- Le paramètre est lié directement à ODataQueryOptions pour obtenir une représentation sous forme d’arborescence de syntaxe abstraite de la requête qui peut ensuite être validée et appliquée à un IQueryable ou IEnumerable.
- Activez la pagination pilotée par le service et la génération de lien de page suivante en spécifiant des limites de résultats sur l’attribut [Queryable].
- Demandez un nombre Inline du nombre total de ressources correspondantes à l’aide de $inlinecount.
- Contrôle la propagation de la valeur null.
- Tout ou partie des opérateurs dans $filter.
- Déduire un modèle EDM (Entity Data Model) par convention ou personnaliser explicitement un modèle d’une manière similaire à Entity Framework code-First.
- Exposer des jeux d’entités par dérivation à partir de EntitySetController.
- Conventions simples et personnalisables pour l’exposition des propriétés de navigation, la manipulation des liens et l’implémentation d’actions OData.
- Routage simplifié à l’aide de la méthode d’extension MapODataRoute.
- Prise en charge du contrôle de version en exposant plusieurs modèles EDM.
- Exposez les $metadata et le document de service afin de pouvoir générer des clients (.NET, Windows Phone, Windows Store, etc.) pour votre API Web.
- Prise en charge des formats détaillés OData Atom, JSON et JSON.
- Créer, mettre à jour, mettre à jour partiellement (PATCH) et supprimer des entités.
- Interroger et manipuler des relations entre des entités.
- Créez des liens de relation qui s’associent à vos itinéraires.
- Types complexes.
- Héritage de type d’entité.
- Propriétés de la collection.
- Enums.
- Actions OData.
- Repose sur la même base que WCF Data Services, à savoir ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Pour plus d’informations sur API Web ASP.NET OData, consultez [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Suivi des API Web ASP.NET

API Web ASP.NET le suivi intègre les données de suivi de vos API Web avec le suivi .NET. Il est désormais activé par défaut dans le modèle de projet d’API Web. Les données de suivi de vos API Web sont envoyées à la fenêtre sortie et sont rendues disponibles par le biais d’IntelliTrace. Le suivi des API Web ASP.NET vous permet de suivre les informations relatives à votre API Web lorsque celles-ci sont hébergées sur Windows Azure grâce à l’intégration avec [windows Azure Diagnostics](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Vous pouvez également installer et activer le suivi API Web ASP.NET dans n’importe quelle application à l’aide du package NuGet API Web ASP.NET Tracing ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Pour plus d’informations sur la configuration et l’utilisation de API Web ASP.NET le suivi, consultez [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>API Web ASP.NET la page d’aide

La page d’aide API Web ASP.NET est désormais incluse par défaut dans le modèle de projet d’API Web. La page d’aide API Web ASP.NET génère automatiquement la documentation des API Web, y compris les points de terminaison HTTP, les méthodes HTTP prises en charge, les paramètres et les charges utiles des messages de demande et de réponse. La documentation est extraite automatiquement des commentaires dans votre code. Vous pouvez également ajouter la page d’aide API Web ASP.NET à n’importe quelle application à l’aide du package NuGet API Web ASP.NET page d’aide ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Pour plus d’informations sur l’installation et la personnalisation de la page d’aide API Web ASP.NET, consultez [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>SignalR ASP.NET

ASP.NET Signalr vous permet d’ajouter facilement des fonctionnalités Web en temps réel à votre application ASP.NET, en utilisant WebSockets si elles sont disponibles et en revenant automatiquement à d’autres techniques quand ce n’est pas le cas.

Pour plus d’informations sur l’utilisation de ASP.NET signaler, consultez [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>URL conviviales ASP.NET

ASP.NET FriendlyURLs permet aux développeurs de Web Forms de générer facilement des URL d’aspect plus propre (sans l’extension. aspx). Elle nécessite peu ou pas de configuration et peut être utilisée avec les applications ASP.NET v 4.0 existantes. La fonctionnalité FriendlyURLs permet également aux développeurs d’ajouter plus facilement un support mobile à leurs applications, en prenant en charge le basculement entre les vues de bureau et mobiles.

Pour plus d’informations sur l’installation et l’utilisation des URL conviviales ASP.NET, consultez [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et modifications avec rupture

Cette section décrit les problèmes connus et les modifications avec rupture qui se trouvent dans la version 2012,2 de ASP.NET et Web Tools.

### <a name="installation-issues"></a>Problèmes d’installation

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Installations en désordre de Visual Studio 2012

L’installation d’une référence supplémentaire de Visual Studio 2012 après l’installation du ASP.NET et Web Tools 2012,2 nécessite une opération de réparation. Examinez la séquence suivante :

1. Installer Visual Studio 2012 Express pour le Web
2. Installer ASP.NET et Web Tools 2012,2
3. Installer Visual Studio 2012 Professional, Premium ou Ultimate

L’étape 2 se traduirait par l’installation des mises à jour d’Express pour le Web. Pour vous assurer que la référence (SKU) supplémentaire installée à l’étape 3 contient la mise à jour, vous devez réparer la ASP.NET et Web Tools 2012,2 pour installer les mises à jour de la dernière référence installée. Cela s’applique également si les références SKU à l’étape 1 et 3 sont inversées.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Installation de Microsoft ASP.NET et Web Tools 2012,2 quand Visual Studio est ouvert

Si VS est ouvert pendant l’installation de Microsoft ASP.NET et Web Tools 2012,2, Visual Studio peut se retrouver dans un état incorrect. Il est recommandé que les utilisateurs ferment toutes les instances de Visual Studio avant de procéder à l’installation.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Annulation de l’installation de ASP.NET et Web Tools 2012,2 au milieu de l’installation

L’annulation du programme d’installation de ASP.NET et Web Tools 2012,2 au milieu de l’installation laisse Visual Studio dans un état incorrect. Pour résoudre ce problème, procédez comme suit : 

- Accédez à Ajout/Suppression de programmes.
- Désinstallez Microsoft ASP.NET et Web Tools 2012,2, le cas échéant.
- Réinstaller Microsoft ASP.NET et Web Tools 2012,2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Après la désinstallation de ASP.NET et Web Tools 2012,2, les modèles ASP.NET MVC 4 et les modèles de site Web Razor v2 sont manquants

La désinstallation de ASP.NET et Web Tools 2012,2 désinstalle également tous les modèles de site Web ASP.NET MVC 4 et Razor v2 de Visual Studio 2012.

La solution de contournement consiste à réparer votre installation de Visual Studio 2012 pour réinstaller les modèles de site Web ASP.NET MVC 4 et Razor v2.

### <a name="tooling-issues"></a>Problèmes liés aux outils

#### <a name="nuget-error-reported-during-project-creation"></a>Erreur NuGet signalée lors de la création du projet

Après l’installation de ASP.NET et Web Tools 2012,2, l’erreur suivante peut s’afficher lors de la création d’un projet MVC 4

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

Le ASP.NET et Web Tools 2012,2 envoie NuGet 2,1 et met à niveau l’extension dans Visual Studio 2012. Dans certains cas, le programme d’installation VSIX ne parvient pas à mettre à jour correctement le VSIX. Les étapes suivantes vous permettront de résoudre ce problème :

1. Démarrer Visual Studio 2012 en tant qu’administrateur
2. Accédez à outils-&gt;extensions et mises à jour et désinstallez NuGet.
3. Fermez Visual Studio.
4. Accédez au dossier d’installation de ASP.NET et Web Tools 2012,2 :

    1. Pour Visual Studio 2012 : **Program Files\Microsoft ASP. NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Pour Visual Studio 2012 Express pour le Web : **Program Files\Microsoft ASP. NET\ASP.NET Web Stack\Visual Studio Express 2012 pour le Web**
5. Double-cliquez sur NuGet. Tools. VSIX pour réinstaller NuGet

### <a name="web-api-issues"></a>Problèmes d’API Web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Analyse des problèmes dans les littéraux $filter et DateTime

L’analyseur d’URI OData ne parvient pas à analyser correctement les littéraux datetime partiels. Par exemple, $filter = Start EQ DateTime' 2012-12-31T12:00 'ne parvient pas à analyser correctement. Une solution de contournement consiste à utiliser le littéral complet, $filter = Start EQ DateTime' 2012-12-31T12:00:00 '.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData ne prend pas en charge les noms de propriété ne respectant pas la casse.

OData ne prend pas en charge les noms de propriété ne respectant pas la casse dans les requêtes OData et le chemin d’accès OData. Consultez WorkItems :

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Si les utilisateurs ont une casse différente côté client JavaScript et côté serveur, ils rencontreront probablement ce problème. Ce problème est lié à la conception dans le protocole OData. Toutefois, de nombreux utilisateurs signalent ce problème. Pour contourner ce comportement, les utilisateurs doivent corriger leurs cas dans l’URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Les conventions de routage OData par défaut ne prennent pas en charge la propriété de navigation de publication/PUT.

Les conventions de routage OData par défaut ne prennent pas en charge la propriété de navigation de publication/PUT. Consultez WorkItem [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366). Nous ne disposons pas de cette Convention couramment utilisée dans les conventions par défaut.

Pour contourner ce contournement, les utilisateurs doivent étendre une nouvelle Convention de routage pour la prendre en charge.

### <a name="facebook-template-issues"></a>Problèmes liés aux modèles Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Le modèle d’application Facebook fonctionne uniquement avec .NET 4,5

Vous devez sélectionner .NET 4,5 dans la liste déroulante Framework de la boîte de dialogue Nouveau projet pour voir le modèle d’application Facebook dans ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Contrôleur de mise à jour en temps réel

Le modèle d’application Facebook permet à l’utilisateur de créer facilement un contrôleur d’API Web pour gérer les mises à jour en temps réel à partir de Facebook. Si votre ordinateur de développement se trouve derrière NAT, votre contrôleur peut ne pas fonctionner sans configuration réseau supplémentaire. Pour plus d’informations, voir ici : [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Les valeurs de chaîne de requête sont en conflit avec les paramètres OAuth Facebook

Les champs suivants sont en conflit avec l’URL de rappel de la boîte de dialogue OAuth Facebook. N’ajoutez pas vos propres valeurs de chaîne de requête avec les noms suivants : code, erreur, erreur\_Description, erreur\_raison.

#### <a name="using-page-inspector-with-facebook-template"></a>Utilisation de Inspecteur de page avec le modèle Facebook

Vous ne pouvez pas utiliser la fonctionnalité Inspecteur de page dans Visual Studio 2012 lors du débogage de votre application Facebook. Le Inspecteur de page ne prend pas en charge les iframes pour le moment.

### <a name="single-page-application-template-issues"></a>Problèmes liés aux modèles d’application à page unique

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Avec la mise à jour de JQuery 1.9/Knockout 2.2.1, lors de l’exécution du projet SPA MVC par défaut, la modification de l’événement de focus d’un nouvel élément TODO n’est pas gérée correctement.

Avec la mise à jour de JQuery 1.9/Knockout 2.2.1, lors de l’exécution du projet SPA MVC par défaut, le nouvel élément TODO Edit Enter n’est plus ciblé vers la nouvelle zone d’édition de l’élément TODO après avoir entré le nouvel élément TODO dans la liste TODO.

Pour faire référence à la solution de contournement [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html), et apporter un correctif similaire à l’exemple de code suivant :

Fichier TODO. Model. js  
 todolist de fonction (données), ajoutez ce qui suit :  
 **Self. isSelected = Ko. observable (false);**

fonction todoList. prototype. addTodo, ajoutez le texte noir suivant :  
 **Self. isSelected (true);**  
 Self. newTodoTitle (&quot;&quot;);

Fichier index. cshtml, ajoutez le texte noir suivant :  
 &lt;de formulaire données-liaison =&quot;envoyer : addTodo&quot;&gt;  
 &lt;entrée Class =&quot;addTodo&quot; type =&quot;Text&quot; Data-bind =&quot;valeur : newTodoTitle, PlaceHolder : « type here to Add », blurOnEnter : true, **HasFocus : IsSelected**, Event : {Blur : addTodo}&quot; /&gt;  
 &lt;/formulaire&gt;
