---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Pages maîtres et ASP.NET AJAX (C#) | Microsoft Docs
author: rick-anderson
description: Décrit les options d’utilisation de ASP.NET AJAX et des pages maîtres. Examine l’utilisation de la classe ScriptManagerProxy. explique comment les différents fichiers JS sont chargés...
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 8cd1d57b4d2aa01654da53ab2b1cc01f71ad8a87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629163"
---
# <a name="master-pages-and-aspnet-ajax-c"></a>Pages maîtres et ASP.NET AJAX (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Décrit les options d’utilisation de ASP.NET AJAX et des pages maîtres. Examine l’utilisation de la classe ScriptManagerProxy. explique comment les différents fichiers JS sont chargés selon que ScriptManager est utilisé ou non dans la page maître ou la page de contenu.

## <a name="introduction"></a>Introduction

Au cours des dernières années, de plus en plus de développeurs ont créé des applications Web compatibles [Ajax](http://en.wikipedia.org/wiki/Ajax_(programming)). Un site Web compatible AJAX utilise un certain nombre de technologies Web associées pour offrir une expérience utilisateur plus réactive. La création d’applications ASP.NET compatibles AJAX est étonnamment facile grâce à l' [infrastructure ASP.NET AJAX](../../../../ajax/index.md)de Microsoft. ASP.NET AJAX est intégré à ASP.NET 3,5 et Visual Studio 2008. Il est également disponible en téléchargement séparé pour les applications ASP.NET 2,0.

Lors de la création de pages Web compatibles AJAX avec l’infrastructure ASP.NET AJAX, vous devez ajouter précisément un [contrôle ScriptManager](https://msdn.microsoft.com/library/bb398863.aspx) à chaque page qui utilise le Framework. Comme son nom l’indique, ScriptManager gère le script côté client utilisé dans les pages Web compatibles AJAX. Au minimum, ScriptManager émet du code HTML qui indique au navigateur de télécharger les fichiers JavaScript qui compensent la bibliothèque cliente ASP.NET AJAX. Il peut également être utilisé pour inscrire des fichiers JavaScript personnalisés, des services Web prenant en charge les scripts et des fonctionnalités de service d’application personnalisées.

Si votre site utilise des pages maîtres (comme il le devrait), vous n’avez pas nécessairement besoin d’ajouter un contrôle ScriptManager à chaque page de contenu unique. au lieu de cela, vous pouvez ajouter un contrôle ScriptManager à la page maître. Ce didacticiel montre comment ajouter le contrôle ScriptManager à la page maître. Elle explique également comment utiliser le contrôle ScriptManagerProxy pour inscrire des scripts personnalisés et des services de script dans une page de contenu spécifique.

> [!NOTE]
> Ce didacticiel n’explore pas la conception ou la création d’applications Web compatibles AJAX avec l’infrastructure ASP.NET AJAX. Pour plus d’informations sur l’utilisation d’AJAX, consultez les vidéos et [didacticiels](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md) [ASP.NET AJAX](../../../videos/aspnet-ajax/index.md) , ainsi que les ressources indiquées dans la section de lecture supplémentaire à la fin de ce didacticiel.

## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Examen du balisage émis par le contrôle ScriptManager

Le contrôle ScriptManager émet le balisage qui indique au navigateur de télécharger les fichiers JavaScript qui compensent la bibliothèque cliente ASP.NET AJAX. Il ajoute également un peu de code JavaScript Inline à la page qui initialise cette bibliothèque. Le balisage suivant montre le contenu qui est ajouté à la sortie rendue d’une page qui comprend un contrôle ScriptManager :

[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

Les balises `<script src="url"></script>` indiquent au navigateur de télécharger et d’exécuter le fichier JavaScript à l' *adresse URL*. Le ScriptManager émet trois balises de ce type ; l’une fait référence au fichier `WebResource.axd`, tandis que les deux autres référencent le fichier `ScriptResource.axd`. Ces fichiers n’existent pas réellement en tant que fichiers dans votre site Web. Au lieu de cela, lorsqu’une demande pour l’un de ces fichiers arrive sur le serveur Web, le moteur ASP.NET examine la chaîne de requête et retourne le contenu JavaScript approprié. Le script fourni par ces trois fichiers JavaScript externes constitue la bibliothèque cliente ASP.NET AJAX Framework. Les autres balises `<script>` émises par ScriptManager incluent un script inline qui initialise cette bibliothèque.

Les références de script externe et le script inline émis par ScriptManager sont essentiels pour une page qui utilise l’infrastructure AJAX ASP.NET, mais n’est pas nécessaire pour les pages qui n’utilisent pas le Framework. Par conséquent, il est possible que vous souhaitiez uniquement ajouter un ScriptManager aux pages qui utilisent l’infrastructure AJAX ASP.NET. Cela suffit, mais si vous avez de nombreuses pages qui utilisent le Framework, vous finirez par ajouter le contrôle ScriptManager à toutes les pages (tâche répétitive) pour indiquer le moins. Vous pouvez également ajouter un ScriptManager à la page maître, qui injecte ensuite ce script nécessaire dans toutes les pages de contenu. Avec cette approche, vous n’avez pas besoin de vous souvenir d’ajouter un ScriptManager à une nouvelle page qui utilise l’infrastructure ASP.NET AJAX, car elle est déjà incluse par la page maître. L’étape 1 vous guide dans l’ajout d’un ScriptManager à la page maître.

> [!NOTE]
> Si vous prévoyez d’inclure des fonctionnalités AJAX dans l’interface utilisateur de votre page maître, vous n’avez aucun choix à inclure dans la page maître.

L’un des inconvénients de l’ajout de ScriptManager à la page maître est que le script ci-dessus est émis dans *chaque* page, qu’il soit nécessaire ou non. Cela a clairement pour conséquence une perte de bande passante pour les pages sur lesquelles ScriptManager est inclus (via la page maître), mais n’utilisez pas les fonctionnalités de l’infrastructure ASP.NET AJAX. Mais quelle est la quantité de bande passante perdue ?

- Le contenu réel émis par ScriptManager (indiqué ci-dessus) totalise un peu plus de 1 Ko.
- Toutefois, les trois fichiers de script externes référencés par l’élément `<script>` comprennent approximativement 450KB de données non compressées ; dans un site Web qui utilise la compression gzip, cette bande passante totale peut être réduite à 100 Ko. Toutefois, ces fichiers de script sont mis en cache par le navigateur pendant un an, ce qui signifie qu’ils ne doivent être téléchargés qu’une seule fois et peuvent ensuite être réutilisés dans d’autres pages du site.

Dans le meilleur des cas, quand les fichiers de script sont mis en cache, le coût total est de 1 Ko, ce qui est négligeable. Dans le pire des cas, toutefois, lorsque les fichiers de script n’ont pas encore été téléchargés et que le serveur Web n’utilise aucune forme de compression, l’accès à la bande passante se situe autour de 450KB, qui peut s’ajouter n’importe où depuis un deuxième ou deux sur une connexion haut débit jusqu’à une minute pendant  utilisateurs sur des modems d’accès à distance. La bonne nouvelle, c’est que, étant donné que les fichiers de script externes sont mis en cache par le navigateur, le pire scénario se produit rarement.

> [!NOTE]
> Si vous ne vous sentez toujours pas à placer le contrôle ScriptManager dans la page maître, considérez le formulaire Web (le balisage `<form runat="server">` dans la page maître). Chaque page ASP.NET qui utilise le modèle de publication doit inclure précisément un formulaire Web. L’ajout d’un Web Form ajoute du contenu supplémentaire : plusieurs champs de formulaire masqués, la balise `<form>` elle-même et, si nécessaire, une fonction JavaScript pour initialiser une publication à partir d’un script. Ce balisage n’est pas nécessaire pour les pages qui ne sont pas publiées. Ce balisage superflu peut être éliminé en supprimant le formulaire Web de la page maître et en l’ajoutant manuellement à chaque page de contenu qui en a besoin. Toutefois, les avantages liés à l’utilisation du formulaire Web dans la page maître l’emportent sur les inconvénients du fait qu’il est ajouté inutilement à certaines pages de contenu.

## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Étape 1 : ajout d’un contrôle ScriptManager à la page maître

Chaque page Web qui utilise l’infrastructure ASP.NET AJAX doit contenir précisément un contrôle ScriptManager. En raison de cette exigence, il est généralement judicieux de placer un seul contrôle ScriptManager sur la page maître pour que toutes les pages de contenu aient le contrôle ScriptManager inclus automatiquement. En outre, le ScriptManager doit précéder l’un des contrôles serveur ASP.NET AJAX, tels que les contrôles UpdatePanel et UpdateProgress. Par conséquent, il est préférable de placer ScriptManager avant tous les contrôles ContentPlaceHolder dans le formulaire Web.

Ouvrez la page maître `Site.master` et ajoutez un contrôle ScriptManager à la page dans le formulaire Web, mais avant l’élément `<div id="topContent">` (voir figure 1). Si vous utilisez Visual Web Developer 2008 ou Visual Studio 2008, le contrôle ScriptManager se trouve dans la boîte à outils sous l’onglet Extensions AJAX. Si vous utilisez Visual Studio 2005, vous devez d’abord installer l’infrastructure AJAX ASP.NET et ajouter les contrôles à la boîte à outils. Visitez le [Wiki ASP.NET AJAX](https://github.com/DevExpress/AjaxControlToolkit/wiki) pour obtenir le framework pour ASP.NET 2,0.

Après avoir ajouté le ScriptManager à la page, remplacez son `ID` `ScriptManager1` par `MyManager`.

[![ajouter ScriptManager à la page maître](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Figure 01**: Ajouter ScriptManager à la page maître ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image3.png))

## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Étape 2 : utilisation de l’infrastructure ASP.NET AJAX à partir d’une page de contenu

Une fois le contrôle ScriptManager ajouté à la page maître, nous pouvons désormais ajouter des fonctionnalités ASP.NET AJAX Framework à n’importe quelle page de contenu. Créons une nouvelle page ASP.NET qui affiche un produit sélectionné de manière aléatoire dans la base de données Northwind. Nous allons utiliser le contrôle Timer de l’infrastructure ASP.NET AJAX pour mettre à jour cet affichage toutes les 15 secondes, en affichant un nouveau produit.

Commencez par créer une nouvelle page dans le répertoire racine nommé `ShowRandomProduct.aspx`. N’oubliez pas de lier cette nouvelle page à la page maître `Site.master`.

[![ajouter une nouvelle page ASP.NET au site Web](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Figure 02**: ajouter une nouvelle page ASP.net au site Web ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image6.png))

Rappelez-vous que dans le didacticiel [*spécification du titre, des balises meta et d’autres en-têtes HTML dans le didacticiel de la page maître,* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) nous avons créé une classe de page de base personnalisée nommée `BasePage` qui a généré le titre de la page si elle n’a pas été définie explicitement. Accédez à la classe code-behind de la page de `ShowRandomProduct.aspx` et dérivez-la de `BasePage` (au lieu de `System.Web.UI.Page`).

Enfin, mettez à jour le fichier `Web.sitemap` pour inclure une entrée pour cette leçon. Ajoutez le balisage suivant sous le `<siteMapNode>` pour la leçon de l’interaction de la page maître à contenu :

[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

L’ajout de cet élément `<siteMapNode>` est reflété dans la liste leçons (voir figure 5).

### <a name="displaying-a-randomly-selected-product"></a>Affichage d’un produit sélectionné de manière aléatoire

Revenez à `ShowRandomProduct.aspx`. À partir du concepteur, faites glisser un contrôle UpdatePanel de la boîte à outils vers le contrôle de contenu `MainContent` et affectez à sa propriété `ID` la valeur `ProductPanel`. Le UpdatePanel représente une région à l’écran qui peut être mise à jour de façon asynchrone par le biais d’une publication de page partielle.

Notre première tâche consiste à afficher des informations sur un produit sélectionné de façon aléatoire dans le UpdatePanel. Commencez par faire glisser un contrôle DetailsView dans le UpdatePanel. Affectez à la propriété `ID` du contrôle DetailsView la valeur `ProductInfo` et effacez ses propriétés `Height` et `Width`. Développez la balise active du contrôle DetailsView et, dans la liste déroulante choisir la source de données, choisissez de lier le contrôle DetailsView à un nouveau contrôle SqlDataSource nommé `RandomProductDataSource`.

[![lier le DetailsView à un nouveau contrôle SqlDataSource](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Figure 03**: lier le contrôle DetailsView à un nouveau contrôle SqlDataSource ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image9.png))

Configurez le contrôle SqlDataSource pour la connexion à la base de données Northwind via le `NorthwindConnectionString` (que nous avons créé dans l' [*interaction avec la page maître à partir du didacticiel de page de contenu*](interacting-with-the-content-page-from-the-master-page-cs.md) ). Lors de la configuration de l’instruction SELECT, choisissez de spécifier une instruction SQL personnalisée, puis entrez la requête suivante :

[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

Le mot clé `TOP 1` dans la clause `SELECT` retourne uniquement le premier enregistrement retourné par la requête. La [fonction`NEWID()`](https://msdn.microsoft.com/library/ms190348.aspx) génère une nouvelle [valeur d’identificateur global unique (Guid)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) et peut être utilisée dans une clause `ORDER BY` pour retourner les enregistrements de la table dans un ordre aléatoire.

[![configurer le SqlDataSource pour retourner un enregistrement unique et sélectionné aléatoirement](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Figure 04**: configurer le SqlDataSource pour retourner un enregistrement unique et sélectionné de manière aléatoire ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image12.png))

Une fois l’Assistant terminé, Visual Studio crée un BoundField pour les deux colonnes retournées par la requête ci-dessus. À ce stade, le balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

La figure 5 illustre la page de `ShowRandomProduct.aspx` lorsque vous l’affichez dans un navigateur. Cliquez sur le bouton Actualiser de votre navigateur pour recharger la page. vous devez voir les valeurs `ProductName` et `UnitPrice` pour un nouvel enregistrement sélectionné de manière aléatoire.

[![le nom et le prix d’un produit aléatoire s’affichent](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Figure 05**: le nom et le prix d’un produit aléatoire s’affichent ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image15.png))

### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Affichage automatique d’un nouveau produit toutes les 15 secondes

L’infrastructure ASP.NET AJAX comprend un contrôle Timer qui effectue une publication (postback) à une heure spécifiée. lors de la publication (postback), l’événement `Tick` du minuteur est déclenché. Si le contrôle Timer est placé dans un UpdatePanel, il déclenche une publication (postback) de page partielle, pendant laquelle nous pouvons relier les données au contrôle DetailsView pour afficher un nouveau produit sélectionné de manière aléatoire.

Pour ce faire, faites glisser un minuteur de la boîte à outils et déposez-le dans le UpdatePanel. Modifiez la `ID` du minuteur de `Timer1` à `ProductTimer` et sa propriété `Interval` de 60000 à 15000. La propriété `Interval` indique le nombre de millisecondes entre les publications (postback); Si vous affectez la valeur 15000, le minuteur déclenche une publication de page partielle toutes les 15 secondes. À ce stade, le balisage déclaratif de votre minuteur doit ressembler à ce qui suit :

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Créez un gestionnaire d’événements pour l’événement `Tick` de la minuterie. Dans ce gestionnaire d’événements, nous devons relier les données au contrôle DetailsView en appelant la méthode `DataBind` du contrôle DetailsView. Cela indique au DetailsView de récupérer à nouveau les données de son contrôle de source de données, qui sélectionne et affiche un nouvel enregistrement sélectionné de manière aléatoire (comme lors du rechargement de la page en cliquant sur le bouton Actualiser du navigateur).

[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

C’est aussi simple que cela ! Réexaminez la page via un navigateur. Au départ, les informations d’un produit aléatoire sont affichées. Si vous regardez l’écran, vous remarquerez qu’au bout de 15 secondes, les informations sur un nouveau produit remplacent par magie l’affichage existant.

Pour mieux voir ce qui se passe, nous allons ajouter un contrôle Label à UpdatePanel qui affiche l’heure de la dernière mise à jour de l’affichage. Ajoutez un contrôle Web Label dans le UpdatePanel, définissez sa `ID` sur `LastUpdateTime`et désactivez sa propriété `Text`. Ensuite, créez un gestionnaire d’événements pour l’événement `Load` de UpdatePanel et affichez l’heure actuelle dans l’étiquette. (L’événement `Load` de UpdatePanel est déclenché à chaque publication (postback) de page complète ou partielle.)

[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Une fois cette modification terminée, la page indique l’heure à laquelle le produit actuellement affiché a été chargé. La figure 6 affiche la page lors de la première visite. La figure 7 illustre la page 15 secondes plus tard après que le contrôle Timer a été « coché » et que UpdatePanel a été actualisé pour afficher des informations sur un nouveau produit.

[![un produit sélectionné de façon aléatoire s’affiche au chargement de la page](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Figure 06**: un produit sélectionné de manière aléatoire s’affiche au chargement de la page ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image18.png))

[![toutes les 15 secondes, un nouveau produit sélectionné de manière aléatoire est affiché](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Figure 07**: toutes les 15 secondes un nouveau produit sélectionné de manière aléatoire est affiché ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image21.png))

## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Étape 3 : utilisation du contrôle ScriptManagerProxy

Avec l’inclusion du script nécessaire pour la bibliothèque cliente ASP.NET AJAX Framework, ScriptManager peut également inscrire des fichiers JavaScript personnalisés, des références à des services Web prenant en charge les scripts et des services d’authentification, d’autorisation et de profil personnalisés. En général, ces personnalisations sont spécifiques à une certaine page. Toutefois, si les fichiers de script personnalisés, les références de service Web, les services d’authentification, d’autorisation ou de profil sont référencés dans le ScriptManager de la page maître, ils sont inclus dans *toutes les* pages du site Web.

Pour ajouter des personnalisations associées à ScriptManager page par page, utilisez le contrôle ScriptManagerProxy. Vous pouvez ajouter un contrôle ScriptManagerProxy à une page de contenu, puis enregistrer le fichier JavaScript personnalisé, la référence du service Web ou l’authentification, l’autorisation ou le service de profil à partir de ScriptManagerProxy. Cela a pour effet d’inscrire ces services pour la page de contenu particulière.

> [!NOTE]
> Une page ASP.NET ne peut avoir qu’un seul contrôle ScriptManager présent. Par conséquent, vous ne pouvez pas ajouter un contrôle ScriptManager à une page de contenu si le contrôle ScriptManager est déjà défini dans la page maître. L’unique objectif de ScriptManagerProxy est de fournir aux développeurs un moyen de définir le ScriptManager dans la page maître, tout en ayant la possibilité d’ajouter des personnalisations ScriptManager page par page.

Pour voir le contrôle ScriptManagerProxy en action, nous allons augmenter l’UpdatePanel dans `ShowRandomProduct.aspx` pour inclure un bouton qui utilise un script côté client pour suspendre ou reprendre le contrôle Timer. Le contrôle Timer a trois méthodes côté client que nous pouvons utiliser pour obtenir les fonctionnalités souhaitées :

- `_startTimer()`-démarre le contrôle Timer
- `_raiseTick()` : fait en sorte que le contrôle Timer « Tick », ce qui revient à publier et à déclencher son événement `Tick` sur le serveur.
- `_stopTimer()` : arrête le contrôle Timer

Nous allons créer un fichier JavaScript avec une variable nommée `timerEnabled` et une fonction nommée `ToggleTimer`. La variable `timerEnabled` indique si le contrôle Timer est actuellement activé ou désactivé ; la valeur par défaut est true. La fonction `ToggleTimer` accepte deux paramètres d’entrée : une référence au bouton pause/reprise et la valeur `id` côté client du contrôle Timer. Cette fonction active ou désactive la valeur de `timerEnabled`, obtient une référence au contrôle Timer, démarre ou arrête la minuterie (selon la valeur de `timerEnabled`) et met à jour le texte d’affichage du bouton sur « pause » ou « Resume ». Cette fonction est appelée chaque fois que l’utilisateur clique sur le bouton pause/reprise.

Commencez par créer un nouveau dossier dans le site Web nommé `Scripts`. Ensuite, ajoutez un nouveau fichier au dossier scripts nommé `TimerScript.js` de type fichier JScript.

[![ajouter un nouveau fichier JavaScript au dossier scripts](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Figure 08**: ajouter un nouveau fichier JavaScript au dossier `Scripts` ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image24.png))

[![un nouveau fichier JavaScript a été ajouté au site Web](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Figure 09**: un nouveau fichier JavaScript a été ajouté au site Web ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image27.png))

Ajoutez ensuite le script suivant au fichier TimerScript. js :

[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

Nous devons maintenant inscrire ce fichier JavaScript personnalisé dans `ShowRandomProduct.aspx`. Revenez à `ShowRandomProduct.aspx` et ajoutez un contrôle ScriptManagerProxy à la page ; définissez ses `ID` sur `MyManagerProxy`. Pour inscrire un fichier JavaScript personnalisé, sélectionnez le contrôle ScriptManagerProxy dans le concepteur, puis accédez au Fenêtre Propriétés. L’une des propriétés est intitulée scripts. La sélection de cette propriété affiche l’éditeur de collections ScriptReference illustré à la figure 10. Cliquez sur le bouton Ajouter pour inclure une nouvelle référence de script, puis entrez le chemin d’accès au fichier de script dans la propriété chemin d’accès : `~/Scripts/TimerScript.js`.

[![ajouter une référence de script au contrôle ScriptManagerProxy](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Figure 10**: ajouter une référence de script au contrôle ScriptManagerProxy ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image30.png))

Après l’ajout de la référence de script, le balisage déclaratif du contrôle ScriptManagerProxy est mis à jour pour inclure une collection `<Scripts>` avec une seule entrée de `ScriptReference`, comme le montre l’extrait de code suivant :

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

L’entrée `ScriptReference` demande à l’élément ScriptManagerProxy d’inclure une référence au fichier JavaScript dans son balisage rendu. Autrement dit, en inscrivant le script personnalisé dans le ScriptManagerProxy, le résultat du rendu de la page de `ShowRandomProduct.aspx` comprend désormais une autre balise `<script src="url"></script>` : `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Nous pouvons maintenant appeler la fonction `ToggleTimer` définie dans `TimerScript.js` à partir du script client dans la page `ShowRandomProduct.aspx`. Ajoutez le code HTML suivant dans le UpdatePanel :

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Un bouton s’affiche avec le texte « pause ». Chaque fois que l’utilisateur clique dessus, la fonction JavaScript `ToggleTimer` est appelée, en passant une référence au bouton et la valeur d’ID du contrôle Timer (`ProductTimer`). Notez la syntaxe permettant d’obtenir la valeur `id` du contrôle Timer. `<%=ProductTimer.ClientID%>` émet la valeur de la propriété `ClientID` du contrôle Timer `ProductTimer`. Dans le didacticiel [*contrôle de nom d’ID dans les pages de contenu*](control-id-naming-in-content-pages-cs.md) , nous avons abordé les différences entre la valeur de `ID` côté serveur et la valeur de `id` côté client qui en résulte, et comment `ClientID` retourne le `id`côté client.

La figure 11 illustre cette page lors de la première visite dans un navigateur. Le minuteur est en cours d’exécution et met à jour les informations sur le produit affichées toutes les 15 secondes. La figure 12 montre l’écran une fois que vous avez cliqué sur le bouton suspendre. Le fait de cliquer sur le bouton suspendre arrête le minuteur et met à jour le texte du bouton sur « reprendre ». Les informations sur le produit sont actualisées (et continuent à s’actualiser toutes les 15 secondes) une fois que l’utilisateur clique sur reprendre.

[![cliquez sur le bouton pause pour arrêter le contrôle Timer](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Figure 11**: cliquer sur le bouton pause pour arrêter le contrôle Timer ([Cliquer pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image33.png))

[![cliquez sur le bouton RESUME pour redémarrer la minuterie](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Figure 12**: cliquer sur le bouton reprendre pour redémarrer la minuterie ([cliquez pour afficher l’image en taille réelle](master-pages-and-asp-net-ajax-cs/_static/image36.png))

## <a name="summary"></a>Récapitulatif

Lors de la génération d’applications Web compatibles AJAX à l’aide de l’infrastructure ASP.NET AJAX, il est impératif que chaque page Web compatible AJAX comprenne un contrôle ScriptManager. Pour faciliter ce processus, nous pouvons ajouter un ScriptManager à la page maître plutôt que de devoir penser à ajouter un ScriptManager à chaque page de contenu. L’étape 1 a montré comment ajouter ScriptManager à la page maître, tandis que l’étape 2 examinait l’implémentation des fonctionnalités AJAX dans une page de contenu.

Si vous devez ajouter des scripts personnalisés, des références à des services Web prenant en charge les scripts ou des services d’authentification, d’autorisation ou de profil personnalisés dans une page de contenu particulière, ajoutez un contrôle ScriptManagerProxy à la page de contenu, puis configurez le personnalisations. L’étape 3 a examiné comment utiliser ScriptManagerProxy pour inscrire un fichier JavaScript personnalisé dans une page de contenu spécifique.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Framework ASP.NET AJAX](../../../../ajax/index.md)
- [Didacticiels ASP.NET AJAX](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Vidéos ASP.NET AJAX](../../../videos/aspnet-ajax/index.md)
- [Création d’une interface utilisateur interactive avec Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Utilisation de NEWID pour trier les enregistrements de manière aléatoire](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Utilisation du contrôle Timer](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs ouvrages ASP/ASP. net et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 3,5 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott peut être contacté au [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, insérez une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](interacting-with-the-content-page-from-the-master-page-cs.md)
> [Suivant](specifying-the-master-page-programmatically-cs.md)
