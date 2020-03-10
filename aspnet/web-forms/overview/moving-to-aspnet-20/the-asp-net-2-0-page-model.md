---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Modèle de page ASP.NET 2,0 | Microsoft Docs
author: microsoft
description: Dans ASP.NET 1. x, les développeurs pouvaient choisir entre un modèle de code inline et un modèle de code code-behind. Code-behind peut être implémenté à l’aide de la valeur SRC...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: bcb71b2b5a484e8756406867e08e8aa699a9024d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547648"
---
# <a name="the-aspnet-20-page-model"></a>Modèle de page ASP.NET 2,0

par [Microsoft](https://github.com/microsoft)

> Dans ASP.NET 1. x, les développeurs pouvaient choisir entre un modèle de code inline et un modèle de code code-behind. Code-behind peut être implémenté à l’aide de l’attribut SRC ou de l’attribut CodeBehind de la directive @Page. Dans ASP.NET 2,0, les développeurs ont toujours le choix entre le code inline et le code-behind, mais des améliorations significatives ont été apportées au modèle code-behind.

Dans ASP.NET 1. x, les développeurs pouvaient choisir entre un modèle de code inline et un modèle de code code-behind. Code-behind peut être implémenté à l’aide de l’attribut SRC ou de l’attribut CodeBehind de la directive @Page. Dans ASP.NET 2,0, les développeurs ont toujours le choix entre le code inline et le code-behind, mais des améliorations significatives ont été apportées au modèle code-behind.

## <a name="improvements-in-the-code-behind-model"></a>Améliorations du modèle code-behind

Afin de bien comprendre les modifications apportées au modèle code-behind dans ASP.NET 2,0, il est préférable d’examiner rapidement le modèle tel qu’il existait dans ASP.NET 1. x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Modèle code-behind dans ASP.NET 1. x

Dans ASP.NET 1. x, le modèle code-behind comprenait un fichier ASPX (le WebForm) et un fichier code-behind contenant du code de programmation. Les deux fichiers étaient connectés à l’aide de la directive @Page dans le fichier ASPX. Chaque contrôle de la page ASPX comportait une déclaration correspondante dans le fichier code-behind en tant que variable d’instance. Le fichier code-behind contenait également du code pour la liaison d’événements et le code généré nécessaire pour le concepteur Visual Studio. Ce modèle fonctionnait assez bien, mais étant donné que chaque élément ASP.NET de la page ASPX nécessitait le code correspondant dans le fichier code-behind, il n’existait aucune véritable séparation du code et du contenu. Par exemple, si un concepteur a ajouté un nouveau contrôle serveur à un fichier ASPX en dehors de l’IDE de Visual Studio, l’application s’arrête en raison de l’absence d’une déclaration pour ce contrôle dans le fichier code-behind.

## <a name="the-code-behind-model-in-aspnet-20"></a>Modèle code-behind dans ASP.NET 2,0

ASP.NET 2,0 s’améliore considérablement sur ce modèle. Dans ASP.NET 2,0, code-behind est implémenté à l’aide des nouvelles *classes partielles* fournies dans ASP.NET 2,0. La classe code-behind dans ASP.NET 2,0 est définie comme une classe partielle, ce qui signifie qu’elle ne contient qu’une partie de la définition de classe. La partie restante de la définition de classe est générée dynamiquement par ASP.NET 2,0 à l’aide de la page ASPX au moment de l’exécution ou lors de la précompilation du site Web. Le lien entre le fichier code-behind et la page ASPX est toujours établi à l’aide de la directive @ page. Toutefois, au lieu d’un attribut CodeBehind ou SRC, ASP.NET 2,0 utilise désormais l’attribut CodeFile. L’attribut Inherits est également utilisé pour spécifier le nom de la classe de la page.

Une directive @ page classique peut se présenter comme suit :

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Une définition de classe classique dans un fichier code-behind ASP.NET 2,0 peut se présenter comme suit :

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C#et Visual Basic sont les seuls langages managés qui prennent actuellement en charge les classes partielles. Par conséquent, les développeurs qui utilisent J# ne seront pas en mesure d’utiliser le modèle code-behind dans ASP.NET 2,0.

Le nouveau modèle améliore le modèle code-behind, car les développeurs disposent désormais de fichiers de code qui contiennent uniquement le code qu’ils ont créé. Il fournit également une véritable séparation du code et du contenu, car il n’existe aucune déclaration de variable d’instance dans le fichier code-behind.

> [!NOTE]
> Étant donné que la classe partielle de la page ASPX est l’endroit où la liaison d’événements a lieu, Visual Basic développeurs peuvent réaliser une légère augmentation des performances en utilisant le mot clé Handles dans le code-behind pour lier les événements. C#n’a pas de mot clé équivalent.

## <a name="new--page-directive-attributes"></a>Nouveaux attributs de directive @ page

ASP.NET 2,0 ajoute de nombreux nouveaux attributs à la directive @ page. Les attributs suivants sont nouveaux dans ASP.NET 2,0.

## <a name="async"></a>Async

L’attribut Async vous permet de configurer la page pour qu’elle soit exécutée de façon asynchrone. Nous allons également aborder les pages asynchrones plus loin dans ce module.

## <a name="asynctimeout"></a>AsyncTimeout

A spécifié le délai d’expiration pour les pages asynchrones. La valeur par défaut est 45 secondes.

## <a name="codefile"></a>CodeFile

L’attribut CodeFile est le remplacement de l’attribut CodeBehind dans Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

L’attribut CodeFileBaseClass est utilisé dans les cas où vous souhaitez que plusieurs pages dérivent d’une classe de base unique. En raison de l’implémentation de classes partielles dans ASP.NET, sans cet attribut, une classe de base qui utilise des champs communs partagés pour référencer des contrôles déclarés dans une page ASPX ne fonctionnerait pas correctement, car le moteur de compilation ASP. Networks créera automatiquement de nouveaux membres en fonction des contrôles de la page. Par conséquent, si vous souhaitez une classe de base commune pour deux pages ou plus dans ASP.NET, vous devrez définir spécifier votre classe de base dans l’attribut CodeFileBaseClass, puis dériver chaque classe de pages de cette classe de base. L’attribut CodeFile est également requis lorsque cet attribut est utilisé.

## <a name="compilationmode"></a>Ait

Cet attribut vous permet de définir la propriété CompilationMode de la page ASPX. La propriété CompilationMode est une énumération contenant les valeurs **Always**, **auto**et **Never**. La valeur par défaut est **toujours**. Le paramètre **auto** empêchera ASP.net de compiler la page de manière dynamique si possible. L’exclusion des pages de la compilation dynamique augmente les performances. Toutefois, si une page qui est exclue contient ce code qui doit être compilé, une erreur est levée lors de l’exploration de la page.

## <a name="enableeventvalidation"></a>EnableEventValidation

Cet attribut spécifie si les événements de publication et de rappel sont validés. Quand cette option est activée, les arguments des événements de publication ou de rappel sont vérifiés pour s’assurer qu’ils proviennent du contrôle serveur qui les a restitués à l’origine.

## <a name="enabletheming"></a>EnableTheming

Cet attribut spécifie si les thèmes ASP.NET sont utilisés ou non dans une page. La valeur par défaut est **false**. Les thèmes ASP.NET sont couverts dans le [module 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Cet attribut spécifie si les pragmas de ligne doivent être ajoutés pendant la compilation. Les pragmas de ligne sont des options utilisées par les débogueurs pour marquer des sections de code spécifiques.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Cet attribut spécifie si JavaScript est injecté dans la page afin de conserver la position de défilement entre les publications. Cet attribut a la **valeur false** par défaut.

Lorsque cet attribut a la **valeur true**, ASP.NET ajoute un &lt;&gt; un bloc de script lors de la publication qui ressemble à ceci :

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Notez que le SRC de ce bloc de script est Resource. axd. Cette ressource n’est pas un chemin d’accès physique. Lorsque ce script est demandé, ASP.NET génère dynamiquement le script.

### <a name="masterpagefile"></a>MasterPageFile

Cet attribut spécifie le fichier de page maître pour la page actuelle. Le chemin peut être relatif ou absolu. Les pages maîtres sont couvertes dans le [module 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Cet attribut vous permet de remplacer les propriétés d’apparence de l’interface utilisateur définies par un thème ASP.NET 2,0. Les thèmes sont couverts dans le [module 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Thème

Spécifie le thème de la page. Si aucune valeur n’est spécifiée pour l’attribut StyleSheetTheme, l’attribut Theme remplace tous les styles appliqués aux contrôles de la page.

## <a name="title"></a>Titre

Définit le titre de la page. La valeur spécifiée ici s’affiche dans l’élément &lt;title&gt; de la page rendue.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Définit la valeur de l’énumération ViewStateEncryptionMode. Les valeurs disponibles sont **Always**, **auto**et **Never**. La valeur par défaut est **auto**. Lorsque cet attribut est défini sur la valeur **auto**, ViewState est chiffré est un contrôle qui le demande en appelant la méthode **RegisterRequiresViewStateEncryption** .

## <a name="setting-public-property-values-via-the--page-directive"></a>Définition des valeurs de propriété publiques via la directive @ page

Une autre nouvelle fonctionnalité de la directive @ page dans ASP.NET 2,0 est la possibilité de définir la valeur initiale des propriétés publiques d’une classe de base. Supposons, par exemple, que vous disposiez d’une propriété publique appelée **someText** dans votre classe de base et que vous aimez l’initialiser en **Hello** lorsqu’une page est chargée. Pour ce faire, il vous suffit de définir la valeur de la directive @ page comme suit :

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

L’attribut **someText** de la directive @ page définit la valeur initiale de la propriété someText dans la classe de base sur *Hello !* . La vidéo ci-dessous est une procédure pas à pas de définition de la valeur initiale d’une propriété publique dans une classe de base à l’aide de la directive @ page.

![](the-asp-net-2-0-page-model/_static/image1.png)

[Ouvrir la vidéo en plein écran](the-asp-net-2-0-page-model/_static/setprop1.wmv)

## <a name="new-public-properties-of-the-page-class"></a>Nouvelles propriétés publiques de la classe page

Les propriétés publiques suivantes sont nouvelles dans ASP.NET 2,0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Retourne le chemin d’accès relatif à l’application à la page ou au contrôle. Par exemple, pour une page située dans http://app/folder/page.aspx, la propriété retourne ~/Folder/.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Retourne le chemin d’accès du répertoire virtuel relatif à la page ou au contrôle. Par exemple, pour une page située dans http://app/folder/page.aspx, la propriété retourne ~/Folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Obtient ou définit le délai d’attente utilisé pour la gestion de page asynchrone. (Les pages asynchrones seront couvertes plus loin dans ce module.)

## <a name="clientquerystring"></a>ClientQueryString

Propriété en lecture seule qui retourne la partie de la chaîne de requête de l’URL demandée. Cette valeur est encodée URL. Vous pouvez utiliser la méthode UrlDecode de la classe HttpServerUtility pour la décoder.

## <a name="clientscript"></a>ClientScript

Cette propriété retourne un objet ClientScriptManager qui peut être utilisé pour gérer les émissions ASP. Files du script côté client. (La classe ClientScriptManager est traitée plus loin dans ce module.)

## <a name="enableeventvalidation"></a>EnableEventValidation

Cette propriété détermine si la validation d’événement est activée pour les événements de publication (postback) et de rappel. Quand cette option est activée, les arguments des événements de publication ou de rappel sont vérifiés pour s’assurer qu’ils proviennent du contrôle serveur qui les a restitués à l’origine.

## <a name="enabletheming"></a>EnableTheming

Cette propriété obtient ou définit une valeur booléenne qui spécifie si un thème ASP.NET 2,0 s’applique à la page.

## <a name="form"></a>Formulaire

Cette propriété retourne le formulaire HTML sur la page ASPX en tant qu’objet HtmlForm.

## <a name="header"></a>Header

Cette propriété retourne une référence à un objet HtmlHead qui contient l’en-tête de page. Vous pouvez utiliser l’objet HtmlHead retourné pour récupérer/définir des feuilles de style, des balises méta, etc.

## <a name="idseparator"></a>IdSeparator

Cette propriété en lecture seule obtient le caractère utilisé pour séparer les identificateurs de contrôle lorsque ASP.NET génère un ID unique pour les contrôles d’une page. Elle n'est pas destinée à être utilisée directement à partir du code.

## <a name="isasync"></a>IsAsync

Cette propriété autorise les pages asynchrones. Les pages asynchrones sont présentées plus loin dans ce module.

## <a name="iscallback"></a>IsCallback

Cette propriété en lecture seule retourne la **valeur true** si la page est le résultat d’un rappel. Les rappels sont décrits plus loin dans ce module.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Cette propriété en lecture seule retourne la **valeur true** si la page fait partie d’une publication (postback) entre pages. Les publications (postbacks) entre pages sont traitées plus loin dans ce module.

## <a name="items"></a>Éléments

Retourne une référence à une instance IDictionary qui contient tous les objets stockés dans le contexte de pages. Vous pouvez ajouter des éléments à cet objet IDictionary et ils seront disponibles pendant toute la durée de vie du contexte.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Cette propriété contrôle si ASP.NET émet un JavaScript qui maintient la position de défilement des pages dans le navigateur après une publication (postback). (Les détails de cette propriété ont été abordés plus haut dans ce module.)

## <a name="master"></a>Master

Cette propriété en lecture seule retourne une référence à l’instance MasterPage pour une page à laquelle une page maître a été appliquée.

## <a name="masterpagefile"></a>MasterPageFile

Obtient ou définit le nom de fichier de la page maître de la page. Cette propriété ne peut être définie que dans la méthode PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Cette propriété obtient ou définit la longueur maximale de l’état des pages, en octets. Si la propriété est définie sur un nombre positif, l’état d’affichage des pages sera divisé en plusieurs champs masqués afin qu’il ne dépasse pas le nombre d’octets spécifié. Si la propriété est un nombre négatif, l’état d’affichage n’est pas divisé en segments.

## <a name="pageadapter"></a>PageAdapter

Retourne une référence à l’objet PageAdapter qui modifie la page pour le navigateur demandeur.

## <a name="previouspage"></a>PreviousPage

Retourne une référence à la page précédente dans le cas d’un serveur. Transfer ou d’une publication (postback) entre pages.

## <a name="skinid"></a>SkinID

Spécifie l’apparence ASP.NET 2,0 à appliquer à la page.

## <a name="stylesheettheme"></a>StyleSheetTheme

Cette propriété obtient ou définit la feuille de style appliquée à une page.

## <a name="templatecontrol"></a>TemplateControl

Retourne une référence au contrôle conteneur pour la page.

## <a name="theme"></a>Thème

Obtient ou définit le nom du thème ASP.NET 2,0 appliqué à la page. Cette valeur doit être définie avant la méthode PreInit.

## <a name="title"></a>Titre

Cette propriété obtient ou définit le titre de la page telle qu’elle est obtenue à partir de l’en-tête pages.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Obtient ou définit le ViewStateEncryptionMode de la page. Consultez une présentation détaillée de cette propriété plus haut dans ce module.

## <a name="new-protected-properties-of-the-page-class"></a>Nouvelles propriétés protégées de la classe page

Voici les nouvelles propriétés protégées de la classe page dans ASP.NET 2,0.

## <a name="adapter"></a>Adaptateur

Retourne une référence au ControlAdapter qui restitue la page sur le périphérique qui l’a demandée.

## <a name="asyncmode"></a>AsyncMode

Cette propriété indique si la page est traitée de façon asynchrone ou non. Elle est destinée à être utilisée par le runtime et non directement dans le code.

## <a name="clientidseparator"></a>ClientIDSeparator

Cette propriété retourne le caractère utilisé comme séparateur lors de la création d’ID client uniques pour les contrôles. Elle est destinée à être utilisée par le runtime et non directement dans le code.

## <a name="pagestatepersister"></a>PageStatePersister

Cette propriété retourne l’objet PageStatePersister pour la page. Cette propriété est principalement utilisée par les développeurs de contrôles ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Cette propriété retourne un suffixe unique qui est ajouté au chemin d’accès du fichier pour la mise en cache des navigateurs. La valeur par défaut est \_\_ufps = et un nombre à 6 chiffres.

## <a name="new-public-methods-for-the-page-class"></a>Nouvelles méthodes publiques pour la classe page

Les méthodes publiques suivantes sont nouvelles dans la classe page de ASP.NET 2,0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Cette méthode enregistre les délégués de gestionnaires d’événements pour l’exécution de page asynchrone. Les pages asynchrones sont présentées plus loin dans ce module.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Applique les propriétés d’une feuille de style pages à la page.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Cette méthode a une tâche asynchrone.

### <a name="getvalidators"></a>GetValidators

Retourne une collection de validateurs pour le groupe de validation spécifié ou le groupe de validation par défaut si aucun n’est spécifié.

## <a name="registerasynctask"></a>RegisterAsyncTask

Cette méthode enregistre une nouvelle tâche asynchrone. Les pages asynchrones sont traitées plus loin dans ce module.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Cette méthode indique à ASP.NET que l’état du contrôle des pages doit être persistant.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Cette méthode indique à ASP.NET que l’ViewState des pages requiert un chiffrement.

## <a name="resolveclienturl"></a>ResolveClientUrl

Retourne une URL relative qui peut être utilisée pour les demandes des clients pour les images, etc.

## <a name="setfocus"></a>SetFocus

Cette méthode définit le focus sur le contrôle qui est spécifié lors du chargement initial de la page.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Cette méthode annule l’inscription du contrôle qui lui est passé comme n’exigeant plus de persistance de l’état du contrôle.

## <a name="changes-to-the-page-lifecycle"></a>Modifications apportées au cycle de vie de la page

Le cycle de vie de la page dans ASP.NET 2,0 n’a pas changé de manière spectaculaire, mais il existe de nouvelles méthodes que vous devez connaître. Le cycle de vie de la page ASP.NET 2,0 est présenté ci-dessous.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (nouveauté dans ASP.NET 2,0)

L’événement PreInit est la première étape du cycle de vie à laquelle un développeur peut accéder. L’ajout de cet événement permet de modifier par programme les thèmes ASP.NET 2,0, les pages maîtres, les propriétés d’accès pour un profil ASP.NET 2,0, etc. Si vous êtes dans un état de publication, il est important de se rendre compte que ViewState n’a pas encore été appliqué aux contrôles à ce stade du cycle de vie. Par conséquent, si un développeur modifie une propriété d’un contrôle à ce niveau, il sera probablement remplacé plus tard dans le cycle de vie des pages.

## <a name="init"></a>Init

L’événement Init n’est pas passé de ASP.NET 1. x. C’est là que vous souhaitez lire ou initialiser les propriétés des contrôles sur votre page. À ce niveau, les pages maîtres, les thèmes, etc., sont déjà appliqués à la page.

## <a name="initcomplete-new-in-20"></a>InitComplete (nouveauté dans 2,0)

L’événement InitComplete est appelé à la fin de l’étape d’initialisation des pages. À ce stade du cycle de vie, vous pouvez accéder aux contrôles de la page, mais leur état n’a pas encore été rempli.

## <a name="preload-new-in-20"></a>PreLoad (nouveauté dans 2,0)

Cet événement est appelé après que toutes les données de publication (postback) ont été appliquées et juste avant le chargement de la page\_.

## <a name="load"></a>chargement

L’événement de chargement n’a pas été modifié par rapport à ASP.NET 1. x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (nouveauté dans 2,0)

L’événement LoadComplete est le dernier événement de l’étape de chargement des pages. À ce niveau, toutes les données de publication (postback) et ViewState ont été appliquées à la page.

## <a name="prerender"></a>PreRender

Si vous souhaitez que ViewState soit correctement géré pour les contrôles ajoutés à la page de manière dynamique, l’événement PreRender est la dernière possibilité de les ajouter.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (nouveauté dans 2,0)

À l’étape PreRenderComplete, tous les contrôles ont été ajoutés à la page et la page est prête à être rendue. L’événement PreRenderComplete est le dernier événement déclenché avant l’enregistrement de l’ViewState des pages.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (nouveauté dans 2,0)

L’événement SaveStateComplete est appelé immédiatement après l’enregistrement de l’état du contrôle et de l’état d’affichage de la page. Il s’agit du dernier événement avant que la page ne soit réellement rendue dans le navigateur.

## <a name="render"></a>Afficher

La méthode Render n’a pas été modifiée depuis ASP.NET 1. x. C’est là que la HtmlTextWriter est initialisée et que la page est rendue dans le navigateur.

## <a name="cross-page-postback-in-aspnet-20"></a>Publication (postback) entre pages dans ASP.NET 2,0

Dans ASP.NET 1. x, il était nécessaire de publier des publications sur la même page. Les publications (postbacks) entre pages ne sont pas autorisées. ASP.NET 2,0 ajoute la possibilité d’effectuer une publication sur une page différente par le biais de l’interface IButtonControl. Tout contrôle qui implémente la nouvelle interface IButtonControl (Button, LinkButton et ImageButton en plus des contrôles personnalisés tiers) peut tirer parti de cette nouvelle fonctionnalité par le biais de l’utilisation de l’attribut PostBackUrl. Le code suivant montre un contrôle Button qui publie sur une deuxième page.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Lorsque la page est publiée, la page qui lance la publication est accessible via la propriété PreviousPage sur la deuxième page. Cette fonctionnalité est implémentée via la nouvelle fonction WebForm\_DoPostBackWithOptions côté client que ASP.NET 2,0 rend sur la page lorsqu’un contrôle publie sur une page différente. Cette fonction JavaScript est fournie par le nouveau gestionnaire Resource. axd qui émet le script au client.

La vidéo ci-dessous est une procédure pas à pas d’une publication (postback) entre pages.

![](the-asp-net-2-0-page-model/_static/image2.png)

[Ouvrir la vidéo en plein écran](the-asp-net-2-0-page-model/_static/xpage1.wmv)

## <a name="more-details-on-cross-page-postbacks"></a>Plus de détails sur les publications (postbacks) entre pages

### <a name="viewstate"></a>ViewState

Vous vous êtes peut-être déjà demandé ce qui se passe à l’ViewState à partir de la première page d’un scénario de publication (postback) entre les pages. Après tout, tout contrôle qui n’implémente pas IPostBackDataHandler conserve son état via ViewState, afin d’avoir accès aux propriétés de ce contrôle sur la deuxième page d’une publication (postback) entre les pages, vous devez avoir accès à l’état d’affichage de la page. ASP.NET 2,0 prend en charge ce scénario à l’aide d’un nouveau champ masqué dans la deuxième page appelée \_\_PREVIOUSPAGE. Le champ de formulaire PREVIOUSPAGE \_\_contient l’ViewState de la première page afin que vous puissiez accéder aux propriétés de tous les contrôles de la deuxième page.

### <a name="circumventing-findcontrol"></a>Contournement de la FindControl

Dans la vidéo pas à pas d’une publication (postback) sur plusieurs pages, j’ai utilisé la méthode FindControl pour obtenir une référence au contrôle TextBox sur la première page. Cette méthode fonctionne bien à cet effet, mais la fonction FindControl est coûteuse et requiert l’écriture de code supplémentaire. Heureusement, ASP.NET 2,0 offre une alternative à la méthode FindControl à cet effet, qui fonctionne dans de nombreux scénarios. La directive PreviousPageType vous permet d’avoir une référence fortement typée à la page précédente à l’aide de la valeur TypeName ou de l’attribut VirtualPath. L’attribut TypeName vous permet de spécifier le type de la page précédente, tandis que l’attribut VirtualPath vous permet de faire référence à la page précédente à l’aide d’un chemin d’accès virtuel. Une fois que vous avez défini la directive PreviousPageType, vous devez ensuite exposer les contrôles, etc., auxquels vous souhaitez autoriser l’accès à l’aide des propriétés publiques.

## <a name="lab-1-cross-page-postback"></a>Publication croisée de l’atelier 1 entre les pages

Dans ce laboratoire, vous allez créer une application qui utilise la nouvelle fonctionnalité de publication entre pages de ASP.NET 2,0.

1. Ouvrez Visual Studio 2005 et créez un nouveau site Web ASP.NET.
2. Ajoutez un nouveau WebForm nommé page2. aspx.
3. Ouvrez le default. aspx dans Mode Création et ajoutez un contrôle Button et un contrôle TextBox. 

    1. Donnez au contrôle Button un ID de **submitButton** et le contrôle TEXTBOX un ID de **nom d’utilisateur**.
    2. Affectez à la propriété PostBackUrl du bouton la valeur page2. aspx.
4. Ouvrez page2. aspx en mode Source.
5. Ajoutez une directive @ PreviousPageType comme indiqué ci-dessous :
6. Ajoutez le code suivant à la page\_chargement du code-behind de page2. aspx : 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Générez le projet en cliquant sur générer dans le menu Générer.
8. Ajoutez le code suivant au code-behind pour default. aspx : 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Modifiez la page\_charge dans page2. aspx comme suit : 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Générez le projet.
11. Exécuter le projet.
12. Entrez votre nom dans la zone de texte, puis cliquez sur le bouton.
13. Quel est le résultat ?

## <a name="asynchronous-pages-in-aspnet-20"></a>Pages asynchrones dans ASP.NET 2,0

De nombreux problèmes de contention dans ASP.NET sont dus à la latence des appels externes (tels que les appels de service Web ou de base de données), à la latence d’e/s de fichier, etc. Lorsqu’une demande est effectuée sur une application ASP.NET, ASP.NET utilise l’un de ses threads de travail pour traiter cette demande. Cette requête possède ce thread jusqu’à ce que la demande soit terminée et que la réponse ait été envoyée. ASP.NET 2,0 cherche à résoudre les problèmes de latence avec ces types de problèmes en ajoutant la possibilité d’exécuter des pages de manière asynchrone. Cela signifie qu’un thread de travail peut démarrer la demande, puis transmettre une exécution supplémentaire à un autre thread, ce qui revient rapidement au pool de threads disponible. Lorsque l’e/s de fichier, l’appel de base de données, etc. sont terminés, un nouveau thread est obtenu à partir du pool de threads pour terminer la demande.

La première étape de l’exécution d’une page de manière asynchrone consiste à définir l’attribut **Async** de la directive page comme suit :

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Cet attribut indique à ASP.NET d’implémenter le IHttpAsyncHandler pour la page.

L’étape suivante consiste à appeler la méthode AddOnPreRenderCompleteAsync à un point du cycle de vie de la page avant le prérendu. (Cette méthode est généralement appelée dans la page\_charge.) La méthode AddOnPreRenderCompleteAsync prend deux paramètres : BeginEventHandler et EndEventHandler. BeginEventHandler retourne un IAsyncResult qui est ensuite transmis en tant que paramètre à EndEventHandler.

La vidéo ci-dessous est une procédure pas à pas d’une demande de page asynchrone.

![](the-asp-net-2-0-page-model/_static/image3.png)

[Ouvrir la vidéo en plein écran](the-asp-net-2-0-page-model/_static/async1.wmv)

> [!NOTE]
> Une page Async ne s’affiche pas dans le navigateur tant que le EndEventHandler n’est pas terminé. Aucun doute, mais certains développeurs considèrent que les demandes asynchrones sont similaires aux rappels asynchrones. Il est important de comprendre qu’elles ne le sont pas. L’avantage des demandes asynchrones est que le premier thread de travail peut être retourné au pool de threads pour traiter de nouvelles demandes, réduisant ainsi la contention en raison de la liaison des e/s, etc.

## <a name="script-callbacks-in-aspnet-20"></a>Rappels de script dans ASP.NET 2,0

Les développeurs Web ont toujours cherché des moyens d’empêcher le scintillement associé à un rappel. Dans ASP.NET 1. x, SmartNavigation était la méthode la plus courante pour éviter le scintillement, mais SmartNavigation provoquait des problèmes pour certains développeurs en raison de la complexité de son implémentation sur le client. ASP.NET 2,0 résout ce problème avec les rappels de script. Les rappels de script utilisent XMLHttp pour effectuer des requêtes sur le serveur Web via JavaScript. La requête XMLHttp retourne des données XML qui peuvent ensuite être manipulées par le biais du DOM du navigateur. Le code XMLHTTP est masqué de l’utilisateur par le nouveau gestionnaire Resource. axd.

Plusieurs étapes sont nécessaires pour configurer un rappel de script dans ASP.NET 2,0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Étape 1 : implémenter l’interface ICallbackEventHandler

Pour que ASP.NET reconnaisse votre page comme participant à un rappel de script, vous devez implémenter l’interface ICallbackEventHandler. Vous pouvez le faire dans votre fichier code-behind de la manière suivante :

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Vous pouvez également effectuer cette opération à l’aide de la directive @ Implements comme suit :

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

En général, vous utilisez la directive @ Implements lors de l’utilisation du code inline ASP.NET.

## <a name="step-2--call-getcallbackeventreference"></a>Étape 2 : appeler GetCallbackEventReference

Comme mentionné précédemment, l’appel XMLHTTP est encapsulé dans le gestionnaire Resource. axd. Lorsque votre page est rendue, ASP.NET ajoute un appel à WebForm\_DoCallBack, un script client fourni par Resource. axd. La fonction WebForm\_DoCallBack remplace l' \_\_fonction de type de publication pour un rappel. N’oubliez pas que \_\_la fonction de publication (PostBack) envoie par programmation le formulaire sur la page. Dans un scénario de rappel, vous souhaitez empêcher une publication, donc \_\_la publication ne suffira pas.

> [!NOTE]
> \_\_DoCallBack est toujours rendu sur la page dans un scénario de rappel de script client. Toutefois, il n’est pas utilisé pour le rappel.

Les arguments pour WebForm\_fonction côté client de DoCallBack sont fournis via la fonction côté serveur GetCallbackEventReference qui serait normalement appelée dans la charge de\_de page. Un appel classique à GetCallbackEventReference peut se présenter comme suit :

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> Dans ce cas, cm est une instance de ClientScriptManager. La classe ClientScriptManager sera traitée plus loin dans ce module.

Il existe plusieurs versions surchargées de GetCallbackEventReference. Dans ce cas, les arguments sont les suivants :

`this`

Référence au contrôle où GetCallbackEventReference est appelé. Dans ce cas, il s’agit de la page elle-même.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Argument de chaîne qui sera passé du code côté client à l’événement côté serveur. Dans ce cas, la messagerie instantanée transmettant la valeur d’une liste déroulante appelée ddlCompany.

`ShowCompanyName`

Nom de la fonction côté client qui acceptera la valeur de retour (sous forme de chaîne) de l’événement de rappel côté serveur. Cette fonction est appelée uniquement lorsque le rappel côté serveur a réussi. Par conséquent, pour des raisons de robustesse, il est généralement recommandé d’utiliser la version surchargée de GetCallbackEventReference qui accepte un argument de chaîne supplémentaire en spécifiant le nom d’une fonction côté client à exécuter en cas d’erreur.

`null`

Chaîne représentant une fonction côté client initialisée avant le rappel sur le serveur. Dans ce cas, il n’y a pas de script de ce type, l’argument est donc null.

`true`

Valeur booléenne spécifiant s’il faut ou non exécuter le rappel de façon asynchrone.

L’appel à WebForm\_DoCallBack sur le client passera ces arguments. Par conséquent, lorsque cette page est rendue sur le client, ce code se présente comme suit :

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Notez que la signature de la fonction sur le client est un peu différente. La fonction côté client passe 5 chaînes et une valeur booléenne. La chaîne supplémentaire (null dans l’exemple ci-dessus) contient la fonction côté client qui gérera les erreurs du rappel côté serveur.

## <a name="step-3--hook-the-client-side-control-event"></a>Étape 3 : raccorder l’événement de contrôle côté client

Notez que la valeur de retour de GetCallbackEventReference ci-dessus a été assignée à une variable de chaîne. Cette chaîne est utilisée pour raccorder un événement côté client pour le contrôle qui initialise le rappel. Dans cet exemple, le rappel est initié par une liste déroulante sur la page, donc je souhaite raccorder l’événement *OnChange* .

Pour raccorder l’événement côté client, ajoutez simplement un gestionnaire au balisage côté client comme suit :

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Rappelez-vous que *cbRef* est la valeur de retour de l’appel à GetCallbackEventReference. Il contient l’appel à WebForm\_DoCallBack qui était indiqué ci-dessus.

## <a name="step-4--register-the-client-side-script"></a>Étape 4 : inscrire le script côté client

Rappelez-vous que l’appel à GetCallbackEventReference spécifiait qu’un script côté client appelé **ShowCompanyName** serait exécuté lorsque le rappel côté serveur réussira. Ce script doit être ajouté à la page à l’aide d’une instance ClientScriptManager. (La classe ClientScriptManager sera abordée plus loin dans ce module.) Pour ce faire, procédez comme suit :

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Étape 5 : appeler les méthodes de l’interface ICallbackEventHandler

Le ICallbackEventHandler contient deux méthodes que vous devez implémenter dans votre code. Il s’agit de **RaiseCallbackEvent** et **GetCallbackEvent**.

**RaiseCallbackEvent** prend une chaîne comme argument et ne retourne rien. L’argument de chaîne est passé à partir de l’appel côté client à WebForm\_DoCallBack. Dans ce cas, cette valeur est l’attribut *value* de la liste déroulante appelée ddlCompany. Votre code côté serveur doit être placé dans la méthode RaiseCallbackEvent. Par exemple, si votre rappel fait une WebRequest par rapport à une ressource externe, ce code doit être placé dans RaiseCallbackEvent.

**GetCallbackEvent** est responsable du traitement du retour du rappel au client. Elle n’accepte aucun argument et retourne une chaîne. La chaîne qu’elle retourne est passée comme argument à la fonction côté client, dans ce cas *ShowCompanyName*.

Une fois que vous avez effectué les étapes ci-dessus, vous êtes prêt à effectuer un rappel de script dans ASP.NET 2,0.

![](the-asp-net-2-0-page-model/_static/image4.png)

[Ouvrir la vidéo en plein écran](the-asp-net-2-0-page-model/_static/callback1.wmv)

Les rappels de script dans ASP.NET sont pris en charge dans n’importe quel navigateur prenant en charge l’exécution d’appels XMLHttp. Cela comprend tous les navigateurs modernes actuellement utilisés. Internet Explorer utilise l’objet ActiveX XMLHttp, tandis que d’autres navigateurs modernes (y compris le futur IE 7) utilisent un objet XMLHttp intrinsèque. Pour déterminer par programme si un navigateur prend en charge les rappels, vous pouvez utiliser la propriété **Request. Browser. SupportCallback** . Cette propriété retourne la **valeur true** si le client qui effectue la demande prend en charge les rappels de script.

## <a name="working-with-client-script-in-aspnet-20"></a>Utilisation d’un script client dans ASP.NET 2,0

Les scripts clients dans ASP.NET 2,0 sont gérés par le biais de l’utilisation de la classe ClientScriptManager. La classe ClientScriptManager effectue le suivi des scripts clients à l’aide d’un type et d’un nom. Cela évite que le même script soit inséré par programmation sur une page plusieurs fois.

> [!NOTE]
> Une fois qu’un script a été inscrit avec succès sur une page, toute nouvelle tentative d’inscription du même script entraîne simplement le non-enregistrement du script une deuxième fois. Aucun script dupliqué n’est ajouté et aucune exception n’est levée. Pour éviter tout calcul inutile, vous pouvez utiliser des méthodes pour déterminer si un script est déjà inscrit afin de ne pas essayer de l’inscrire plus d’une fois.

Les méthodes de ClientScriptManager doivent être familières à tous les développeurs ASP.NET actuels :

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Cette méthode ajoute un script en haut de la page rendue. Cela est utile pour ajouter des fonctions qui seront appelées explicitement sur le client.

Il existe deux versions surchargées de cette méthode. Trois des quatre arguments sont communs. Il s'agit des éléments suivants :

`type (string)`

L’argument de ***type*** identifie un type pour le script. Il est généralement judicieux d’utiliser le type de la page (this. GetType ()) pour le type.

`key (string)`

L’argument ***Key*** est une clé définie par l’utilisateur pour le script. Celui-ci doit être unique pour chaque script. Si vous tentez d’ajouter un script avec la même clé et le même type qu’un script déjà ajouté, il ne sera pas ajouté.

`script (string)`

L’argument ***script*** est une chaîne contenant le script à ajouter. Il est recommandé d’utiliser un StringBuilder pour créer le script, puis d’utiliser la méthode ToString () sur StringBuilder pour assigner l’argument de ***script*** .

Si vous utilisez le RegisterClientScriptBlock surchargé qui accepte uniquement trois arguments, vous devez inclure les éléments de script (&lt;&gt; de script et &lt;/script&gt;) dans votre script.

Vous pouvez choisir d’utiliser la surcharge de RegisterClientScriptBlock qui accepte un quatrième argument. Le quatrième argument est une valeur booléenne qui spécifie si ASP.NET doit ajouter des éléments de script pour vous. Si cet argument a la **valeur true**, votre script ne doit pas inclure les éléments de script explicitement.

Utilisez la méthode IsClientScriptBlockRegistered pour déterminer si un script a déjà été inscrit. Cela vous permet d’éviter une tentative de réinscription d’un script qui a déjà été inscrit.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (nouveauté dans 2,0)

La balise RegisterClientScriptInclude crée un bloc de script qui établit un lien vers un fichier de script externe. Il a deux surcharges. L’un prend une clé et une URL. Le deuxième ajoute un troisième argument qui spécifie le type.

Par exemple, le code suivant génère un bloc de script qui crée un lien vers jsfunctions. js à la racine du dossier scripts de l’application :

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Ce code génère le code suivant dans la page rendue :

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Le bloc de script est affiché en bas de la page.

Utilisez la méthode IsClientScriptIncludeRegistered pour déterminer si un script a déjà été inscrit. Cela vous permet d’éviter une tentative de réinscription d’un script.

## <a name="registerstartupscript"></a>RegisterStartupScript

La méthode RegisterStartupScript accepte les mêmes arguments que la méthode RegisterClientScriptBlock. Un script enregistré avec RegisterStartupScript s’exécute après le chargement de la page, mais avant l’événement côté client OnLoad. Dans 1. X, les scripts inscrits avec RegisterStartupScript étaient placés juste avant la balise de fermeture &lt;/formulaire&gt;, tandis que les scripts inscrits avec RegisterClientScriptBlock étaient placés juste après la balise de&gt; de formulaire d’ouverture &lt;. Dans ASP.NET 2,0, les deux sont placés juste avant la balise de fermeture &lt;/formulaire&gt;.

> [!NOTE]
> Si vous inscrivez une fonction avec RegisterStartupScript, cette fonction ne s’exécutera pas tant que vous ne l’aurez pas explicitement appelée dans le code côté client.

Utilisez la méthode IsStartupScriptRegistered pour déterminer si un script a déjà été inscrit et éviter de tenter à nouveau d’enregistrer un script.

## <a name="other-clientscriptmanager-methods"></a>Autres méthodes ClientScriptManager

Voici quelques-unes des autres méthodes utiles de la classe ClientScriptManager.

|  <strong>GetCallbackEventReference</strong>   |                                                 Consultez les rappels de script plus haut dans ce module.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Obtient une référence JavaScript (JavaScript :&lt;appel&gt;) qui peut être utilisée pour effectuer une publication à partir d’un événement côté client.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Obtient une chaîne qui peut être utilisée pour initialiser une publication à partir du client.                                    |
|      <strong>GetWebResourceUrl</strong>       | Retourne une URL à une ressource incorporée dans un assembly. Doit être utilisé conjointement avec <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Inscrit une ressource Web avec la page. Il s’agit de ressources incorporées dans un assembly et gérées par le nouveau gestionnaire Resource. axd.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Inscrit un champ de formulaire masqué avec la page.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Inscrit le code côté client qui s’exécute lorsque le formulaire HTML est envoyé.                                   |
