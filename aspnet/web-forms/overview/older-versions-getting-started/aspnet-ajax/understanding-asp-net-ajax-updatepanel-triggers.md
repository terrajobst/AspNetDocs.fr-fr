---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Fonctionnement des déclencheurs UpdatePanel ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Lorsque vous travaillez dans l’éditeur de balises dans Visual Studio, vous pouvez remarquer (à partir d’IntelliSense) qu’il y a deux éléments enfants d’un contrôle UpdatePanel. L’une des Wh...
ms.author: riande
ms.date: 03/12/2008
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: b1cc869f373d4f8283b4d92af74707c3f11fef61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547452"
---
# <a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Présentation des déclencheurs UpdatePanel d’ASP.NET AJAX

par [Scott Cate](https://github.com/scottcate)

[Télécharger PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Lorsque vous travaillez dans l’éditeur de balises dans Visual Studio, vous pouvez remarquer (à partir d’IntelliSense) qu’il y a deux éléments enfants d’un contrôle UpdatePanel. L’un d’eux est l’élément déclencheurs, qui spécifie les contrôles sur la page (ou le contrôle utilisateur, si vous en utilisez un) qui déclenchera un rendu partiel du contrôle UpdatePanel dans lequel réside l’élément.

## <a name="introduction"></a>Introduction

La technologie ASP.NET de Microsoft propose un modèle de programmation orienté objet et piloté par les événements, et l’intègre aux avantages du code compilé. Toutefois, son modèle de traitement côté serveur présente plusieurs inconvénients inhérents à la technologie, dont la plupart peuvent être traités par les nouvelles fonctionnalités incluses dans les extensions Microsoft ASP.NET 3,5 AJAX. Ces extensions permettent de nombreuses nouvelles fonctionnalités clientes riches, y compris le rendu partiel des pages sans nécessiter une actualisation complète de la page, la possibilité d’accéder aux services Web via le script client (y compris l’API de profilage ASP.NET) et une API côté client étendue. conçu pour mettre en miroir un grand nombre des schémas de contrôle vus dans le jeu de contrôles côté serveur ASP.NET.

Ce livre blanc examine les fonctionnalités des déclencheurs XML du composant `UpdatePanel` AJAX ASP.NET. Les déclencheurs XML donnent un contrôle granulaire sur les composants qui peuvent provoquer un rendu partiel pour des contrôles UpdatePanel spécifiques.

Ce livre blanc est basé sur la version bêta 2 de la .NET Framework 3,5 et de Visual Studio 2008. Les extensions ASP.NET AJAX, précédemment un assembly complémentaire ciblé à ASP.NET 2,0, sont désormais intégrées dans la bibliothèque de classes de base .NET Framework. Ce livre blanc suppose également que vous travaillez avec Visual Studio 2008, et non Visual Web Developer Express, et vous fournira des procédures pas à pas en fonction de l’interface utilisateur de Visual Studio (bien que les listes de code soient entièrement compatibles, quelle que soit la environnement de développement).

## <a name="triggers"></a>*Déclencheurs*

Les déclencheurs pour un UpdatePanel donné, par défaut, incluent automatiquement tous les contrôles enfants qui appellent une publication, y compris les contrôles TextBox (par exemple) dont la propriété `AutoPostBack` a la valeur **true**. Toutefois, les déclencheurs peuvent également être inclus de manière déclarative à l’aide du balisage. Cette opération s’effectue dans la section `<triggers>` de la déclaration de contrôle UpdatePanel. Bien que les déclencheurs soient accessibles via la propriété de collection `Triggers`, il est recommandé d’inscrire tous les déclencheurs de rendu partiels au moment de l’exécution (par exemple, si un contrôle n’est pas disponible au moment de la conception) à l’aide de la méthode `RegisterAsyncPostBackControl(Control)` de l’objet ScriptManager de votre page, dans l’événement `Page_Load`. N’oubliez pas que les pages sont sans État et que vous devez donc réinscrire ces contrôles à chaque fois qu’ils sont créés.

L’inclusion automatique d’un déclencheur enfant peut également être désactivée (afin que les contrôles enfants qui créent des publications ne déclenchent pas automatiquement des rendus partiels) en affectant à la propriété `ChildrenAsTriggers` la **valeur false**. Cela vous permet de bénéficier de la plus grande flexibilité pour attribuer les contrôles spécifiques susceptibles d’appeler un rendu de page et est recommandé, afin qu’un développeur s’abonne pour répondre à un événement, plutôt que de gérer les événements susceptibles de se produire.

Notez que lorsque les contrôles UpdatePanel sont imbriqués, lorsque UpdateMode a la valeur **Conditional**, si l’UpdatePanel enfant est déclenché, alors que le parent n’est pas, seul le UpdatePanel enfant est actualisé. Toutefois, si le UpdatePanel parent est actualisé, alors l’UpdatePanel enfant est également actualisé.

## <a name="the-lttriggersgt-element"></a>*&lt;déclencheurs&gt; élément*

Lorsque vous travaillez dans l’éditeur de balises dans Visual Studio, vous pouvez remarquer (à partir d’IntelliSense) qu’il y a deux éléments enfants d’un contrôle `UpdatePanel`. L’élément le plus fréquemment vu est l’élément `<ContentTemplate>`, qui encapsule essentiellement le contenu qui sera détenu par le panneau de mise à jour (le contenu pour lequel nous activez le rendu partiel). L’autre élément est l’élément `<Triggers>`, qui spécifie les contrôles sur la page (ou le contrôle utilisateur, si vous en utilisez un) qui déclenchera un rendu partiel du contrôle UpdatePanel dans lequel le &lt;déclenche&gt; élément.

L’élément `<Triggers>` peut contenir n’importe quel nombre de deux nœuds enfants : `<asp:AsyncPostBackTrigger>` et `<asp:PostBackTrigger>`. Ils acceptent deux attributs, `ControlID` et `EventName`et peuvent spécifier n’importe quel contrôle dans l’unité d’encapsulation actuelle (par exemple, si votre contrôle UpdatePanel réside dans un contrôle utilisateur Web, vous ne devez pas tenter de référencer un contrôle sur la page sur laquelle le contrôle utilisateur résidera).

L’élément `<asp:AsyncPostBackTrigger>` est particulièrement utile en ce sens qu’il peut cibler n’importe quel événement à partir d’un contrôle qui existe en tant qu’enfant de *n’importe quel* contrôle UpdatePanel dans l’unité d’encapsulation, pas seulement le UpdatePanel sous lequel ce déclencheur est un enfant. Ainsi, tous les contrôles peuvent être effectués pour déclencher une mise à jour de page partielle.

De même, l’élément `<asp:PostBackTrigger>` peut être utilisé pour déclencher un rendu de page partiel, mais un qui nécessite un aller-retour complet au serveur. Cet élément déclencheur peut également être utilisé pour forcer un rendu de page entière lorsqu’un contrôle déclenche normalement un rendu de page partiel (par exemple, lorsqu’un contrôle `Button` existe dans l’élément `<ContentTemplate>` d’un contrôle UpdatePanel). Là encore, l’élément PostBackTrigger peut spécifier n’importe quel contrôle qui est un enfant de n’importe quel contrôle UpdatePanel dans l’unité actuelle d’encapsulation.

## <a name="lttriggersgt-element-reference"></a>*&lt;déclencheurs&gt; référence d’élément*

*Descendants de balisage :*

| **Tag** | **Description** |
| --- | --- |
| &lt;asp : AsyncPostBackTrigger&gt; | Spécifie un contrôle et un événement qui entraînent une mise à jour de page partielle pour le UpdatePanel qui contient cette référence de déclencheur. |
| &lt;asp : PostBackTrigger&gt; | Spécifie un contrôle et un événement qui entraînent une mise à jour de page complète (actualisation de page complète). Cette balise peut être utilisée pour forcer une actualisation complète lorsqu’un contrôle déclencherait un rendu partiel. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Procédure pas à pas : déclencheurs Cross-UpdatePanel*

1. Créez une nouvelle page ASP.NET avec un objet ScriptManager défini pour activer le rendu partiel. Ajoutez deux UpdatePanel à cette page : dans le premier, incluez un contrôle Label (Label1) et deux contrôles Button (button1 et button2). Button1 doit indiquer Cliquer pour mettre à jour et button2 doit indiquer Cliquer pour mettre à jour ce ou une ligne. Dans le second UpdatePanel, incluez uniquement un contrôle Label (Label2), mais définissez sa propriété ForeColor sur une valeur autre que la valeur par défaut pour le différencier.
2. Affectez à la propriété UpdateMode des deux balises UpdatePanel la valeur **Conditional**.

**Liste 1 : balisage pour default. aspx :** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Dans le gestionnaire d’événements Click pour Button1, définissez Label1. Text et Label2. Text sur un nom dépendant du temps (par exemple, DateTime. Now. ToLongTimeString ()). Pour le gestionnaire d’événements Click pour Button2, définissez uniquement Label1. Text sur la valeur qui dépend du temps.

**Liste 2 : codebehind (tronqué) dans default.aspx.cs :** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Appuyez sur F5 pour générer et exécuter le projet. Notez que lorsque vous cliquez sur mettre à jour les deux panneaux, les deux étiquettes changent de texte ; Toutefois, lorsque vous cliquez sur mettre à jour ce panneau, seules les mises à jour Label1 sont mises à jour.

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))

## <a name="under-the-hood"></a>*Rouages du système*

À l’aide de l’exemple que nous venons de construire, nous pouvons examiner ce que fait ASP.NET AJAX et comment nos déclencheurs multivolets UpdatePanel fonctionnent. Pour ce faire, nous allons utiliser le code HTML source de la page générée, ainsi que l’extension Mozilla Firefox appelée FireBug, nous pouvons examiner facilement les publications (postback) AJAX. Nous utiliserons également l’outil .NET Reflector de Lutz Roeder. Ces deux outils sont disponibles en ligne librement et se trouvent dans une recherche Internet.

Un examen du code source de la page n’affiche presque rien du tout. les contrôles UpdatePanel sont rendus sous la forme de conteneurs `<div>`, et nous pouvons voir les ressources de script incluses dans le `<asp:ScriptManager>`. Il existe également de nouveaux appels spécifiques à AJAX au PageRequestManager qui sont internes à la bibliothèque de scripts client AJAX. Enfin, nous voyons les deux conteneurs UpdatePanel : l’un avec les boutons de `<input>` rendus avec les deux contrôles `<asp:Label>` rendus sous la forme de conteneurs `<span>`. (Si vous examinez l’arborescence DOM dans FireBug, vous remarquerez que les étiquettes sont estompées pour indiquer qu’elles ne produisent pas de contenu visible).

Cliquez sur le bouton mettre à jour ce panneau, et notez que le UpdatePanel supérieur est mis à jour avec l’heure actuelle du serveur. Dans FireBug, choisissez l’onglet Console pour pouvoir examiner la demande. Examinez d’abord les paramètres de la demande de publication :

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))

Notez que UpdatePanel a indiqué au code AJAX côté serveur l’arborescence de contrôle qui a été déclenchée avec le paramètre ScriptManager1 : `Button1` du contrôle `UpdatePanel1`. Maintenant, cliquez sur le bouton mettre à jour les deux panneaux. Ensuite, en examinant la réponse, nous voyons une série de variables délimitées par des barres verticales qui sont définies dans une chaîne. plus précisément, nous voyons le plus haut UpdatePanel, `UpdatePanel1`, a l’intégralité de son HTML envoyé au navigateur. La bibliothèque de scripts client AJAX remplace le contenu HTML d’origine de UpdatePanel par le nouveau contenu via la propriété `.innerHTML`, de sorte que le serveur envoie le contenu modifié à partir du serveur au format HTML.

Maintenant, cliquez sur le bouton mettre à jour les deux panneaux, puis examinez les résultats du serveur. Les résultats sont très similaires : les deux UpdatePanel reçoivent le nouveau code HTML du serveur. Comme avec le rappel précédent, l’état de la page supplémentaire est envoyé.

Comme nous pouvons le voir, dans la mesure où aucun code spécial n’est utilisé pour effectuer un postback AJAX, la bibliothèque de scripts client AJAX est en mesure d’intercepter les publications de formulaire sans code supplémentaire. Les contrôles serveur utilisent automatiquement JavaScript pour qu’ils n’envoient pas automatiquement le formulaire-ASP.NET injecte automatiquement le code pour la validation de formulaire et l’état déjà, principalement réalisé par l’inclusion automatique des ressources de script, la classe PostBackOptions et la classe ClientScriptManager.

Par exemple, considérez un contrôle de case à cocher ; Examinez le code machine de la classe dans .NET Reflector. Pour ce faire, assurez-vous que votre assembly System. Web est ouvert et accédez à la classe `System.Web.UI.WebControls.CheckBox`, en ouvrant la méthode `RenderInputTag`. Recherchez une condition conditionnelle qui vérifie la propriété `AutoPostBack` :

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))

Lorsque la publication automatique est activée sur un contrôle `CheckBox` (via la propriété AutoPostBack true), la balise `<input>` résultante est donc restituée avec un script de gestion des événements ASP.NET dans son attribut `onclick`. L’interception de la soumission du formulaire, puis permet à ASP.NET AJAX d’être injecté dans la page de manière non intrusive, ce qui contribue à éviter toute modification avec rupture potentielle qui peut se produire en utilisant un remplacement de chaîne éventuellement inexact. En outre, cela permet à *n’importe quel* contrôle ASP.NET personnalisé d’utiliser la puissance de ASP.NET AJAX sans aucun code supplémentaire pour prendre en charge son utilisation dans un conteneur UpdatePanel.

La fonctionnalité `<triggers>` correspond aux valeurs initialisées dans l’appel PageRequestManager à \_updateControls (Notez que la bibliothèque de scripts client ASP.NET AJAX utilise la Convention selon laquelle les méthodes, les événements et les noms de champs qui commencent par un trait de soulignement sont marqués comme internes et ne sont pas destinés à être utilisés en dehors de la bibliothèque elle-même). Avec elle, nous pouvons observer les contrôles qui sont destinés à provoquer des publications (postback) AJAX.

Par exemple, nous allons ajouter deux contrôles supplémentaires à la page, en laissant entièrement un contrôle en dehors des UpdatePanel et en laissant un contrôle dans un UpdatePanel. Nous allons ajouter un contrôle de case à cocher dans l’UpdatePanel supérieur et supprimer un DropDownList avec un nombre de couleurs défini dans la liste. Voici le nouveau balisage :

**Liste 3 : nouveau balisage**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Et voici le nouveau code-behind :

**Liste 4 : codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

L’idée derrière cette page est que la liste déroulante sélectionne l’une des trois couleurs pour afficher la deuxième étiquette, que la case à cocher détermine si elle est en gras, et si les étiquettes affichent la date et l’heure. La case à cocher ne doit pas provoquer de mise à jour AJAX, mais la liste déroulante doit, même si elle n’est pas hébergée dans un UpdatePanel.

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))

Comme indiqué dans la capture d’écran ci-dessus, le bouton le plus récent sur lequel vous pouvez cliquer était le bouton droit mettre à jour ce panneau, ce qui a mis à jour le temps le plus élevé indépendamment du temps inférieur. La date a également été déplacée entre les clics, car la date est visible dans l’étiquette du bas. Enfin, l’intérêt est la couleur de l’étiquette inférieure : elle a été mise à jour plus récemment que le texte de l’étiquette, ce qui démontre que l’état du contrôle est important et que les utilisateurs s’attendent à ce qu’elle soit conservée via des publications AJAX. *Toutefois*, l’heure n’a pas été mise à jour. L’heure a été automatiquement régénérée par la persistance de la \_\_champ VIEWSTATE de la page qui est interprétée par le runtime ASP.NET lorsque le contrôle a été rerendu sur le serveur. Le code du serveur ASP.NET AJAX ne reconnaît pas les méthodes dans lesquelles les contrôles changent d’État ; Il se repeuple simplement à partir de l’état d’affichage, puis exécute les événements appropriés.

Toutefois, il faut noter que j’ai initialisé l’heure dans la page\_événement de chargement, le temps aurait été incrémenté correctement. Par conséquent, les développeurs doivent faire attention au fait que le code approprié est exécuté pendant les gestionnaires d’événements appropriés et éviter l’utilisation du chargement de page\_lorsqu’un gestionnaire d’événements de contrôle est approprié.

## <a name="summary"></a>Récapitulatif

Le contrôle UpdatePanel des extensions ASP.NET AJAX est polyvalent et peut utiliser un certain nombre de méthodes pour identifier les événements de contrôle qui doivent provoquer la mise à jour de celui-ci. Il prend en charge la mise à jour automatique par ses contrôles enfants, mais il peut également répondre aux événements de contrôle ailleurs sur la page.

Pour réduire la charge de traitement du serveur, il est recommandé de définir la propriété `ChildrenAsTriggers` d’un UpdatePanel sur `false`, et de faire en sorte que les événements soient activés au lieu d’être inclus par défaut. Cela empêche également les événements inutiles de provoquer des effets potentiellement indésirables, y compris la validation et les modifications apportées aux champs d’entrée. Ces types de bogues peuvent être difficiles à isoler, car la page se met à jour de façon transparente pour l’utilisateur, et la cause peut donc ne pas être immédiatement évidente.

En examinant le fonctionnement interne du modèle d’interception de la publication de formulaire AJAX ASP.NET, nous avons pu déterminer qu’il utilise l’infrastructure déjà fournie par ASP.NET. Ce faisant, il préserve la compatibilité maximale avec les contrôles conçus à l’aide de la même infrastructure, et les intrus au minimum sur tout JavaScript supplémentaire écrit pour la page.

## <a name="bio"></a>Équivalence

Rob Paveza est développeur d’applications .NET senior chez Terralever ([www.Terralever.com](http://www.terralever.com)), une société de marketing interactive de pointe dans Tempe, AZ. Il peut être contacté à [robpaveza@gmail.com](mailto:robpaveza@gmail.com)et son blog se trouve sur [http://geekswithblogs.net/robp/](http://geekswithblogs.net/robp/).

Scott Cate travaille avec les technologies Web de Microsoft depuis 1997 et est le Président de myKB.com ([www.myKB.com](http://www.myKB.com)), où il se spécialise dans l’écriture d’applications ASP.net axées sur les solutions logicielles de la base de connaissances. Scott peut être contacté par e-mail à [scott.cate@myKB.com](mailto:scott.cate@myKB.com) ou son blog à l’adresse [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Précédent](understanding-partial-page-updates-with-asp-net-ajax.md)
> [Suivant](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
