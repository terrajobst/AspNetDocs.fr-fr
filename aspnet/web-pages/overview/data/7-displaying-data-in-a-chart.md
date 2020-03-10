---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Affichage des données dans un graphique avec pages Web ASP.NET (Razor) | Microsoft Docs
author: microsoft
description: Ce chapitre explique comment afficher des données dans un graphique. Dans les chapitres précédents, vous avez appris à afficher les données manuellement et dans une grille. Ce chapitre explique...
ms.author: riande
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 6dad67d4e3d38d57a761c567d937d714a3184ea9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627490"
---
# <a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Affichage de données dans un graphique avec pages Web ASP.NET (Razor)

par [Microsoft](https://github.com/microsoft)

> Cet article explique comment utiliser un graphique pour afficher les données d’un site Web pages Web ASP.NET (Razor) à l’aide du programme d’assistance `Chart`.
> 
> **Ce que vous allez apprendre**:
> 
> - Comment afficher des données dans un graphique.
> - Comment styliser des graphiques à l’aide de thèmes intégrés.
> - Comment enregistrer des graphiques et comment les mettre en cache pour de meilleures performances.
> 
> Voici les fonctionnalités de programmation ASP.NET présentées dans l’article :
> 
> - Le programme d’assistance `Chart`.
> 
> > [!NOTE]
> > Les informations contenues dans cet article s’appliquent à pages Web ASP.NET 1,0 et pages Web 2.

<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>Application d’assistance du graphique

Lorsque vous souhaitez afficher vos données sous forme graphique, vous pouvez utiliser `Chart` Helper. Le `Chart` Helper peut restituer une image qui affiche des données dans divers types de graphiques. Il prend en charge de nombreuses options de mise en forme et d’étiquetage. Le `Chart` Helper peut afficher plus de 30 types de graphiques, y compris tous les types de graphiques que vous connaissez peut-être dans Microsoft Excel ou d’autres &#8212; graphiques en aires, des graphiques à barres, des histogrammes, des graphiques en courbes et des graphiques en secteurs, ainsi que des graphiques plus spécialisés comme les graphiques boursiers.

| **Graphique en aires** ![Description : image du type de graphique en aires](7-displaying-data-in-a-chart/_static/image1.jpg) | **Graphique à barres** ![Description : image du type de graphique à barres](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Histogramme ![Description** : image du type d’histogramme](7-displaying-data-in-a-chart/_static/image3.jpg) | **Graphique en courbes** ![Description : image du type de graphique en courbes](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Graphique à secteurs** ![Description : image du type de graphique à secteurs](7-displaying-data-in-a-chart/_static/image5.jpg) | **Graphique boursier** ![Description : image du type de graphique boursier](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Éléments de graphique

Les graphiques affichent des données et des éléments supplémentaires tels que des légendes, des axes, des séries, etc. L’illustration suivante montre un grand nombre des éléments de graphique que vous pouvez personnaliser lorsque vous utilisez le programme d’assistance `Chart`. Cet article vous montre comment définir certains (pas tous) de ces éléments.

![Description : image présentant les éléments de graphique](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Création d’un graphique à partir de données

Les données que vous affichez dans un graphique peuvent provenir d’un tableau, des résultats retournés par une base de données ou de données qui se trouvent dans un fichier XML.

### <a name="using-an-array"></a>Utilisation d’un tableau

Comme expliqué dans [Introduction à la programmation de pages Web ASP.net à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890), un tableau vous permet de stocker une collection d’éléments similaires dans une variable unique. Vous pouvez utiliser des tableaux pour contenir les données que vous souhaitez inclure dans votre graphique.

Cette procédure montre comment créer un graphique à partir de données dans des tableaux, en utilisant le type de graphique par défaut. Il montre également comment afficher le graphique dans la page.

1. Créez un nouveau fichier nommé *ChartArrayBasic. cshtml*.
2. Remplacez le contenu existant par ce qui suit : 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Le code commence par créer un nouveau graphique et définit sa largeur et sa hauteur. Vous spécifiez le titre du graphique à l’aide de la méthode `AddTitle`. Pour ajouter des données, vous utilisez la méthode `AddSeries`. Dans cet exemple, vous utilisez les paramètres `name`, `xValue`et `yValues` de la méthode `AddSeries`. Le paramètre `name` est affiché dans la légende du graphique. Le paramètre `xValue` contient un tableau de données qui s’affiche le long de l’axe horizontal du graphique. Le paramètre `yValues` contient un tableau de données utilisé pour tracer les points verticaux du graphique.

    La méthode `Write` restitue en fait le graphique. Dans ce cas, étant donné que vous n’avez pas spécifié de type de graphique, le programme d’assistance `Chart` restitue son graphique par défaut, qui est un histogramme.
3. Exécutez la page dans le navigateur. Le navigateur affiche le graphique. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Utilisation d’une requête de base de données pour les données de graphique

Si les informations que vous souhaitez représenter dans un graphique se trouvent dans une base de données, vous pouvez exécuter une requête de base de données, puis utiliser les données des résultats pour créer le graphique. Cette procédure vous montre comment lire et afficher les données de la base de données créée dans l’article [Introduction à l’utilisation d’une base de données dans des Sites pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Ajoutez un dossier de *données d’application\_* à la racine du site Web si le dossier n’existe pas encore.
2. Dans le dossier *application\_Data* , ajoutez le fichier de base de données nommé *SmallBakery. sdf* qui est décrit dans [Introduction à l’utilisation d’une base de données dans des sites pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Créez un nouveau fichier nommé *ChartDataQuery. cshtml*.
4. Remplacez le contenu existant par ce qui suit :   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Le code ouvre d’abord la base de données SmallBakery et l’attribue à une variable nommée `db`. Cette variable représente un objet `Database` qui peut être utilisé pour lire et écrire dans la base de données. Ensuite, le code exécute une requête SQL pour obtenir le nom et le prix de chaque produit. Le code crée un nouveau graphique et lui transmet la requête de base de données en appelant la méthode `DataBindTable` du graphique. Cette méthode prend deux paramètres : le paramètre `dataSource` pour les données de la requête, et le paramètre `xField` vous permet de définir la colonne de données qui est utilisée pour l’axe x du graphique.

    En guise d’alternative à l’utilisation de la méthode `DataBindTable`, vous pouvez utiliser la méthode `AddSeries` de l’application auxiliaire `Chart`. La méthode `AddSeries` vous permet de définir les paramètres `xValue` et `yValues`. Par exemple, au lieu d’utiliser la méthode `DataBindTable` comme suit :

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Vous pouvez utiliser la méthode `AddSeries` comme suit :

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Les deux affichent les mêmes résultats. La méthode `AddSeries` est plus flexible, car vous pouvez spécifier le type de graphique et les données de manière plus explicite, mais la méthode `DataBindTable` est plus facile à utiliser si vous n’avez pas besoin de la flexibilité supplémentaire.
5. Exécutez la page dans un navigateur. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Utilisation de données XML

La troisième option pour l’utilisation de graphiques consiste à utiliser un fichier XML comme données pour le graphique. Cela nécessite que le fichier XML ait également un fichier de schéma (fichier *. xsd* ) qui décrit la structure XML. Cette procédure vous montre comment lire des données à partir d’un fichier XML.

1. Dans le dossier *application\_Data* , créez un nouveau fichier XML nommé *Data. xml*.
2. Remplacez le code XML existant par le code suivant, qui est une partie des données XML relatives aux employés d’une société fictive. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. Dans le dossier de données de l' *application\_* , créez un nouveau fichier XML nommé *Data. xsd*. (Notez que cette fois, l’extension est *. xsd*.)
4. Remplacez le code XML existant par ce qui suit : 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. À la racine du site Web, créez un nouveau fichier nommé *ChartDataXML. cshtml*.
6. Remplacez le contenu existant par ce qui suit : 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Le code crée d’abord un objet `DataSet`. Cet objet permet de gérer les données lues à partir du fichier XML et de les organiser en fonction des informations contenues dans le fichier de schéma. (Notez que le haut du code comprend l’instruction `using SystemData`. Cela est nécessaire pour pouvoir utiliser l’objet `DataSet`. Pour plus d’informations, consultez [&quot;l’utilisation des instructions&quot; et des noms complets](#SB_UsingStatements) plus loin dans cet article.)

    Ensuite, le code crée un objet `DataView` en fonction du jeu de données. La vue de &#8212; données fournit un objet auquel le graphique peut se lier, en lecture et en tracé. Le graphique est lié aux données à l’aide de la méthode `AddSeries`, comme vous l’avez vu précédemment lors de la création d’un graphique des données du tableau, sauf que cette fois, les paramètres `xValue` et `yValues` sont définis sur l’objet `DataView`.

    Cet exemple montre également comment spécifier un type de graphique particulier. Lorsque les données sont ajoutées à la méthode `AddSeries`, le paramètre `chartType` est également défini pour afficher un graphique à secteurs.
7. Exécutez la page dans un navigateur. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Instructions « using » et noms qualifiés complets
> 
> La .NET Framework qui pages Web ASP.NET avec syntaxe Razor est basée sur plusieurs milliers de composants (classes). Pour faciliter la gestion de l’utilisation de toutes ces classes, celles-ci sont organisées en *espaces de noms*, qui sont comparables aux bibliothèques. Par exemple, l’espace de noms `System.Web` contient des classes qui prennent en charge la communication entre le navigateur et le serveur, l’espace de noms `System.Xml` contient des classes utilisées pour créer et lire des fichiers XML, et l’espace de noms `System.Data` contient des classes qui vous permettent d’utiliser des données.
> 
> Pour accéder à une classe donnée dans le .NET Framework, le code doit connaître non seulement le nom de la classe, mais également l’espace de noms dans lequel se trouve la classe. Par exemple, pour utiliser le programme d’assistance `Chart`, le code doit rechercher la classe `System.Web.Helpers.Chart`, qui associe l’espace de noms (`System.Web.Helpers`) au nom de la classe (`Chart`). C’est ce que l’on appelle le *nom* &#8212; complet de la classe son emplacement complet et non ambigu au sein de l’étendue de la .NET Framework. Dans le code, cela ressemble à ce qui suit :
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Toutefois, il est fastidieux (et sujet aux erreurs) de devoir utiliser ces noms longs et complets chaque fois que vous souhaitez faire référence à une classe ou à un programme d’assistance. Par conséquent, pour faciliter l’utilisation des noms de classe, vous pouvez *Importer* les espaces de noms qui vous intéressent, qui est généralement une simple poignée parmi les nombreux espaces de noms du .NET Framework. Si vous avez importé un espace de noms, vous pouvez utiliser simplement un nom de classe (`Chart`) à la place du nom complet (`System.Web.Helpers.Chart`). Lorsque votre code s’exécute et rencontre un nom de classe, il peut rechercher uniquement dans les espaces de noms que vous avez importés pour trouver cette classe.
> 
> Quand vous utilisez pages Web ASP.NET avec syntaxe Razor pour créer des pages Web, vous utilisez généralement le même ensemble de classes à chaque fois, y compris la classe `WebPage`, les différentes applications auxiliaires, etc. Pour vous faire gagner le travail d’importation des espaces de noms appropriés chaque fois que vous créez un site Web, ASP.NET est configuré de sorte qu’il importe automatiquement un ensemble d’espaces de noms de base pour chaque site Web. C’est la raison pour laquelle vous n’avez pas eu à traiter les espaces de noms ou à importer jusqu’à maintenant. toutes les classes avec lesquelles vous avez travaillé se trouvent dans des espaces de noms qui sont déjà importés pour vous.
> 
> Toutefois, vous devez parfois travailler avec une classe qui ne se trouve pas dans un espace de noms qui est automatiquement importé pour vous. Dans ce cas, vous pouvez utiliser le nom qualifié complet de cette classe, ou vous pouvez importer manuellement l’espace de noms qui contient la classe. Pour importer un espace de noms, vous utilisez l’instruction `using` (`import` dans Visual Basic), comme vous l’avez vu dans un exemple plus haut dans l’article.
> 
> Par exemple, la classe `DataSet` se trouve dans l’espace de noms `System.Data`. L’espace de noms `System.Data` n’est pas automatiquement disponible pour les pages Razor ASP.NET. Par conséquent, pour travailler avec la classe `DataSet` à l’aide de son nom qualifié complet, vous pouvez utiliser du code comme celui-ci :
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Si vous devez utiliser la classe `DataSet` à plusieurs reprises, vous pouvez importer un espace de noms comme celui-ci, puis utiliser simplement le nom de la classe dans le code :
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Vous pouvez ajouter des instructions `using` pour tous les autres espaces de noms .NET Framework que vous souhaitez référencer. Toutefois, comme nous l’avons vu, vous n’aurez pas besoin de le faire souvent, car la plupart des classes avec lesquelles vous travaillerez se trouvent dans des espaces de noms importés automatiquement par ASP.NET pour une utilisation dans les pages *. cshtml* et *. vbhtml* .

<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Affichage de graphiques dans une page Web

Dans les exemples que vous avez vus jusqu’à présent, vous créez un graphique, puis le graphique est rendu directement dans le navigateur sous la forme d’un graphique. Dans de nombreux cas, toutefois, vous souhaitez afficher un graphique en tant que partie d’une page, et non simplement dans le navigateur. Pour ce faire, vous devez effectuer un processus en deux étapes. La première étape consiste à créer une page qui génère le graphique, comme vous l’avez déjà vu.

La deuxième étape consiste à afficher l’image résultante dans une autre page. Pour afficher l’image, vous utilisez un élément de `<img>` HTML, de la même façon que vous pouvez afficher une image. Toutefois, au lieu de référencer un fichier. *jpg* ou *. png* , l’élément `<img>` fait référence au fichier *. cshtml* qui contient le programme d’assistance `Chart` qui crée le graphique. Lorsque la page d’affichage s’exécute, l’élément `<img>` obtient la sortie de l’application auxiliaire `Chart` et restitue le graphique.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Créez un fichier nommé *ShowChart. cshtml*.
2. Remplacez le contenu existant par ce qui suit : 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Le code utilise l’élément `<img>` pour afficher le graphique que vous avez créé précédemment dans le fichier *ChartArrayBasic. cshtml* .
3. Exécutez la page Web dans un navigateur. Le fichier *ShowChart. cshtml* affiche l’image du graphique en fonction du code contenu dans le fichier *ChartArrayBasic. cshtml* .

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Stylisation d’un graphique

L’application auxiliaire `Chart` prend en charge un grand nombre d’options qui vous permettent de personnaliser l’apparence du graphique. Vous pouvez définir des couleurs, des polices, des bordures, etc. Un moyen simple de personnaliser l’apparence d’un graphique consiste à utiliser un *thème*. Les thèmes sont des collections d'informations qui spécifient de quelle manière restituer un graphique à l'aide de couleurs, d'étiquettes, de palettes, de bordures et d'effets. (Notez que le style d’un graphique n’indique pas le type de graphique.)

Le tableau suivant répertorie les thèmes intégrés.

| Thème | Description |
| --- | --- |
| `Vanilla` | Affiche les colonnes rouges sur un arrière-plan blanc. |
| `Blue` | Affiche les colonnes bleues sur un arrière-plan dégradé bleu. |
| `Green` | Affiche les colonnes bleues sur un arrière-plan dégradé vert. |
| `Yellow` | Affiche les colonnes orange sur un arrière-plan dégradé jaune. |
| `Vanilla3D` | Affiche les colonnes rouges 3D sur un arrière-plan blanc. |

Vous pouvez spécifier le thème à utiliser lorsque vous créez un nouveau graphique.

1. Créez un nouveau fichier nommé *ChartStyleGreen. cshtml*.
2. Remplacez le contenu existant de la page par ce qui suit :

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Ce code est le même que dans l’exemple précédent qui utilise la base de données pour les données, mais il ajoute le paramètre `theme` lors de la création de l’objet `Chart`. L’exemple suivant montre le code modifié :

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Exécutez la page dans un navigateur. Vous voyez les mêmes données qu’avant, mais le graphique semble plus soigné : 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Enregistrement d’un graphique

Lorsque vous utilisez le programme d’assistance `Chart` comme vous l’avez vu jusqu’à présent dans cet article, le programme d’assistance recrée le graphique à partir de zéro chaque fois qu’il est appelé. Si nécessaire, le code du graphique interroge également à nouveau la base de données ou relit le fichier XML pour obtenir les données. Dans certains cas, cette opération peut être complexe, par exemple si la base de données que vous interrogez est volumineuse ou si le fichier XML contient beaucoup de données. Même si le graphique n’implique pas beaucoup de données, le processus de création dynamique d’une image prend en compte les ressources du serveur et, si de nombreuses personnes demandent la page ou les pages qui affichent le graphique, il peut y avoir un impact sur les performances de votre site Web.

Pour vous aider à réduire l’impact potentiel sur les performances de la création d’un graphique, vous pouvez créer un graphique la première fois que vous en avez besoin, puis l’enregistrer. Lorsque le graphique est à nouveau nécessaire, au lieu de le régénérer, vous pouvez simplement extraire la version enregistrée et la rendre.

Vous pouvez enregistrer un graphique des manières suivantes :

- Mettez en cache le graphique dans la mémoire de l’ordinateur (sur le serveur).
- Enregistrez le graphique en tant que fichier image.
- Enregistrez le graphique sous forme de fichier XML. Cette option vous permet de modifier le graphique avant de l’enregistrer.

### <a name="caching-a-chart"></a>Mise en cache d’un graphique

Une fois que vous avez créé un graphique, vous pouvez le mettre en cache. La mise en cache d’un graphique signifie qu’il n’est pas nécessaire de le recréer s’il doit être à nouveau affiché. Lorsque vous enregistrez un graphique dans le cache, vous lui donnez une clé qui doit être unique pour ce graphique.

Les graphiques enregistrés dans le cache peuvent être supprimés si le serveur ne dispose pas de suffisamment de mémoire. En outre, le cache est effacé si votre application redémarre pour une raison quelconque. Par conséquent, la méthode standard pour travailler avec un graphique mis en cache consiste à toujours vérifier d’abord si elle est disponible dans le cache et, si ce n’est pas le cas, à la créer ou à la recréer.

1. À la racine de votre site Web, créez un fichier nommé *ShowCachedChart. cshtml*.
2. Remplacez le contenu existant par ce qui suit : 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    La balise `<img>` comprend un attribut `src` qui pointe vers le fichier *ChartSaveToCache. cshtml* et transmet une clé à la page sous la forme d’une chaîne de requête. La clé contient la valeur &quot;&quot;myChartKey. Le fichier *ChartSaveToCache. cshtml* contient le programme d’assistance `Chart` qui crée le graphique. Vous allez créer cette page dans un moment.

    À la fin de la page, il y a un lien vers une page nommée *ClearCache. cshtml*. Il s’agit d’une page que vous allez également créer bientôt. Vous avez besoin de *ClearCache. cshtml* uniquement pour tester la mise en cache pour cet exemple : il ne s’agit pas d’un lien ou d’une page que vous auriez normalement inclus lors de l’utilisation de graphiques mis en cache.
3. À la racine de votre site Web, créez un nouveau fichier nommé *ChartSaveToCache. cshtml*.
4. Remplacez le contenu existant par ce qui suit :

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Le code vérifie d’abord si quelque chose a été passé comme valeur de clé dans la chaîne de requête. Si c’est le cas, le code essaie de lire un graphique en dehors du cache en appelant la méthode `GetFromCache` et en lui passant la clé. S’il s’avère qu’il n’y a rien dans le cache sous cette clé (ce qui se produit la première fois que le graphique est demandé), le code crée le graphique comme d’habitude. Une fois le graphique terminé, le code l’enregistre dans le cache en appelant `SaveToCache`. Cette méthode requiert une clé (afin que le graphique puisse être demandé ultérieurement) et la durée pendant laquelle le graphique doit être enregistré dans le cache. (L’heure exacte de mise en cache d’un graphique dépend de la fréquence à laquelle vous pensiez que les données qu’il représente peuvent changer.) La méthode `SaveToCache` nécessite également un paramètre &#8212; `slidingExpiration` si cette propriété a la valeur true, le compteur de délai d’attente est réinitialisé chaque fois que l’utilisateur accède au graphique. Dans ce cas, cela signifie que l’entrée de cache du graphique expire 2 minutes après la dernière fois qu’un utilisateur a accédé au graphique. (L’alternative à l’expiration décalée est l’expiration absolue, ce qui signifie que l’entrée de cache expire exactement 2 minutes après qu’elle a été placée dans le cache, quelle que soit la fréquence d’accès.)

    Enfin, le code utilise la méthode `WriteFromCache` pour extraire et restituer le graphique à partir du cache. Notez que cette méthode est à l’extérieur du bloc `if` qui vérifie le cache, car elle obtient le graphique du cache, que le graphique y soit ou ait dû être généré et enregistré dans le cache.

    Notez que dans l’exemple, la méthode `AddTitle` comprend un horodatage. (Elle ajoute la date et l’heure &#8212; actuelles &#8212; `DateTime.Now` au titre.)
5. Créez une nouvelle page nommée *ClearCache. cshtml* et remplacez son contenu par ce qui suit :

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Cette page utilise le programme d’assistance `WebCache` pour supprimer le graphique mis en cache dans *ChartSaveToCache. cshtml*. Comme indiqué précédemment, vous n’avez normalement pas besoin d’une page comme celle-ci. Vous la créez ici uniquement pour faciliter le test de la mise en cache.
6. Exécutez la page Web *ShowCachedChart. cshtml* dans un navigateur. La page affiche l’image de graphique en fonction du code contenu dans le fichier *ChartSaveToCache. cshtml* . Prenez note de ce que l’horodateur indique dans le titre du graphique. 

    ![Description : image du graphique de base avec un horodateur dans le titre du graphique](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Fermez le navigateur.
8. Réexécutez *ShowCachedChart. cshtml* . Notez que l’horodateur est le même que précédemment, ce qui indique que le graphique n’a pas été régénéré, mais qu’il a été lu à la place à partir du cache.
9. Dans *ShowCachedChart. cshtml*, cliquez sur le lien **effacer le cache** . Vous accédez alors à *ClearCache. cshtml*, qui indique que le cache a été effacé.
10. Cliquez sur le lien **revenir à ShowCachedChart. cshtml** ou réexécutez *ShowCachedChart. cshtml* à partir de WebMatrix. Notez que cette fois, l’horodateur a changé, car le cache a été effacé. Par conséquent, le code devait régénérer le graphique et le remettre dans le cache.

### <a name="saving-a-chart-as-an-image-file"></a>Enregistrement d’un graphique en tant que fichier image

Vous pouvez également enregistrer un graphique en tant que fichier image (par exemple, sous la forme d’un fichier *. jpg* ) sur le serveur. Vous pouvez ensuite utiliser le fichier image comme vous le feriez pour une image. L’avantage est que le fichier est stocké au lieu d’être enregistré dans un cache temporaire. Vous pouvez enregistrer une nouvelle image de graphique à des moments différents (par exemple, toutes les heures), puis conserver un enregistrement permanent des modifications qui se produisent dans le temps. Notez que vous devez vous assurer que votre application Web est autorisée à enregistrer un fichier dans le dossier sur le serveur où vous souhaitez placer le fichier image.

1. À la racine de votre site Web, créez un dossier nommé *\_ChartFiles* s’il n’existe pas déjà.
2. À la racine de votre site Web, créez un nouveau fichier nommé *ChartSave. cshtml*.
3. Remplacez le contenu existant par ce qui suit :

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Le code vérifie d’abord si le fichier *. jpg* existe en appelant la méthode `File.Exists`. Si le fichier n’existe pas, le code crée un nouveau `Chart` à partir d’un tableau. Cette fois-ci, le code appelle la méthode `Save` et passe le paramètre `path` pour spécifier le chemin d’accès et le nom de fichier de l’emplacement où enregistrer le graphique. Dans le corps de la page, un élément `<img>` utilise le chemin d’accès pour pointer vers le fichier *. jpg* à afficher.
4. Exécutez le fichier *ChartSave. cshtml* .
5. Revenez à WebMatrix. Notez qu’un fichier image nommé *chart01. jpg* a été enregistré dans le dossier *\_ChartFiles* .

### <a name="saving-a-chart-as-an-xml-file"></a>Enregistrement d’un graphique en tant que fichier XML

Enfin, vous pouvez enregistrer un graphique sous la forme d’un fichier XML sur le serveur. L’avantage de cette méthode par rapport à la mise en cache du graphique ou à l’enregistrement du graphique dans un fichier est que vous pouvez modifier le code XML avant d’afficher le graphique si vous le souhaitez. Votre application doit avoir des autorisations de lecture/écriture pour le dossier sur le serveur sur lequel vous souhaitez placer le fichier image.

1. À la racine de votre site Web, créez un nouveau fichier nommé *ChartSaveXml. cshtml*.
2. Remplacez le contenu existant par ce qui suit :

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Ce code est similaire au code que vous avez vu précédemment pour le stockage d’un graphique dans le cache, à ceci près qu’il utilise un fichier XML. Le code vérifie d’abord si le fichier XML existe en appelant la méthode `File.Exists`. Si le fichier existe, le code crée un nouvel objet `Chart` et passe le nom de fichier en tant que paramètre `themePath`. Cela crée le graphique en fonction de ce qui se trouve dans le fichier XML. Si le fichier XML n’existe pas encore, le code crée un graphique comme normal, puis appelle `SaveXml` pour l’enregistrer. Le graphique est rendu à l’aide de la méthode `Write`, comme vous l’avez vu précédemment.

    Comme pour la page qui a montré la mise en cache, ce code comprend un horodateur dans le titre du graphique.
3. Créez une nouvelle page nommée *ChartDisplayXMLChart. cshtml* et ajoutez-lui le balisage suivant : 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Exécutez la page *ChartDisplayXMLChart. cshtml* . Le graphique est affiché. Prenez note de l’horodateur dans le titre du graphique.
5. Fermez le navigateur.
6. Dans WebMatrix, cliquez avec le bouton droit sur le dossier *\_ChartFiles* , cliquez sur **Actualiser**, puis ouvrez le dossier. Le fichier *XMLChart. xml* de ce dossier a été créé par le programme d’assistance `Chart`. 

    ![Description : le dossier _ChartFiles qui indique le fichier XMLChart. XML créé par le programme d’assistance du graphique.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Réexécutez la page *ChartDisplayXMLChart. cshtml* . Le graphique affiche le même horodatage que la première fois que vous exécutez la page. Cela est dû au fait que le graphique est généré à partir du code XML que vous avez enregistré précédemment.
8. Dans WebMatrix, ouvrez le dossier *\_ChartFiles* et supprimez le fichier *XMLChart. xml* .
9. Réexécutez la page *ChartDisplayXMLChart. cshtml* . Cette fois-ci, l’horodateur est mis à jour, car le `Chart` Helper devait recréer le fichier XML. Si vous le souhaitez, vérifiez le dossier *\_ChartFiles* et notez que le fichier XML est de retour.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Présentation de l’utilisation d’une base de données dans des sites pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Utilisation de la mise en cache dans les sites pages Web ASP.NET pour améliorer les performances](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Chart, classe](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (référence des API pages Web ASP.net sur MSDN)
