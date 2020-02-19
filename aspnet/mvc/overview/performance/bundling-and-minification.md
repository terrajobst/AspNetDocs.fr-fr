---
uid: mvc/overview/performance/bundling-and-minification
title: Regroupement et minimisation | Microsoft Docs
author: Rick-Anderson
description: Le regroupement et la minimisation sont deux techniques que vous pouvez utiliser dans ASP.NET 4,5 pour améliorer le temps de chargement des demandes. Le regroupement et la minimisation améliorent le temps de chargement par reducin...
ms.author: riande
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 61bfe5dbac04b57e1461183b66ead2f01fe0734c
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457763"
---
# <a name="bundling-and-minification"></a>Groupement et minimisation

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Le regroupement et la minimisation sont deux techniques que vous pouvez utiliser dans ASP.NET 4,5 pour améliorer le temps de chargement des demandes. Le regroupement et la minimisation améliorent le temps de chargement en réduisant le nombre de demandes au serveur et en réduisant la taille des ressources demandées (telles que CSS et JavaScript).

La plupart des navigateurs principaux actuels limitent le nombre de [connexions simultanées](http://www.browserscope.org/?category=network) par nom d’hôte à six. Cela signifie que si six demandes sont traitées, les demandes supplémentaires de ressources sur un ordinateur hôte sont mises en file d’attente par le navigateur. Dans l’image ci-dessous, les onglets réseau des outils de développement Internet Explorer F12 affichent le minutage des ressources requises par la vue à propos d’un exemple d’application.

![B/M](bundling-and-minification/_static/image1.png)

Les barres grises indiquent l’heure à laquelle la demande est mise en file d’attente par le navigateur qui attend la limite de six connexions. La barre jaune représente la durée de la demande jusqu’au premier octet, c’est-à-dire le temps nécessaire pour envoyer la demande et recevoir la première réponse du serveur. Les barres bleues indiquent le temps nécessaire pour recevoir les données de réponse du serveur. Vous pouvez double-cliquer sur un élément multimédia pour obtenir des informations détaillées sur la temporisation. Par exemple, l’image suivante montre les détails de minutage du chargement du fichier */scripts/myscripts/JavaScript6.js* .

![](bundling-and-minification/_static/image2.png)

L’image précédente montre l’événement **Start** , qui indique l’heure à laquelle la demande a été mise en file d’attente, car le navigateur limite le nombre de connexions simultanées. Dans ce cas, la demande a été mise en file d’attente pendant 46 millisecondes en attendant la fin d’une autre requête.

## <a name="bundling"></a>Regroupement

Le regroupement est une nouvelle fonctionnalité de ASP.NET 4,5 qui facilite la combinaison ou le regroupement de plusieurs fichiers dans un seul fichier. Vous pouvez créer des packs CSS, JavaScript et autres. Moins de fichiers signifie moins de requêtes HTTP et peuvent améliorer les performances de chargement de la première page.

L’illustration suivante montre la même vue de minuterie de la vue about présentée précédemment, mais cette fois avec le regroupement et la minimisation activés.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Réduction

La minimisation effectue une grande variété d’optimisations du code pour les scripts ou la CSS, par exemple la suppression des espaces et des commentaires inutiles et le raccourcissement des noms de variables en un seul caractère. Considérons la fonction JavaScript suivante.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Après la minimisation, la fonction est réduite à ce qui suit :

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

En plus de supprimer les commentaires et les espaces superflus, les noms de variables et les paramètres suivants ont été renommés (raccourcis) comme suit :

| **Ressource d’origine** | **Renommé** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>Impact du regroupement et de la minimisation

Le tableau suivant présente plusieurs différences importantes entre la liste de toutes les ressources individuellement et l’utilisation du regroupement et de la minimisation (B/M) dans l’exemple de programme.

|  | **Utilisation de B/M** | **Sans B/M** | **Modification** |
| --- | --- | --- | --- |
| **Demandes de fichier** | 9 | 34 | 256% |
| **Ko envoyés** | 3.26 | 11.92 | 266% |
| **Ko reçus** | 388.51 | 530 | 36% |
| **Temps de chargement** | 510 MS | 780 MS | 53% |

Les octets envoyés ont subi une réduction significative du regroupement, car les navigateurs sont relativement détaillés avec les en-têtes HTTP qu’ils appliquent sur les requêtes. La réduction des octets reçus n’est pas aussi importante, car les fichiers les plus volumineux (*scripts\\jQuery-UI-1.8.11. min. js* et *scripts\\jQuery-1.7.1. min. js*) sont déjà minimisés. Remarque : le minutage de l’exemple de programme a utilisé l’outil [Fiddler](http://www.fiddler2.com/fiddler2/) pour simuler un réseau lent. (Dans le menu **règles** de Fiddler, sélectionnez **performances** , puis **simuler les vitesses de modem**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Débogage du code JavaScript groupé et minimisés

Il est facile de déboguer votre code JavaScript dans un environnement de développement (où l' [élément compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) du fichier *Web. config* a la valeur `debug="true"`), car les fichiers JavaScript ne sont pas regroupés ou minimisés. Vous pouvez également déboguer une version Release où vos fichiers JavaScript sont regroupés et minimisés. À l’aide des outils de développement Internet Explorer F12, vous déboguez une fonction JavaScript incluse dans un bundle minimisés à l’aide de l’approche suivante :

1. Sélectionnez l’onglet **script** , puis cliquez sur le bouton **Démarrer le débogage** .
2. Sélectionnez le bundle contenant la fonction JavaScript que vous souhaitez déboguer à l’aide du bouton composants.  
    ![](bundling-and-minification/_static/image4.png)
3. Mettez en forme le JavaScript minimisés en sélectionnant le **bouton de Configuration** ![](bundling-and-minification/_static/image5.png), puis en sélectionnant **format JavaScript**.
4. Dans la zone de texte **Rechercher un script** , sélectionnez le nom de la fonction que vous souhaitez déboguer. Dans l’image suivante, **AddAltToImg** a été entré dans la zone de texte Rechercher dans le **script** .  
    ![](bundling-and-minification/_static/image6.png)

Pour plus d’informations sur le débogage avec les outils de développement F12, consultez l’article MSDN [utilisation de la touche f12 outils de développement pour déboguer les erreurs JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Contrôle du regroupement et de la minimisation

Le regroupement et la minimisation sont activés ou désactivés en définissant la valeur de l’attribut de débogage dans l' [élément compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) du fichier *Web. config* . Dans le code XML suivant, `debug` a la valeur true pour que le regroupement et la minimisation soient désactivés.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Pour activer le regroupement et la minimisation, définissez la valeur `debug` sur « false ». Vous pouvez remplacer le paramètre *Web. config* par la propriété `EnableOptimizations` de la classe `BundleTable`. Le code suivant active le regroupement et la minimisation et remplace tout paramètre dans le fichier *Web. config* .

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> Sauf si `EnableOptimizations` est `true` ou si l’attribut de débogage de l' [élément compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) dans le fichier *Web. config* est défini sur `false`, les fichiers ne seront pas regroupés ou minimisés. En outre, la version. min des fichiers ne sera pas utilisée, les versions de débogage complètes seront sélectionnées. `EnableOptimizations` remplace l’attribut debug dans l' [élément compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) du fichier *Web. config*

## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Utilisation du regroupement et de la minimisation avec ASP.NET Web Forms et les pages Web

- Pour les pages Web, consultez l’entrée de blog [Ajout de l’optimisation Web à un site Web pages](https://docs.microsoft.com/archive/blogs/rickandy/adding-web-optimization-to-a-web-pages-site).
- Pour Web Forms, consultez l’entrée de blog [Ajout de regroupement et minimisation à Web Forms](https://docs.microsoft.com/archive/blogs/rickandy/adding-bundling-and-minification-to-web-forms).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Utilisation du regroupement et de la minimisation avec ASP.NET MVC

Dans cette section, nous allons créer un projet MVC ASP.NET pour examiner le regroupement et la minimisation. Commencez par créer un nouveau projet ASP.NET MVC Internet nommé **MvcBM** sans modifier les valeurs par défaut.

Ouvrez l' *application\\\_démarrer\\fichier BundleConfig.cs* et examinez la méthode `RegisterBundles` qui est utilisée pour créer, inscrire et configurer des offres groupées. Le code suivant illustre une partie de la méthode `RegisterBundles`.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Le code précédent crée un lot JavaScript nommé *~/bundles/jQuery* qui comprend tous les fichiers appropriés (c’est-à-dire débogage ou minimisés, mais pas. *vsdoc*) dans le dossier *scripts* qui correspondent à la chaîne générique « ~/scripts/jQuery-{version}.js ». Pour ASP.NET MVC 4, cela signifie qu’avec une configuration Debug, le fichier *jQuery-1.7.1. js* est ajouté au bundle. Dans une configuration Release, *jQuery-1.7.1. min. js* sera ajouté. L’infrastructure de regroupement suit plusieurs conventions courantes, telles que :

- Sélection du fichier « . min » pour la mise en sortie quand *FileX. min. js* et *FileX. js* existent.
- Sélection de la version non « . min » pour Debug.
- Fichiers « -vsdoc » ignorés (par exemple, *jQuery-1.7.1-vsdoc. js*), utilisés uniquement par IntelliSense.

Le `{version}` caractère générique correspondant indiqué ci-dessus est utilisé pour créer automatiquement un bundle jQuery avec la version appropriée de jQuery dans votre dossier de *scripts* . Dans cet exemple, l’utilisation d’un caractère générique offre les avantages suivants :

- Vous permet d’utiliser NuGet pour effectuer une mise à jour vers une version jQuery plus récente sans modifier le code de regroupement précédent ou les références jQuery dans vos pages d’affichage.
- Sélectionne automatiquement la version complète des configurations de débogage et la version « . min » pour les versions release.

## <a name="using-a-cdn"></a>Utilisation d’un CDN

 Le code suivant remplace l’offre jQuery locale par un bundle jQuery de CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

Dans le code ci-dessus, jQuery sera demandé à partir du CDN en mode release et la version Debug de jQuery sera extraite localement en mode débogage. Lorsque vous utilisez un CDN, vous devez disposer d’un mécanisme de secours en cas d’échec de la demande CDN. Le fragment de balisage suivant à partir de la fin du fichier de disposition indique que le script a été ajouté pour demander jQuery si le CDN échoue.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Création d’un bundle

La classe [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `Include` méthode prend un tableau de chaînes, où chaque chaîne est un chemin d’accès virtuel à la ressource. Le code suivant de la méthode `RegisterBundles` dans l' *application\\\_Start\\fichier BundleConfig.cs* montre comment plusieurs fichiers sont ajoutés à un bundle :

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

La classe [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `IncludeDirectory` méthode est fournie pour ajouter tous les fichiers d’un répertoire (et éventuellement tous ses sous-répertoires) qui correspondent à un modèle de recherche. La classe [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `IncludeDirectory` API est illustrée ci-dessous :

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Les offres groupées sont référencées dans les vues à l’aide de la méthode Render, (`Styles.Render` pour CSS et `Scripts.Render` pour JavaScript). Le balisage suivant des *vues\\fichier shared\\\_Layout. cshtml* montre comment les vues de projet Internet ASP.NET par défaut référencent les lots CSS et JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Notez que les méthodes Render prennent un tableau de chaînes, ce qui vous permet d’ajouter plusieurs offres groupées en une seule ligne de code. En général, vous souhaitez utiliser les méthodes Render qui créent le code HTML nécessaire pour référencer l’élément multimédia. Vous pouvez utiliser la méthode `Url` pour générer l’URL de l’élément multimédia sans le balisage nécessaire pour faire référence à l’élément multimédia. Supposons que vous souhaitiez utiliser le nouvel attribut HTML5 [Async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) . Le code suivant montre comment faire référence à Modernizr à l’aide de la méthode `Url`.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Utilisation du caractère générique «\*» pour sélectionner des fichiers

Le chemin d’accès virtuel spécifié dans la méthode `Include` et le modèle de recherche dans la méthode `IncludeDirectory` peuvent accepter un caractère générique «\*» comme préfixe ou suffixe dans le dernier segment de chemin d’accès. La chaîne de recherche ne respecte pas la casse. La méthode `IncludeDirectory` a la possibilité de rechercher des sous-répertoires.

Imaginez un projet avec les fichiers JavaScript suivants :

- *Scripts\\Common\\AddAltToImg. js*
- *Scripts\\Common\\ToggleDiv. js*
- *Scripts\\Common\\ToggleImg. js*
- *Scripts\\\\s courantes Sub1\\ToggleLinks. js*

![Répertoire imag](bundling-and-minification/_static/image7.png)

Le tableau suivant montre les fichiers ajoutés à un regroupement à l’aide du caractère générique, comme indiqué ci-dessous :

| **Call** | **Fichiers ajoutés ou exception levée** |
| --- | --- |
| Include ("~/Scripts/Common/\*. js") | *AddAltToImg. js*, *ToggleDiv. js*, *ToggleImg. js* |
| Include ("~/Scripts/Common/T\*. js") | Exception de modèle non valide. Le caractère générique est uniquement autorisé sur le préfixe ou le suffixe. |
| Include ("~/Scripts/Common/\*og.\*») | Exception de modèle non valide. Un seul caractère générique est autorisé. |
| Include ("~/Scripts/Common/T\*") | *ToggleDiv. js*, *ToggleImg. js* |
| Include ("~/Scripts/Common/\*") | Exception de modèle non valide. Un segment de caractère générique pur n’est pas valide. |
| IncludeDirectory ("~/Scripts/Common", "T\*") | *ToggleDiv. js*, *ToggleImg. js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", true) | *ToggleDiv. js*, *ToggleImg. js*, *ToggleLinks. js* |

L’ajout explicite de chaque fichier à un bundle est généralement le chargement par défaut des fichiers avec caractères génériques, pour les raisons suivantes :

- Par défaut, l’ajout de scripts par défaut permet de les charger dans l’ordre alphabétique, ce qui n’est généralement pas ce que vous voulez. Les fichiers CSS et JavaScript doivent souvent être ajoutés dans un ordre spécifique (non alphabétique). Vous pouvez réduire ce risque en ajoutant une implémentation personnalisée de [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) , mais l’ajout explicite de chaque fichier est moins sujet aux erreurs. Par exemple, vous pouvez ajouter de nouvelles ressources à un dossier à l’avenir, ce qui peut nécessiter la modification de votre implémentation de [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) .
- Les fichiers spécifiques ajoutés à un répertoire à l’aide du chargement de caractères génériques peuvent être inclus dans toutes les vues qui font référence à cette offre groupée. Si le script de vue spécifique est ajouté à un bundle, vous pouvez obtenir une erreur JavaScript sur d’autres vues qui référencent le bundle.
- Les fichiers CSS qui importent d’autres fichiers entraînent le chargement des fichiers importés deux fois. Par exemple, le code suivant crée un bundle avec la plupart des fichiers CSS de thème de l’interface utilisateur jQuery chargés à deux reprises. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Le sélecteur de caractères génériques «\*. CSS » réunit chaque fichier CSS dans le dossier, y compris le *contenu\\thèmes\\base\\le fichier jQuery. UI. All. CSS* . Le fichier *jQuery. UI. All. CSS* importe d’autres fichiers CSS.

## <a name="bundle-caching"></a>Mise en cache de Bundle

Les offres groupées définissent l’en-tête HTTP Expires un an à partir de la création du bundle. Si vous accédez à une page précédemment affichée, Fiddler indique qu’IE n’effectue pas de demande conditionnelle pour le bundle, autrement dit, qu’il n’y a aucune requête HTTP obtenir provenant d’IE pour les offres groupées et aucune réponse HTTP 304 du serveur. Vous pouvez forcer IE à effectuer une demande conditionnelle pour chaque bundle avec la touche F5 (ce qui génère une réponse HTTP 304 pour chaque Bundle). Vous pouvez forcer une actualisation complète en utilisant ^ F5 (ce qui génère une réponse HTTP 200 pour chaque bundle.)

L’illustration suivante montre l’onglet **mise en cache** du volet de réponse Fiddler :

![image de mise en cache Fiddler](bundling-and-minification/_static/image8.png)

Requête.   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 est pour le bundle **AllMyScripts** et contient une paire de chaînes de requête **v = R0sLDicvP58AIXN\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. La chaîne de requête **v** a un jeton de valeur qui est un identificateur unique utilisé pour la mise en cache. Tant que le bundle n’est pas modifié, l’application ASP.NET demande le bundle **AllMyScripts** à l’aide de ce jeton. Si un fichier du bundle change, l’infrastructure d’optimisation ASP.NET génère un nouveau jeton, garantissant ainsi que les demandes de navigateur pour le bundle obtiendront le dernier bundle.

Si vous exécutez les outils de développement IE9 F12 et que vous accédez à une page précédemment chargée, IE affiche incorrectement les demandes d’accès conditionnel effectuées à chaque Bundle et le serveur retournant HTTP 304. Vous pouvez lire Pourquoi IE9 a des problèmes pour déterminer si une demande conditionnelle a été effectuée dans l’entrée de blog [à l’aide de CDN et expire pour améliorer les performances du site Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>MOINS, CoffeeScript, SCSS, regroupement Sass.

L’infrastructure de regroupement et de minimisation fournit un mécanisme de traitement des langages intermédiaires tels que [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [less](http://www.dotlesscss.org/) ou [CoffeeScript](http://coffeescript.org/), et applique des transformations telles que la minimisation au Bundle résultant. Par exemple, pour ajouter des fichiers [. less](http://www.dotlesscss.org/) à votre projet MVC 4 :

1. Créez un dossier pour votre contenu moins cher. L’exemple suivant utilise le dossier *Content\\MyLess* .
2. Ajoutez **le package NuGet** [. moins sans](http://www.dotlesscss.org/) point à votre projet.  
    ![installation sans point NuGet](bundling-and-minification/_static/image9.png)
3. Ajoutez une classe qui implémente l’interface [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) . Pour la transformation. less, ajoutez le code suivant à votre projet.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Créez un groupe de fichiers moins avec les `LessTransform` et la transformation [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) . Ajoutez le code suivant à la méthode `RegisterBundles` dans le *\\d’application _Start\\fichier BundleConfig.cs* .

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Ajoutez le code suivant à toutes les vues qui font référence à l’offre moins groupée.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Considérations relatives au Bundle

Une bonne Convention à suivre lors de la création d’offres groupées consiste à inclure « Bundle » comme préfixe dans le nom du bundle. Cela empêchera un éventuel [conflit de routage](https://forums.asp.net/post/5012037.aspx).

Une fois que vous avez mis à jour un fichier dans un bundle, un nouveau jeton est généré pour le paramètre de chaîne de requête de Bundle et le bundle complet doit être téléchargé la prochaine fois qu’un client demande une page contenant le bundle. Dans le balisage traditionnel où chaque élément multimédia est listé individuellement, seul le fichier modifié est téléchargé. Les ressources qui changent fréquemment peuvent ne pas être de bons candidats pour le regroupement.

Le regroupement et la minimisation améliorent principalement le temps de chargement de la première page de la demande. Une fois qu’une page Web a été demandée, le navigateur met en cache les éléments multimédias (JavaScript, CSS et images), de sorte que le regroupement et la minimisation n’offrent aucune amélioration des performances lors de la demande de la même page ou des pages sur le même site qui demandent les mêmes ressources. Si vous ne définissez pas correctement l’en-tête Expires sur vos ressources et que vous n’utilisez pas le regroupement et la minimisation, les heuristiques d’actualisation des navigateurs marqueront les ressources obsolètes après quelques jours et le navigateur nécessitera une demande de validation pour chaque ressource. Dans ce cas, le regroupement et la minimisation offrent une augmentation des performances après la première demande de page. Pour plus d’informations, consultez le blog [utilisation de CDN et Expires pour améliorer les performances d’un site Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

La limitation du navigateur de six connexions simultanées par nom d’hôte peut être atténuée à l’aide d’un [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Étant donné que le CDN aura un nom d’hôte différent de celui de votre site d’hébergement, les demandes de ressources du CDN ne seront pas décomptées par rapport à la limite de six connexions simultanées à votre environnement d’hébergement. Un CDN peut également fournir des avantages courants en matière de mise en cache de packages et de mise en cache Edge.

Les lots doivent être partitionnés par les pages qui en ont besoin. Par exemple, le modèle MVC ASP.NET par défaut pour une application Internet crée un bundle de validation jQuery distinct de jQuery. Comme les vues par défaut créées n’ont pas d’entrée et ne publient pas de valeurs, elles n’incluent pas le bundle de validation.

L’espace de noms `System.Web.Optimization` est implémenté dans *System. Web. Optimization. dll*. Elle tire parti de la bibliothèque webgraissage (*webgraisse. dll*) pour les capacités de minimisation, qui à son tour utilise *Antlr3. Runtime. dll*.

*J’utilise Twitter pour effectuer des publications rapides et partager des liens. Mon handle Twitter est*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Ressources supplémentaires

- Vidéo :[regroupement et optimisation](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) par [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Ajout de l’optimisation Web à un site Web pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Ajout d’un regroupement et minimisation à Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Implications sur les performances du regroupement et de la minimisation sur la navigation Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) par [clément F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [L’utilisation de CDN et expire pour améliorer les performances des sites Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) par Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Réduire le temps RTT (aller-retour)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Contributeurs

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
