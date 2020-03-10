---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Utiliser des contrôleurs et des vues pour implémenter une interface utilisateur de liste/détails | Microsoft Docs
author: microsoft
description: L’étape 4 montre comment ajouter un contrôleur à l’application qui tire parti de notre modèle pour fournir aux utilisateurs une expérience de navigation de liste de données/détails...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 74319fe5ea4c79b50140834349e2fdf86420cfbb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600743"
---
# <a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Utiliser des contrôleurs et des vues pour implémenter une interface utilisateur liste/détails

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 4 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.
> 
> L’étape 4 montre comment ajouter un contrôleur à l’application qui tire parti de notre modèle pour fournir aux utilisateurs une expérience de navigation de liste de données/détails pour les dîners sur notre site NerdDinner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner étape 4 : contrôleurs et vues

Avec les infrastructures Web traditionnelles (Classic ASP, PHP, ASP.NET Web Forms, etc.), les URL entrantes sont généralement mappées à des fichiers sur le disque. Par exemple : une requête pour une URL telle que « /Products.aspx » ou « /Products.php » peut être traitée par un fichier « Products. aspx » ou « Products. php ».

Les frameworks MVC basés sur le Web mappent les URL au code du serveur d’une manière légèrement différente. Au lieu de mapper les URL entrantes aux fichiers, elles mappent les URL aux méthodes sur les classes. Ces classes sont appelées « contrôleurs » et sont responsables du traitement des demandes HTTP entrantes, de la gestion des entrées d’utilisateur, de la récupération et de l’enregistrement des données et de la détermination de la réponse à renvoyer au client (afficher le code HTML, télécharger un fichier, rediriger vers un autre URL, etc.).

Maintenant que nous avons créé un modèle de base pour notre application NerdDinner, l’étape suivante consiste à ajouter un contrôleur à l’application qui tire parti de ce dernier pour fournir aux utilisateurs une expérience de navigation de liste de données/détails pour les dîners sur notre site.

### <a name="adding-a-dinnerscontroller-controller"></a>Ajout d’un contrôleur DinnersController

Nous allons commencer par cliquer avec le bouton droit sur le dossier « Controllers » dans notre projet Web, puis sélectionner la commande de menu **Ajouter-&gt;Controller** (vous pouvez également exécuter cette commande en tapant Ctrl-M, Ctrl-C) :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

La boîte de dialogue « Ajouter un contrôleur » s’affiche :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Nous allons nommer le nouveau contrôleur « DinnersController », puis cliquer sur le bouton « Ajouter ». Visual Studio ajoute ensuite un fichier DinnersController.cs sous notre répertoire \Controllers :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

La nouvelle classe DinnersController est également ouverte dans l’éditeur de code.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Ajout de méthodes d’action index () et Details () à la classe DinnersController

Nous souhaitons permettre aux visiteurs qui utilisent notre application de parcourir une liste de dîners à venir et de leur permettre de cliquer sur n’importe quel dîner de la liste pour afficher des détails spécifiques. Pour ce faire, nous publions les URL suivantes à partir de notre application :

| **URL** | **Fonction** |
| --- | --- |
| */Dinners/* | Afficher une liste HTML des dîners à venir |
| */Dinners/Details/[id]* | Affichez des détails sur un dîner spécifique indiqué par un paramètre « ID » incorporé dans l’URL, qui correspondra à l’DinnerID du dîner dans la base de données. Par exemple:/Dinners/Details/2 affiche une page HTML avec des détails sur le dîner dont la valeur DinnerID est 2. |

Nous publierons les implémentations initiales de ces URL en ajoutant deux « méthodes d’action » publiques à notre classe DinnersController, comme ci-dessous :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Nous allons ensuite exécuter l’application NerdDinner et utiliser notre navigateur pour les appeler. Si vous tapez l’URL *« /dinners/ »* , la méthode *index ()* est exécutée et la réponse suivante est renvoyée :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Si vous tapez sur l’URL *« /dinners/Details/2 »* , la méthode *Details ()* est exécutée et la réponse suivante est renvoyée :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Vous vous demandez peut-être Comment ASP.NET MVC a-t-il appris à créer notre classe DinnersController et à appeler ces méthodes ? Pour comprendre ce que nous allons voir rapidement comment fonctionne le routage.

### <a name="understanding-aspnet-mvc-routing"></a>Fonctionnement du routage ASP.NET MVC

ASP.NET MVC inclut un moteur de routage d’URL puissant qui offre une grande flexibilité pour contrôler la façon dont les URL sont mappées aux classes de contrôleur. Elle nous permet de personnaliser complètement la manière dont ASP.NET MVC choisit la classe de contrôleur à créer, la méthode à appeler, ainsi que de configurer les différentes façons dont les variables peuvent être analysées automatiquement à partir de l’URL/QueryString et transmises à la méthode en tant qu’arguments de paramètre. Il offre la possibilité d’optimiser totalement un site pour le SEO (optimisation du moteur de recherche) et de publier toute structure d’URL souhaitée à partir d’une application.

Par défaut, les nouveaux projets MVC ASP.NET sont fournis avec un ensemble préconfiguré de règles de routage d’URL déjà inscrites. Cela nous permet de prendre en main facilement une application sans avoir à configurer explicitement quoi que ce soit. Les inscriptions de règle de routage par défaut se trouvent dans la classe « application » de nos projets, que nous pouvons ouvrir en double-cliquant sur le fichier « global. asax » à la racine de notre projet :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Les règles de routage ASP.NET MVC par défaut sont inscrites dans la méthode « RegisterRoutes » de cette classe :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Les routes. L’appel de méthode MapRoute () "ci-dessus inscrit une règle de routage par défaut qui mappe les URL entrantes aux classes de contrôleur à l’aide du format d’URL :"/{Controller}/{action}/{ID} "– où" Controller "est le nom de la classe de contrôleur à instancier," action "est le nom d’une méthode publique à appeler, et" ID "est un paramètre facultatif incorporé dans l’URL qui peut être passé comme argument à la méthode. Le troisième paramètre passé à l’appel de méthode "MapRoute ()" est un ensemble de valeurs par défaut à utiliser pour les valeurs de contrôleur/action/ID dans le cas où elles ne sont pas présentes dans l’URL (Controller = "orig", action = "index", ID = "").

Voici un tableau qui montre comment une variété d’URL est mappée à l’aide de la règle de routage «<em>/{Controllers}/{action}/{ID} »</em>par défaut :

| **URL** | **Controller (classe)** | **Méthode d’action** | **Paramètres passés** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Détails (ID) | id=2 |
| */Dinners/Edit/5* | DinnersController | Modifier (ID) | id=5 |
| */Dinners/Create* | DinnersController | Create() | N/A |
| */Dinners* | DinnersController | Index() | N/A |
| */Home* | HomeController | Index() | N/A |
| */* | HomeController | Index() | N/A |

Les trois dernières lignes affichent les valeurs par défaut (Controller = orig, action = index, ID = "") utilisées. Étant donné que la méthode « index » est enregistrée comme nom d’action par défaut si aucune n’est spécifiée, les URL « /dinners » et « /Home » entraînent l’appel de la méthode d’action index () sur leurs classes de contrôleur. Étant donné que le contrôleur « d’origine » est inscrit en tant que contrôleur par défaut s’il n’est pas spécifié, l’URL « / » provoque la création du HomeController et la méthode d’action index () sur ce dernier à appeler.

Si vous n’aimez pas ces règles de routage d’URL par défaut, la bonne nouvelle est qu’elles sont faciles à modifier. il suffit de les modifier dans la méthode RegisterRoutes ci-dessus. Pour notre application NerdDinner, cependant, nous n’allons pas modifier les règles de routage d’URL par défaut. au lieu de cela, nous allons simplement les utiliser en l’État.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Utilisation du DinnerRepository à partir de notre DinnersController

Remplaçons maintenant l’implémentation actuelle des méthodes d’action index () et Details () de DinnersController avec les implémentations qui utilisent notre modèle.

Nous allons utiliser la classe DinnerRepository que nous avons créée précédemment pour implémenter le comportement. Nous allons commencer par ajouter une instruction « using » qui fait référence à l’espace de noms « NerdDinner. Models », puis déclarer une instance de notre DinnerRepository en tant que champ dans notre classe DinnerController.

Plus loin dans ce chapitre, nous allons présenter le concept d' « injection de dépendances » et montrer une autre façon pour nos contrôleurs d’obtenir une référence à un DinnerRepository qui permet d’améliorer les tests unitaires, mais pour le moment, nous allons simplement créer une instance de notre DinnerRepository Inline comme ci-dessous.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Nous sommes maintenant prêts à générer une réponse HTML à l’aide de nos objets de modèle de données récupérés.

### <a name="using-views-with-our-controller"></a>Utilisation des vues avec notre contrôleur

Bien qu’il soit possible d’écrire du code dans nos méthodes d’action pour assembler du code HTML, puis utiliser la méthode d’assistance *Response. Write ()* pour le renvoyer au client, cette approche devient assez complexe. Une meilleure approche consiste à uniquement exécuter la logique d’application et de données à l’intérieur de nos méthodes d’action DinnersController, puis à passer les données nécessaires pour afficher une réponse HTML à un modèle de « vue » distinct qui est responsable de la génération de la représentation HTML. de celui-ci. Comme nous le verrons dans un moment, un modèle « View » est un fichier texte qui contient généralement une combinaison de balisage HTML et de code de rendu incorporé.

La séparation de la logique du contrôleur de notre rendu de la vue apporte plusieurs grands avantages. En particulier, il permet d’appliquer une « séparation des préoccupations » claire entre le code de l’application et le code de rendu/de rendu de l’interface utilisateur. Cela simplifie grandement la logique d’application de test unitaire, à partir de la logique de rendu d’interface utilisateur. Cela facilite la modification ultérieure des modèles de rendu de l’interface utilisateur sans avoir à apporter de modifications au code de l’application. De plus, il peut faciliter la collaboration entre les développeurs et les concepteurs sur des projets.

Nous pouvons mettre à jour notre classe DinnersController pour indiquer que nous voulons utiliser un modèle de vue pour renvoyer une réponse de l’interface utilisateur HTML en remplaçant les signatures de méthode de nos deux méthodes d’action par un type de retour « void » pour avoir un type de retour « ActionResult ». Nous pouvons ensuite appeler la méthode d’assistance *View ()* sur la classe de base Controller pour retourner un objet « ViewResult » comme ci-dessous :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

La signature de la méthode d’assistance *View ()* que nous utilisons ci-dessus ressemble à ceci :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Le premier paramètre de la méthode d’assistance *View ()* est le nom du fichier de modèle de vue que vous souhaitez utiliser pour afficher la réponse html. Le deuxième paramètre est un objet de modèle qui contient les données dont le modèle de vue a besoin pour afficher la réponse HTML.

Dans notre méthode d’action index (), nous appelons la méthode d’assistance *View ()* et nous indiquons que nous souhaitons afficher une liste HTML des dîners à l’aide d’un modèle de vue « index ». Nous transmettons le modèle de vue une séquence de dîner d’objets à partir de laquelle générer la liste :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

Dans notre méthode d’action Details (), nous tentons de récupérer un objet dîner à l’aide de l’ID fourni dans l’URL. Si un dîner valide est trouvé, nous appelons la méthode d’assistance *View ()* , indiquant que nous souhaitons utiliser un modèle de vue « Details » pour afficher l’objet Dinner récupéré. Si un dîner non valide est demandé, nous restituons un message d’erreur utile qui indique que le dîner n’existe pas à l’aide d’un modèle de vue « NotFound » (et d’une version surchargée de la méthode d’assistance de *vue ()* qui prend simplement le nom du modèle) :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Nous allons maintenant implémenter les modèles de vue « NotFound », « Details » et « index ».

### <a name="implementing-the-notfound-view-template"></a>Implémentation du modèle de vue « NotFound »

Nous allons commencer par implémenter le modèle de vue « NotFound », qui affiche un message d’erreur convivial indiquant que le dîner demandé est introuvable.

Nous allons créer un nouveau modèle de vue en positionnant notre curseur de texte dans une méthode d’action de contrôleur, puis cliquer avec le bouton droit et choisir la commande de menu « Ajouter une vue » (nous pouvons également exécuter cette commande en tapant Ctrl-M, Ctrl-V) :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Cela entraîne l’affichage d’une boîte de dialogue « Ajouter une vue » comme ci-dessous. Par défaut, la boîte de dialogue prérenseigne le nom de la vue à créer pour correspondre au nom de la méthode d’action dans laquelle se trouvait le curseur lorsque la boîte de dialogue a été lancée (dans ce cas « Details »). Étant donné que nous voulons tout d’abord implémenter le modèle « NotFound », nous allons remplacer ce nom de vue et le définir sur « NotFound » à la place :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio crée un nouveau modèle de vue « NotFound. aspx » pour nous dans le répertoire « \Views\Dinners » (qu’il créera également si le répertoire n’existe pas encore) :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Il ouvrira également notre nouveau modèle de vue « NotFound. aspx » dans l’éditeur de code :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Les modèles de vue par défaut ont deux « régions de contenu » où nous pouvons ajouter du contenu et du code. La première nous permet de personnaliser le « titre » de la page HTML renvoyée. La deuxième nous permet de personnaliser le « contenu principal » de la page HTML renvoyée.

Pour implémenter notre modèle de vue « NotFound », nous allons ajouter du contenu de base :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Nous pouvons ensuite l’essayer dans le navigateur. Pour ce faire, nous allons demander l’URL *« /dinners/Details/9999 »* . Cela fera référence à un dîner qui n’existe pas actuellement dans la base de données et fera en sorte que notre méthode d’action DinnersController. Details () affiche notre modèle de vue « NotFound » :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Une chose que vous remarquerez dans la capture d’écran ci-dessus est que notre modèle de vue de base a hérité d’une série de code HTML qui entoure le contenu principal à l’écran. Cela est dû au fait que notre modèle de vue utilise un modèle de « page maître » qui nous permet d’appliquer une disposition cohérente à tous les affichages sur le site. Nous expliquerons comment les pages maîtres fonctionnent mieux dans une partie ultérieure de ce didacticiel.

### <a name="implementing-the-details-view-template"></a>Implémentation du modèle de vue « Details »

Nous allons maintenant implémenter le modèle de vue « Details » (détails), qui génère le code HTML pour un seul modèle de dîner.

Pour ce faire, nous allons positionner notre curseur de texte dans la méthode d’action Details, puis cliquer avec le bouton droit et choisir la commande de menu « Add View » (ou appuyer sur Ctrl + M, Ctrl-V) :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

La boîte de dialogue « Ajouter une vue » s’affiche. Nous allons conserver le nom de la vue par défaut (« Details »). Nous allons également activer la case à cocher « créer une vue fortement typée » dans la boîte de dialogue et sélectionner (à l’aide de la liste déroulante de zone de liste modifiable) le nom du type de modèle que nous passons du contrôleur à la vue. Pour cette vue, nous transmettons un objet dîner (le nom complet de ce type est : « NerdDinner. Models. Dinner ») :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Contrairement au modèle précédent, où nous avons choisi de créer une « vue vide », nous choisissons automatiquement « échafauder » la vue à l’aide d’un modèle « détails ». Nous pouvons l’indiquer en modifiant la liste déroulante « Afficher le contenu » dans la boîte de dialogue ci-dessus.

« Génération de modèles automatique » génère une implémentation initiale de notre modèle de vue détails en fonction de l’objet dîner que nous transmettons à celui-ci. Cela vous permet de commencer rapidement à utiliser notre implémentation de modèle de vue.

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio crée un nouveau fichier de modèle de vue « Details. aspx » pour nous dans notre répertoire « \Views\Dinners » :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Il ouvrira également notre nouveau modèle de vue « Details. aspx » dans l’éditeur de code. Elle contient une implémentation de l’échafaudage initial d’un mode Détails basé sur un modèle de dîner. Le moteur de génération de modèles automatique utilise la réflexion .NET pour examiner les propriétés publiques exposées sur la classe qui l’a passé et ajoutera le contenu approprié en fonction de chaque type qu’il trouve :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Nous pouvons demander l’URL *« /dinners/Details/1 »* pour voir à quoi ressemble cette implémentation de l’échafaudage « détails » dans le navigateur. L’utilisation de cette URL affichera l’un des dîners que nous avons ajoutés manuellement à notre base de données lors de sa création initiale :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Cela nous permet d’être rapidement opérationnel et nous fournit une implémentation initiale de notre vue Details. aspx. Nous pouvons ensuite le modifier pour personnaliser l’interface utilisateur à notre satisfaction.

Lorsque nous examinons le modèle details. aspx plus en détail, nous constatons qu’il contient du code HTML statique et du code de rendu incorporé. &lt;%%&gt; code pépites exécuter le code lors du rendu du modèle de vue, et &lt;% =%&gt; code pépites exécuter le code qu’il contient, puis afficher le résultat dans le flux de sortie du modèle.

Nous pouvons écrire du code dans notre vue qui accède à l’objet de modèle « dîner » qui a été transmis par notre contrôleur à l’aide d’une propriété fortement typée « Model ». Visual Studio fournit un code complet-IntelliSense lors de l’accès à cette propriété « Model » dans l’éditeur :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Nous allons effectuer quelques ajustements afin que la source de notre modèle de vue des détails finaux ressemble à ce qui suit :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Lorsque nous revenons à l’URL *« /dinners/Details/1 »* , elle s’affiche désormais comme ci-dessous :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implémentation du modèle de vue « index »

Nous allons maintenant implémenter le modèle de vue « index », qui générera une liste de dîners à venir. Pour ce faire, nous allons positionner notre curseur de texte dans la méthode d’action d’index, puis cliquer avec le bouton droit et choisir la commande de menu « Ajouter une vue » (ou appuyer sur Ctrl + M, Ctrl-V).

Dans la boîte de dialogue « Ajouter une vue », nous conservons le modèle de vue nommé « index » et cocher « créer une vue fortement typée ». Cette fois, nous allons choisir de générer automatiquement un modèle de vue « List », puis de sélectionner « NerdDinner. Models. Dinner » comme type de modèle passé à la vue (car nous avons indiqué que nous créons une structure « List » pour que la boîte de dialogue Ajouter une vue considère que nous sommes passage d’une séquence de dîner d’objets de notre contrôleur à la vue) :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio crée un nouveau fichier de modèle d’affichage « index. aspx » pour nous dans notre répertoire « \Views\Dinners ». Il « génère une structure » dans une implémentation initiale qui fournit une liste de tables HTML des dîners que nous passons à la vue.

Lorsque nous exécutons l’application et que vous accédez à l’URL *« /dinners/ »* , elle affiche notre liste de dîners de la façon suivante :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

La solution de table ci-dessus nous donne une présentation sous forme de grille de nos données de dîner, ce qui n’est pas tout ce que nous voulons pour notre liste de dîners pour les particuliers. Nous pouvons mettre à jour le modèle de vue index. aspx et le modifier pour répertorier moins de colonnes de données, et utiliser un élément &lt;UL&gt; pour les restituer au lieu d’un tableau à l’aide du code ci-dessous :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Nous utilisons le mot clé « var » dans l’instruction foreach ci-dessus à mesure que nous effectuons une boucle sur chaque dîner dans notre modèle. Ceux qui ne connaissent C# pas 3,0 peuvent penser que l’utilisation de « var » signifie que l’objet dîner est à liaison tardive. Au lieu de cela, il signifie que le compilateur utilise l’inférence de type par rapport à la propriété fortement typée « Model » (qui est de type « IEnumerable&lt;Dinner&gt;») et compile la variable locale « Dinner » comme type de dîner, ce qui signifie que nous obtenons une fonctionnalité IntelliSense complète et une vérification au moment de la compilation dans les blocs de code :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Lorsque nous avons cliqué sur Actualiser sur l’URL */dinners* dans notre navigateur, notre vue mise à jour ressemble maintenant à ce qui suit :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

C’est une meilleure pratique, mais elle n’est pas encore complète. Notre dernière étape consiste à permettre aux utilisateurs finaux de cliquer sur des dîners individuels dans la liste et d’afficher des détails les concernant. Nous allons implémenter cela en affichant les éléments Hyperlink HTML qui sont liés à la méthode d’action Details sur notre DinnersController.

Nous pouvons générer ces liens hypertexte dans notre vue d’index de l’une des deux façons suivantes. La première consiste à créer manuellement du code HTML &lt;un&gt; éléments comme ci-dessous, où nous incorporons &lt;blocs%%&gt; dans le &lt;un élément HTML&gt; :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Une autre approche que nous pouvons utiliser est de tirer parti de la méthode d’assistance « HTML. ActionLink () » intégrée dans ASP.NET MVC qui prend en charge la création par programme d’un code HTML &lt;un élément&gt; qui est lié à une autre méthode d’action sur un contrôleur :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Premier paramètre du code html. la méthode d’assistance ActionLink () est le texte de lien à afficher (dans le cas présent, le titre du dîner), le deuxième paramètre est le nom de l’action de contrôleur pour laquelle nous voulons générer le lien (dans le cas présent, la méthode Details) et le troisième paramètre est un ensemble de paramètres à envoyer à l’action (implémenté en tant que type anonyme avec le nom/ Dans ce cas, nous spécifions le paramètre « ID » du dîner que nous voulons lier, et parce que la règle de routage d’URL par défaut dans ASP.NET MVC est « {Controller}/{Action}/{id} », la méthode d’assistance HTML. ActionLink () génère la sortie suivante :

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Pour notre vue index. aspx, nous allons utiliser l’approche de la méthode d’assistance HTML. ActionLink () et faire en sorte que chaque dîner dans la liste soit lié à l’URL des détails appropriée :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Et maintenant, lorsque nous avons atteint l’URL */dinners* , notre liste de dîner se présente comme suit :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Lorsque nous cliquons sur l’un des dîners de la liste, nous allons accéder à des détails :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Nommage basé sur une Convention et structure de répertoire \Views

Par défaut, les applications MVC ASP.NET utilisent une structure d’affectation de noms de répertoire basée sur une Convention lors de la résolution des modèles de vue. Cela permet aux développeurs d’éviter d’avoir à qualifier complètement un chemin d’accès d’emplacement lors du référencement de vues à partir d’une classe de contrôleur. Par défaut, ASP.NET MVC recherche le fichier de modèle de vue dans le répertoire * \Views\[ControllerName]\* sous l’application.

Par exemple, nous travaillons sur la classe DinnersController, qui fait explicitement référence à trois modèles de vue : « index », « Details » et « NotFound ». Par défaut, ASP.NET MVC recherche ces affichages dans le répertoire *\Views\Dinners* sous le répertoire racine de l’application :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Notez qu’il existe actuellement trois classes de contrôleur au sein du projet (DinnersController, HomeController et AccountController – les deux derniers ont été ajoutés par défaut lors de la création du projet), et il y a trois sous-répertoires (un pour chaque ) dans le répertoire \Views.

Les vues référencées à partir des contrôleurs de démarrage et de comptes résolvent automatiquement leurs modèles de vue à partir des répertoires *\Views\Home* et *\Views\Account* respectifs. Le sous-répertoire *\views\shared.* fournit un moyen de stocker les modèles de vue qui sont réutilisés sur plusieurs contrôleurs au sein de l’application. Quand ASP.NET MVC tente de résoudre un modèle de vue, il commence par vérifier dans le répertoire spécifique *\Views\[Controller]* , et s’il ne peut pas trouver le modèle de vue, il s’affiche dans le répertoire *\views\shared.*

Lorsqu’il s’agit de nommer des modèles de vue individuels, il est recommandé de faire en sorte que le modèle de vue partage le même nom que la méthode d’action qui a provoqué son rendu. Par exemple, au-dessus de notre méthode d’action « index », vous utilisez la vue « index » pour afficher le résultat de la vue, et la méthode d’action « Details » utilise la vue « Details » pour afficher ses résultats. Il est ainsi plus facile de voir rapidement quel modèle est associé à chaque action.

Les développeurs n’ont pas besoin de spécifier explicitement le nom du modèle de vue lorsque le modèle de vue porte le même nom que la méthode d’action appelée sur le contrôleur. Au lieu de cela, nous pouvons simplement passer l’objet de modèle à la méthode d’assistance « View () » (sans spécifier le nom de la vue) et ASP.NET MVC déduit automatiquement que nous voulons utiliser le modèle de vue *\Views\[ControllerName]\[ActionName]* sur le disque pour le restituer.

Cela nous permet de nettoyer notre code de contrôleur un peu et d’éviter la duplication du nom deux fois dans notre code :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Le code ci-dessus est tout ce qui est nécessaire pour implémenter une expérience attrayante de liste/détails pour le site.

#### <a name="next-step"></a>Étape suivante

Nous disposons à présent d’une expérience de navigation de dîner intéressante.

Nous allons maintenant activer la prise en charge de la modification des formulaires de données CRUD (créer, lire, mettre à jour, supprimer).

> [!div class="step-by-step"]
> [Précédent](build-a-model-with-business-rule-validations.md)
> [Suivant](provide-crud-create-read-update-delete-data-form-entry-support.md)
