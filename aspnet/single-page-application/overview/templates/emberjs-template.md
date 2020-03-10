---
uid: single-page-application/overview/templates/emberjs-template
title: Modèle EmberJS | Microsoft Docs
author: xqiu
description: Modèle EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1aefa46dd0841b1b06675409cc8a09f9a218d7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578504"
---
# <a name="emberjs-template"></a>Modèle EmberJS

par [Xinyang Qiu](https://github.com/xqiu)

> Le modèle MVC EmberJS est écrit par Nathan Totten, Thiago Santos et Xinyang Qiu.
> 
> [Télécharger le modèle MVC EmberJS](https://go.microsoft.com/fwlink/?LinkId=282647)

Le modèle SPA EmberJS est conçu pour vous aider à créer rapidement des applications Web interactives côté client à l’aide de EmberJS.

« Une application à page unique » (SPA) est le terme général pour une application Web qui charge une seule page HTML, puis met à jour la page dynamiquement, au lieu de charger de nouvelles pages. Après le chargement initial de la page, le SPA discute avec le serveur via des demandes AJAX.

![](emberjs-template/_static/image1.png)

AJAX n’est pas nouveau, mais aujourd’hui, il existe des infrastructures JavaScript qui facilitent la création et la maintenance d’une application SPA sophistiquée de grande taille. En outre, HTML 5 et CSS3 facilitent la création d’interfaces utilisateur riches.

Le modèle SPA EmberJS utilise la bibliothèque JavaScript [Ember](http://emberjs.com/) pour gérer les mises à jour de page à partir des demandes Ajax. Ember. js utilise la liaison de données pour synchroniser la page avec les données les plus récentes. De cette façon, vous n’êtes pas obligé d’écrire le code qui parcourt les données JSON et met à jour le DOM. Au lieu de cela, vous placez des attributs déclaratifs dans le code HTML qui indiquent à Ember. js comment présenter les données.

Côté serveur, le modèle EmberJS est presque identique au [modèle KNOCKOUTJS Spa](../introduction/knockoutjs-template.md). Il utilise ASP.NET MVC pour servir des documents HTML et API Web ASP.NET pour gérer les requêtes AJAX à partir du client. Pour plus d’informations sur ces aspects du modèle, reportez-vous à la documentation du [modèle KnockoutJS](../introduction/knockoutjs-template.md) . Cette rubrique se concentre sur les différences entre le modèle Knockout et le modèle EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Créer un projet de modèle SPA EmberJS

Téléchargez et installez le modèle en cliquant sur le bouton de téléchargement ci-dessus. Vous devrez peut-être redémarrer Visual Studio.

Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  . Sous **visuel C#** , sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **application Web ASP.NET MVC 4**. Nommez le projet, puis cliquez sur **OK**.

![](emberjs-template/_static/image2.png)

Dans l’Assistant **nouveau projet** , sélectionnez le **projet Ember. js Spa**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Vue d’ensemble du modèle EmberJS SPA

Le modèle EmberJS utilise une combinaison de jQuery, Ember. js, guidon. js pour créer une interface utilisateur lisse et interactive.

Ember. js est une bibliothèque JavaScript qui utilise un modèle MVC côté client.

- Un *modèle*, écrit dans le langage de création de guidon, décrit l’interface utilisateur de l’application. En mode mise en sortie, le [compilateur guidon](https://github.com/Myslik/csharp-ember-handlebars) est utilisé pour regrouper et compiler le modèle de guidon.
- Un *modèle* stocke les données d’application qu’il obtient à partir du serveur (listes TODO et éléments TODO).
- Un *contrôleur* stocke l’état de l’application. Les contrôleurs présentent souvent des données de modèle aux modèles correspondants.
- Une *vue* traduit les événements primitifs de l’application et les transmet au contrôleur.
- Un *routeur* gère l’état de l’application, en conservant la synchronisation des URL et des modèles.

En outre, la bibliothèque de données Ember peut être utilisée pour synchroniser des objets JSON (obtenus à partir du serveur via une API RESTful) et les modèles clients.

Le modèle SPA EmberJS organise les scripts en huit couches :

- WebAPI\_adapter. js, WebAPI\_serializer. js : étend la bibliothèque de données Ember pour qu’elle fonctionne avec API Web ASP.NET.
- Scripts/Helper. js : définit les nouvelles applications auxiliaires de GUID de Ember.
- Scripts/App. js : crée l’application et configure l’adaptateur et le sérialiseur.
- Scripts/App/Models/\*. js : définit les modèles.
- Scripts/app/views/\*. js : définit les vues.
- Scripts/App/Controllers/\*. js : définit les contrôleurs.
- Scripts/application/itinéraires, scripts/App/Router. js : définit les itinéraires.
- Templates/\*. BH : définit les modèles de guidon.

Examinons quelques-uns de ces scripts plus en détail.

## <a name="models"></a>Modèles

Les modèles sont définis dans le dossier scripts/App/Models. Il existe deux fichiers de modèle : todoItem. js et todoList. js.

**TODO. Model. js** définit les modèles côté client (navigateur) pour les listes de tâches. Il existe deux classes de modèle : todoItem et todoList. Dans Ember, les modèles sont des sous-classes de DS. Modélisation. Un modèle peut avoir des propriétés avec des attributs :

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Les modèles peuvent définir des relations avec d’autres modèles :

[!code-css[Main](emberjs-template/samples/sample2.css)]

Les modèles peuvent avoir des propriétés calculées qui sont liées à d’autres propriétés :

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Les modèles peuvent avoir des fonctions observateur, qui sont appelées lorsqu’une propriété observée change :

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Affichages

Les affichages sont définis dans le dossier scripts/app/views. Une vue traduit les événements à partir de l’interface utilisateur de l’application. Un gestionnaire d’événements peut rappeler des fonctions de contrôleur ou simplement appeler directement le contexte de données.

Par exemple, le code suivant provient de views/TodoItemEditView. js. Il définit la gestion des événements pour un champ de texte d’entrée.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Contrôleur

Les contrôleurs sont définis dans le dossier scripts/App/Controllers. Pour représenter un modèle unique, étendez `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Un contrôleur peut également représenter une collection de modèles en étendant `Ember.ArrayController`. Par exemple, TodoListController représente un tableau d’objets `todoList`. Le contrôleur trie par ID todoList, dans l’ordre décroissant :

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Le contrôleur définit une fonction nommée `addTodoList`, qui crée un nouveau todoList et l’ajoute au tableau. Pour voir comment cette fonction est appelée, ouvrez le fichier de modèle nommé todoListTemplate. html dans le dossier modèles. Le code de modèle suivant lie un bouton à la fonction `addTodoList` :

[!code-html[Main](emberjs-template/samples/sample8.html)]

Le contrôleur contient également une propriété `error`, qui contient un message d’erreur. Voici le code du modèle permettant d’afficher le message d’erreur (également dans todoListTemplate. html) :

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Routes

Router. js définit les itinéraires et le modèle par défaut à afficher, définit l’état de l’application et met en correspondance les URL avec les itinéraires :

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute. js charge des données pour TodoListRoute en remplaçant la fonction setupController :

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember utilise des conventions d’affectation de noms pour faire correspondre les URL, les noms d’itinéraires, les contrôleurs et les modèles. Pour plus d’informations, consultez [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) dans la documentation EmberJS.

## <a name="templates"></a>Modèles

Le dossier modèles contient quatre modèles :

- application. BH : modèle par défaut qui est rendu lorsque l’application est démarrée.
- about. BH : modèle pour l’itinéraire « /about ».
- index. BH : modèle pour l’itinéraire racine « / ».
- todoList. BH : modèle pour l’itinéraire « /todo ».
- \_barre de navigation. BH : le modèle définit le menu de navigation.

Le modèle d’application agit comme une page maître. Elle contient un en-tête, un pied de page et un « {{Outlet}} » pour insérer d’autres modèles dans en fonction de l’itinéraire. Pour plus d’informations sur les modèles d’application dans Ember, consultez [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

Le modèle « /todoList » contient deux expressions de boucle. La boucle extérieure est `{{#each controller}}`, et la boucle interne est `{{#each todos}}`. Le code suivant illustre une vue intégrée `Ember.Checkbox`, une `App.TodoItemEditView`personnalisée et un lien avec une action `deleteTodo`.

[!code-html[Main](emberjs-template/samples/sample12.html)]

La classe `HtmlHelperExtensions`, définie dans Controllers/HtmlHelperExtensions. cs, définit une fonction d’assistance pour mettre en cache et insérer des fichiers modèles lorsque **Debug** a la valeur **true** dans le fichier Web. config. Cette fonction est appelée à partir du fichier de vue MVC ASP.NET défini dans views/domotique/App. cshtml :

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Appelée sans argument, la fonction affiche tous les fichiers de modèle dans le dossier de modèles. Vous pouvez également spécifier un sous-dossier ou un fichier de modèle spécifique.

Lorsque **Debug** a la **valeur false** dans Web. config, l’application comprend l’élément Bundle « ~/bundles/Templates ». Cet élément de regroupement est ajouté dans BundleConfig.cs, à l’aide de la bibliothèque du compilateur guidon :

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
