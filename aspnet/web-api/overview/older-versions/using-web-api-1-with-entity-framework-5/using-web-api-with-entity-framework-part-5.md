---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Partie 5 : création d’une interface utilisateur dynamique avec Knockout. js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: bbdeba756de7986cfeb92aa10a57f4f101382f99
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599994"
---
# <a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Partie 5 : création d’une interface utilisateur dynamique avec Knockout. js

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Création d’une interface utilisateur dynamique avec Knockout.js

Dans cette section, nous allons utiliser Knockout. js pour ajouter des fonctionnalités à la vue d’administration.

[Knockout. js](http://knockoutjs.com/) est une bibliothèque JavaScript qui facilite la liaison des contrôles HTML aux données. Knockout. js utilise le modèle MVVM (Model-View-ViewModel).

- Le *modèle* est la représentation côté serveur des données dans le domaine d’entreprise (dans notre cas, Products et Orders).
- La *vue* est la couche de présentation (html).
- Le *modèle d’affichage* est un objet JavaScript qui contient les données du modèle. Le modèle d’affichage est une abstraction du code de l’interface utilisateur. Il n’a aucune connaissance de la représentation HTML. Au lieu de cela, il représente les fonctionnalités abstraites de la vue, telles que « une liste d’éléments ».

La vue est liée aux données du modèle d’affichage. Les mises à jour du modèle d’affichage sont automatiquement reflétées dans la vue. Le modèle de vue obtient également des événements de la vue, tels que des clics de bouton, et effectue des opérations sur le modèle, telles que la création d’une commande.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Tout d’abord, nous allons définir le modèle d’affichage. Après cela, nous allons lier le balisage HTML au modèle d’affichage.

Ajoutez la section Razor suivante à admin. cshtml :

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Vous pouvez ajouter cette section n’importe où dans le fichier. Lorsque la vue est restituée, la section apparaît au bas de la page HTML, juste avant la balise de fermeture &lt;/Body&gt;.

Tout le script de cette page sera placé dans la balise de script indiquée par le commentaire :

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Tout d’abord, définissez une classe de modèle d’affichage :

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**Ko. observableArray** est un type spécial d’objet dans Knockout, appelé *observable*. Dans la [documentation de Knockout. js](http://knockoutjs.com/documentation/observables.html): un observable est un « objet JavaScript qui peut notifier les modifications aux abonnés. » Lorsque le contenu d’un observable change, la vue est automatiquement mise à jour pour correspondre.

Pour remplir le tableau `products`, effectuez une demande AJAX à l’API Web. Rappelez-vous que nous avons stocké l’URI de base pour l’API dans le conteneur d’affichage (voir la [partie 4](using-web-api-with-entity-framework-part-4.md) du didacticiel).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Ensuite, ajoutez des fonctions au modèle d’affichage pour créer, mettre à jour et supprimer des produits. Ces fonctions soumettent les appels AJAX à l’API Web et utilisent les résultats pour mettre à jour le modèle d’affichage.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

À présent, la partie la plus importante : lorsque le DOM est chargé, appelez la fonction **Ko. applyBindings** et transmettez une nouvelle instance de l' `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

La méthode **Ko. applyBindings** active Knockout et relie le modèle d’affichage à la vue.

Maintenant que nous avons un modèle de vue, nous pouvons créer les liaisons. Dans Knockout. js, vous pouvez le faire en ajoutant `data-bind` attributs aux éléments HTML. Par exemple, pour lier une liste HTML à un tableau, utilisez la liaison de `foreach` :

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

La liaison `foreach` itère au sein du tableau et crée des éléments enfants pour chaque objet dans le tableau. Les liaisons sur les éléments enfants peuvent faire référence aux propriétés sur les objets du tableau.

Ajoutez les liaisons suivantes à la liste « UPDATE-Products » :

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

L’élément `<li>` se produit dans la portée de la liaison **foreach** . Cela signifie que Knockout restituera l’élément une fois pour chaque produit dans le tableau `products`. Toutes les liaisons au sein de l’élément `<li>` font référence à cette instance du produit. Par exemple, `$data.Name` fait référence à la propriété `Name` sur le produit.

Pour définir les valeurs des entrées de texte, utilisez la liaison de `value`. Les boutons sont liés aux fonctions de la vue de modèle, à l’aide de la liaison de `click`. L’instance Product est transmise en tant que paramètre à chaque fonction. Pour plus d’informations, la [documentation de Knockout. js](http://knockoutjs.com/documentation/observables.html) contient de bonnes descriptions des différentes liaisons.

Ensuite, ajoutez une liaison pour l’événement **Submit** sur le formulaire ajouter un produit :

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Cette liaison appelle la fonction `create` sur le modèle d’affichage pour créer un nouveau produit.

Voici le code complet de la vue d’administration :

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Exécutez l’application, connectez-vous avec le compte d’administrateur, puis cliquez sur le lien « admin ». Vous devez voir la liste des produits et être en mesure de créer, mettre à jour ou supprimer des produits.

> [!div class="step-by-step"]
> [Précédent](using-web-api-with-entity-framework-part-4.md)
> [Suivant](using-web-api-with-entity-framework-part-6.md)
