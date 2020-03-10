---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Contrôles serveur | Microsoft Docs
author: microsoft
description: ASP.NET 2,0 améliore les contrôles serveur de nombreuses façons. Dans ce module, nous allons aborder certaines des modifications architecturales apportées à la façon dont ASP.NET 2,0 et Visual Studio 200...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: c02a633013f061c09141d4f98871848c011a799e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641441"
---
# <a name="server-controls"></a>Contrôles serveur

par [Microsoft](https://github.com/microsoft)

> ASP.NET 2,0 améliore les contrôles serveur de nombreuses façons. Dans ce module, nous allons aborder certaines des modifications architecturales apportées à la façon dont ASP.NET 2,0 et Visual Studio 2005 traitent les contrôles serveur.

ASP.NET 2,0 améliore les contrôles serveur de nombreuses façons. Dans ce module, nous allons aborder certaines des modifications architecturales apportées à la façon dont ASP.NET 2,0 et Visual Studio 2005 traitent les contrôles serveur.

## <a name="view-state"></a>État d’affichage

La modification principale de l’état d’affichage dans ASP.NET 2,0 est une réduction spectaculaire de la taille. Prenons l’exemple d’une page contenant uniquement un contrôle calendrier. Voici l’état d’affichage dans ASP.NET 1,1.

[!code-css[Main](server-controls/samples/sample1.css)]

Voici maintenant l’état d’affichage sur une page identique dans ASP.NET 2,0.

[!code-css[Main](server-controls/samples/sample2.css)]

Il s’agit d’une modification assez importante. en considérant que l’état d’affichage est reporté sur le câble, cette modification peut permettre aux développeurs d’accroître considérablement les performances. La réduction de la taille de l’état d’affichage est principalement due à la façon dont nous la gérant en interne. N’oubliez pas que l’état d’affichage est une chaîne encodée en base64. Pour mieux comprendre les modifications apportées à l’état d’affichage dans ASP.NET 2,0, observons les valeurs décodées des exemples ci-dessus.

Voici l’état d’affichage 1,1 décodé :

[!code-css[Main](server-controls/samples/sample3.css)]

Cela peut sembler incompréhensible, mais il y a un modèle ici. Dans ASP.NET 1. x, nous avons utilisé des caractères uniques pour identifier les types de données et les valeurs délimitées à l’aide des &lt;&gt; caractères. Le « t » dans l’exemple d’état d’affichage ci-dessus représente un triplet. L’triplet contient une paire de ArrayLists (le « l » représente une ArrayList.) L’une de ces ArrayList contient un Int32 (« i ») avec une valeur de 1 et l’autre contient un autre triplet. L’triplet contient une paire de ArrayLists, etc. Le point important à retenir est que nous utilisons des triplets qui contiennent des paires, que nous identifions les types de données via une lettre et que nous utilisons les caractères &lt; et &gt; comme délimiteurs.

Dans ASP.NET 2,0, l’état d’affichage décodé est légèrement différent.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Vous devez noter une modification énorme de l’aspect de l’état d’affichage décodé. Cette modification comporte plusieurs fondements architecturaux. L’état d’affichage dans ASP.NET 1. x a utilisé le LosFormatter pour sérialiser les données. Dans 2,0, nous utilisons la nouvelle classe ObjectStateFormatter. Cette classe a été spécifiquement conçue pour faciliter la sérialisation et la désérialisation de l’état d’affichage et de l’état du contrôle. (L’état du contrôle sera abordé dans la section suivante.) Il existe de nombreux avantages en modifiant la méthode selon laquelle la sérialisation et la désérialisation ont lieu. L’un des plus spectaculaires est le fait que contrairement à LosFormatter qui utilise un TextWriter, ObjectStateFormatter utilise un BinaryWriter. Cela permet à ASP.NET 2,0 de stocker l’état d’affichage d’une série d’octets au lieu de chaînes. Prenez, par exemple, un entier. Dans ASP.NET 1,1, un entier nécessitait 4 octets d’état d’affichage. Dans ASP.NET 2,0, ce même entier nécessite 1 octet. D’autres améliorations ont été apportées pour réduire la quantité d’état d’affichage stockée. Les valeurs DateTime, par exemple, sont maintenant stockées à l’aide de TickCount au lieu d’une chaîne.

Comme si tout cela ne suffisait pas, une attention particulière a été accordée au fait que l’un des plus grands consommateurs de l’état d’affichage dans 1. x était le DataGrid et les contrôles similaires. L’un des inconvénients majeurs des contrôles, tels que le DataGrid dans lequel l’état d’affichage est concerné, est qu’il contient souvent de grandes quantités d’informations répétées. Dans ASP.NET 1. x, les informations répétées étaient simplement stockées à plusieurs reprises, ce qui a entraîné un gonflement de l’état d’affichage. Dans ASP.NET 2,0, nous utilisons la nouvelle classe IndexedString pour stocker ces données. Si une chaîne est répétée, nous stockons simplement le jeton pour le IndexedString et l’index dans une table d’objets IndexedString en cours d’exécution.

## <a name="control-state"></a>État du contrôle

L’une des principales poignées que les développeurs avaient avec l’état d’affichage était la taille qu’il a ajoutées à la charge utile HTTP. Comme mentionné précédemment, l’un des plus grands consommateurs de l’état d’affichage est le contrôle DataGrid. Pour éviter les énormes quantités d’état d’affichage générées par un DataGrid, de nombreux développeurs ont simplement désactivé l’état d’affichage pour ce contrôle. Malheureusement, cette solution n’était pas toujours bonne. L’état d’affichage dans ASP.NET 1. x contient non seulement les données nécessaires à la fonctionnalité correcte du contrôle. Elle contient également des informations relatives à l’état de l’interface utilisateur du contrôle. Cela signifie que si vous souhaitez autoriser la pagination sur un DataGrid, vous devez activer l’état d’affichage même si vous n’avez pas besoin de toutes les informations d’interface utilisateur contenues dans l’état d’affichage. Il s’agit d’un scénario tout-en-un.

Dans ASP.NET 2,0, l’état du contrôle résout ce problème correctement via l’introduction de l’état du contrôle. L’état du contrôle contient les données qui sont absolument nécessaires pour la fonctionnalité appropriée d’un contrôle. Contrairement à l’état d’affichage, l’état du contrôle ne peut pas être désactivé. Par conséquent, il est important que les données stockées dans l’état du contrôle soient contrôlées avec soin.

> [!NOTE]
> L’état du contrôle est persistant avec l’état d’affichage dans le champ de formulaire masqué \_\_VIEWSTATE.

Cette vidéo est une procédure pas à pas d’état d’affichage et d’État du contrôle.

![](server-controls/_static/image1.png)

[Ouvrir la vidéo en plein écran](server-controls/_static/state1.wmv)

Pour permettre à un contrôle serveur de lire et d’écrire dans l’état du contrôle, vous devez effectuer trois étapes.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Étape 1 : appeler la méthode RegisterRequiresControlState

La méthode RegisterRequiresControlState informe ASP.NET qu’un contrôle doit rendre persistant l’état du contrôle. Il accepte un argument de type Control qui est le contrôle en cours d’enregistrement.

Il est important de noter que l’inscription n’est pas conservée de la demande à la demande. Par conséquent, cette méthode doit être appelée à chaque demande si un contrôle doit rendre persistant l’état du contrôle. Il est recommandé d’appeler la méthode dans OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Étape 2 : remplacer SaveControlState

La méthode SaveControlState enregistre les modifications d’état de contrôle pour un contrôle depuis la dernière publication. Elle retourne un objet représentant l’état du contrôle.

## <a name="step-3-override-loadcontrolstate"></a>Étape 3 : remplacer LoadControlState

La méthode LoadControlState charge l’état enregistré dans un contrôle. La méthode accepte un argument de type Object qui conserve l’état enregistré du contrôle.

## <a name="full-xhtml-compliance"></a>Conformité XHTML complète

N’importe quel développeur Web connaît l’importance des normes dans les applications Web. Pour assurer la maintenance d’un environnement de développement basé sur des normes, ASP.NET 2,0 est entièrement conforme à XHTML. Par conséquent, toutes les balises sont rendues selon les normes XHTML dans les navigateurs qui prennent en charge HTML 4,0 ou une version ultérieure.

La définition DOCTYPE dans ASP.NET 1,1 était la suivante :

[!code-html[Main](server-controls/samples/sample6.html)]

Dans ASP.NET 2,0, la définition DOCTYPE par défaut est la suivante :

[!code-html[Main](server-controls/samples/sample7.html)]

Si vous le souhaitez, vous pouvez modifier la conformité XHTML par défaut via le nœud xhtmlConformance dans le fichier de configuration. Par exemple, le nœud suivant dans le fichier Web. config remplacera la conformité XHTML par XHTML 1,0 strict :

[!code-xml[Main](server-controls/samples/sample8.xml)]

Si vous le souhaitez, vous pouvez également configurer ASP.NET pour utiliser la configuration héritée utilisée dans ASP.NET 1. x, comme suit :

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Rendu adaptatif à l’aide d’adaptateurs

Dans ASP.NET 1. x, le fichier de configuration contenait une section &lt;browserCaps&gt; qui remplissait un objet HttpBrowserCapabilities. Cet objet permettait à un développeur de déterminer quel appareil effectue une demande particulière et de restituer le code de manière appropriée. Dans ASP.NET 2,0, le modèle a été amélioré et utilise désormais la nouvelle classe ControlAdapter. La classe ControlAdapter remplace des événements dans le cycle de vie du contrôle et contrôle le rendu des contrôles en fonction des fonctionnalités de l’agent utilisateur. Les fonctionnalités d’un agent utilisateur spécifique sont définies par un fichier de définition de navigateur (fichier avec une extension de fichier. Browser) stocké dans le c:\Windows\Microsoft.NET\Framework\v2.0.\*\*\*\*\Config\browsers.

> [!NOTE]
> La classe ControlAdapter est une classe abstraite.

À l’instar de la section &lt;browserCaps&gt; dans 1. x, le fichier de définition de navigateur utilise une expression régulière pour analyser la chaîne de l’agent utilisateur afin d’identifier le navigateur demandeur. Il définit des fonctionnalités particulières pour cet agent utilisateur. ControlAdapter restitue le contrôle via la méthode Render. Par conséquent, si vous substituez la méthode Render, vous ne devez pas appeler Render sur la classe de base. Cela peut entraîner un rendu à deux reprises, une fois pour l’adaptateur et une fois pour le contrôle lui-même.

## <a name="developing-a-custom-adapter"></a>Développement d’un adaptateur personnalisé

Vous pouvez développer votre propre adaptateur personnalisé en héritant de ControlAdapter. En outre, vous pouvez hériter de la classe abstraite PageAdapter dans les cas où un adaptateur est nécessaire pour une page. Le mappage des contrôles à votre adaptateur personnalisé s’effectue à l’aide de l’élément &lt;controlAdapters&gt; dans le fichier de définition de navigateur. Par exemple, le code XML suivant d’un fichier de définition de navigateur mappe le contrôle de menu à la classe MenuAdapter :

[!code-html[Main](server-controls/samples/sample10.html)]

À l’aide de ce modèle, il devient assez facile pour un développeur de contrôle de cibler un appareil ou un navigateur particulier. Il est également assez simple pour un développeur d’avoir un contrôle total sur la façon dont les pages sont rendues sur chaque appareil.

## <a name="per-device-rendering"></a>Rendu par appareil

Les propriétés du contrôle serveur dans ASP.NET 2,0 peuvent être spécifiées par appareil à l’aide d’un préfixe spécifique au navigateur. Par exemple, le code ci-dessous modifie le texte d’une étiquette en fonction de l’appareil utilisé pour parcourir la page.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Quand la page contenant cette étiquette est parcourue à partir d’Internet Explorer, l’étiquette affiche le texte indiquant « vous accédez à partir d’Internet Explorer ». Lorsque vous accédez à la page à partir de Firefox, l’étiquette affiche le texte « vous explorez à partir de Firefox ». Quand la page est parcourue à partir d’un autre appareil, elle affiche « vous accédez à partir d’un appareil inconnu ». Une propriété peut être spécifiée à l’aide de cette syntaxe spéciale.

## <a name="setting-focus"></a>Définir le focus

Les développeurs ASP.NET 1. x ont souvent demandé comment définir le focus initial sur un contrôle particulier. Par exemple, sur une page de connexion, il est utile que la zone de texte ID d’utilisateur soit active lors du premier chargement de la page. Dans ASP.NET 1. x, cela nécessite l’écriture d’un script côté client. Bien qu’un tel script soit une tâche simple, il n’est plus nécessaire dans ASP.NET 2,0 grâce à la méthode SetFocus. La méthode SetFocus accepte un argument indiquant le contrôle qui doit recevoir le focus. Cet argument peut être l’ID client du contrôle sous forme de chaîne ou le nom du contrôle serveur en tant qu’objet de contrôle. Par exemple, pour définir le focus initial sur un contrôle TextBox appelé txtUserID lors du premier chargement de la page, ajoutez le code suivant à la page\_charge :

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--ou

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2,0 utilise le gestionnaire Resource. axd (abordé précédemment) pour afficher une fonction côté client qui définit le focus. Le nom de la fonction côté client est WebForm\_l’autofocus comme indiqué ici :

[!code-html[Main](server-controls/samples/sample14.html)]

Vous pouvez également utiliser la méthode Focus pour un contrôle pour définir le focus initial sur ce contrôle. La méthode Focus dérive de la classe Control et est disponible pour tous les contrôles ASP.NET 2,0. Il est également possible de définir le focus sur un contrôle particulier lorsqu’une erreur de validation se produit. Cela sera traité dans un module ultérieur.

## <a name="new-server-controls-in-aspnet-20"></a>Nouveaux contrôles serveur dans ASP.NET 2,0

Voici les nouveaux contrôles serveur dans ASP.NET 2,0. Nous allons examiner plus en détail certains d’entre eux dans les modules ultérieurs.

## <a name="imagemap-control"></a>Contrôle ImageMap

Le contrôle ImageMap vous permet d’ajouter des zones réactives à une image qui peut lancer une publication ou accéder à une URL. Trois types de zones réactives sont disponibles : CircleHotSpot, RectangleHotSpot et PolygonHotSpot. Les zones réactives sont ajoutées via un éditeur de collections dans Visual Studio ou par programme dans le code. Aucune interface utilisateur n’est disponible pour dessiner des zones réactives sur une image. Les coordonnées et la taille ou le rayon de la zone réactive doivent être spécifiés de façon déclarative. Il n’y a pas non plus de représentation visuelle d’une zone réactive dans le concepteur. Si une zone réactive est configurée pour accéder à une URL, l’URL est spécifiée via la propriété NavigateUrl de la zone réactive. Dans le cas d’un hotspot de publication, la propriété PostBackValue vous permet de passer une chaîne dans la publication qui peut être récupérée dans le code côté serveur.

![Éditeur de collections HotSpot dans Visual Studio](server-controls/_static/image1.jpg)

**Figure 1**: éditeur de collections hotspot dans Visual Studio

## <a name="bulletedlist-control"></a>BulletedList, contrôle

Le contrôle BulletedList est une liste à puces qui peut facilement être liée aux données. La liste peut être triée (numérotée) ou non ordonnée via la propriété BulletStyle. Chaque élément de la liste est représenté par un objet ListItem.

![Contrôle BulletedList dans Visual Studio](server-controls/_static/image1.gif)

**Figure 2**: contrôle BulletedList dans Visual Studio

## <a name="hiddenfield-control"></a>HiddenField, contrôle

Le contrôle HiddenField ajoute un champ de formulaire masqué à votre page, dont la valeur est disponible dans le code côté serveur. La valeur d’un champ de formulaire masqué est généralement censée rester inchangée entre les publications. Toutefois, un utilisateur malveillant peut modifier la valeur avant la publication. Dans ce cas, le contrôle HiddenField déclenche l’événement ValueChanged. Si vous avez des informations sensibles dans le contrôle HiddenField et que vous souhaitez vous assurer qu’elles restent inchangées, vous devez gérer l’événement ValueChanged dans votre code.

## <a name="fileupload-control"></a>FileUpload, contrôle

Le contrôle FileUpload dans ASP.NET 2,0 permet de charger des fichiers sur un serveur Web via une page ASP.NET. Ce contrôle est assez similaire à la classe HtmlInputFile ASP.NET 1. x, à quelques exceptions près. Dans ASP.NET 1. x, il était recommandé de vérifier la valeur null de la propriété PostedFile afin de déterminer si vous avez un fichier correct. Le contrôle FileUpload dans ASP.NET 2,0 ajoute une nouvelle propriété HasFile que vous pouvez utiliser dans le même but et c’est un peu plus efficace.

La propriété PostedFile est toujours disponible pour l’accès à un objet HttpPostedFile, mais certaines fonctionnalités de HttpPostedFile sont désormais disponibles intrinsèquement avec le contrôle FileUpload. Par exemple, pour enregistrer un fichier téléchargé dans ASP.NET 1. x, vous appelez la méthode SaveAs sur l’objet HttpPostedFile. En utilisant le contrôle FileUpload dans ASP.NET 2,0, vous appelez la méthode SaveAs sur le contrôle FileUpload lui-même.

Une autre modification significative du comportement 2,0 (et probablement la modification la plus importante) est qu’il n’est plus nécessaire de charger un fichier entier chargé en mémoire avant de l’enregistrer. Dans 1. x, tout fichier téléchargé est enregistré entièrement dans la mémoire avant d’être écrit sur le disque. Cette architecture empêche le chargement de fichiers volumineux.

Dans ASP.NET 2,0, l’attribut requestLengthDiskThreshold de l’élément httpRuntime vous permet de configurer le nombre de kilo-octets qui sont conservés dans une mémoire tampon avant d’être écrits sur le disque.

**Important**: la documentation MSDN (et la documentation ailleurs) spécifie que cette valeur est exprimée en octets (et non en kilo-octets) et que la valeur par défaut est 256. La valeur est en fait spécifiée en kilo-octets et la valeur par défaut est 80. En ayant une valeur par défaut de 80K, nous garantissons que la mémoire tampon ne finit pas sur le tas des objets volumineux.

## <a name="wizard-control"></a>Contrôle Wizard

Il est assez courant de rencontrer des développeurs ASP.NET qui éprouvent des difficultés à rassembler des informations dans une série de « pages » à l’aide de panneaux ou en effectuant un transfert d’une page à l’autres. Plus souvent, ce n’est pas le cas, mais il s’agit d’une opération qui prend du temps. Le nouveau contrôle Wizard résout les problèmes en autorisant les étapes linéaires et non linéaires dans une interface d’assistant que les utilisateurs connaissent. Le contrôle Wizard présente des formulaires d’entrée en une série d’étapes. Chaque étape est d’un type particulier spécifié par la propriété StepType du contrôle. Les types d’étapes disponibles sont les suivants :

| **Type d’étape** | **Explication** |
| --- | --- |
| Auto | L’Assistant détermine automatiquement le type d’étape en fonction de sa position dans la hiérarchie d’étape. |
| Start | La première étape, souvent utilisée pour présenter une instruction introductive. |
| Étape | Étape normale. |
| Terminer | Dernière étape, généralement utilisée pour présenter un bouton pour terminer l’Assistant. |
| Terminé | Affiche un message communiquant la réussite ou l’échec. |

> [!NOTE]
> Le contrôle de l’Assistant effectue le suivi de son état à l’aide de l’état du contrôle ASP.NET. Par conséquent, la propriété EnableViewState peut avoir la valeur false sans préjudiciable.

Cette vidéo est une procédure pas à pas du contrôle de l’Assistant.

![](server-controls/_static/image2.png)

[Ouvrir la vidéo en plein écran](server-controls/_static/wizard1.wmv)

## <a name="localize-control"></a>Localiser le contrôle

Le contrôle Localize est semblable à un contrôle Literal. Toutefois, le contrôle Localize a une propriété **mode** qui contrôle la façon dont le balisage qui lui est ajouté est rendu. La propriété mode prend en charge les valeurs suivantes :

| **Mode** | **Explication** |
| --- | --- |
| Transform | Le balisage est transformé selon le protocole du navigateur qui effectue la demande. |
| Passage | Le balisage est rendu tel quel. |
| Encoder | Le balisage ajouté au contrôle est encodé à l’aide de HtmlEncode. |

## <a name="multiview-and-view-controls"></a>Contrôles MultiView et View

Le contrôle MultiView agit comme un conteneur pour les contrôles d’affichage, et le contrôle View agit comme un conteneur (un peu comme un contrôle Panel) pour d’autres contrôles. Chaque vue d’un contrôle MultiView est représentée par un contrôle View unique. Le premier contrôle View de la MultiView est View 0, le second est View 1, etc. Vous pouvez basculer entre les vues en spécifiant le ActiveViewIndex du contrôle MultiView.

## <a name="substitution-control"></a>Contrôle de substitution

Le contrôle de substitution est utilisé conjointement avec la mise en cache ASP.NET. Dans les cas où vous souhaitez tirer parti de la mise en cache, mais que vous avez des parties d’une page qui doivent être mises à jour à chaque requête (en d’autres termes, des parties d’une page qui sont exemptes de la mise en cache), le composant de substitution fournit une solution exceptionnelle. Le contrôle n’affiche pas réellement de sortie proprement dite. Au lieu de cela, elle est liée à une méthode dans le code côté serveur. Lorsque la page est demandée, la méthode est appelée et le balisage retourné est rendu à la place du contrôle de substitution.

La méthode à laquelle le contrôle de substitution est lié est spécifiée via la propriété **MethodName** . Cette méthode doit respecter les critères suivants :

- Il doit s’agir d’une méthode statique (Shared en VB).
- Il accepte un paramètre de type HttpContext.
- Elle retourne une chaîne représentant le balisage qui doit remplacer le contrôle sur la page.

Le contrôle de substitution n’a pas la possibilité de modifier un autre contrôle sur la page, mais il a accès au HttpContext actuel via son paramètre.

## <a name="gridview-control"></a>Contrôle GridView

Le contrôle GridView est le remplacement du contrôle DataGrid. Ce contrôle sera traité plus en détail dans un module ultérieur.

## <a name="detailsview-control"></a>Contrôle DetailsView

Le contrôle DetailsView vous permet d’afficher un enregistrement unique à partir d’une source de données et de le modifier ou de le supprimer. Il est abordé plus en détail dans un module ultérieur.

## <a name="formview-control"></a>FormView, contrôle

Le contrôle FormView est utilisé pour afficher un enregistrement unique à partir d’une source de donnée dans une interface configurable. Il est abordé plus en détail dans un module ultérieur.

## <a name="accessdatasource-control"></a>AccessDataSource, contrôle

Le contrôle AccessDataSource est utilisé pour lier une base de données Access. Il est abordé plus en détail dans un module ultérieur.

## <a name="objectdatasource-control"></a>Contrôle ObjectDataSource

Le contrôle ObjectDataSource est utilisé pour prendre en charge une architecture à trois niveaux, de sorte que les contrôles peuvent être liés aux données à un objet métier de couche intermédiaire plutôt qu’à un modèle à deux niveaux dans lequel les contrôles sont liés directement à la source de données. Il sera abordé plus en détail dans un module ultérieur.

## <a name="xmldatasource-control"></a>XmlDataSource, contrôle

Le contrôle XmlDataSource est utilisé pour lier des données à une source de données XML. Il est abordé plus en détail dans un module ultérieur.

## <a name="sitemapdatasource-control"></a>SiteMapDataSource, contrôle

Le contrôle SiteMapDataSource fournit la liaison de données pour les contrôles de navigation de site en fonction d’un plan de site. Il sera abordé plus en détail dans un module ultérieur.

## <a name="sitemappath-control"></a>SiteMapPath, contrôle

Le contrôle SiteMapPath affiche une série de liens de navigation communément appelés breadcrumbs. Il est abordé plus en détail dans un module ultérieur.

## <a name="menu-control"></a>Contrôle Menu

Le contrôle menu affiche des menus dynamiques en DHTML. Il est abordé plus en détail dans un module ultérieur.

## <a name="treeview-control"></a>TreeView, contrôle

Le contrôle TreeView permet d’afficher une arborescence hiérarchique des données. Il est abordé plus en détail dans un module ultérieur.

## <a name="login-control"></a>Contrôle de connexion

Le contrôle de connexion fournit un mécanisme permettant de se connecter à un site Web. Il est abordé plus en détail dans un module ultérieur.

## <a name="loginview-control"></a>Contrôle LoginView

Le contrôle LoginView permet d’afficher différents modèles en fonction de l’état de connexion d’un utilisateur. Il est abordé plus en détail dans un module ultérieur.

## <a name="passwordrecovery-control"></a>Contrôle PasswordRecovery

Le contrôle PasswordRecovery est utilisé pour récupérer les mots de passe oubliés par les utilisateurs d’une application ASP.NET. Il est abordé plus en détail dans un module ultérieur.

## <a name="loginstatus"></a>LoginStatus

Le contrôle LoginStatus affiche l’état de la connexion d’un utilisateur. Il est abordé plus en détail dans un module ultérieur.

## <a name="loginname"></a>LoginName

Le contrôle LoginName affiche le nom d’utilisateur d’un utilisateur après s’être connecté à une application ASP.NET. Il est abordé plus en détail dans un module ultérieur.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard est un Assistant configurable qui donne aux utilisateurs la possibilité de créer un compte d’appartenance ASP.NET pour une utilisation dans une application ASP.NET. Il est abordé plus en détail dans un module ultérieur.

## <a name="changepassword"></a>Connaissant

Le contrôle ChangePassword permet aux utilisateurs de modifier leur mot de passe pour une application ASP.NET. Il est abordé plus en détail dans un module ultérieur.

## <a name="various-webparts"></a>Différents composants WebPart

ASP.NET 2,0 est fourni avec différents composants WebPart. Celles-ci seront traitées en détail dans un module ultérieur.
