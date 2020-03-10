---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Validation avec l’interface IDataErrorInfo (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther vous montre comment afficher des messages d’erreur de validation personnalisés en implémentant l’interface IDataErrorInfo dans une classe de modèle.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 938b180da02b1963acffd021d18621d75d1d0447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542559"
---
# <a name="validating-with-the-idataerrorinfo-interface-c"></a>Validation avec l’interface IDataErrorInfo (C#)

par [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther vous montre comment afficher des messages d’erreur de validation personnalisés en implémentant l’interface IDataErrorInfo dans une classe de modèle.

L’objectif de ce didacticiel est d’expliquer une approche de l’exécution de la validation dans une application MVC ASP.NET. Vous allez apprendre comment empêcher une personne de soumettre un formulaire HTML sans fournir de valeurs pour les champs de formulaire requis. Dans ce didacticiel, vous allez apprendre à effectuer la validation à l’aide de l’interface IErrorDataInfo.

## <a name="assumptions"></a>Assumptions (Hypothèses)

Dans ce didacticiel, je vais utiliser la base de données MoviesDB et la table de base de données movies. Cette table présente les colonnes suivantes :

<a id="0.5_table01"></a>

| **Nom de la colonne** | **Type de données** | **Null autorisé** |
| --- | --- | --- |
| Id | Int | False |
| Titre | Nvarchar(100) | False |
| Directeur | Nvarchar(100) | False |
| DateReleased | DateTime | False |

Dans ce didacticiel, j’utilise le Entity Framework Microsoft pour générer mes classes de modèle de base de données. La classe Movie générée par le Entity Framework est affichée à la figure 1.

[![l’entité film](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Figure 01**: entité de film ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))

> [!NOTE] 
> 
> Pour en savoir plus sur l’utilisation de la Entity Framework pour générer vos classes de modèle de base de données, consultez mon didacticiel intitulé Création de classes de modèle avec l’Entity Framework.

## <a name="the-controller-class"></a>La classe Controller

Nous utilisons le contrôleur d’hébergement pour répertorier les films et créer de nouveaux films. Le code de cette classe est contenu dans la liste 1.

**Liste 1-Controllers\HomeController.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

La classe de contrôleur d’hébergement de la liste 1 contient deux actions de création (). La première action affiche le formulaire HTML pour la création d’un nouveau film. La deuxième action Create () effectue l’insertion réelle du nouveau film dans la base de données. La deuxième action Create () est appelée lorsque le formulaire affiché par la première action Create () est envoyé au serveur.

Notez que la deuxième action Create () contient les lignes de code suivantes :

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

La propriété IsValid retourne la valeur false en cas d’erreur de validation. Dans ce cas, la vue Create qui contient le formulaire HTML pour la création d’un film est réaffichée.

## <a name="creating-a-partial-class"></a>Création d’une classe partielle

La classe Movie est générée par la Entity Framework. Vous pouvez voir le code de la classe Movie si vous développez le fichier MoviesDBModel. edmx dans la fenêtre Explorateur de solutions et que vous ouvrez le fichier MoviesDBModel.Designer.cs dans l’éditeur de code (voir la figure 2).

[![le code de l’entité film](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Figure 02**: code de l’entité Movie ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))

La classe Movie est une classe partielle. Cela signifie que nous pouvons ajouter une autre classe partielle portant le même nom pour étendre les fonctionnalités de la classe Movie. Nous allons ajouter notre logique de validation à la nouvelle classe partielle.

Ajoutez la classe dans Listing 2 au dossier Models.

**Liste 2-Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Notez que la classe de la liste 2 comprend le modificateur *Partial* . Toutes les méthodes ou propriétés que vous ajoutez à cette classe font partie de la classe Movie générée par l’Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Ajout de méthodes partielles OnChanging et OnChanged

Lorsque l’Entity Framework génère une classe d’entité, le Entity Framework ajoute automatiquement des méthodes partielles à la classe. Le Entity Framework génère des méthodes partielles OnChanging et OnChanged qui correspondent à chaque propriété de la classe.

Dans le cas de la classe Movie, le Entity Framework crée les méthodes suivantes :

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

La méthode OnChanging est appelée juste avant la modification de la propriété correspondante. La méthode OnChanged est appelée juste après la modification de la propriété.

Vous pouvez tirer parti de ces méthodes partielles pour ajouter une logique de validation à la classe Movie. La classe Update Movie de la liste 3 vérifie que les valeurs non vides des propriétés Title et Director sont affectées.

> [!NOTE] 
> 
> Une méthode partielle est une méthode définie dans une classe que vous n’êtes pas obligé d’implémenter. Si vous n’implémentez pas de méthode partielle, le compilateur supprime la signature de méthode et tous les appels à la méthode afin qu’il n’y ait aucun coût d’exécution associé à la méthode partielle. Dans l’éditeur de Visual Studio Code, vous pouvez ajouter une méthode partielle en tapant le mot clé *Partial* suivi d’un espace pour afficher la liste des partielles à implémenter.

**Liste 3-Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Par exemple, si vous tentez d’affecter une chaîne vide à la propriété Title, un message d’erreur est assigné à un dictionnaire nommé \_Errors.

À ce stade, rien ne se produit lorsque vous affectez une chaîne vide à la propriété Title et qu’une erreur est ajoutée au champ private \_Errors. Nous devons implémenter l’interface IDataErrorInfo pour exposer ces erreurs de validation à l’infrastructure MVC ASP.NET.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implémentation de l’interface IDataErrorInfo

L’interface IDataErrorInfo fait partie du .NET Framework depuis la première version. Cette interface est très simple :

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Si une classe implémente l’interface IDataErrorInfo, l’infrastructure MVC ASP.NET utilise cette interface lors de la création d’une instance de la classe. Par exemple, l’action créer () du contrôleur d’hébergement accepte une instance de la classe Movie :

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

L’infrastructure MVC ASP.NET crée l’instance du film passé à l’action Create () à l’aide d’un classeur de modèles (DefaultModelBinder). Le Binder de modèle est chargé de créer une instance de l’objet Movie en liant les champs de formulaire HTML à une instance de l’objet Movie.

Le DefaultModelBinder détecte si une classe implémente l’interface IDataErrorInfo. Si une classe implémente cette interface, le Binder de modèle appelle IDataErrorInfo. cet indexeur pour chaque propriété de la classe. Si l’indexeur retourne un message d’erreur, le Binder de modèle ajoute automatiquement ce message d’erreur à l’état du modèle.

Le DefaultModelBinder vérifie également la propriété IDataErrorInfo. Error. Cette propriété est destinée à représenter les erreurs de validation non spécifiques à une propriété associées à la classe. Par exemple, vous souhaiterez peut-être appliquer une règle de validation qui dépend des valeurs de plusieurs propriétés de la classe Movie. Dans ce cas, vous retournez une erreur de validation à partir de la propriété Error.

La classe Movie mise à jour de la liste 4 implémente l’interface IDataErrorInfo.

**Liste 4-Models\Movie.cs (implémente IDataErrorInfo)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

Dans la liste 4, la propriété d’indexeur vérifie la collection d’erreurs \_pour déterminer si elle contient une clé qui correspond au nom de propriété passé à l’indexeur. Si aucune erreur de validation n’est associée à la propriété, une chaîne vide est retournée.

Vous n’avez pas besoin de modifier le contrôleur d’hébergement de quelque façon que ce soit pour utiliser la classe Movie modifiée. La page affichée à la figure 3 illustre ce qui se passe quand aucune valeur n’est entrée pour les champs de formulaire de titre ou de directeur.

[![créer automatiquement des méthodes d’action](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Figure 03**: formulaire avec des valeurs manquantes ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))

Notez que la valeur DateReleased est validée automatiquement. Étant donné que la propriété DateReleased n’accepte pas les valeurs NULL, DefaultModelBinder génère une erreur de validation pour cette propriété automatiquement lorsqu’elle n’a pas de valeur. Si vous souhaitez modifier le message d’erreur pour la propriété DateReleased, vous devez créer un classeur de modèles personnalisé.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à utiliser l’interface IDataErrorInfo pour générer des messages d’erreur de validation. Tout d’abord, nous avons créé une classe de film partielle qui étend les fonctionnalités de la classe de film partielle générée par la Entity Framework. Ensuite, nous avons ajouté une logique de validation aux méthodes partielles de la classe Movie OnTitleChanging () et OnDirectorChanging (). Enfin, nous avons implémenté l’interface IDataErrorInfo pour exposer ces messages de validation à l’infrastructure ASP.NET MVC.

> [!div class="step-by-step"]
> [Précédent](performing-simple-validation-cs.md)
> [Suivant](validating-with-a-service-layer-cs.md)
