---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Utiliser ViewData et implémenter des classes ViewModel | Microsoft Docs
author: microsoft
description: L’étape 6 montre comment active la prise en charge des scénarios d’édition de formulaires plus riches et aborde deux approches qui peuvent être utilisées pour passer des données des contrôleurs aux vues :...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: ca9775417c2e25952511a73096fb76d5d4edaea2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541607"
---
# <a name="use-viewdata-and-implement-viewmodel-classes"></a>Utiliser ViewData et implémenter des classes ViewModel

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 6 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.
> 
> L’étape 6 montre comment active la prise en charge des scénarios d’édition de formulaires plus riches, et aborde deux approches qui peuvent être utilisées pour passer des données des contrôleurs aux vues : ViewData et ViewModel.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner étape 6 : ViewData et ViewModel

Nous avons abordé un certain nombre de scénarios de publication de formulaires et expliqué comment implémenter la prise en charge de la création, de la mise à jour et de la suppression (CRUD). Nous allons maintenant faire de notre implémentation DinnersController et activer la prise en charge des scénarios de modification de formulaires plus riches. En procédant ainsi, nous aborderons deux approches qui peuvent être utilisées pour passer des données des contrôleurs aux vues : ViewData et ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Transmission de données à partir de contrôleurs vers des modèles de vue

L’une des caractéristiques de définition du modèle MVC est la « séparation des préoccupations » stricte qui permet d’appliquer les différents composants d’une application. Les modèles, les contrôleurs et les vues ont chacun des rôles et des responsabilités bien définis, et ils communiquent entre eux de manière bien définie. Cela facilite la promotion de la testabilité et de la réutilisation du code.

Lorsqu’une classe de contrôleur décide de restituer une réponse HTML à un client, il est chargé de passer explicitement au modèle de vue toutes les données nécessaires pour afficher la réponse. Les modèles de vue ne doivent jamais effectuer de récupération de données ou de logique d’application, et doivent plutôt se limiter à un seul code de rendu piloté par le contrôleur.

À ce stade, les données de modèle passées par notre classe DinnersController à nos modèles de vue sont simples et simples, c’est-à-dire une liste d’objets dîner dans le cas de index () et un objet dîner unique dans le cas des détails (), Edit (), Create () et Delete (). Au fur et à mesure que nous ajoutons des fonctionnalités d’interface utilisateur à notre application, nous allons souvent devoir transmettre des réponses HTML au sein de nos modèles de vue. Par exemple, il se peut que nous souhaitions modifier le champ « Country » (pays) au sein de nos vues Edit et Create pour qu’il s’agit d’une zone de texte HTML vers un contrôle DropDownList. Au lieu de coder en dur la liste déroulante des noms de pays dans le modèle de vue, il est possible de la générer à partir d’une liste de pays pris en charge que nous remplissons de manière dynamique. Nous aurons besoin d’un moyen de passer l’objet dîner *et* la liste des pays pris en charge de notre contrôleur à nos modèles de vue.

Examinons deux façons de procéder.

### <a name="using-the-viewdata-dictionary"></a>Utilisation du dictionnaire ViewData

La classe de base Controller expose une propriété de dictionnaire « ViewData » qui peut être utilisée pour passer des éléments de données supplémentaires de contrôleurs à des vues.

Par exemple, pour prendre en charge le scénario dans lequel nous voulons modifier la zone de texte « Country » dans notre vue de modification en passant d’une zone de texte HTML à un contrôle DropDownList, nous pouvons mettre à jour notre méthode d’action Edit () pour passer (en plus d’un objet dîner) un objet SelectList qui peut être utilisé comme m odèle d’un DropDownList de pays.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Le constructeur du SelectList ci-dessus accepte une liste de comtés pour remplir le downlist de dépôt avec, ainsi que la valeur actuellement sélectionnée.

Nous pouvons ensuite mettre à jour notre modèle de vue Edit. aspx pour utiliser la méthode d’assistance HTML. DropDownList () à la place de la méthode d’assistance HTML. TextBox () que nous avons utilisée précédemment :

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

La méthode d’assistance HTML. DropDownList () ci-dessus accepte deux paramètres. Le premier est le nom de l’élément de formulaire HTML à générer. Le deuxième est le modèle « SelectList » que nous avons passé via le dictionnaire ViewData. Nous utilisons le C# mot clé « As » pour convertir le type dans le dictionnaire en SelectList.

Et maintenant, lorsque nous exécutons notre application et que vous accédez à l’URL */dinners/Edit/1* dans notre navigateur, nous voyons que notre interface utilisateur de modification a été mise à jour pour afficher un DropDownList de pays au lieu d’une zone de texte :

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Étant donné que nous affichons également le modèle de vue Edit à partir de la méthode HTTP-après modification (dans les scénarios où des erreurs se produisent), nous voulons nous assurer que nous mettons également à jour cette méthode pour ajouter SelectList à ViewData lorsque le modèle de vue est rendu dans des scénarios d’erreur :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Maintenant, notre scénario de modification de DinnersController prend en charge un DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Utilisation d’un modèle ViewModel

L’approche du dictionnaire ViewData présente l’avantage d’être relativement rapide et facile à implémenter. Toutefois, certains développeurs n’aiment pas utiliser de dictionnaires basés sur chaîne, puisque les fautes d’orthographe peuvent entraîner des erreurs qui ne seront pas interceptées au moment de la compilation. Le dictionnaire ViewData non typé requiert également l’utilisation de l’opérateur « as » ou d’un cast lors de l’utilisation d' C# un langage fortement typé comme dans un modèle de vue.

Une autre approche que nous pourrions utiliser est souvent appelée modèle « ViewModel ». Lors de l’utilisation de ce modèle, nous créons des classes fortement typées qui sont optimisées pour nos scénarios d’affichage spécifiques, et qui exposent des propriétés pour les valeurs/contenus dynamiques requis par nos modèles de vue. Nos classes de contrôleur peuvent ensuite remplir et transmettre ces classes optimisées en vue à notre modèle de vue à utiliser. Cela permet la sécurité des types, la vérification au moment de la compilation et l’IntelliSense de l’éditeur dans les modèles de vue.

Par exemple, pour activer les scénarios de modification de formulaire, nous pouvons créer une classe « DinnerFormViewModel » comme ci-dessous, qui expose deux propriétés fortement typées : un objet dîner et le modèle SelectList nécessaire pour remplir la DropDownList de pays :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Nous pouvons ensuite mettre à jour notre méthode d’action Edit () pour créer le DinnerFormViewModel à l’aide de l’objet dîner récupéré à partir de notre référentiel, puis le transmettre à notre modèle de vue :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Nous allons ensuite mettre à jour notre modèle de vue afin qu’il attende un « DinnerFormViewModel » au lieu d’un objet « dîner » en modifiant l’attribut « Inherits » en haut de la page Edit. aspx de la façon suivante :

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

À l’issue de cette opération, la valeur IntelliSense de la propriété « Model » dans notre modèle de vue est mise à jour pour refléter le modèle d’objet du type DinnerFormViewModel que nous transmettons :

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Nous pouvons ensuite mettre à jour notre code d’affichage pour y travailler. Notez que nous ne modifions pas les noms des éléments d’entrée que nous créons (les éléments de formulaire seront toujours nommés « title », « Country »), mais nous mettons à jour les méthodes d’assistance HTML pour récupérer les valeurs à l’aide de la classe DinnerFormViewModel :

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Nous allons également mettre à jour notre méthode Edit poster pour utiliser la classe DinnerFormViewModel lors du rendu des erreurs :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Nous pouvons également mettre à jour nos méthodes d’action Create () pour réutiliser exactement la même classe *DinnerFormViewModel* afin d’activer la DropDownList des pays dans ceux-ci également. Voici l’implémentation HTTP-is :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Voici l’implémentation de la méthode HTTP-POSTCONNEXION Create :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Et maintenant, nos deux écrans de modification et de création prennent en charge Drop-downlists pour le choix du pays.

### <a name="custom-shaped-viewmodel-classes"></a>Classes ViewModel personnalisées

Dans le scénario ci-dessus, notre classe DinnerFormViewModel expose directement l’objet de modèle dîner en tant que propriété, ainsi qu’une propriété de modèle SelectList de prise en charge. Cette approche fonctionne bien pour les scénarios où l’interface utilisateur HTML que nous voulons créer dans notre modèle de vue correspond relativement étroitement à nos objets de modèle de domaine.

Dans les scénarios où cela n’est pas le cas, une option que vous pouvez utiliser consiste à créer une classe de ViewModel personnalisée dont le modèle d’objet est optimisé pour la consommation par la vue, et qui peut paraître complètement différent de l’objet de modèle de domaine sous-jacent. Par exemple, il peut potentiellement exposer des noms de propriété et/ou des propriétés d’agrégation collectés à partir de plusieurs objets de modèle.

Les classes ViewModel personnalisées peuvent être utilisées à la fois pour passer des données des contrôleurs à des vues à restituer, ainsi que pour aider à gérer les données de formulaire publiées dans la méthode d’action d’un contrôleur. Pour ce scénario ultérieur, vous pouvez faire en sorte que la méthode d’action met à jour un objet ViewModel avec les données publiées par formulaire, puis utilise l’instance ViewModel pour mapper ou récupérer un objet de modèle de domaine réel.

Les classes ViewModel personnalisées peuvent fournir une grande flexibilité, et il est important de rechercher le code de rendu dans vos modèles de vue ou le code de publication de formulaire à l’intérieur de vos méthodes d’action, en commençant à devenir trop compliqué. Il s’agit souvent d’un signe que vos modèles de domaine ne correspondent pas correctement à l’interface utilisateur que vous générez et qu’une classe ViewModel de forme personnalisée intermédiaire peut être utile.

### <a name="next-step"></a>Étape suivante

Voyons maintenant comment nous pouvons utiliser des partiels et des pages maîtres pour réutiliser et partager l’interface utilisateur dans notre application.

> [!div class="step-by-step"]
> [Précédent](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Suivant](re-use-ui-using-master-pages-and-partials.md)
