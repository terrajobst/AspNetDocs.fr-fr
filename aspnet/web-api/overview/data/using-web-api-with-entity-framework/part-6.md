---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Créer le client JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 74f2cc4e5e401d690042b05b028dfc0c46ae282a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622345"
---
# <a name="create-the-javascript-client"></a>Créer le client JavaScript

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez créer le client pour l’application, à l’aide de HTML, JavaScript et de la bibliothèque [Knockout. js](http://knockoutjs.com/) . Nous allons créer l’application cliente par étapes :

- Afficher une liste de livres.
- Présentation des détails d’un livre.
- Ajout d’un nouveau livre.

La bibliothèque Knockout utilise le modèle MVVM (Model-View-ViewModel) :

- Le **modèle** est la représentation côté serveur des données dans le domaine d’entreprise (dans notre cas, livres et auteurs).
- La **vue** est la couche de présentation (html).
- Le **modèle de vue** est un objet JavaScript qui contient les modèles. Le modèle de vue est une abstraction du code de l’interface utilisateur. Il n’a aucune connaissance de la représentation HTML. Au lieu de cela, il représente les fonctionnalités abstraites de la vue, par exemple &quot;une liste de livres&quot;.

La vue est liée aux données du modèle de vue. Les mises à jour du modèle de vue sont automatiquement reflétées dans la vue. Le modèle de vue obtient également les événements de la vue, tels que les clics de bouton.

![](part-6/_static/image1.png)

Cette approche facilite la modification de la disposition et de l’interface utilisateur de votre application, car vous pouvez modifier les liaisons sans réécrire de code. Par exemple, vous pouvez afficher une liste d’éléments sous la forme d’un `<ul>`, puis le modifier ultérieurement en une table.

## <a name="add-the-knockout-library"></a>Ajouter la bibliothèque Knockout

Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**. Sélectionnez ensuite **Console du gestionnaire de package**. Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :

[!code-console[Main](part-6/samples/sample1.cmd)]

Cette commande ajoute les fichiers Knockout au dossier scripts.

## <a name="create-the-view-model"></a>Créer le modèle de vue

Ajoutez un fichier JavaScript nommé App. js au dossier scripts. (Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier scripts, sélectionnez **Ajouter**, puis sélectionnez **fichier JavaScript**.) Collez le code suivant :

[!code-javascript[Main](part-6/samples/sample2.js)]

Dans Knockout, la classe `observable` active la liaison de données. Lorsque le contenu d’un observable change, l’observable notifie tous les contrôles liés aux données, afin qu’ils puissent se mettre à jour eux-mêmes. (La classe `observableArray` est la version de tableau de *observable*.) Pour commencer, notre modèle de vue a deux observables :

- `books` contient la liste de livres.
- `error` contient un message d’erreur si un appel AJAX échoue.

La méthode `getAllBooks` effectue un appel AJAX pour récupérer la liste de livres. Il exécute ensuite un push du résultat dans le tableau de `books`.

La méthode `ko.applyBindings` fait partie de la bibliothèque Knockout. Il prend le modèle de vue en tant que paramètre et définit la liaison de données.

## <a name="add-a-script-bundle"></a>Ajouter un bundle de scripts

Le regroupement est une fonctionnalité de ASP.NET 4,5 qui facilite la combinaison ou le regroupement de plusieurs fichiers dans un seul fichier. Le regroupement réduit le nombre de demandes au serveur, ce qui peut améliorer le temps de chargement des pages.

Ouvrez le fichier d’application\_Start/BundleConfig. cs. Ajoutez le code suivant à la méthode RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Précédent](part-5.md)
> [Suivant](part-7.md)
