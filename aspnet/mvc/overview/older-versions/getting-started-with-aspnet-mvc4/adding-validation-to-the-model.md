---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Ajout de la validation au modèle | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : une version mise à jour de ce didacticiel est disponible ici et utilise ASP.NET MVC 5 et Visual Studio 2013. C’est plus sécurisé, bien plus simple à suivre et à faire une démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: c9f6699c5d3500d4c1fcade9252aeb9dd92983da
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455956"
---
# <a name="adding-validation-to-the-model"></a>Ajout de la validation au modèle

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) et utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sûr, bien plus simple à suivre et qui illustre davantage de fonctionnalités.

Dans cette section, vous allez ajouter une logique de validation au modèle `Movie`, et vous vous assurerez que les règles de validation sont appliquées chaque fois qu’un utilisateur tente de créer ou de modifier un film à l’aide de l’application.

## <a name="keeping-things-dry"></a>Conservation des éléments secs

L’un des principes de conception de base de ASP.NET MVC est DRY (&quot;Ne vous répétez pas&quot;). ASP.NET MVC vous encourage à spécifier des fonctionnalités ou un comportement une seule fois, puis à les répercuter dans une application. Cela réduit la quantité de code que vous devez écrire et rend le code que vous écrivez moins sujette aux erreurs et plus facile à gérer.

La prise en charge de la validation fournie par ASP.NET MVC et Entity Framework Code First est un excellent exemple du principe DRY en action. Vous pouvez spécifier de façon déclarative des règles de validation à un seul emplacement (dans la classe de modèle) et les règles sont appliquées partout dans l’application.

Voyons comment vous pouvez tirer parti de cette prise en charge de validation dans l’application de film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Ajout de règles de validation au modèle Movie

Vous commencerez par ajouter une logique de validation à la classe `Movie`.

Ouvrez le fichier *Movie.cs*. Ajoutez une instruction `using` en haut du fichier qui fait référence à l’espace de noms [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Notez que l’espace de noms ne contient pas de `System.Web`. DataAnnotations fournit un ensemble intégré d’attributs de validation que vous pouvez appliquer de façon déclarative à toute classe ou propriété.

À présent, mettez à jour la classe `Movie` pour tirer parti des attributs de validation intégrés [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)et [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) . Utilisez le code suivant comme exemple d’application des attributs.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Exécutez l’application et vous obtiendrez une nouvelle fois l’erreur d’exécution suivante :

***Le modèle sauvegardant le contexte’MovieDBContext’a changé depuis la création de la base de données. Envisagez d’utiliser Migrations Code First pour mettre à jour la base de données ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Nous allons utiliser les migrations pour mettre à jour le schéma. Générez la solution, puis ouvrez la fenêtre de **console du gestionnaire de package** et entrez les commandes suivantes :

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMigration` classe dérivée avec le nom spécifié (*AddDataAnnotationsMig*), et dans la méthode `Up` vous pouvez voir le code qui met à jour les contraintes de schéma. Les champs `Title` et `Genre` ne peuvent plus être null (autrement dit, vous devez entrer une valeur) et le champ `Rating` a une longueur maximale de 5.

Les attributs de validation spécifient le comportement que vous souhaitez appliquer sur les propriétés du modèle sur lesquels ils sont appliqués. L’attribut `Required` indique qu’une propriété doit avoir une valeur ; dans cet exemple, un film doit avoir des valeurs pour les propriétés `Title`, `ReleaseDate`, `Genre`et `Price` afin d’être valide. L’attribut `Range` contraint une valeur à une plage spécifiée. L’attribut `StringLength` vous permet de définir la longueur maximale d’une propriété de chaîne, et éventuellement sa longueur minimale. Les types intrinsèques (par exemple, `decimal, int, float, DateTime`) sont requis par défaut et n’ont pas besoin de l’attribut `Required`.

Code First garantit que les règles de validation que vous spécifiez sur une classe de modèle sont appliquées avant que l’application n’enregistre les modifications dans la base de données. Par exemple, le code ci-dessous lèvera une exception lorsque la méthode `SaveChanges` est appelée, car plusieurs valeurs de propriété obligatoires `Movie` sont manquantes et le prix est égal à zéro (ce qui est en dehors de la plage valide).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

L’application automatique des règles de validation par le .NET Framework aide à rendre votre application plus robuste. Cela garantit également que vous n’oublierez pas de valider un élément et que vous n’autoriserez pas par inadvertance l’insertion de données incorrectes dans la base de données.

Voici une liste complète de code pour le fichier *Movie.cs* mis à jour :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Interface utilisateur d’erreur de validation dans ASP.NET MVC

Réexécutez l’application et accédez à l’URL */movies* .

Cliquez sur le lien **Create New** pour ajouter un nouveau film. Remplissez le formulaire avec des valeurs non valides, puis cliquez sur le bouton **créer** .

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> pour prendre en charge la validation jQuery pour les paramètres régionaux autres que l’anglais qui utilisent une virgule (&quot;,&quot;) pour une virgule décimale, vous devez inclure *global. js* et votre fichier *cultures/Globally. cultures. js* spécifique (à partir de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) et JavaScript pour utiliser `Globalize.parseFloat`. Le code suivant montre les modifications apportées au fichier Views\Movies\Edit.cshtml pour travailler avec la culture &quot;fr-FR&quot; :

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Notez que le formulaire a automatiquement utilisé une couleur de bordure rouge pour mettre en surbrillance les zones de texte qui contiennent des données non valides et a émis un message d’erreur de validation approprié à côté de chacun d’eux. Les erreurs sont appliquées à la fois côté client (à l’aide de JavaScript et jQuery) et côté serveur (au cas où un utilisateur aurait désactivé JavaScript).

L’un des avantages réels est que vous n’avez pas besoin de modifier une seule ligne de code dans la classe `MoviesController` ou dans la vue *Create. cshtml* afin d’activer cette interface utilisateur de validation. Le contrôleur et les vues créées précédemment dans ce didacticiel ont détecté les règles de validation que vous avez spécifiées à l’aide des attributs de validation sur les propriétés de la classe de modèle `Movie`.

Vous avez peut-être remarqué que les propriétés `Title` et `Genre`, l’attribut required n’est pas appliqué tant que vous n’avez pas envoyé le formulaire (appuyez sur le bouton **Create** ) ou entrez du texte dans le champ d’entrée et que vous l’avez supprimé. Pour un champ qui est initialement vide (par exemple, les champs de la vue Create View) et qui n’a que l’attribut required et aucun autre attribut de validation, vous pouvez effectuer les opérations suivantes pour déclencher la validation :

1. Dans le champ.
2. Entrez du texte.
3. Appuyez sur Tab.
4. Revenez au champ.
5. Supprimez le texte.
6. Appuyez sur Tab.

La séquence ci-dessus déclenchera la validation requise sans toucher au bouton Submit. Le fait de cliquer simplement sur le bouton envoyer sans entrer aucun des champs déclenchera la validation côté client. Les données de formulaire ne sont pas envoyées au serveur tant qu’il y a des erreurs de validation côté client. Vous pouvez tester cela en plaçant un point d’arrêt dans la méthode HTTP Après ou à l’aide de l' [outil Fiddler](http://fiddler2.com/fiddler2/) ou des [outils de développement](https://msdn.microsoft.com/ie/aa740478)Internet Explorer 9 F12.

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Comment la validation se produit dans la méthode Create View et Create action

Vous vous demandez peut-être comment l’interface utilisateur de validation a été générée sans aucune mise à jour du code dans le contrôleur ou dans les vues. La liste suivante montre à quoi ressemblent les méthodes `Create` de la classe `MovieController`. Ils n’ont pas changé par rapport à la façon dont vous les avez créés précédemment dans ce didacticiel.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

La première méthode d’action (HTTP GET) `Create` affiche le formulaire de création initial. La deuxième version (`[HttpPost]`) gère la publication de formulaire. La seconde méthode `Create` (la version `HttpPost`) appelle `ModelState.IsValid` pour vérifier si le film a des erreurs de validation. L’appel de cette méthode évalue tous les attributs de validation qui ont été appliqués à l’objet. Si l’objet comporte des erreurs de validation, la méthode `Create` réaffiche le formulaire. S’il n’y a pas d’erreur, la méthode enregistre le nouveau film dans la base de données. Dans notre exemple de film que nous utilisons, **le formulaire n’est pas publié sur le serveur quand des erreurs de validation sont détectées côté client ; la deuxième** **méthode `Create`n’est jamais appelée**. Si vous désactivez JavaScript dans votre navigateur, la validation du client est désactivée et la méthode HTTP HTTP `Create` appelle `ModelState.IsValid` pour vérifier si le film contient des erreurs de validation.

Vous pouvez définir un point d’arrêt dans la méthode `HttpPost Create` et vérifier que la méthode n’est jamais appelée. La validation côté client n’enverra pas les données du formulaire quand des erreurs de validation seront détectées. Si vous désactivez JavaScript dans votre navigateur et que vous envoyez ensuite le formulaire avec des erreurs, le point d’arrêt sera atteint. Vous bénéficiez toujours d’une validation complète sans JavaScript. L’illustration suivante montre comment désactiver JavaScript dans Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

L’illustration suivante montre comment désactiver JavaScript dans le navigateur FireFox.

![](adding-validation-to-the-model/_static/image5.png)

L’illustration suivante montre comment désactiver JavaScript avec le navigateur Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Vous trouverez ci-dessous le modèle de vue *Create. cshtml* que vous avez généré au préalable dans le didacticiel. Il est utilisé par les méthodes d’action ci-dessus à la fois pour afficher le formulaire initial et pour le réafficher en cas d’erreur.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Notez que le code utilise un programme d’assistance `Html.EditorFor` pour générer la sortie de l’élément `<input>` pour chaque propriété `Movie`. En regard de cette application d’assistance, il s’agit d’un appel à la méthode d’assistance `Html.ValidationMessageFor`. Ces deux méthodes d’assistance fonctionnent avec l’objet de modèle passé par le contrôleur à la vue (dans ce cas, un objet `Movie`). Ils recherchent automatiquement les attributs de validation spécifiés sur le modèle et affichent les messages d’erreur appropriés.

L’avantage de cette approche est que ni le contrôleur, ni le modèle de vue Create ne savent quoi que ce soit sur les règles de validation réelles appliquées ou sur les messages d’erreur spécifiques affichés. Les règles de validation et les chaînes d’erreur sont spécifiées uniquement dans la classe `Movie`. Ces mêmes règles de validation sont appliquées automatiquement à la vue édition et à tous les autres modèles de vues que vous pouvez créer pour modifier votre modèle.

Si vous souhaitez modifier la logique de validation ultérieurement, vous pouvez le faire à un seul emplacement en ajoutant des attributs de validation au modèle (dans cet exemple, la classe `movie`). Vous n’aurez pas à vous soucier des différentes parties de l’application qui pourraient être incohérentes avec la façon dont les règles sont appliquées. Toute la logique de validation sera définie à un seul emplacement et utilisée partout. Le code est ainsi très propre, facile à mettre à jour et évolutif. Et cela signifie que vous respecterez entièrement le principe DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Ajout de la mise en forme au modèle Movie

Ouvrez le fichier *Movie.cs* et examinez la classe `Movie`. L’espace de noms [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fournit des attributs de mise en forme en plus de l’ensemble intégré d’attributs de validation. Nous avons déjà appliqué une valeur d’énumération [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) à la date de sortie et aux champs de prix. Le code suivant montre les propriétés `ReleaseDate` et `Price` avec l’attribut [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) approprié.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Les attributs de [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) ne sont pas des attributs de validation, ils sont utilisés pour indiquer au moteur de vue comment afficher le code html. Dans l’exemple ci-dessus, l’attribut `DataType.Date` affiche les dates de la vidéo en tant que dates uniquement, sans heure. Par exemple, les attributs de [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) suivants ne valident pas le format des données :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Les attributs listés ci-dessus fournissent uniquement des conseils pour le moteur d’affichage pour mettre en forme les données (et fournissent des attributs tels que &lt;un&gt; pour les URL et &lt;a href =&quot;mailto :EmailAddress. com&quot;&gt; pour la messagerie. Vous pouvez utiliser l’attribut [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) pour valider le format des données.

Une autre approche de l’utilisation des attributs de `DataType`, vous pouvez définir explicitement une valeur de [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) . Le code suivant montre la propriété release date avec une chaîne de format de date (à savoir, &quot;d&quot;). Vous pouvez l’utiliser pour spécifier que vous ne souhaitez pas l’heure dans le cadre de la date de publication.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

La classe `Movie` complète est indiquée ci-dessous.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Exécutez l’application et accédez au contrôleur `Movies`. La date de publication et le prix sont correctement formatés. L’image ci-dessous montre la date et le prix de la version à l’aide de &quot;fr-FR&quot; en tant que culture.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

L’image ci-dessous montre les mêmes données que celles affichées avec la culture par défaut (anglais des États-Unis).

![](adding-validation-to-the-model/_static/image8.png)

Dans la partie suivante de la série, nous allons examiner l’application et apporter des améliorations aux méthodes `Details` et `Delete` générées automatiquement.

> [!div class="step-by-step"]
> [Précédent](adding-a-new-field-to-the-movie-model-and-table.md)
> [Suivant](examining-the-details-and-delete-methods.md)
