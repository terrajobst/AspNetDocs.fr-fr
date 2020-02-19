---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profiler et déboguer votre application ASP.NET MVC avec l’aperçu | Microsoft Docs
author: Rick-Anderson
description: L’aperçu est une famille de packages NuGet Open source prospère et croissante qui fournit des informations détaillées sur les performances, le débogage et le diagnostic pour ASP.NET a...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: d3689147a3bc3aa1f4180c377d2483a94bdd95a9
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457659"
---
# <a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Profiler et déboguer votre application ASP.NET MVC avec Glimpse

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> L’aperçu est une famille de packages NuGet Open source prospère et croissante qui fournit des informations détaillées sur les performances, le débogage et le diagnostic pour les applications ASP.NET. Il est facile d’installer, léger, ultra-rapide et d’afficher des mesures de performances clés en bas de chaque page. Elle vous permet d’accéder à votre application lorsque vous avez besoin de savoir ce qui se passe sur le serveur. L’aperçu fournit des informations précieuses, nous vous recommandons de l’utiliser tout au long de votre cycle de développement, y compris votre environnement de test Azure. Bien que [Fiddler](http://www.telerik.com/fiddler) et les [outils de développement F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) fournissent une vue côté client, l’outil aperçu fournit une vue détaillée du serveur. Ce didacticiel se concentre sur l’utilisation des packages ASP.NET MVC et EF, mais de nombreux autres packages sont disponibles. Dans la mesure du possible, je vais créer un lien vers les [documents d’aperçu](http://getglimpse.com/Docs/) appropriés que je vous aide à entretenir. L’aperçu est un projet open source, vous pouvez également contribuer au code source et aux documents.

- [Installation de l’aperçu](#ig)
- [Activer l’Aperçu pour localhost](#eg)
- [Onglet chronologie](#Time)
- [Liaison de données](#mb)
- [Itinéraires](#route)
- [Utilisation de l’aperçu sur Azure](#da)
- [Ressources supplémentaires](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Installation de l’aperçu

Vous pouvez installer l’aperçu à partir de la console du gestionnaire de package NuGet ou de la console **gérer les packages NuGet** . Pour cette démonstration, je vais installer les packages Mvc5 et EF6 :

![installer l’aperçu à partir de NuGet DLG](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Recherche d' *aperçu. EF*

![Aperçu. EF de NuGet install DLG](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

En sélectionnant **packages installés**, vous pouvez voir les modules dépendants d’aperçu installés :

![Packages d’aperçu installés à partir de la DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Les commandes suivantes installent les modules MVC5 et EF6 à partir de la console du gestionnaire de package :

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Activer l’Aperçu pour localhost

Accédez à http://localhost:&lt;p Trier #&gt;/Glimpse.axd, puis cliquez sur le bouton <strong>activer l’aperçu</strong> .

![Page d’aperçu axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Si votre barre de favoris est affichée, vous pouvez glisser-déplacer les boutons d’aperçu et les ajouter en tant que bookmarklets :

![IE avec les bookmarklets d’aperçu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Vous pouvez maintenant naviguer dans votre application et l' **affichage des têtes d’affichage** (HUD) s’affiche au bas de la page.

![Page du gestionnaire de contacts avec HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

La [page Aperçu HUD](http://getglimpse.com/Docs/Heads-up-Display) détaille les informations de minutage indiquées ci-dessus. Les données de performances discrètes affichées par HUD peuvent vous informer immédiatement d’un problème, avant d’aller au cycle de test. En cliquant sur le &quot;g&quot; dans le coin inférieur droit, vous affichez le panneau d’aperçu :

![Panneau d’aperçu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Dans l’image ci-dessus, l' [onglet exécution](http://getglimpse.com/Docs/Execution-Tab) est sélectionné, qui affiche les détails de la minuterie des actions et des filtres dans le pipeline. Vous pouvez voir mon [minuteur de filtre Stop Watch](http://www.nuget.org/packages/StopWatch/) démarrer à l’étape 6 du pipeline. Bien que mon minuteur clair puisse fournir des données de profil/minutage utiles, il manque tout le temps passé dans l’autorisation et le rendu de la vue. Vous pouvez en savoir plus sur mon minuteur au niveau du [profil et l’heure de votre application ASP.NET MVC jusqu’à Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). La page [onglets](http://getglimpse.com/Docs/Tabs) fournit des liens vers des informations détaillées sur chaque onglet.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Onglet chronologie

J’ai modifié le [didacticiel EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) en suspens de Tom Dykstra avec la modification de code suivante dans le contrôleur des enseignants :

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Le code ci-dessus me permet de transmettre une chaîne de requête (`eager`) pour contrôler le chargement hâtif ou explicite des données. Dans l’image ci-dessous, le chargement explicite est utilisé et la page minutage affiche chaque inscription chargée dans le `Index` méthode d’action :

![chargement explicite](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

Dans le code suivant, hâtif est spécifié et chaque inscription est extraite après l’appel de la vue `Index` :

![hâtif est spécifié](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Vous pouvez pointer sur un segment temporel pour obtenir des informations détaillées sur la temporisation :

![pointer pour voir le minutage détaillé](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Liaison de modèle

L' [onglet liaison de modèle](http://getglimpse.com/Docs/Model-Binding-Tab) fournit une multitude d’informations pour vous aider à comprendre comment vos variables de formulaire sont liées et pourquoi certains ne sont pas liés comme prévu. L’image ci-dessous montre les **?** , sur laquelle vous pouvez cliquer pour afficher la page d’aide de l’aperçu de cette fonctionnalité.

![vue de liaison du modèle aperçu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Itinéraires

 L’onglet itinéraires d’aperçu peut vous aider à déboguer et à comprendre le routage. Dans l’image ci-dessous, l’itinéraire du produit est sélectionné (et il s’affiche en vert, une convention d’aperçu). ![nom de produit sélectionné](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) les contraintes de routage, les zones et les jetons de données sont également affichés. Pour plus d’informations, consultez [itinéraires d’aperçu](http://getglimpse.com/Docs/Routes-Tab) et [routage d’attributs dans ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) . 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Utilisation de l’aperçu sur Azure

La stratégie de sécurité aperçu par défaut permet d’afficher uniquement les données d’aperçu à partir de l’hôte local. Vous pouvez modifier cette stratégie de sécurité afin de pouvoir afficher ces données sur un serveur distant (par exemple, une application Web sur Azure). Pour les environnements de test sur Azure, ajoutez la marque mise en surbrillance au bas du fichier *Web. config* pour activer l’aperçu :

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Avec cette modification seule, n’importe quel utilisateur peut voir vos données d’aperçu sur un site distant. Envisagez d’ajouter le balisage ci-dessus à un profil de publication afin qu’il ne soit déployé qu’un appliqué lorsque vous utilisez ce profil de publication (par exemple, votre profil test Azure). Pour limiter les données d’aperçu, nous allons ajouter le rôle `canViewGlimpseData` et autoriser uniquement les utilisateurs de ce rôle à afficher les données d’aperçu.

Supprimez les commentaires du fichier *GlimpseSecurityPolicy.cs* et remplacez l’appel [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) `Administrator` par le rôle `canViewGlimpseData` :

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Sécurité : les données enrichies fournies par l’aperçu peuvent exposer la sécurité de votre application. Microsoft n’a pas effectué d’audit de sécurité sur l’Aperçu pour une utilisation sur les applications de production.

Pour plus d’informations sur l’ajout de rôles, consultez le didacticiel mon [déploiement d’une application web ASP.NET MVC 5 sécurisée avec appartenance, OAuth et SQL Database dans Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) .

<a id="addRes"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Déployer une application ASP.NET MVC 5 sécurisée avec appartenance, OAuth et SQL Database à Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Configuration](http://getglimpse.com/Docs/Configuration) de l’aperçu-page document sur la configuration des onglets, de la stratégie Runtime, de la journalisation, etc.
