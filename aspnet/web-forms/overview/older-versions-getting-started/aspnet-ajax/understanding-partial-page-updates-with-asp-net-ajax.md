---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Fonctionnement des mises à jour de pages partielles avec ASP.NET AJAX | Microsoft Docs
author: scottcate
description: La fonctionnalité la plus visible des extensions ASP.NET AJAX est peut-être la possibilité d’effectuer des mises à jour de pages partielles ou incrémentielles sans effectuer une publication (postback) complète sur t...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 4b87cb8f58dbd7f27b16bcb0d488ff361770d4fe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78545968"
---
# <a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Présentation des mises à jour de page partielles avec ASP.NET AJAX

par [Scott Cate](https://github.com/scottcate)

[Télécharger PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> La fonctionnalité la plus visible des extensions ASP.NET AJAX est peut-être la possibilité d’effectuer des mises à jour de pages partielles ou incrémentielles sans effectuer une publication (postback) complète sur le serveur, sans modification du code et modification minimale du balisage. Les avantages sont importants : l’état de vos éléments multimédias (tels qu’Adobe Flash ou Windows Media) est inchangé, les coûts de bande passante sont réduits et le client ne rencontre pas le scintillement généralement associé à une publication (postback).

## <a name="introduction"></a>Introduction

La technologie ASP.NET de Microsoft propose un modèle de programmation orienté objet et piloté par les événements, et l’intègre aux avantages du code compilé. Toutefois, son modèle de traitement côté serveur présente plusieurs inconvénients inhérents à la technologie :

- Les mises à jour de page requièrent un aller-retour au serveur, ce qui nécessite une actualisation de page.
- Les allers-retours ne conservent pas les effets générés par JavaScript ou d’autres technologies côté client (comme Adobe Flash)
- Pendant la publication, les navigateurs autres que Microsoft Internet Explorer ne prennent pas en charge la restauration automatique de la position de défilement. Et même dans Internet Explorer, il y a toujours un scintillement lorsque la page est actualisée.
- Les publications peuvent impliquer une grande quantité de bande passante, car la \_\_champ de formulaire VIEWSTATE peut croître, en particulier lors de la gestion de contrôles tels que le contrôle GridView ou les répéteurs.
- Il n’existe aucun modèle unifié pour l’accès aux services Web par le biais de JavaScript ou d’une autre technologie côté client.

Entrez les extensions AJAX ASP.NET de Microsoft. AJAX, qui correspond à **un** **J** avascript **a** ND **X** ml, est une infrastructure intégrée permettant de fournir des mises à jour de page incrémentielles par le biais de JavaScript multiplateforme, composé de code côté serveur comprenant l’infrastructure Microsoft Ajax, et d’un composant de script appelé bibliothèque de scripts Microsoft Ajax. Les extensions ASP.NET AJAX fournissent également une prise en charge multiplateforme pour l’accès aux services Web ASP.NET via JavaScript.

Ce livre blanc examine la fonctionnalité de mise à jour partielle des pages des extensions ASP.NET AJAX, qui comprend le composant ScriptManager, le contrôle UpdatePanel et le contrôle UpdateProgress, et prend en compte les scénarios dans lesquels elles doivent ou non être utilisé.

Ce livre blanc est basé sur la version bêta 2 de Visual Studio 2008 et le .NET Framework 3,5, qui intègre les extensions AJAX ASP.NET dans la bibliothèque de classes de base (où il s’agissait auparavant d’un composant additionnel disponible pour ASP.NET 2,0). Ce livre blanc suppose également que vous utilisez Visual Studio 2008 et non Visual Web Developer Express Edition. certains modèles de projet référencés peuvent ne pas être disponibles pour les utilisateurs de Visual Web Developer Express.

## <a name="partial-page-updates"></a>Mises à jour de pages partielles

La fonctionnalité la plus visible des extensions ASP.NET AJAX est peut-être la possibilité d’effectuer des mises à jour de pages partielles ou incrémentielles sans effectuer une publication (postback) complète sur le serveur, sans modification du code et modification minimale du balisage. Les avantages sont importants : l’état de vos éléments multimédias (tels qu’Adobe Flash ou Windows Media) est inchangé, les coûts de bande passante sont réduits et le client ne rencontre pas le scintillement généralement associé à une publication (postback).

La possibilité d’intégrer un rendu de page partiel est intégrée à ASP.NET avec des modifications minimes dans votre projet.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Procédure pas à pas : intégration du rendu partiel dans un projet existant

1. Dans Microsoft Visual Studio 2008, créez un nouveau projet de site Web ASP.NET en accédant à <em>fichier</em> <em>-&gt; nouveau</em> <em>-&gt; site web</em> et en sélectionnant ASP.net site Web dans la boîte de dialogue. Vous pouvez le nommer comme vous le souhaitez, et vous pouvez l’installer dans le système de fichiers ou dans Internet Information Services (IIS).
2. La page vide par défaut s’affiche avec le balisage de base ASP.NET (un formulaire côté serveur et une directive de `@Page`). Déposez une étiquette appelée `Label1` et un bouton appelé `Button1` dans la page de l’élément Form. Vous pouvez définir leurs propriétés de texte comme vous le souhaitez.
3. Dans Mode Création, double-cliquez sur `Button1` pour générer un gestionnaire d’événements code-behind. Dans ce gestionnaire d’événements, définissez `Label1.Text` sur lequel vous avez cliqué sur le bouton ! .

**Liste 1 : balisage pour default. aspx avant l’activation du rendu partiel**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Liste 2 : codebehind (tronqué) dans default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Appuyez sur F5 pour lancer votre site Web. Visual Studio vous invite à ajouter un fichier Web. config pour activer le débogage. faites-le. Lorsque vous cliquez sur le bouton, vous remarquez que la page est actualisée pour modifier le texte de l’étiquette et qu’il y a un bref scintillement lorsque la page est redessinée.
2. Après avoir fermé la fenêtre de votre navigateur, revenez à Visual Studio et à la page de balisage. Faites défiler la boîte à outils Visual Studio vers le dessous et recherchez l’onglet Extensions AJAX. (Si vous ne disposez pas de cet onglet parce que vous utilisez une version antérieure des extensions AJAX ou Atlas, consultez la procédure pas à pas pour inscrire les éléments de la boîte à outils des extensions AJAX plus loin dans ce livre blanc ou installer la version actuelle avec le Windows Installer téléchargeable à partir du site Web).

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Cliquez pour afficher l’image en taille réelle](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))

1. <em>Problème connu :</em> Si vous installez Visual Studio 2008 sur un ordinateur sur lequel Visual Studio 2005 est déjà installé avec les extensions AJAX ASP.NET 2,0, Visual Studio 2008 importera les éléments de la boîte à outils des extensions AJAX. Vous pouvez déterminer si c’est le cas en examinant l’info-bulle des composants. ils doivent indiquer la version 3.5.0.0. S’ils indiquent la version 2.0.0.0, vous avez importé vos anciens éléments de boîte à outils et devrez les importer manuellement à l’aide de la boîte de dialogue choisir des éléments de boîte à outils dans Visual Studio. Vous ne pouvez pas ajouter de contrôles de version 2 par le biais du concepteur.

2. Avant le début de la balise `<asp:Label>`, créez une ligne d’espace blanc, puis double-cliquez sur le contrôle UpdatePanel dans la boîte à outils. Notez qu’une nouvelle directive `@Register` est incluse en haut de la page, indiquant que les contrôles de l’espace de noms System. Web. UI doivent être importés à l’aide du préfixe `asp:`.
3. Faites glisser la balise de `</asp:UpdatePanel>` de fermeture au-delà de la fin de l’élément Button, afin que l’élément soit correctement formé avec les contrôles Label et Button encapsulés.
4. Après l’ouverture de la balise `<asp:UpdatePanel>`, commencez à ouvrir une nouvelle balise. Notez que IntelliSense vous invite à saisir deux options. Dans ce cas, créez une balise `<ContentTemplate>`. Veillez à inclure cette balise autour de votre étiquette et du bouton afin que le balisage soit correctement formé.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Cliquez pour afficher l’image en taille réelle](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))

1. N’importe où dans l’élément `<form>`, incluez un contrôle ScriptManager en double-cliquant sur l’élément `ScriptManager` dans la boîte à outils.
2. Modifiez la balise `<asp:ScriptManager>` de manière à inclure l’attribut `EnablePartialRendering= true`.

**Liste 3 : balisage pour default. aspx avec rendu partiel activé**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Ouvrez votre fichier Web. config. Notez que Visual Studio a ajouté automatiquement une référence de compilation à System. Web. extensions. dll.

1. Nouveautés de Visual Studio 2008 : le fichier Web. config qui est fourni avec les modèles de projet de site Web ASP.NET inclut automatiquement toutes les références nécessaires aux extensions AJAX ASP.NET, et inclut des sections commentées des informations de configuration qui peuvent être sans commenté pour activer des fonctionnalités supplémentaires. Visual Studio 2005 avait des modèles similaires lors de l’installation des extensions 2,0 ASP.NET AJAX. Toutefois, dans Visual Studio 2008, les extensions AJAX sont refusées par défaut (autrement dit, elles sont référencées par défaut, mais peuvent être supprimées en tant que références).

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Cliquez pour afficher l’image en taille réelle](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))

1. Appuyez sur F5 pour lancer votre site Web. Notez qu’aucune modification de code source n’a été requise pour prendre en charge le rendu partiel : seul le balisage a été modifié.

Lorsque vous lancez votre site Web, vous devez voir que le rendu partiel est maintenant activé, car lorsque vous cliquez sur le bouton, il n’y a pas de scintillement, et aucune modification n’est apportée à la position de défilement de la page (cet exemple ne montre pas cela). Si vous devez examiner la source rendue de la page après avoir cliqué sur le bouton, cela confirme qu’en fait une publication n’a pas eu lieu. le texte de l’étiquette d’origine fait toujours partie du balisage source, et l’étiquette a été modifiée par le biais de JavaScript.

Visual Studio 2008 ne semble pas être fourni avec un modèle prédéfini pour un site Web compatible ASP.NET AJAX. Toutefois, un tel modèle était disponible dans Visual Studio 2005 si les extensions AJAX Visual Studio 2005 et ASP.NET 2,0 ont été installées. Par conséquent, la configuration d’un site Web et le démarrage avec le modèle de site Web compatible AJAX seront probablement encore plus faciles, car le modèle doit inclure un fichier Web. config entièrement configuré (prenant en charge toutes les extensions ASP.NET AJAX, y compris l’accès aux services Web). et sérialisation JSON-JavaScript Object Notation) et comprend un UpdatePanel et ContentTemplate dans la page principale de Web Forms par défaut. L’activation du rendu partiel avec cette page par défaut revient simplement à revisiter l’étape 10 de cette procédure pas à pas et à déposer des contrôles sur la page.

## <a name="the-scriptmanager-control"></a>Le contrôle ScriptManager

## <a name="scriptmanager-control-reference"></a>Référence du contrôle ScriptManager

Propriétés activées pour le balisage :

| **Nom de la propriété** | **Type** | **Description** |
| --- | --- | --- |
| AllowCustomErrors-redirection | Bool | Spécifie s’il faut utiliser la section d’erreur personnalisée du fichier Web. config pour gérer les erreurs. |
| AsyncPostBackError-Message | Chaîne | Obtient ou définit le message d’erreur envoyé au client si une erreur est générée. |
| AsyncPostBack-Timeout | Int32 | Obtient ou définit la durée par défaut pendant laquelle un client doit attendre la fin de la requête asynchrone. |
| EnableScript-Globalization | Bool | Obtient ou définit une valeur indiquant si la globalisation de script est activée. |
| EnableScript-Localization | Bool | Obtient ou définit une valeur indiquant si la localisation de script est activée. |
| ScriptLoadTimeout | Int32 | Détermine le nombre de secondes autorisées pour le chargement de scripts dans le client |
| ScriptMode | Enum (auto, Debug, Release, Inherit) | Obtient ou définit s’il faut restituer les versions Release des scripts |
| ScriptPath | Chaîne | Obtient ou définit le chemin d’accès racine de l’emplacement des fichiers de script à envoyer au client. |

Propriétés de code uniquement :

| **Nom de la propriété** | **Type** | **Description** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | Obtient des détails sur le proxy du service d’authentification ASP.NET qui sera envoyé au client. |
| IsDebuggingEnabled | Bool | Obtient une valeur indiquant si le débogage de script et de code est activé. |
| IsInAsyncPostback | Bool | Obtient une valeur indiquant si la page est actuellement dans une demande de publication asynchrone. |
| ProfileService | ProfileService-Manager | Obtient des détails sur le proxy du service de profilage ASP.NET qui sera envoyé au client. |
| scripts ; | Collection&lt;de référence de script&gt; | Obtient une collection de références de script qui seront envoyées au client. |
| Services | Service de&lt;de collection-référence&gt; | Obtient une collection de références de proxy de service Web qui seront envoyées au client. |
| SupportsPartialRendering | Bool | Obtient une valeur indiquant si le client actuel prend en charge le rendu partiel. Si cette propriété retourne la **valeur false**, toutes les demandes de page seront des publications (postbacks) standard. |

Méthodes de code publiques :

| **Nom de la méthode** | **Type** | **Description** |
| --- | --- | --- |
| SetFocus (chaîne) | Void | Définit le focus du client sur un contrôle particulier lorsque la demande est terminée. |

Descendants de balisage :

| **Tag** | **Description** |
| --- | --- |
| &lt;AuthenticationService&gt; | Fournit des détails sur le proxy au service d’authentification ASP.NET. |
| &lt;ProfileService&gt; | Fournit des détails sur le proxy au service de profilage ASP.NET. |
| &lt;Scripts&gt; | Fournit des références de script supplémentaires. |
| &lt;asp : ScriptReference&gt; | Indique une référence de script spécifique. |
| &lt;Service&gt; | Fournit des références de service Web supplémentaires qui auront des classes proxy générées. |
| &lt;asp : ServiceReference&gt; | Indique une référence de service Web spécifique. |

Le contrôle ScriptManager est le principal noyau pour les extensions ASP.NET AJAX. Il permet d’accéder à la bibliothèque de scripts (y compris le système étendu de type de script côté client), prend en charge le rendu partiel et fournit une prise en charge complète des services ASP.NET supplémentaires (tels que l’authentification et le profilage, mais également d’autres services Web). Le contrôle ScriptManager fournit également la prise en charge de la globalisation et de la localisation pour les scripts clients.

## <a name="providing-alternative-and-supplemental-scripts"></a>Fournir des scripts alternatifs et supplémentaires

Tandis que les extensions Microsoft ASP.NET 2,0 AJAX incluent l’ensemble du code de script dans les éditions Debug et Release en tant que ressources incorporées dans les assemblys référencés, les développeurs sont libres de rediriger ScriptManager vers des fichiers de script personnalisés, ainsi que de s’inscrire autres scripts nécessaires.

Pour remplacer la liaison par défaut pour les scripts inclus en général (tels que ceux qui prennent en charge l’espace de noms sys. WebForms et le système de typage personnalisé), vous pouvez vous inscrire pour l’événement `ResolveScriptReference` de la classe ScriptManager. Lorsque cette méthode est appelée, le gestionnaire d’événements a la possibilité de modifier le chemin d’accès au fichier de script en question ; le gestionnaire de scripts enverra ensuite une copie différente ou personnalisée des scripts au client.

En outre, les références de script (représentées par la classe `ScriptReference`) peuvent être incluses par programmation ou par le biais du balisage. Pour ce faire, vous pouvez modifier par programmation la collection `ScriptManager.Scripts`, ou inclure des balises `<asp:ScriptReference>` sous la balise `<Scripts>`, qui est un enfant de premier niveau du contrôle ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>Gestion des erreurs personnalisée pour UpdatePanel

Bien que les mises à jour soient gérées par les déclencheurs spécifiés par les contrôles UpdatePanel, la prise en charge de la gestion des erreurs et des messages d’erreur personnalisés est gérée par l’instance de contrôle ScriptManager d’une page. Pour ce faire, vous exposez un événement, `AsyncPostBackError`, à la page qui peut ensuite fournir une logique de gestion des exceptions personnalisée.

En consommant l’événement AsyncPostBackError, vous pouvez spécifier la propriété `AsyncPostBackErrorMessage`, ce qui entraîne le déclenchement d’une boîte d’alerte à l’achèvement du rappel.

La personnalisation côté client est également possible au lieu d’utiliser la zone d’alerte par défaut. par exemple, vous souhaiterez peut-être afficher un élément `<div>` personnalisé plutôt que la boîte de dialogue modale par défaut du navigateur. Dans ce cas, vous pouvez gérer l’erreur dans le script client :

**Liste 5 : script côté client pour l’affichage d’erreurs personnalisées**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Tout simplement, le script ci-dessus inscrit un rappel avec le runtime AJAX côté client pour le moment où la requête asynchrone est terminée. Il vérifie ensuite si une erreur a été signalée et, si tel est le cas, traite les détails de celui-ci, en indiquant enfin au runtime que l’erreur a été gérée dans le script personnalisé.

## <a name="globalization-and-localization-support"></a>Prise en charge de la globalisation et de la localisation

Le contrôle ScriptManager fournit une prise en charge complète de la localisation des chaînes de script et des composants de l’interface utilisateur. Toutefois, cette rubrique est en dehors de la portée de ce livre blanc. Pour plus d’informations, consultez le livre blanc sur la prise en charge de la globalisation dans ASP.NET AJAX Extensions.

## <a name="the-updatepanel-control"></a>Le contrôle UpdatePanel

## <a name="updatepanel-control-reference"></a>Référence du contrôle UpdatePanel

Propriétés activées pour le balisage :

| **Nom de la propriété** | **Type** | **Description** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Spécifie si les contrôles enfants appellent automatiquement l’actualisation lors de la publication. |
| RenderMode | enum (bloc, Inline) | Spécifie la façon dont le contenu sera présenté visuellement. |
| UpdateMode | enum (Always, Conditional) | Spécifie si UpdatePanel est toujours actualisé pendant un rendu partiel ou s’il est actualisé uniquement lorsqu’un déclencheur est atteint. |

Propriétés de code uniquement :

| **Nom de la propriété** | **Type** | **Description** |
| --- | --- | --- |
| IsInPartialRendering | bool | Obtient une valeur indiquant si UpdatePanel prend en charge le rendu partiel pour la requête actuelle. |
| ContentTemplate | ITemplate | Obtient le modèle de balisage pour la demande de mise à jour. |
| ContentTemplateContainer | Contrôle | Obtient le modèle de programmation pour la demande de mise à jour. |
| déclencheurs | UpdatePanel-TriggerCollection | Obtient la liste des déclencheurs associés à l’UpdatePanel actuel. |

Méthodes de code publiques :

| **Nom de la méthode** | **Type** | **Description** |
| --- | --- | --- |
| Update() | Void | Met à jour l’UpdatePanel spécifié par programme. Permet à une requête de serveur de déclencher un rendu partiel d’un UpdatePanel qui n’est pas déclenché autrement. |

Descendants de balisage :

| **Tag** | **Description** |
| --- | --- |
| &lt;ContentTemplate&gt; | Spécifie le balisage à utiliser pour restituer le résultat de rendu partiel. Enfant de &lt;asp : UpdatePanel&gt;. |
| &lt;Déclencheurs&gt; | Spécifie une collection de *n* contrôles associés à la mise à jour de cet UpdatePanel. Enfant de &lt;asp : UpdatePanel&gt;. |
| &lt;asp : AsyncPostBackTrigger&gt; | Spécifie un déclencheur qui appelle un rendu de page partiel pour l’UpdatePanel donné. Cela peut ou non être un contrôle comme descendant de l’UpdatePanel en question. Granularité du nom de l’événement. Enfant de &lt;déclencheurs&gt;. |
| &lt;asp : PostBackTrigger&gt; | Spécifie un contrôle qui provoque l’actualisation de la page entière. Cela peut ou non être un contrôle comme descendant de l’UpdatePanel en question. Granularité de l’objet. Enfant de &lt;déclencheurs&gt;. |

Le contrôle `UpdatePanel` est le contrôle qui délimite le contenu côté serveur qui prendra part à la fonctionnalité de rendu partiel des extensions AJAX. Il n’existe aucune limite au nombre de contrôles UpdatePanel pouvant figurer dans une page et ils peuvent être imbriqués. Chaque UpdatePanel est isolé, afin que chacun puisse fonctionner indépendamment (vous pouvez avoir deux UpdatePanel s’exécutant en même temps, en affichant différentes parties de la page, indépendamment de la publication de la page).

Le contrôle UpdatePanel traite principalement les déclencheurs de contrôle. par défaut, tout contrôle contenu dans le `ContentTemplate` d’un UpdatePanel qui crée une publication (postback) est inscrit en tant que déclencheur pour le UpdatePanel. Cela signifie que l’UpdatePanel peut fonctionner avec les contrôles liés aux données par défaut (tels que le GridView), avec les contrôles utilisateur, et ils peuvent être programmés dans le script.

Par défaut, lorsqu’un rendu de page partiel est déclenché, tous les contrôles UpdatePanel de la page sont actualisés, que les contrôles UpdatePanel soient définis ou non pour une telle action. Par exemple, si un UpdatePanel définit un contrôle de bouton et que vous cliquez sur ce contrôle de bouton, tous les contrôles UpdatePanel de cette page sont actualisés par défaut. Cela est dû au fait que, par défaut, la propriété `UpdateMode` du UpdatePanel est définie sur `Always`. Vous pouvez également définir la propriété UpdateMode sur `Conditional`, ce qui signifie que UpdatePanel est actualisé uniquement si un déclencheur spécifique est atteint.

## <a name="custom-control-notes"></a>Notes de contrôle personnalisées

Un UpdatePanel peut être ajouté à n’importe quel contrôle utilisateur ou contrôle personnalisé. Toutefois, la page sur laquelle ces contrôles sont inclus doit également inclure un contrôle ScriptManager avec la propriété EnablePartialRendering définie sur **true**.

L’une des façons dont vous pouvez en tenir compte lors de l’utilisation de contrôles Web personnalisés consiste à substituer la méthode `CreateChildControls()` protégée de la classe `CompositeControl`. En procédant ainsi, vous pouvez injecter un UpdatePanel entre les enfants du contrôle et le monde extérieur si vous déterminez que la page prend en charge le rendu partiel. dans le cas contraire, vous pouvez simplement superposer les contrôles enfants dans un conteneur `Control` instance.

## <a name="updatepanel-considerations"></a>Considérations relatives à UpdatePanel

UpdatePanel fonctionne comme une zone noire, en encapsulant les publications ASP.NET dans le contexte d’un XMLHttpRequest JavaScript. Toutefois, il existe des considérations de performances significatives à garder à l’esprit, à la fois en termes de comportement et de vitesse. Pour comprendre le fonctionnement d’UpdatePanel, vous devez examiner l’échange AJAX pour vous permettre de mieux décider quand son utilisation est appropriée. L’exemple suivant utilise un site existant et, Mozilla Firefox avec l’extension Firebug (Firebug capture les données XMLHttpRequest).

Prenons un formulaire qui, entre autres choses, a une zone de texte Code postal qui est censée remplir un champ ville et état sur un formulaire ou un contrôle. Ce formulaire collecte en fin de compte les informations d’appartenance, notamment le nom, l’adresse et les informations de contact d’un utilisateur. Il y a de nombreuses considérations de conception à prendre en compte, en fonction des exigences d’un projet spécifique.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Cliquez pour afficher l’image en taille réelle](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Cliquez pour afficher l’image en taille réelle](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))

Dans l’itération d’origine de cette application, un contrôle a été créé et incorporait l’intégralité des données d’inscription de l’utilisateur, y compris le code postal, la ville et l’État. Le contrôle entier a été encapsulé dans un UpdatePanel et déposé sur un formulaire Web. Lorsque le code postal est entré par l’utilisateur, UpdatePanel détecte l’événement (l’événement TextChanged correspondant dans le serveur principal, soit en spécifiant des déclencheurs, soit en utilisant la propriété ChildrenAsTriggers définie sur true). AJAX publie tous les champs dans le UpdatePanel, tels qu’ils sont capturés par FireBug (voir le diagramme à droite).

Comme l’indique la capture d’écran, les valeurs de chaque contrôle de l’UpdatePanel sont remises (dans ce cas, elles sont toutes vides), ainsi que le champ ViewState. Tout ce qui a été dit, sur 9kb de données est envoyé, alors que seuls cinq octets de données étaient nécessaires pour effectuer cette requête particulière. La réponse est encore plus encombrée : au total, 57kb est envoyé au client, simplement pour mettre à jour un champ de texte et un champ de liste déroulante.

Il peut également être intéressant de voir Comment ASP.NET AJAX met à jour la présentation. La partie réponse de la demande de mise à jour de UpdatePanel s’affiche dans l’affichage de la console Firebug sur la gauche. Il s’agit d’une chaîne spéciale délimitée par des barres verticales qui est décomposée par le script client, puis réassemblée sur la page. Plus précisément, ASP.NET AJAX définit la propriété *InnerHtml* de l’élément HTML sur le client qui représente votre UpdatePanel. À mesure que le navigateur recrée le DOM, il y a un léger retard, en fonction de la quantité d’informations qui doivent être traitées.

La régénération du DOM déclenche un certain nombre de problèmes supplémentaires :

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Cliquez pour afficher l’image en taille réelle](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))

- Si l’élément HTML ayant le focus est dans le UpdatePanel, il perd le focus. Ainsi, pour les utilisateurs qui ont appuyé sur la touche Tab pour quitter la zone de texte Code postal, la prochaine destination aurait été la zone de texte City. Toutefois, une fois que l’UpdatePanel a actualisé l’affichage, le formulaire n’avait plus le focus et l’appui sur la touche Tab aurait commencé à mettre en surbrillance les éléments de focus (tels que les liens).
- Si n’importe quel type de script côté client personnalisé est utilisé pour accéder aux éléments DOM, les références rendues persistantes par les fonctions peuvent devenir obsolètes après une publication (postback) partielle.

Les UpdatePanel ne sont pas destinés à être des solutions de rattrapage. Au lieu de cela, ils fournissent une solution rapide pour certaines situations, notamment le prototypage, les mises à jour de contrôle réduit et fournissent une interface familière aux développeurs ASP.NET qui peuvent être familiarisés avec le modèle objet .NET, mais moins, avec le DOM. Il existe plusieurs alternatives qui peuvent améliorer les performances, selon le scénario de l’application :

- Envisagez d’utiliser PageMethods et JSON (JavaScript Object Notation) permet au développeur d’appeler des méthodes statiques sur une page comme si un appel de service Web était appelé. Étant donné que les méthodes sont statiques, aucun État n’est nécessaire ; l’appelant de script fournit les paramètres, et le résultat est retourné de manière asynchrone.
- Envisagez d’utiliser un service Web et JSON si un seul contrôle doit être utilisé dans plusieurs emplacements d’une application. Cela nécessite très peu de travail spécial et fonctionne de manière asynchrone.

L’incorporation de fonctionnalités via des services Web ou des méthodes de page présente également des inconvénients. Tout d’abord, les développeurs ASP.NET ont généralement tendance à créer de petits composants de fonctionnalités dans des contrôles utilisateur (fichiers. ascx). Les méthodes de page ne peuvent pas être hébergées dans ces fichiers ; ils doivent être hébergés dans la classe de la page. aspx réelle. Les services Web, de la même façon, doivent être hébergés dans la classe. asmx. En fonction de l’application, cette architecture peut violer le principe de responsabilité unique, en ce que la fonctionnalité d’un composant unique est désormais répartie entre deux ou plusieurs composants physiques qui peuvent avoir peu ou pas de liens cohérents.

Enfin, si une application requiert l’utilisation de UpdatePanel, les instructions suivantes doivent être utiles pour le dépannage et la maintenance.

- **Imbriquez les UpdatePanel le moins possible, non seulement dans les unités, mais également dans les unités de code.** Par exemple, si vous avez un UpdatePanel sur une page qui encapsule un contrôle, alors que ce contrôle contient également un UpdatePanel, qui contient un autre contrôle contenant un UpdatePanel, il s’agit d’une imbrication de plusieurs unités. Cela permet de conserver clairement les éléments qui doivent être actualisés et d’empêcher les actualisations inattendues des UpdatePanel enfants.
- **Conservez la valeur false pour la propriété *ChildrenAsTriggers* et définissez explicitement les événements de déclenchement.** L’utilisation de la collection `<Triggers>` est un moyen bien plus clair de gérer les événements et peut empêcher un comportement inattendu, en aidant les tâches de maintenance et en forçant un développeur à s’abonner à un événement.
- **Utilisez la plus petite unité possible pour obtenir des fonctionnalités.** Comme indiqué dans la discussion sur le service de code postal, le fait d’envelopper uniquement le minimum minimal réduit le temps nécessaire au serveur, le traitement total et l’encombrement de l’échange client-serveur, ce qui améliore les performances.

## <a name="the-updateprogress-control"></a>Contrôle UpdateProgress

## <a name="updateprogress-control-reference"></a>Référence du contrôle UpdateProgress

Propriétés activées pour le balisage :

| **Nom de la propriété** | **Type** | **Description** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | Chaîne | Spécifie l’ID de l’UpdatePanel sur lequel ce UpdateProgress doit signaler. |
| DisplayAfter | Int | Spécifie le délai d’attente en millisecondes avant que ce contrôle ne s’affiche après le début de la requête asynchrone. |
| DynamicLayout | bool | Spécifie si la progression est rendue dynamiquement. |

Descendants de balisage :

| **Tag** | **Description** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Contient l’ensemble de modèles de contrôle pour le contenu qui sera affiché avec ce contrôle. |

Le contrôle UpdateProgress vous donne une mesure de commentaires afin de conserver l’intérêt de vos utilisateurs lors du travail nécessaire au transport vers le serveur. Cela peut aider vos utilisateurs à savoir que vous effectuez une tâche, même si elle n’est pas visible, en particulier dans la mesure où la plupart des utilisateurs sont utilisés pour actualiser la page et voir la mise en surbrillance de la barre d’État.

Notez que les contrôles UpdateProgress peuvent apparaître n’importe où dans une hiérarchie de pages. Toutefois, dans les cas où une publication (postback) partielle est lancée à partir d’un UpdatePanel enfant (où un UpdatePanel est imbriqué dans un autre UpdatePanel), les publications qui déclenchent le UpdatePanel enfant entraînent l’affichage des modèles UpdateProgress pour l’enfant UpdatePanel et le UpdatePanel parent. Toutefois, si le déclencheur est un enfant direct de l’UpdatePanel parent, seuls les modèles UpdateProgress associés au parent sont affichés.

## <a name="summary"></a>Récapitulatif

Les extensions Microsoft ASP.NET AJAX sont des produits élaborés conçus pour aider à rendre le contenu Web plus accessible et fournir une expérience utilisateur plus riche à vos applications Web. Dans le cadre des extensions ASP.NET AJAX, les contrôles de rendu de page partiels, notamment ScriptManager, les contrôles UpdatePanel et UpdateProgress, sont certains des composants les plus visibles de la boîte à outils.

Le composant ScriptManager intègre la configuration du client JavaScript pour les extensions, et permet aux différents composants côté serveur et côté client de fonctionner ensemble avec un investissement de développement minimal.

Le contrôle UpdatePanel est la balise magique apparente-le balisage dans le UpdatePanel peut avoir un code-behind côté serveur et ne pas déclencher d’actualisation de page. Les contrôles UpdatePanel peuvent être imbriqués et peuvent dépendre de contrôles dans d’autres UpdatePanel. Par défaut, les UpdatePanel gèrent les publications (postbacks) appelées par leurs contrôles descendants, bien que cette fonctionnalité puisse être affinée, soit de manière déclarative, soit par programme.

Lors de l’utilisation du contrôle UpdatePanel, les développeurs doivent être conscients de l’impact sur les performances. Les alternatives possibles incluent les services Web et les méthodes de page, bien que la conception de l’application soit prise en compte.

Le contrôle UpdateProgress permet à l’utilisateur de savoir qu’elle n’est pas ignorée, et que la demande Behind-the-Scenes est en cours alors que la page ne fait rien pour répondre à l’entrée utilisateur. Il comprend également la possibilité d’abandonner les résultats de rendu partiels.

Ensemble, ces outils aident à créer une expérience utilisateur riche et transparente en rendant le travail du serveur moins apparent pour l’utilisateur et en interrompant moins le Workflow.

## <a name="bio"></a>Équivalence

Scott Cate travaille avec les technologies Web de Microsoft depuis 1997 et est le Président de myKB.com ([www.myKB.com](http://www.myKB.com)), où il se spécialise dans l’écriture d’applications ASP.net axées sur les solutions logicielles de la base de connaissances. Scott peut être contacté par e-mail à [scott.cate@myKB.com](mailto:scott.cate@myKB.com) ou son blog à l’adresse [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Next](understanding-asp-net-ajax-updatepanel-triggers.md)
