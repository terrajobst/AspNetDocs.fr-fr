---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Application à page unique : modèle KnockoutJS | Microsoft Docs'
author: MikeWasson
description: Modèle Knockout
ms.author: riande
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 3a551db1caa9636eb7f2e04c287d3ef371263584
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578693"
---
# <a name="single-page-application-knockoutjs-template"></a>Application à page unique : modèle KnockoutJS

par [Mike Wasson](https://github.com/MikeWasson)

> Le modèle MVC Knockout fait partie des ASP.NET et Web Tools 2012,2
> 
> [Télécharger ASP.NET et Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650)

La mise à jour ASP.NET et Web Tools 2012,2 comprend un modèle d’application à page unique (SPA) pour ASP.NET MVC 4. Ce modèle est conçu pour vous aider à créer rapidement des applications Web interactives côté client.

« Une application à page unique » (SPA) est le terme général pour une application Web qui charge une seule page HTML, puis met à jour la page dynamiquement, au lieu de charger de nouvelles pages. Après le chargement initial de la page, le SPA discute avec le serveur via des demandes AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX n’est pas nouveau, mais aujourd’hui, il existe des infrastructures JavaScript qui facilitent la création et la maintenance d’une application SPA sophistiquée de grande taille. En outre, HTML 5 et CSS3 facilitent la création d’interfaces utilisateur riches.

Pour vous aider à démarrer, le modèle SPA crée un exemple d’application « liste des tâches ». Dans ce didacticiel, nous allons suivre une visite guidée du modèle. Tout d’abord, nous allons examiner l’application de la liste des tâches, puis examiner les éléments technologiques qui le font fonctionner.

## <a name="create-a-new-spa-template-project"></a>Créer un projet de modèle SPA

Configuration requise :

- Visual Studio 2012 ou Visual Studio Express 2012 pour le Web
- ASP.NET Web Tools 2012,2 Update. Vous pouvez installer la mise à jour [ici](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Démarrez Visual Studio et sélectionnez **nouveau projet** dans la page de démarrage. Ou, dans le menu **fichier** , sélectionnez **nouveau** , puis **projet**.

Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  . Sous **visuel C#** , sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **application Web ASP.NET MVC 4**. Nommez le projet, puis cliquez sur **OK**.

![](knockoutjs-template/_static/image2.png)

Dans l’Assistant **nouveau projet** , sélectionnez **application à page unique**.

![](knockoutjs-template/_static/image3.png)

Appuyez sur F5 pour générer et exécuter l'application. Lorsque l’application s’exécute pour la première fois, elle affiche un écran de connexion.

![](knockoutjs-template/_static/image4.png)

Cliquez sur le lien &quot;vous inscrire&quot; et créez un nouvel utilisateur.

![](knockoutjs-template/_static/image5.png)

Une fois que vous êtes connecté, l’application crée une liste TODO par défaut avec deux éléments. Vous pouvez cliquer sur « Ajouter une liste TODO » pour ajouter une nouvelle liste.

![](knockoutjs-template/_static/image6.png)

Renommez la liste, ajoutez des éléments à la liste et vérifiez-les. Vous pouvez également supprimer des éléments ou supprimer une liste entière. Les modifications sont automatiquement conservées dans une base de données sur le serveur (en réalité, la base de données locale à ce stade, car vous exécutez l’application localement).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architecture du modèle SPA

Ce diagramme montre les principaux blocs de construction de l’application.

![](knockoutjs-template/_static/image8.png)

Côté serveur, ASP.NET MVC sert le HTML et gère également l’authentification basée sur les formulaires.

API Web ASP.NET gère toutes les demandes relatives à ToDoLists et ToDoItems, y compris l’obtention, la création, la mise à jour et la suppression. Le client échange des données avec l’API Web au format JSON.

Entity Framework (EF) est la couche O/RM. Elle sert de base au monde orienté objet de ASP.NET et à la base de données sous-jacente. La base de données utilise la base de données locale, mais vous pouvez la modifier dans le fichier Web. config. En général, vous utilisez la base de données locale pour le développement local, puis vous déployez sur une base de données SQL sur le serveur, à l’aide de la migration du code d’erreur EF.

Côté client, la bibliothèque Knockout. js gère les mises à jour de page à partir des demandes AJAX. Knockout utilise la liaison de données pour synchroniser la page avec les données les plus récentes. De cette façon, vous n’êtes pas obligé d’écrire le code qui parcourt les données JSON et met à jour le DOM. Au lieu de cela, vous placez des attributs déclaratifs dans le code HTML qui indiquent à Knockout comment présenter les données.

L’un des grands avantages de cette architecture est qu’elle sépare la couche de présentation de la logique d’application. Vous pouvez créer la partie API Web sans connaître l’aspect de votre page Web. Côté client, vous créez un « modèle de vue » pour représenter ces données, et le modèle de vue utilise Knockout pour effectuer une liaison au code HTML. Cela vous permet de modifier facilement le code HTML sans modifier le modèle de vue. (Nous allons examiner Knockout un peu plus tard.)

## <a name="models"></a>Modèles

Dans le projet Visual Studio, le dossier modèles contient les modèles qui sont utilisés côté serveur. (Il existe également des modèles côté client ; nous allons y accéder.)

![](knockoutjs-template/_static/image9.png)

**TodoItem, TodoList**

Il s’agit des modèles de base de données pour Entity Framework Code First. Notez que ces modèles ont des propriétés qui pointent les unes vers les autres. `ToDoList` contient une collection de ToDoItems, et chaque `ToDoItem` a une référence à son ToDoList parent. Ces propriétés sont appelées propriétés de navigation, et représentent la relation un-à-plusieurs à la liste des tâches et à ses éléments de tâche.

La classe `ToDoItem` utilise également l’attribut **[ForeignKey]** pour spécifier que `ToDoListId` est une clé étrangère dans la table `ToDoList`. Ce code indique à EF d’ajouter une contrainte de clé étrangère à la base de données.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Ces classes définissent les données qui seront envoyées au client. « DTO » signifie « objet de transfert de données ». Le DTO définit la manière dont les entités seront sérialisées en JSON. En général, il existe plusieurs raisons d’utiliser les DTO :

- Pour contrôler les propriétés qui sont sérialisées. Le DTO peut contenir un sous-ensemble des propriétés du modèle de domaine. Vous pouvez effectuer cette opération pour des raisons de sécurité (pour masquer des données sensibles) ou simplement pour réduire la quantité de données que vous envoyez.
- Pour modifier la forme des données, par exemple pour aplatir une structure de données plus complexe.
- Pour éviter toute logique métier du DTO (séparation des préoccupations).
- Si vos modèles de domaine ne peuvent pas être sérialisés pour une raison quelconque. Par exemple, les références circulaires peuvent provoquer des problèmes lors de la sérialisation d’un objet. il existe des façons de gérer ce problème dans l’API Web (consultez [gestion des références d’objets circulaires](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); Toutefois, l’utilisation d’un DTO évite tout simplement le problème.

Dans le modèle SPA, les DTO contiennent les mêmes données que les modèles de domaine. Toutefois, ils sont toujours utiles car ils évitent les références circulaires à partir des propriétés de navigation, et ils illustrent le modèle de DTO général.

**AccountModels.cs**

Ce fichier contient des modèles pour l’appartenance à un site. La classe `UserProfile` définit le schéma des profils utilisateur dans la base de la BDD d’appartenance. (Dans ce cas, la seule information est l’ID utilisateur et le nom d’utilisateur.) Les autres classes de modèle de ce fichier sont utilisées pour créer les formulaires d’inscription et de connexion de l’utilisateur.

## <a name="entity-framework"></a>Entity Framework

Le modèle SPA utilise EF Code First. Dans Code First développement, vous définissez les modèles en premier dans le code, puis EF utilise le modèle pour créer la base de données. Vous pouvez également utiliser EF avec une base de données existante ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

La classe `TodoItemContext` dans le dossier Models dérive de **DbContext**. Cette classe fournit le « collage » entre les modèles et EF. Le `TodoItemContext` contient une collection `ToDoItem` et une collection `TodoList`. Pour interroger la base de données, il vous suffit d’écrire une requête LINQ sur ces collections. Par exemple, voici comment vous pouvez sélectionner toutes les listes de tâches pour l’utilisateur « Alice » :

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Vous pouvez également ajouter de nouveaux éléments à la collection, mettre à jour des éléments ou supprimer des éléments de la collection et conserver les modifications apportées à la base de données.

## <a name="aspnet-web-api-controllers"></a>Contrôleurs de API Web ASP.NET

Dans API Web ASP.NET, les contrôleurs sont des objets qui gèrent les requêtes HTTP. Comme mentionné précédemment, le modèle SPA utilise l’API Web pour activer les opérations CRUD sur les instances `ToDoList` et `ToDoItem`. Les contrôleurs se trouvent dans le dossier Controllers de la solution.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: gère les requêtes HTTP pour les éléments de tâche
- `TodoListController`: gère les requêtes HTTP pour les listes de tâches.

Ces noms sont significatifs, car l’API Web correspond au chemin d’accès de l’URI au nom du contrôleur. (Pour savoir comment l’API Web achemine les requêtes HTTP vers les contrôleurs, consultez [routage dans API Web ASP.net](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Examinons la classe `ToDoListController`. Il contient un membre de données unique :

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

Le `TodoItemContext` est utilisé pour communiquer avec EF, comme décrit précédemment. Les méthodes sur le contrôleur implémentent les opérations CRUD. L’API Web mappe les requêtes HTTP du client aux méthodes du contrôleur, comme suit :

| Demande HTTP | Méthode du contrôleur | Description |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Obtient une collection de listes de tâches. |
| Obtient l'*ID* /API/TODO/ | `GetTodoList` | Obtient une liste de tâches par ID |
| *ID* /API/todo/put | `PutTodoList` | Met à jour une liste de tâches. |
| POST /api/todo | `PostTodoList` | Crée une liste des tâches. |
| SUPPRIMER l'*ID* /API/TODO/ | `DeleteTodoList` | Supprime une liste TODO. |

Notez que les URI de certaines opérations contiennent des espaces réservés pour la valeur d’ID. Par exemple, pour supprimer une liste à l’aide de l’ID 42, l’URI est `/api/todo/42`.

Pour en savoir plus sur l’utilisation de l’API Web pour les opérations CRUD, consultez [création d’une API Web qui prend en charge les opérations CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Le code de ce contrôleur est relativement simple. Voici quelques points intéressants :

- La méthode `GetTodoLists` utilise une requête LINQ pour filtrer les résultats en utilisant l’ID de l’utilisateur connecté. De cette façon, un utilisateur voit uniquement les données qui lui appartiennent. Notez également qu’une instruction SELECT est utilisée pour convertir les instances `ToDoList` en instances `TodoListDto`.
- Les méthodes PUT et poster vérifient l’état du modèle avant de modifier la base de données. Si **ModelState. IsValid** a la valeur false, ces méthodes retournent http 400, requête incorrecte. Pour en savoir plus sur la validation de modèle dans l’API Web, consultez [validation de modèle](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- La classe Controller est également décorée avec l’attribut **[Authorize]** . Cet attribut vérifie si la requête HTTP est authentifiée. Si la demande n’est pas authentifiée, le client reçoit le protocole HTTP 401, non autorisé. Pour plus d’informations sur l’authentification [, consultez authentification et autorisation dans API Web ASP.net](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

La classe `TodoController` est très similaire à `TodoListController`. La plus grande différence est qu’elle ne définit pas de méthode d’extraction, car le client obtiendra les éléments de tâche avec chaque liste de tâches.

## <a name="mvc-controllers-and-views"></a>Contrôleurs et vues MVC

Les contrôleurs MVC se trouvent également dans le dossier Controllers de la solution. `HomeController` restitue le code HTML principal pour l’application. La vue du contrôleur d’hébergement est définie dans views/orig/index. cshtml. La vue d’hébergement affiche un contenu différent selon que l’utilisateur a ouvert une session ou non :

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Lorsque les utilisateurs sont connectés, ils voient l’interface utilisateur principale. Dans le cas contraire, ils voient le panneau de connexion. Notez que ce rendu conditionnel se produit côté serveur. N’essayez jamais de masquer le contenu sensible côté&#8212;client tout ce que vous envoyez dans une réponse http est visible par une personne qui surveille les messages http bruts.

## <a name="client-side-javascript-and-knockoutjs"></a>JavaScript côté client et Knockout. js

Passons à présent du côté serveur de l’application au client. Le modèle SPA utilise une combinaison de jQuery et Knockout. js pour créer une interface utilisateur fluide et interactive. Knockout. js est une bibliothèque JavaScript qui facilite la liaison du code HTML aux données. Knockout. js utilise un modèle appelé « Model-View-ViewModel ».

- Le modèle est les données de domaine (listes de tâches et éléments ToDo).
- La vue est le document HTML.
- Le modèle d’affichage est un objet JavaScript qui contient les données du modèle. Le modèle d’affichage est une abstraction du code de l’interface utilisateur. Il n’a aucune connaissance de la représentation HTML. Au lieu de cela, il représente des fonctionnalités abstraites de la vue, telles que « une liste d’éléments ToDo ».

La vue est liée aux données du modèle d’affichage. Les mises à jour du modèle d’affichage sont automatiquement reflétées dans la vue. Les liaisons fonctionnent également dans l’autre direction. Les événements dans le DOM (tels que les clics) sont liés aux fonctions sur le modèle de vue, qui déclenchent des appels AJAX.

Le modèle SPA organise le code JavaScript côté client en trois couches :

- TODO. DataContext. js : envoie des demandes AJAX.
- TODO. Model. js : définit les modèles.
- TODO. ViewModel. js : définit le modèle de vue.

![](knockoutjs-template/_static/image11.png)

Ces fichiers de script se trouvent dans le dossier scripts/app de la solution.

![](knockoutjs-template/_static/image12.png)

**TODO. DataContext** gère tous les appels AJAX aux contrôleurs d’API Web. (Les appels AJAX pour la connexion sont définis ailleurs, dans ajaxLogin. js.)

**TODO. Model. js** définit les modèles côté client (navigateur) pour les listes de tâches. Il existe deux classes de modèle : todoItem et todoList.

La plupart des propriétés des classes de modèle sont de type « Ko. observable ». Observables est la manière dont Knockout effectue sa magie. À partir de la [documentation Knockout](http://knockoutjs.com/documentation/introduction.html): un observable est un « objet JavaScript qui peut notifier les modifications aux abonnés. » Lorsque la valeur d’un observable change, Knockout met à jour tous les éléments HTML liés à ces observables. Par exemple, todoItem a observables pour les propriétés Title et isDone :

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Vous pouvez également vous abonner à un observable dans le code. Par exemple, la classe todoItem s’abonne aux modifications apportées aux propriétés « isDone » et « title » :

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Afficher le modèle**

Le modèle de vue est défini dans todo. ViewModel. js. Le modèle de vue est le point central où l’application lie les éléments de page HTML aux données de domaine. Dans le modèle SPA, le modèle de vue contient un tableau observable de todoLists. Le code suivant dans le modèle de vue indique à Knockout d’appliquer les liaisons :

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>Liaison de données et HTML

Le code HTML principal de la page est défini dans views/orig/index. cshtml. Étant donné que nous utilisons la liaison de données, le code HTML est uniquement un modèle pour ce qui est réellement rendu. Knockout utilise des liaisons *déclaratives* . Vous liez des éléments de page à des données en ajoutant un attribut « Data-bind » à l’élément. Voici un exemple très simple tiré de la documentation Knockout :

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

Dans cet exemple, Knockout met à jour le contenu de l’élément **&lt;span&gt;** avec la valeur de `myItems.count()`. Chaque fois que cette valeur change, Knockout met à jour le document.

Knockout fournit un certain nombre de types de liaison différents. Voici quelques-unes des liaisons utilisées dans le modèle SPA :

- **foreach**: vous permet d’itérer au sein d’une boucle et d’appliquer la même balise à chaque élément de la liste. Il est utilisé pour restituer les listes de tâches et les éléments de tâche. Dans le **foreach**, les liaisons sont appliquées aux éléments de la liste.
- **visible**: permet d’activer/de désactiver la visibilité. Masquer le balisage lorsqu’une collection est vide ou rendre visible le message d’erreur.
- **valeur**: utilisée pour remplir les valeurs de formulaire.
- **Click**: lie un événement Click à une fonction sur le modèle de vue.

## <a name="anti-csrf-protection"></a>Protection anti-CSRF

La falsification de requête intersites (CSRF) est une attaque dans laquelle un site malveillant envoie une demande à un site vulnérable dans lequel l’utilisateur est actuellement connecté. Pour prévenir les attaques CSRF, ASP.NET MVC utilise des *jetons anti-contrefaçon*, également appelés jetons de vérification de demande. L’idée est que le serveur place un jeton généré de manière aléatoire dans une page Web. Lorsque le client envoie des données au serveur, il doit inclure cette valeur dans le message de demande.

Les jetons anti-contrefaçon fonctionnent parce que la page malveillante ne peut pas lire les jetons de l’utilisateur, en raison des stratégies de même origine. (Les stratégies de même origine empêchent les documents hébergés sur deux sites différents d’accéder au contenu de l’autre.)

ASP.NET MVC offre une prise en charge intégrée des jetons anti-contrefaçon, par le biais de la classe anti- [contrefaçon](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) et de l’attribut [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) . Actuellement, cette fonctionnalité n’est pas intégrée à l’API Web. Toutefois, le modèle SPA comprend une implémentation personnalisée pour l’API Web. Ce code est défini dans la classe `ValidateHttpAntiForgeryTokenAttribute`, qui se trouve dans le dossier Filters de la solution. Pour en savoir plus sur l’anti-CSRF dans l’API Web, consultez [prévention des attaques de falsification de requête intersite (CSRF)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Conclusion

Le modèle SPA est conçu pour vous permettre de commencer rapidement à écrire des applications Web modernes et interactives. Elle utilise la bibliothèque Knockout. js pour séparer la présentation (balisage HTML) de la logique de données et de l’application. Mais Knockout n’est pas la seule bibliothèque JavaScript que vous pouvez utiliser pour créer un SPA. Si vous souhaitez explorer d’autres options, jetez un coup d’œil aux [modèles Spa créés par la communauté](../templates/index.md).
