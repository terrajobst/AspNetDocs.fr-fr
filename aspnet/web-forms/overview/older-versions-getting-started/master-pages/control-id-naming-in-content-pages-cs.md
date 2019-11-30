---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: Nom de l’ID de contrôle dansC#les pages de contenu () | Microsoft Docs
author: rick-anderson
description: Illustre la façon dont les contrôles ContentPlaceHolder servent de conteneur d’attribution de noms et, par conséquent, l’utilisation par programmation d’un contrôle difficile (via FindControl)...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: e849e5860dc988e112cc3a65d976c16ecdf77416
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74624462"
---
# <a name="control-id-naming-in-content-pages-c"></a>Contrôler le nommage des ID dans les pages de contenu (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> Illustre la façon dont les contrôles ContentPlaceHolder servent de conteneur d’attribution de noms et, par conséquent, l’utilisation par programmation d’un contrôle difficile (via FindControl). Examine ce problème et les solutions de contournement. Explique également comment accéder par programme à la valeur ClientID résultante.

## <a name="introduction"></a>Introduction

Tous les contrôles serveur ASP.NET incluent une propriété `ID` qui identifie de façon unique le contrôle et est le moyen d’accès par programmation au contrôle dans la classe code-behind. De même, les éléments d’un document HTML peuvent inclure un attribut `id` qui identifie de façon unique l’élément ; ces valeurs de `id` sont souvent utilisées dans le script côté client pour référencer par programmation un élément HTML particulier. À partir de cela, vous pouvez supposer que lorsqu’un contrôle serveur ASP.NET est rendu au format HTML, sa valeur `ID` est utilisée comme `id` valeur de l’élément HTML rendu. Ce n’est pas nécessairement le cas car, dans certains cas, un seul contrôle avec une valeur `ID` unique peut apparaître plusieurs fois dans le balisage rendu. Imaginez un contrôle GridView qui comprend un TemplateField avec un contrôle Web label avec une valeur `ID` de ProductName. Lorsque le GridView est lié à sa source de données au moment de l’exécution, cette étiquette est répétée une fois pour chaque ligne GridView. Chaque étiquette rendue a besoin d’une valeur de `id` unique.

Pour gérer ces scénarios, ASP.NET permet à certains contrôles d’être dénotés comme des conteneurs d’attribution de noms. Un conteneur d’attribution de noms sert de nouvel espace de noms `ID`. Les contrôles serveur qui apparaissent dans le conteneur d’attribution de noms ont leur rendu `id` valeur préfixé avec le `ID` du contrôle de conteneur d’attribution de noms. Par exemple, les classes `GridView` et `GridViewRow` sont des conteneurs d’attribution de noms. Par conséquent, un contrôle Label défini dans un TemplateField de GridView avec `ID` ProductName reçoit un rendu `id` valeur de `GridViewID_GridViewRowID_ProductName`. Étant donné que *GridViewRowID* est unique pour chaque ligne GridView, les valeurs `id` résultantes sont uniques.

> [!NOTE]
> L' [interface`INamingContainer`](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) est utilisée pour indiquer qu’un contrôle serveur ASP.net particulier doit fonctionner comme un conteneur d’attribution de noms. L’interface `INamingContainer` n’écrit pas les méthodes que le contrôle serveur doit implémenter. au lieu de cela, il est utilisé comme marqueur. En générant le balisage rendu, si un contrôle implémente cette interface, le moteur ASP.NET préfixe automatiquement sa valeur `ID` à ses descendants `id` valeurs d’attribut rendues. Ce processus est abordé plus en détail à l’étape 2.

Les conteneurs d’attribution de noms modifient non seulement la valeur de l’attribut rendu `id`, mais affectent également la manière dont le contrôle peut être référencé par programmation à partir de la classe code-behind de la page ASP.NET. La méthode `FindControl("controlID")` est couramment utilisée pour référencer par programmation un contrôle Web. Toutefois, `FindControl` ne pénètre pas dans les conteneurs d’attribution de noms. Par conséquent, vous ne pouvez pas utiliser directement la méthode `Page.FindControl` pour référencer des contrôles dans un GridView ou un autre conteneur d’attribution de noms.

Comme vous l’avez peut-être présumez, les pages maîtres et les ContentPlaceHolders sont implémentés en tant que conteneurs d’attribution de noms. Dans ce didacticiel, nous examinons comment les pages maîtres affectent l’élément HTML `id` valeurs et les manières de référencer par programmation des contrôles Web dans une page de contenu à l’aide de `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Étape 1 : ajout d’une nouvelle page ASP.NET

Pour illustrer les concepts abordés dans ce didacticiel, nous allons ajouter une nouvelle page ASP.NET à notre site Web. Créez une nouvelle page de contenu nommée `IDIssues.aspx` dans le dossier racine, en la liant à la page maître `Site.master`.

![Ajouter la page de contenu IDIssues. aspx au dossier racine](control-id-naming-in-content-pages-cs/_static/image1.png)

**Figure 01**: ajouter la page de contenu `IDIssues.aspx` au dossier racine

Visual Studio crée automatiquement un contrôle de contenu pour chacune des quatre ContentPlaceHolders de la page maître. Comme indiqué dans le didacticiel sur les [*multiples ContentPlaceHolders et le contenu par défaut*](multiple-contentplaceholders-and-default-content-cs.md) , si un contrôle de contenu n’est pas présent, le contenu ContentPlaceHolder par défaut de la page maître est émis à la place. Étant donné que le `QuickLoginUI` et `LeftColumnContent` ContentPlaceHolders contiennent le balisage par défaut approprié pour cette page, continuez et supprimez les contrôles de contenu correspondants de `IDIssues.aspx`. À ce stade, le balisage déclaratif de la page de contenu doit ressembler à ce qui suit :

[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

Dans le didacticiel [*spécification du titre, des balises meta et d’autres en-têtes HTML du didacticiel de la page maître,* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) nous avons créé une classe de page de base personnalisée (`BasePage`) qui configure automatiquement le titre de la page s’il n’est pas défini explicitement. Pour que la page `IDIssues.aspx` utilise cette fonctionnalité, la classe code-behind de la page doit dériver de la classe `BasePage` (au lieu de `System.Web.UI.Page`). Modifiez la définition de la classe code-behind afin qu’elle ressemble à ceci :

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

Enfin, mettez à jour le fichier `Web.sitemap` pour inclure une entrée pour cette nouvelle leçon. Ajoutez un élément `<siteMapNode>` et définissez ses attributs `title` et `url` sur « contrôler les problèmes d’appellation d’ID » et `~/IDIssues.aspx`, respectivement. Après cela, le balisage de votre fichier `Web.sitemap` doit ressembler à ce qui suit :

[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

Comme le montre la figure 2, la nouvelle entrée de plan de site dans `Web.sitemap` est immédiatement reflétée dans la section leçons de la colonne de gauche.

![La section leçons contient maintenant un lien vers &quot;problèmes d’attribution de noms aux ID de contrôle&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**Figure 02**: la section leçons contient maintenant un lien vers les « problèmes de nom d’ID de contrôle »

## <a name="step-2-examining-the-renderedidchanges"></a>Étape 2 : examen des modifications apportées au`ID`rendu

Pour mieux comprendre les modifications apportées par le moteur ASP.NET aux valeurs de `id` rendu des contrôles serveur, nous allons ajouter quelques contrôles Web à la page `IDIssues.aspx`, puis afficher le balisage rendu envoyé au navigateur. Plus précisément, tapez le texte « Veuillez entrer votre âge : », suivi d’un contrôle Web TextBox. Plus loin dans la page, ajoutez un contrôle Web Button et un contrôle Web Label. Définissez les propriétés `ID` et `Columns` de la zone de texte sur `Age` et 3, respectivement. Définissez les propriétés `Text` et `ID` du bouton sur « Submit » et `SubmitButton`. Effacez la propriété `Text` de l’étiquette et affectez à son `ID` la valeur `Results`.

À ce stade, le balisage déclaratif de votre contrôle de contenu doit ressembler à ce qui suit :

[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

La figure 3 montre la page affichée dans le concepteur de Visual Studio.

[![la page comprend trois contrôles Web : une zone de texte, un bouton et une étiquette](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**Figure 03**: la page comprend trois contrôles Web : une zone de texte, un bouton et une étiquette ([cliquez pour afficher l’image en taille réelle](control-id-naming-in-content-pages-cs/_static/image5.png))

Accédez à la page via un navigateur, puis affichez la source HTML. Comme le montre le balisage ci-dessous, les valeurs `id` des éléments HTML pour les contrôles Web TextBox, Button et label sont une combinaison des valeurs `ID` des contrôles Web et des valeurs `ID` des conteneurs d’attribution de noms dans la page.

[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

Comme indiqué précédemment dans ce didacticiel, la page maître et ses ContentPlaceHolders servent de conteneurs d’attribution de noms. Par conséquent, les deux fournissent le rendu `ID` valeurs de leurs contrôles imbriqués. Prenez l’attribut `id` de la zone de texte, par exemple : `ctl00_MainContent_Age`. N’oubliez pas que la valeur `ID` du contrôle TextBox était `Age`. Il est préfixé avec la valeur `ID` de son contrôle ContentPlaceHolder, `MainContent`. En outre, cette valeur est préfixée avec la valeur `ID` de la page maître, `ctl00`. L’effet net est une valeur d’attribut `id` constituée des valeurs `ID` de la page maître, du contrôle ContentPlaceHolder et de la zone de texte elle-même.

La figure 4 illustre ce comportement. Pour déterminer le rendu `id` de la zone de texte `Age`, commencez par la valeur `ID` du contrôle TextBox, `Age`. Ensuite, vous pouvez travailler dans la hiérarchie des contrôles. À chaque conteneur d’attribution de noms (nœuds avec une couleur de pêche), préfixez le `id` affiché en cours avec le `id`du conteneur d’attribution de noms.

![Les attributs d’ID rendus sont basés sur les valeurs d’ID des conteneurs d’attribution de noms](control-id-naming-in-content-pages-cs/_static/image6.png)

**Figure 04**: les attributs `id` rendus sont basés sur les valeurs `ID` des conteneurs d’attribution de noms

> [!NOTE]
> Comme nous l’avons vu, la partie `ctl00` de l’attribut `id` rendu constitue la valeur `ID` de la page maître, mais vous vous demandez peut-être comment cette valeur de `ID` a été obtenue. Nous ne l’avons pas spécifié n’importe où dans notre page maître ou de contenu. La plupart des contrôles serveur dans une page ASP.NET sont ajoutés explicitement via le balisage déclaratif de la page. Le contrôle `MainContent` ContentPlaceHolder a été explicitement spécifié dans le balisage de `Site.master`; la zone de texte `Age` a été définie `IDIssues.aspx`balise. Nous pouvons spécifier les valeurs de `ID` pour ces types de contrôles via le Fenêtre Propriétés ou à partir de la syntaxe déclarative. D’autres contrôles, tels que la page maître elle-même, ne sont pas définis dans le balisage déclaratif. Par conséquent, leurs valeurs `ID` doivent être générées automatiquement pour nous. Le moteur ASP.NET définit les valeurs `ID` au moment de l’exécution pour les contrôles dont les ID n’ont pas été définis explicitement. Elle utilise le modèle d’affectation de noms `ctlXX`, où *XX* est une valeur entière qui croît séquentiellement.

Étant donné que la page maître elle-même sert de conteneur d’attribution de noms, les contrôles Web définis dans la page maître ont également des valeurs d’attribut `id` rendu modifiées. Par exemple, l’étiquette de `DisplayDate` que nous avons ajoutée à la page maître dans le didacticiel [*création d’une disposition à l’ensemble du site avec des pages maîtres*](creating-a-site-wide-layout-using-master-pages-cs.md) contient le balisage rendu suivant :

[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

Notez que l’attribut `id` comprend la valeur de `ID` de la page maître (`ctl00`) et la valeur `ID` du contrôle Web de l’étiquette (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Étape 3 : référencer par programmation des contrôles Web via`FindControl`

Chaque contrôle serveur ASP.NET comprend une méthode `FindControl("controlID")` qui recherche dans les descendants du contrôle un contrôle nommé *ControlID*. Si un tel contrôle est trouvé, il est retourné ; Si aucun contrôle correspondant n’est trouvé, `FindControl` retourne `null`.

`FindControl` est utile dans les scénarios où vous devez accéder à un contrôle, mais que vous n’avez pas de référence directe à celui-ci. Lorsque vous utilisez des contrôles Web de données tels que GridView, par exemple, les contrôles dans les champs du contrôle GridView sont définis une seule fois dans la syntaxe déclarative, mais au moment de l’exécution, une instance du contrôle est créée pour chaque ligne GridView. Par conséquent, les contrôles générés au moment de l’exécution existent, mais nous n’avons pas de référence directe disponible à partir de la classe code-behind. Nous devons donc utiliser `FindControl` pour travailler par programmation avec un contrôle spécifique dans les champs de GridView. (Pour plus d’informations sur l’utilisation de `FindControl` pour accéder aux contrôles dans les modèles d’un contrôle Web de données, consultez [mise en forme personnalisée basée sur des données](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md).) Ce même scénario se produit lors de l’ajout dynamique de contrôles Web à un formulaire Web, une rubrique présentée dans [création d’Dynamic Data interfaces utilisateur d’entrée](https://msdn.microsoft.com/library/aa479330.aspx).

Pour illustrer l’utilisation de la méthode `FindControl` pour rechercher des contrôles dans une page de contenu, créez un gestionnaire d’événements pour l’événement `Click` du `SubmitButton`. Dans le gestionnaire d’événements, ajoutez le code suivant, qui référence par programmation la zone de texte `Age` et `Results` étiquette à l’aide de la méthode `FindControl`, puis affiche un message dans `Results` en fonction de l’entrée de l’utilisateur.

> [!NOTE]
> Bien entendu, nous n’avons pas besoin d’utiliser `FindControl` pour référencer les contrôles Label et TextBox pour cet exemple. Nous pourrions les référencer directement par le biais de leurs valeurs de propriété `ID`. J’utilise `FindControl` ici pour illustrer ce qui se passe lors de l’utilisation de `FindControl` à partir d’une page de contenu.

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

Alors que la syntaxe utilisée pour appeler la méthode `FindControl` diffère légèrement dans les deux premières lignes de `SubmitButton_Click`, elles sont sémantiquement équivalentes. Rappelez-vous que tous les contrôles serveur ASP.NET incluent une méthode `FindControl`. Cela comprend la classe `Page`, à partir de laquelle toutes les classes code-behind ASP.NET doivent dériver de. Par conséquent, l’appel de `FindControl("controlID")` équivaut à appeler `Page.FindControl("controlID")`, en supposant que vous n’avez pas substitué la méthode `FindControl` dans votre classe code-behind ou dans une classe de base personnalisée.

Après avoir entré ce code, accédez à la page `IDIssues.aspx` dans un navigateur, entrez votre âge, puis cliquez sur le bouton « Envoyer ». Quand vous cliquez sur le bouton « Submit », une `NullReferenceException` est déclenchée (voir figure 5).

[![une exception NullReferenceException est déclenchée](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**Figure 05**: un `NullReferenceException` est déclenché ([cliquez pour afficher l’image en taille réelle](control-id-naming-in-content-pages-cs/_static/image9.png))

Si vous définissez un point d’arrêt dans le gestionnaire d’événements `SubmitButton_Click`, vous verrez que les deux appels à `FindControl` retourner une valeur `null`. La `NullReferenceException` est déclenchée lorsque nous tentons d’accéder à la propriété `Text` de la zone de texte `Age`.

Le problème est que `Control.FindControl` effectue des recherches uniquement dans les descendants du *contrôle*qui se trouvent *dans le même conteneur d’attribution de noms*. Étant donné que la page maître constitue un nouveau conteneur d’attribution de noms, un appel à `Page.FindControl("controlID")` ne fait jamais la teneur de l’objet de page maître `ctl00`. (Reportez-vous à la figure 4 pour afficher la hiérarchie des contrôles, qui affiche l’objet `Page` en tant que parent de l’objet page maître `ctl00`.) Par conséquent, les `Results` libellé et `Age` zone de texte sont introuvables et les valeurs de `null`sont affectées à `ResultsLabel` et `AgeTextBox`.

Il existe deux solutions de contournement à ce défi : nous pouvons descendre dans la file d’attente, un conteneur d’attribution de noms à la fois, au contrôle approprié. ou nous pouvons créer notre propre méthode de `FindControl` qui permet d’attribuer des noms aux conteneurs. Examinons chacune de ces options.

### <a name="drilling-into-the-appropriate-naming-container"></a>Extraction dans le conteneur d’attribution de noms approprié

Pour utiliser `FindControl` pour référencer l’étiquette `Results` ou `Age` zone de texte, nous devons appeler `FindControl` à partir d’un contrôle ancêtre dans le même conteneur d’attribution de noms. Comme illustré à la figure 4, le contrôle `MainContent` ContentPlaceHolder est le seul ancêtre de `Results` ou `Age` qui se trouve dans le même conteneur d’attribution de noms. En d’autres termes, l’appel de la méthode `FindControl` à partir du contrôle `MainContent`, comme indiqué dans l’extrait de code ci-dessous, retourne correctement une référence aux contrôles `Results` ou `Age`.

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

Toutefois, nous ne pouvons pas utiliser le `MainContent` ContentPlaceHolder de la classe code-behind de la page de contenu à l’aide de la syntaxe ci-dessus, car ContentPlaceHolder est défini dans la page maître. Au lieu de cela, nous devons utiliser `FindControl` pour obtenir une référence à `MainContent`. Remplacez le code dans le gestionnaire d’événements `SubmitButton_Click` par les modifications suivantes :

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

Si vous accédez à la page via un navigateur, entrez votre âge, puis cliquez sur le bouton « envoyer », un `NullReferenceException` est déclenché. Si vous définissez un point d’arrêt dans le gestionnaire d’événements `SubmitButton_Click`, vous verrez que cette exception se produit lors de la tentative d’appel de la méthode `FindControl` de l’objet `MainContent`. L’objet `MainContent` est `null`, car la méthode `FindControl` ne peut pas localiser un objet nommé « MainContent ». La raison sous-jacente est la même qu’avec l’étiquette `Results` et les contrôles TextBox `Age` : `FindControl` démarre sa recherche à partir du haut de la hiérarchie des contrôles et ne pénètre pas dans les conteneurs d’attribution de noms, mais le `MainContent` ContentPlaceHolder est dans la page maître, qui est un conteneur d’attribution de noms.

Avant de pouvoir utiliser `FindControl` pour obtenir une référence à `MainContent`, nous avons d’abord besoin d’une référence au contrôle de page maître. Une fois que nous avons une référence à la page maître, nous pouvons obtenir une référence à l' `MainContent` ContentPlaceHolder via `FindControl` et, à partir de là, des références aux `Results` étiquette et `Age` zone de texte (à l’aide de `FindControl`). Mais comment obtenir une référence à la page maître ? En inspectant les attributs `id` dans le balisage rendu, il est évident que la valeur `ID` de la page maître est `ctl00`. Par conséquent, nous pourrions utiliser `Page.FindControl("ctl00")` pour obtenir une référence à la page maître, puis utiliser cet objet pour obtenir une référence à `MainContent`, et ainsi de suite. L’extrait de code suivant illustre cette logique :

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

Bien que ce code fonctionne certainement, il suppose que la `ID` générée automatiquement de la page maître sera toujours `ctl00`. Il n’est jamais judicieux de faire des hypothèses sur les valeurs générées automatiquement.

Heureusement, une référence à la page maître est accessible via la propriété `Master` de la classe `Page`. Par conséquent, au lieu d’utiliser `FindControl("ctl00")` pour obtenir une référence de la page maître afin d’accéder à l' `MainContent` ContentPlaceHolder, nous pouvons utiliser à la place `Page.Master.FindControl("MainContent")`. Mettez à jour le gestionnaire d’événements `SubmitButton_Click` à l’aide du code suivant :

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

Cette fois, en visitant la page via un navigateur, en saisissant votre âge et en cliquant sur le bouton « envoyer », vous affichez le message dans le `Results` étiquette, comme prévu.

[![l’âge de l’utilisateur est affiché dans l’étiquette](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**Figure 06**: l’âge de l’utilisateur est affiché dans l’étiquette ([cliquez pour afficher l’image en taille réelle](control-id-naming-in-content-pages-cs/_static/image12.png))

### <a name="recursively-searching-through-naming-containers"></a>Recherche récursive dans les conteneurs d’attribution de noms

La raison pour laquelle l’exemple de code précédent a référencé le contrôle `MainContent` ContentPlaceHolder à partir de la page maître, puis l’étiquette `Results` et les contrôles de zone de texte `Age` de `MainContent`, est que la méthode `Control.FindControl` effectue uniquement des recherches dans le conteneur d’attribution de noms du *contrôle*. Avoir des `FindControl` rester dans le conteneur d’attribution de noms est logique dans la plupart des scénarios, car deux contrôles dans deux conteneurs d’attribution de noms différents peuvent avoir les mêmes valeurs de `ID`. Considérez le cas d’un GridView qui définit un contrôle Web Label nommé `ProductName` dans l’un de ses TemplateFields. Lorsque les données sont liées au GridView lors de l’exécution, une `ProductName` étiquette est créée pour chaque ligne GridView. Si `FindControl` recherchés dans tous les conteneurs d’attribution de noms et que nous avons appelé `Page.FindControl("ProductName")`, quelle instance d’étiquette le `FindControl` doit-il retourner ? Le `ProductName` étiquette de la première ligne GridView ? Celui de la dernière ligne ?

Par conséquent, le conteneur d’attribution de noms de *contrôle*de `Control.FindControl` Search est un bon sens dans la plupart des cas. Toutefois, il existe d’autres cas, comme celui qui nous intéresse, où nous avons un `ID` unique dans tous les conteneurs d’attribution de noms et que vous souhaitez éviter de faire référence à chaque conteneur d’attribution de noms dans la hiérarchie des contrôles pour accéder à un contrôle. La présence d’un variant `FindControl` qui recherche de manière récursive tous les conteneurs d’attribution de noms est également logique. Malheureusement, le .NET Framework n’inclut pas une telle méthode.

La bonne nouvelle, c’est que nous pouvons créer notre propre méthode de `FindControl` qui recherche de manière récursive tous les conteneurs d’attribution de noms. En fait, en utilisant des *méthodes d’extension* , nous pouvons ajouter une méthode de `FindControlRecursive` à la classe `Control` pour accompagner sa méthode `FindControl` existante.

> [!NOTE]
> Les méthodes d’extension sont une fonctionnalité C# nouvelle de 3,0 et Visual Basic 9, qui sont les langages fournis avec le .NET Framework version 3,5 et Visual Studio 2008. En résumé, les méthodes d’extension permettent à un développeur de créer une nouvelle méthode pour un type de classe existant par le biais d’une syntaxe spéciale. Pour plus d’informations sur cette fonctionnalité utile, reportez-vous à mon article, extension de la [fonctionnalité de type de base à l’aide de méthodes d’extension](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).

Pour créer la méthode d’extension, ajoutez un nouveau fichier au dossier `App_Code` nommé `PageExtensionMethods.cs`. Ajoutez une méthode d’extension nommée `FindControlRecursive` qui prend comme entrée un paramètre `string` nommé `controlID`. Pour que les méthodes d’extension fonctionnent correctement, il est vital que la classe elle-même et ses méthodes d’extension soient marquées `static`. En outre, toutes les méthodes d’extension doivent accepter comme premier paramètre un objet du type auquel la méthode d’extension s’applique, et ce paramètre d’entrée doit être précédé du mot clé `this`.

Ajoutez le code suivant au fichier de classe `PageExtensionMethods.cs` pour définir cette classe et la méthode d’extension `FindControlRecursive` :

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

Une fois ce code en place, revenez à la classe code-behind de la page `IDIssues.aspx` et commentez les appels de méthode `FindControl` en cours. Remplacez-les par des appels à `Page.FindControlRecursive("controlID")`. Les méthodes d’extension s’affichent directement dans les listes déroulantes IntelliSense. Comme le montre la figure 7, lorsque vous tapez page et que vous appuyez sur période, la méthode `FindControlRecursive` est incluse dans la liste déroulante IntelliSense avec les autres méthodes de classe `Control`.

[les méthodes d’extension ![sont incluses dans les listes déroulantes IntelliSense](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**Figure 07**: les méthodes d’extension sont incluses dans les listes déroulantes IntelliSense ([cliquez pour afficher l’image en taille réelle](control-id-naming-in-content-pages-cs/_static/image15.png))

Entrez le code suivant dans le gestionnaire d’événements `SubmitButton_Click`, puis testez-le en visitant la page, en saisissant votre âge et en cliquant sur le bouton « Submit » (envoyer). Comme illustré à la figure 6, le résultat obtenu est le message « vous avez une ancienne année ! »

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> Étant donné que les méthodes d' C# extension sont nouvelles pour 3,0 et Visual Basic 9, si vous utilisez Visual Studio 2005, vous ne pouvez pas utiliser de méthodes d’extension. Au lieu de cela, vous devez implémenter la méthode `FindControlRecursive` dans une classe d’assistance. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) a un tel exemple dans son billet de blog, [ASP.net Maser pages et `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx).

## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Étape 4 : utilisation de la valeur d’attribut`id`correcte dans le script côté client

Comme indiqué dans l’introduction de ce didacticiel, l’attribut `id` rendu d’un contrôle Web est souvent utilisé dans le script côté client pour référencer par programmation un élément HTML particulier. Par exemple, le code JavaScript suivant fait référence à un élément HTML par son `id`, puis affiche sa valeur dans une boîte de message modale :

[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

Rappelez-vous que dans les pages ASP.NET qui n’incluent pas de conteneur d’attribution de noms, l’attribut `id` de l’élément HTML rendu est identique à la valeur de la propriété `ID` du contrôle Web. Pour cette raison, il est tentant de coder en dur des valeurs d’attribut `id` dans du code JavaScript. Autrement dit, si vous savez que vous souhaitez accéder au contrôle Web `Age` TextBox par le biais d’un script côté client, faites-le via un appel à `document.getElementById("Age")`.

Le problème de cette approche est que lors de l’utilisation de pages maîtres (ou d’autres contrôles de conteneur d’attribution de noms), le `id` HTML rendu n’est pas synonyme de la propriété `ID` du contrôle Web. Votre première inclinaison peut consister à accéder à la page via un navigateur et à afficher la source pour déterminer l’attribut `id` réel. Une fois que vous connaissez la valeur de `id` rendue, vous pouvez la coller dans l’appel à `getElementById` pour accéder à l’élément HTML dont vous avez besoin pour utiliser le script côté client. Cette approche n’est pas idéale, car certaines modifications apportées à la hiérarchie des contrôles de la page ou les modifications apportées aux propriétés de `ID` des contrôles de nommage modifient l’attribut `id` résultant, ce qui rompt votre code JavaScript.

La bonne nouvelle, c’est que la valeur d’attribut `id` rendue est accessible dans le code côté serveur par le biais de la [propriété`ClientID`](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx)du contrôle Web. Vous devez utiliser cette propriété pour déterminer la valeur d’attribut `id` utilisée dans le script côté client. Par exemple, pour ajouter une fonction JavaScript à la page qui, lorsqu’elle est appelée, affiche la valeur de la zone de texte `Age` dans une zone de message modale, ajoutez le code suivant au gestionnaire d’événements `Page_Load` :

[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

Le code ci-dessus injecte la valeur de la propriété ClientID de la `Age` TextBox dans l’appel JavaScript à `getElementById`. Si vous accédez à cette page via un navigateur et que vous affichez la source HTML, vous trouverez le code JavaScript suivant :

[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

Notez que la valeur d’attribut `id` correcte, `ctl00_MainContent_Age`, apparaît dans l’appel à `getElementById`. Étant donné que cette valeur est calculée au moment de l’exécution, elle fonctionne indépendamment des modifications ultérieures apportées à la hiérarchie des contrôles de page.

> [!NOTE]
> Cet exemple JavaScript montre simplement comment ajouter une fonction JavaScript qui fait correctement référence à l’élément HTML rendu par un contrôle serveur. Pour utiliser cette fonction, vous devez créer un code JavaScript supplémentaire pour appeler la fonction lorsque le document est chargé ou quand une action spécifique de l’utilisateur s’est dépassée. Pour plus d’informations sur ces rubriques et les rubriques connexes, consultez [utilisation du script côté client](https://msdn.microsoft.com/library/aa479302.aspx).

## <a name="summary"></a>Récapitulatif

Certains contrôles serveur ASP.NET agissent comme des conteneurs d’attribution de noms, ce qui affecte le rendu `id` valeurs d’attribut de leurs contrôles descendants, ainsi que la portée des contrôles canevas par la méthode `FindControl`. En ce qui concerne les pages maîtres, la page maître elle-même et ses contrôles ContentPlaceHolder sont des conteneurs d’attribution de noms. Par conséquent, nous avons besoin d’un peu plus de travail pour référencer par programmation les contrôles dans la page de contenu à l’aide de `FindControl`. Dans ce didacticiel, nous avons examiné deux techniques : l’exploration dans le contrôle ContentPlaceHolder et l’appel de sa méthode `FindControl` ; et de déployer notre propre implémentation de `FindControl` qui effectue des recherches récursives dans tous les conteneurs d’attribution de noms.

Outre les problèmes côté serveur, les conteneurs d’attribution de noms introduisent en ce qui concerne le référencement des contrôles Web, mais il existe également des problèmes côté client. En l’absence de conteneurs d’attribution de noms, la valeur de propriété `ID` du contrôle Web et la valeur d’attribut `id` rendue sont les mêmes. Toutefois, avec l’ajout d’un conteneur d’attribution de noms, l’attribut `id` rendu comprend à la fois les valeurs `ID` du contrôle Web et le ou les conteneurs d’attribution de noms dans le ascendance de sa hiérarchie de contrôle. Ces problèmes d’affectation de noms ne sont pas un problème tant que vous utilisez la propriété `ClientID` du contrôle Web pour déterminer la valeur d’attribut `id` rendue dans votre script côté client.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Pages maîtres et `FindControl` ASP.NET](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Création d’interfaces utilisateur d’entrée Dynamic Data](https://msdn.microsoft.com/library/aa479330.aspx)
- [Extension de la fonctionnalité de type de base avec les méthodes d’extension](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Guide pratique pour référencer le contenu de la page maître ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Maître pages : conseils, astuces et interruptions](http://www.odetocode.com/articles/450.aspx)
- [Utilisation d’un script côté client](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs ouvrages ASP/ASP. net et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 3,5 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott peut être contacté au [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Zack Jones et Barnerjee. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](urls-in-master-pages-cs.md)
> [Suivant](interacting-with-the-master-page-from-the-content-page-cs.md)
