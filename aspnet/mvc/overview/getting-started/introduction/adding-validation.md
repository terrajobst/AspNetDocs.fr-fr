---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Ajout de la validation | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/06/2019
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 67df1a473cd13a651c1276054b93f34323479082
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519022"
---
# <a name="adding-validation"></a>Ajouter une validation

par [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

Dans cette section, vous allez ajouter une logique de validation au modèle `Movie`, et vous vous assurerez que les règles de validation sont appliquées chaque fois qu’un utilisateur tente de créer ou de modifier un film à l’aide de l’application.

## <a name="keeping-things-dry"></a>Conservation des éléments secs

L’un des principes de conception de base de ASP.NET MVC est [Dry](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;ne vous répétez pas&quot;). ASP.NET MVC vous encourage à spécifier des fonctionnalités ou un comportement une seule fois, puis à les répercuter dans une application. Cela réduit la quantité de code que vous devez écrire et rend le code que vous écrivez moins sujette aux erreurs et plus facile à gérer.

La prise en charge de la validation fournie par ASP.NET MVC et Entity Framework Code First est un excellent exemple du principe DRY en action. Vous pouvez spécifier de façon déclarative des règles de validation à un seul emplacement (dans la classe de modèle) et les règles sont appliquées partout dans l’application.

Voyons comment vous pouvez tirer parti de cette prise en charge de validation dans l’application de film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Ajout de règles de validation au modèle Movie

Vous commencerez par ajouter une logique de validation à la classe `Movie`.

Ouvrez le fichier *Movie.cs*. Notez que l’espace de noms [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ne contient pas `System.Web`. DataAnnotations fournit un ensemble intégré d’attributs de validation que vous pouvez appliquer de façon déclarative à toute classe ou propriété. (Il contient également des attributs de mise en forme tels que [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) qui facilitent la mise en forme et ne fournissent aucune validation.)

À présent, mettez à jour la classe `Movie` pour tirer parti des attributs de validation intégrés [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)et [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) . Remplacez la classe `Movie` par le code suivant :

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

L’attribut [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) définit la longueur maximale de la chaîne et définit cette limitation sur la base de données. par conséquent, le schéma de base de données change. Cliquez avec le bouton droit sur le tableau **films** dans l **'Explorateur de serveurs** , puis cliquez sur **ouvrir la définition de table**:

![](adding-validation/_static/image1.png)

Dans l’image ci-dessus, vous pouvez voir que tous les champs de chaîne sont définis sur [nvarchar (max)](https://technet.microsoft.com/library/ms186939.aspx). Nous allons utiliser les migrations pour mettre à jour le schéma. Générez la solution, puis ouvrez la fenêtre de **console du gestionnaire de package** et entrez les commandes suivantes :

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMigration` classe dérivée portant le nom spécifié (`DataAnnotations`), et dans la méthode `Up` vous pouvez voir le code qui met à jour les contraintes de schéma :

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

Le champ `Genre` n’est plus Nullable (autrement dit, vous devez entrer une valeur). Le champ `Rating` a une longueur maximale de 5 et `Title` a une longueur maximale de 60. La longueur minimale de 3 sur `Title` et la plage sur `Price` n’ont pas créé de modifications de schéma.

Examinez le schéma du film :

![](adding-validation/_static/image2.png)

Les champs de chaîne affichent les nouvelles limites de longueur et la `Genre` n’est plus vérifiée comme Nullable.

Les attributs de validation spécifient le comportement que vous souhaitez appliquer sur les propriétés du modèle sur lesquels ils sont appliqués. Les attributs `Required` et `MinimumLength` indiquent qu’une propriété doit avoir une valeur, mais rien n’empêche un utilisateur d’entrer un espace blanc pour satisfaire cette validation. L’attribut [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) est utilisé pour limiter les caractères qui peuvent être entrés. Dans le code ci-dessus, `Genre` et `Rating` doivent utiliser uniquement des lettres (les espaces blancs, les chiffres et les caractères spéciaux ne sont pas autorisés). L’attribut [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) limite une valeur à dans une plage spécifiée. L’attribut `StringLength` vous permet de définir la longueur maximale d’une propriété de chaîne, et éventuellement sa longueur minimale. Les types valeur (tels que `decimal, int, float, DateTime`) sont obligatoires par nature et n’ont pas besoin de l’attribut `Required`.

Code First garantit que les règles de validation que vous spécifiez sur une classe de modèle sont appliquées avant que l’application n’enregistre les modifications dans la base de données. Par exemple, le code ci-dessous lèvera une exception [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) quand la méthode `SaveChanges` est appelée, car plusieurs valeurs de propriété `Movie` requises sont manquantes :

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Le code ci-dessus lève l’exception suivante :

*La validation a échoué pour une ou plusieurs entités. Pour plus d’informations, consultez la propriété « EntityValidationErrors ».*

L’application automatique des règles de validation par le .NET Framework aide à rendre votre application plus robuste. Cela garantit également que vous n’oublierez pas de valider un élément et que vous n’autoriserez pas par inadvertance l’insertion de données incorrectes dans la base de données.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Interface utilisateur d’erreur de validation dans ASP.NET MVC

Exécutez l’application et accédez à l’URL */movies* .

Cliquez sur le lien **Create New** pour ajouter un nouveau film. Remplissez le formulaire avec des valeurs non valides. Dès que la validation côté client jQuery détecte l’erreur, elle affiche un message d’erreur.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> pour prendre en charge la validation jQuery pour les paramètres régionaux autres que l’anglais qui utilisent une virgule (",") pour une virgule décimale, vous devez inclure la globalisation NuGet comme décrit précédemment dans ce didacticiel.

Notez que le formulaire a automatiquement utilisé une couleur de bordure rouge pour mettre en surbrillance les zones de texte qui contiennent des données non valides et a émis un message d’erreur de validation approprié à côté de chacun d’eux. Les erreurs sont appliquées à la fois côté client (à l’aide de JavaScript et jQuery) et côté serveur (au cas où un utilisateur aurait désactivé JavaScript).

L’un des avantages réels est que vous n’avez pas besoin de modifier une seule ligne de code dans la classe `MoviesController` ou dans la vue *Create. cshtml* afin d’activer cette interface utilisateur de validation. Le contrôleur et les vues créées précédemment dans ce didacticiel ont détecté les règles de validation que vous avez spécifiées à l’aide des attributs de validation sur les propriétés de la classe de modèle `Movie`. Testez la validation à l’aide de la méthode d’action `Edit`. La même validation est appliquée.

Les données de formulaire ne sont pas envoyées au serveur tant qu’il y a des erreurs de validation côté client. Vous pouvez le vérifier en plaçant un point d’arrêt dans la méthode HTTP Après, en utilisant l' [outil Fiddler](http://fiddler2.com/fiddler2/)ou les [outils de développement](https://msdn.microsoft.com/ie/aa740478)Internet Explorer F12.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Comment la validation se produit dans la méthode Create View et Create action

Vous vous demandez peut-être comment l’interface utilisateur de validation a été générée sans aucune mise à jour du code dans le contrôleur ou dans les vues. La liste suivante montre à quoi ressemblent les méthodes `Create` de la classe `MovieController`. Ils n’ont pas changé par rapport à la façon dont vous les avez créés précédemment dans ce didacticiel.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

La première méthode d’action (HTTP GET) `Create` affiche le formulaire de création initial. La deuxième version (`[HttpPost]`) gère la publication de formulaire. La deuxième méthode `Create` (la version `HttpPost`) vérifie `ModelState.IsValid` pour voir si le film contient des erreurs de validation. L’obtention de cette propriété évalue tous les attributs de validation qui ont été appliqués à l’objet. Si l’objet comporte des erreurs de validation, la méthode `Create` réaffiche le formulaire. S’il n’y a pas d’erreur, la méthode enregistre le nouveau film dans la base de données. Dans notre exemple de film, **le formulaire n’est pas publié sur le serveur quand des erreurs de validation sont détectées côté client ; la deuxième** `Create` **méthode n’est jamais appelée**. Si vous désactivez JavaScript dans votre navigateur, la validation du client est désactivée et la méthode HTTP HTTP `Create` obtient `ModelState.IsValid` pour vérifier si le film contient des erreurs de validation.

Vous pouvez définir un point d’arrêt dans la méthode `HttpPost Create` et vérifier que la méthode n’est jamais appelée. La validation côté client n’enverra pas les données du formulaire quand des erreurs de validation seront détectées. Si vous désactivez JavaScript dans votre navigateur et que vous envoyez ensuite le formulaire avec des erreurs, le point d’arrêt sera atteint. Vous bénéficiez toujours d’une validation complète sans JavaScript. L’illustration suivante montre comment désactiver JavaScript dans Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

L’illustration suivante montre comment désactiver JavaScript dans le navigateur FireFox.

![](adding-validation/_static/image7.png)

L’illustration suivante montre comment désactiver JavaScript dans le navigateur Chrome.

![](adding-validation/_static/image8.png)

Vous trouverez ci-dessous le modèle de vue *Create. cshtml* que vous avez généré au préalable dans le didacticiel. Il est utilisé par les méthodes d’action ci-dessus à la fois pour afficher le formulaire initial et pour le réafficher en cas d’erreur.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Notez que le code utilise un programme d’assistance `Html.EditorFor` pour générer la sortie de l’élément `<input>` pour chaque propriété `Movie`. En regard de cette application d’assistance, il s’agit d’un appel à la méthode d’assistance `Html.ValidationMessageFor`. Ces deux méthodes d’assistance fonctionnent avec l’objet de modèle passé par le contrôleur à la vue (dans ce cas, un objet `Movie`). Ils recherchent automatiquement les attributs de validation spécifiés sur le modèle et affichent les messages d’erreur appropriés.

Le grand avantage de cette approche est que ni le contrôleur ni le modèle de vue `Create` ne savent rien des règles de validation appliquées ou des messages d’erreur affichés. Les règles de validation et les chaînes d’erreur sont spécifiées uniquement dans la classe `Movie`. Ces mêmes règles de validation sont automatiquement appliquées à la vue `Edit` et à tous les autres modèles de vues que vous pouvez créer et qui modifient votre modèle.

Si vous souhaitez modifier la logique de validation ultérieurement, vous pouvez le faire à un seul emplacement en ajoutant des attributs de validation au modèle (dans cet exemple, la classe `movie`). Vous n’aurez pas à vous soucier des différentes parties de l’application qui pourraient être incohérentes avec la façon dont les règles sont appliquées. Toute la logique de validation sera définie à un seul emplacement et utilisée partout. Le code est ainsi très propre, facile à mettre à jour et évolutif. Et cela signifie que vous respecterez pleinement le principe *sec* .

## <a name="using-datatype-attributes"></a>Utilisation d’attributs DataType

Ouvrez le fichier *Movie.cs* et examinez la classe `Movie`. L’espace de noms [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fournit des attributs de mise en forme en plus de l’ensemble intégré d’attributs de validation. Nous avons déjà appliqué une valeur d’énumération [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) à la date de sortie et aux champs de prix. Le code suivant montre les propriétés `ReleaseDate` et `Price` avec l’attribut [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) approprié.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

Les attributs de [type](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) de données fournissent uniquement des conseils pour le moteur d’affichage pour mettre en forme les données (et fournissent des attributs tels que `<a>` pour les URL et les `<a href="mailto:EmailAddress.com">` pour la messagerie électronique. Vous pouvez utiliser l’attribut [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) pour valider le format des données. L’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) est utilisé pour spécifier un type de données qui est plus spécifique que le type intrinsèque de la base de données, il ne s’agit ***pas*** d’attributs de validation. Dans le cas présent, nous voulons uniquement effectuer le suivi de la date, pas de la date et de l’heure. L' [énumération DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fournit de nombreux types de données, tels que *date, Time, PhoneNumber, Currency, EmailAddress* et bien plus encore. L’attribut `DataType` peut également permettre à l’application de fournir automatiquement des fonctionnalités propres au type. Par exemple, un lien `mailto:` peut être créé pour [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)et un sélecteur de date peut être fourni pour [DataType. date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) dans les navigateurs qui prennent en charge [HTML5](http://html5.org/). Les attributs [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) émettent des attributs HTML 5 [Data-](http://ejohn.org/blog/html-5-data-attributes/) (prononced *Data Dash*) que les navigateurs HTML 5 peuvent comprendre. Les attributs de [type de données](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) ne fournissent aucune validation.

`DataType.Date` ne spécifie pas le format de la date qui s’affiche. Par défaut, le champ de données est affiché conformément aux formats par défaut basés sur le [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)du serveur.

L’attribut `DisplayFormat` est utilisé pour spécifier explicitement le format de date :

[!code-csharp[Main](adding-validation/samples/sample8.cs)]

Le paramètre `ApplyFormatInEditMode` spécifie que la mise en forme spécifiée doit également être appliquée lorsque la valeur est affichée dans une zone de texte pour modification. (Vous ne voulez peut-être pas que pour certains champs, par exemple, pour les valeurs monétaires, vous ne voudrez peut-être pas que le symbole monétaire dans la zone de texte soit modifié.)

Vous pouvez utiliser l’attribut [displayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) seul, mais il est généralement judicieux d’utiliser également l’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) . L’attribut `DataType` transmet la *sémantique* des données au lieu de les afficher sur un écran, et offre les avantages suivants, que vous ne pouvez pas obtenir avec `DisplayFormat`:

- Le navigateur peut activer des fonctionnalités HTML5 (par exemple, pour afficher un contrôle de calendrier, les paramètres régionaux, des liens de devise appropriés, des liens de messagerie, etc.).
- Par défaut, le navigateur restitue les données à l’aide du format approprié en fonction de vos [paramètres régionaux](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- L’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) peut permettre à MVC de choisir le modèle de champ approprié pour le rendu des données (la [displayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) si elle est utilisée par elle-même, utilise le modèle de chaîne). Pour plus d’informations, consultez [modèles ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)de Brad Wilson. (Bien qu’il soit écrit pour MVC 2, cet article s’applique toujours à la version actuelle de ASP.NET MVC.)

Si vous utilisez l’attribut `DataType` avec un champ date, vous devez spécifier l’attribut `DisplayFormat` également afin de garantir que le champ s’affiche correctement dans les navigateurs Chrome. Pour plus d’informations, consultez [ce thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> la validation jQuery ne fonctionne pas avec l’attribut [Range](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) et [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Par exemple, le code suivant affiche toujours une erreur de validation côté client, même quand la date se trouve dans la plage spécifiée :
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> Vous devez désactiver la validation de date jQuery pour utiliser l’attribut [Range](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) avec [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Il n’est généralement pas recommandé de compiler des dates dures dans vos modèles. par conséquent, l’utilisation de l’attribut [Range](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) et de [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx) est déconseillée.

Le code suivant illustre la combinaison d’attributs sur une seule ligne :

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=4,6,10,12)]

Dans la partie suivante de la série, nous allons examiner l’application et apporter des améliorations aux méthodes `Details` et `Delete` générées automatiquement.

> [!div class="step-by-step"]
> [Précédent](adding-a-new-field.md)
> [Suivant](examining-the-details-and-delete-methods.md)
