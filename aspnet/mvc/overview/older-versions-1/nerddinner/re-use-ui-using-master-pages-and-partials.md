---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Réutilisation de l’interface utilisateur à l’aide des pages maîtres et des partiels | Microsoft Docs
author: microsoft
description: L’étape 7 vous apprendra comment appliquer le « principe sec » dans nos modèles de vue pour éliminer la duplication de code, en utilisant des modèles de vue partiels et des pages maîtres.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 0b17cb6ac14b7f187bf1f175097a37907689d46e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580331"
---
# <a name="re-use-ui-using-master-pages-and-partials"></a>Réutiliser l’interface utilisateur avec des pages maîtres et des vues partielles

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 7 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.
> 
> L’étape 7 vous apprendra comment appliquer le « principe sec » dans nos modèles de vue pour éliminer la duplication de code, en utilisant des modèles de vue partiels et des pages maîtres.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner étape 7 : partielles et pages maîtres

L’une des philosophies de conception utilisées par ASP.NET MVC est le principe de « ne pas se répéter » (communément appelé « sécher »). Une conception sèche permet d’éliminer la duplication du code et de la logique, ce qui accélère le développement et la gestion des applications.

Nous avons déjà vu le principe à sec appliqué dans plusieurs de nos scénarios NerdDinner. Quelques exemples : notre logique de validation est implémentée dans notre couche de modèle, ce qui lui permet de s’appliquer à la fois dans les scénarios de modification et de création dans notre contrôleur. Nous utilisons à nouveau le modèle de vue « NotFound » dans les méthodes d’action Edit, Details et DELETE. Nous utilisons un modèle d’attribution de noms de convention avec nos modèles de vue, ce qui évite d’avoir à spécifier explicitement le nom quand nous appelons la méthode d’assistance View (). et nous utilisons à nouveau la classe DinnerFormViewModel pour les scénarios d’action de modification et de création.

Voyons maintenant comment nous pouvons appliquer le « principe sec » dans nos modèles de vue pour éliminer également la duplication de code.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Revisiter les modèles de vue Edit et Create

Actuellement, nous utilisons deux modèles de vue : « Edit. aspx » et « Create. aspx » pour afficher l’interface utilisateur du dîner. Une comparaison visuelle rapide de celles-ci met en évidence la façon dont elles sont similaires. Voici à quoi ressemble le formulaire de création :

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Voici à quoi ressemble le formulaire « Edit » :

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Il n’y a pas vraiment de différence. En dehors du titre et du texte d’en-tête, la disposition du formulaire et les contrôles d’entrée sont identiques.

Si nous avions ouvert les modèles de vue « Edit. aspx » et « Create. aspx », nous allons constater qu’ils contiennent des données de mise en page et de contrôle d’entrée identiques. Cette duplication signifie que nous devons apporter des modifications à deux reprises chaque fois que vous introduisez ou modifiez une nouvelle propriété de dîner, ce qui n’est pas correct.

### <a name="using-partial-view-templates"></a>Utilisation de modèles de vue partielle

ASP.NET MVC prend en charge la possibilité de définir des modèles de « vue partielle » qui peuvent être utilisés pour encapsuler la logique de rendu de vue pour une sous-partie d’une page. Les « partiels » offrent un moyen utile de définir une fois la logique de rendu de la vue, puis de la réutiliser dans plusieurs emplacements d’une application.

Pour aider à « sécher » notre modification. aspx et à créer la duplication du modèle de vue. aspx, nous pouvons créer un modèle de vue partielle nommé « DinnerForm. ascx » qui encapsule la disposition de formulaire et les éléments d’entrée communs aux deux. Pour ce faire, cliquez avec le bouton droit sur le répertoire/Views/Dinners et choisissez la commande de menu « Add-&gt;View » :

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

La boîte de dialogue « Ajouter une vue » s’affiche. Nous allons nommer la nouvelle vue que nous voulons créer « DinnerForm », activer la case à cocher « créer une vue partielle » dans la boîte de dialogue et indiquer que nous lui transmettons une classe DinnerFormViewModel :

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio crée un nouveau modèle de vue « DinnerForm. ascx » pour nous dans le répertoire « \Views\Dinners ».

Nous pouvons ensuite copier/coller la disposition de formulaire/le code de contrôle d’entrée en double à partir des modèles de vue Edit. aspx/Create. aspx dans notre nouveau modèle de vue partielle « DinnerForm. ascx » :

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Nous pouvons ensuite mettre à jour les modèles de vue Edit et Create pour appeler le modèle partiel DinnerForm et éliminer la duplication de formulaire. Pour ce faire, nous appelons html. RenderPartial (« DinnerForm ») dans nos modèles de vue :

##### <a name="createaspx"></a>Créer. aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Vous pouvez qualifier explicitement le chemin d’accès du modèle partiel souhaité lors de l’appel de html. RenderPartial (par exemple : ~ views/dîners/DinnerForm. ascx "). Dans notre code ci-dessus, toutefois, nous utilisons le modèle d’affectation de noms basé sur des conventions dans ASP.NET MVC et spécifions simplement « DinnerForm » comme nom du partiel à restituer. Dans ce cas, ASP.NET MVC regarde d’abord dans le répertoire des vues basées sur des conventions (pour DinnersController, il s’agit de/Views/Dinners). S’il ne trouve pas le modèle partiel, il le recherche dans le répertoire/Views/Shared

Lorsque html. RenderPartial () est appelé uniquement avec le nom de la vue partielle, ASP.NET MVC passe à la vue partielle les mêmes objets dictionary Model et ViewData que ceux utilisés par le modèle de vue appelant. En guise d’alternative, il existe des versions surchargées de html. RenderPartial () qui vous permettent de passer un autre objet de modèle et/ou un dictionnaire ViewData pour la vue partielle à utiliser. Cela est utile dans les scénarios où vous souhaitez uniquement passer un sous-ensemble du modèle/ViewModel complet.

| **Rubrique secondaire : pourquoi &lt;%%&gt; au lieu de &lt;% =%&gt;?** |
| --- |
| L’une des choses subtiles que vous avez pu remarquer avec le code ci-dessus est que nous utilisons un bloc &lt;%%&gt; au lieu d’un bloc &lt;% =%&gt; lors de l’appel de html. RenderPartial (). &lt;% =%&gt; blocs dans ASP.NET indiquent qu’un développeur souhaite afficher une valeur spécifiée (par exemple : &lt;% = "Hello"%&gt; affiche "Hello"). à la place, &lt;%%&gt; blocs indiquent que le développeur souhaite exécuter du code, et que toute sortie rendue à l’intérieur doit être effectuée explicitement (par exemple : &lt;% Response. Write ("Hello")%&gt;. La raison pour laquelle nous utilisons un bloc &lt;%%&gt; avec notre code html. RenderPartial ci-dessus est que la méthode html. RenderPartial () ne retourne pas de chaîne et renvoie le contenu directement au flux de sortie du modèle de vue appelant. Pour des raisons d’efficacité des performances, cela évite d’avoir à créer un objet de chaîne temporaire (potentiellement très volumineux). Cela réduit l’utilisation de la mémoire et améliore le débit global de l’application. Une erreur courante lors de l’utilisation de html. RenderPartial () consiste à oublier d’ajouter un point-virgule à la fin de l’appel lorsque celui-ci se trouve dans un bloc &lt;%%&gt;. Par exemple, ce code génère une erreur du compilateur : &lt;% html. RenderPartial ("DinnerForm")%&gt; vous devez écrire : &lt;% html. RenderPartial ("DinnerForm"); %&gt; cela est dû au fait que &lt;les blocs de%%&gt; sont des instructions de code C# autonomes, et quand les instructions de code doivent se terminer par un point-virgule. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Utilisation de modèles de vue partiel pour clarifier le code

Nous avons créé le modèle de vue partielle « DinnerForm » pour éviter de dupliquer la logique de rendu de la vue à plusieurs endroits. Il s’agit de la raison la plus courante pour créer des modèles de vue partiels.

Il est parfois judicieux de créer des vues partielles, même quand elles sont appelées uniquement à un seul emplacement. Les modèles de vue très complexes peuvent souvent devenir beaucoup plus faciles à lire lorsque la logique de rendu de leur vue est extraite et partitionnée en un ou plusieurs modèles partiels bien nommés.

Par exemple, considérez l’extrait de code ci-dessous à partir du fichier site. Master dans notre projet (que nous examinerons bientôt). Le code est relativement simple à lire, en partie parce que la logique permettant d’afficher un lien de connexion/déconnexion en haut à droite de l’écran est encapsulée dans la partie « contrôle LogOnUserControl » :

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Chaque fois que vous vous rendez compte que vous tentez de comprendre le balisage HTML/code dans un modèle de vue, déterminez s’il n’est pas plus clair si une partie de celui-ci a été extraite et refactorie en vues partielles bien nommées.

### <a name="master-pages"></a>Pages maîtres

En plus de prendre en charge les vues partielles, ASP.NET MVC prend également en charge la possibilité de créer des modèles de « page maître » qui peuvent être utilisés pour définir la disposition commune et le code HTML de niveau supérieur d’un site. Les contrôles d’espace réservé de contenu peuvent ensuite être ajoutés à la page maître pour identifier les régions remplaçables qui peuvent être remplacées ou « remplies » par les vues. Cela offre un moyen très efficace (et sec) pour appliquer une disposition commune à l’échelle d’une application.

Par défaut, les nouveaux projets MVC ASP.NET ont un modèle de page maître qui leur est automatiquement ajouté. Cette page maître est nommée « site. Master » et se trouve dans le dossier \Views\Shared\ :

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Le fichier site. Master par défaut se présente comme indiqué ci-dessous. Il définit le code html externe du site, ainsi qu’un menu de navigation en haut. Il contient deux contrôles d’espace réservé de contenu remplaçables : un pour le titre et l’autre pour l’emplacement où le contenu principal d’une page doit être remplacé :

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Tous les modèles de vue que nous avons créés pour notre application NerdDinner (« List », « Details », « Edit », « Create », « NotFound », etc.) ont été basés sur ce modèle site. Master. Cela est indiqué par le biais de l’attribut « MasterPageFile » qui a été ajouté par défaut au haut &lt;la directive% @ page%&gt; quand nous avons créé nos vues à l’aide de la boîte de dialogue « Ajouter une vue » :

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Cela signifie que nous pouvons modifier le contenu site. Master et faire en sorte que les modifications soient automatiquement appliquées et utilisées lors du rendu de l’un de nos modèles de vue.

Nous allons mettre à jour la section d’en-tête de site. Master afin que l’en-tête de notre application soit « NerdDinner » au lieu de « mon application MVC ». Nous allons également mettre à jour notre menu de navigation pour que le premier onglet soit « Rechercher un dîner » (géré par la méthode d’action d’index () du HomeController) et ajouter un nouvel onglet appelé « héberger un dîner » (géré par la méthode d’action Create () de DinnersController) :

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Lorsque nous enregistrons le fichier site. Master et actualisons notre navigateur, nous voyons que les modifications de l’en-tête s’affichent dans toutes les vues de notre application. Exemple :

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

Et avec l’URL */dinners/Edit/[id]* :

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Étape suivante

Les sections partielles et les pages maîtres fournissent des options très flexibles qui vous permettent d’organiser correctement les vues. Vous constaterez qu’elles vous permettent d’éviter de dupliquer le contenu et le code de la vue, et de rendre vos modèles de vue plus faciles à lire et à gérer.

Nous allons maintenant revoir le scénario de liste que nous avons créé précédemment et activer la prise en charge de la pagination évolutive.

> [!div class="step-by-step"]
> [Précédent](use-viewdata-and-implement-viewmodel-classes.md)
> [Suivant](implement-efficient-data-paging.md)
