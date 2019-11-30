---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implémentation de la fonctionnalité CRUD de base avec le Entity Framework dans l’application ASP.NET MVC (2 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: eba146d2975e0dcf243facf7c205c4acfe40b6f6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595326"
---
# <a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implémentation de la fonctionnalité CRUD de base avec le Entity Framework dans l’application ASP.NET MVC (2 sur 10)

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et de Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels depuis le début ou [Télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et démarrer ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [Téléchargez le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire le problème. Vous pouvez généralement trouver la solution au problème en comparant votre code au code terminé. Pour obtenir des erreurs courantes et comment les résoudre, consultez [Erreurs et solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Dans le didacticiel précédent, vous avez créé une application MVC qui stocke et affiche des données à l’aide de la Entity Framework et SQL Server base de données locale. Dans ce didacticiel, vous allez examiner et personnaliser le code CRUD (créer, lire, mettre à jour, supprimer) que la génération de modèles automatique MVC crée automatiquement pour vous dans les contrôleurs et les vues.

> [!NOTE]
> Il est courant d’implémenter le modèle de référentiel pour créer une couche d’abstraction entre votre contrôleur et la couche d’accès aux données. Pour simplifier ces didacticiels, vous n’allez pas implémenter un référentiel jusqu’à un didacticiel ultérieur de cette série.

Dans ce didacticiel, vous allez créer les pages Web suivantes :

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Création d’une page de détails

Le code généré automatiquement pour la page `Index` des élèves a quitté la propriété `Enrollments`, car cette propriété contient une collection. Dans la page `Details`, vous affichez le contenu de la collection dans un tableau HTML.

 Dans *Controllers\StudentController.cs*, la méthode d’action pour la vue `Details` utilise la méthode `Find` pour récupérer une seule entité `Student`. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 La valeur de clé est passée à la méthode en tant que paramètre `id` et provient de données d’itinéraire dans le lien hypertexte **Détails** sur la page d’index. 

1. Ouvrez *Views\Student\Details.cshtml*. Chaque champ est affiché à l’aide d’un programme d’assistance `DisplayFor`, comme indiqué dans l’exemple suivant : 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Après l' `EnrollmentDate` champ et immédiatement avant la balise de `fieldset` de fermeture, ajoutez du code pour afficher une liste d’inscriptions, comme indiqué dans l’exemple suivant :

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Ce code parcourt en boucle les entités dans la propriété de navigation `Enrollments`. Pour chaque `Enrollment` entité dans la propriété, elle affiche le titre du cours et le grade. Le titre du cours est extrait de l’entité `Course` stockée dans la propriété de navigation `Course` de l’entité `Enrollments`. Toutes ces données sont récupérées automatiquement à partir de la base de données lorsqu’elles sont nécessaires. (En d’autres termes, vous utilisez le chargement différé ici. Vous n’avez pas spécifié le *chargement hâtif* pour la propriété de navigation `Courses`. par conséquent, la première fois que vous tentez d’accéder à cette propriété, une requête est envoyée à la base de données pour récupérer les données. Vous pouvez en savoir plus sur le chargement différé et le chargement hâtif dans le didacticiel sur les [données de lecture](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) plus loin dans cette série.)
3. Exécutez la page en sélectionnant l’onglet **students** et en cliquant sur un lien **Détails** pour Alexander Carson. Vous voyez la liste des cours et des notes de l’étudiant sélectionné :

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Mise à jour de la page de création

1. Dans *Controllers\StudentController.cs*, remplacez la méthode d’action `HttpPost``Create` par le code suivant pour ajouter un bloc `try-catch` et l' [attribut bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) à la méthode de génération de modèles automatique : 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Ce code ajoute l’entité `Student` créée par le Binder de modèle ASP.NET MVC au jeu d’entités `Students`, puis enregistre les modifications dans la base de données. (Le*classeur de modèles* fait référence à la fonctionnalité MVC ASP.net qui facilite l’utilisation des données envoyées par un formulaire ; un classeur de modèles convertit les valeurs de formulaire publiées en types CLR et les passe à la méthode d’action dans les paramètres. Dans ce cas, le Binder de modèle instancie une entité `Student` pour vous en utilisant les valeurs de propriété de la collection `Form`.)

    L’attribut `ValidateAntiForgeryToken` permet d’éviter les attaques [par falsification de requête intersites](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) .

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.cshtml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Exécutez la page en sélectionnant l’onglet **students** et en cliquant sur **créer nouveau**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Certaines validations de données fonctionnent par défaut. Entrez les noms et une date non valide, puis cliquez sur **créer** pour afficher le message d’erreur.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Le code en surbrillance suivant illustre la vérification de la validation du modèle.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Modifiez la date en spécifiant une valeur valide, telle que 9/1/2005, puis cliquez sur **créer** pour voir le nouvel étudiant apparaître dans la page **index** .

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Mise à jour de la page modifier la publication

Dans *Controllers\StudentController.cs*, la méthode `Edit` `HttpGet` (celle sans l’attribut `HttpPost`) utilise la méthode `Find` pour récupérer l’entité `Student` sélectionnée, comme vous l’avez vu dans la méthode `Details`. Vous n’avez pas besoin de modifier cette méthode.

Toutefois, remplacez le `HttpPost` `Edit` méthode d’action par le code suivant pour ajouter un bloc `try-catch` et l' [attribut bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Ce code est similaire à ce que vous avez vu dans la méthode `HttpPost` `Create`. Toutefois, au lieu d’ajouter l’entité créée par le classeur de modèles au jeu d’entités, ce code définit un indicateur sur l’entité qui indique qu’elle a été modifiée. Lorsque la méthode [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) est appelée, l’indicateur [modifié](https://msdn.microsoft.com/library/system.data.entitystate.aspx) entraîne la création d’instructions SQL par l’Entity Framework pour mettre à jour la ligne de la base de données. Toutes les colonnes de la ligne de base de données seront mises à jour, y compris celles que l’utilisateur n’a pas modifiées et les conflits d’accès concurrentiel sont ignorés. (Vous apprendrez à gérer l’accès concurrentiel dans un didacticiel ultérieur de cette série.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>États d’entité et méthodes Attach et SaveChanges

Le contexte de base de données effectue le suivi de la synchronisation ou non des entités en mémoire avec leurs lignes correspondantes dans la base de données, et ces informations déterminent ce qui se passe quand vous appelez la méthode `SaveChanges`. Par exemple, lorsque vous transmettez une nouvelle entité à la méthode [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) , l’état de cette entité est défini sur `Added`. Ensuite, lorsque vous appelez la méthode [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , le contexte de base de données émet une commande SQL `INSERT`.

Une entité peut être dans l’un des[États suivants](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. L’entité n’existe pas encore dans la base de données. La méthode `SaveChanges` doit émettre une instruction `INSERT`.
- `Unchanged`. La méthode `SaveChanges` ne doit rien faire avec cette entité. Quand vous lisez une entité dans la base de données, l’entité a d’abord cet état.
- `Modified`. Tout ou partie des valeurs de propriété de l’entité ont été modifiées. La méthode `SaveChanges` doit émettre une instruction `UPDATE`.
- `Deleted`. L’entité a été marquée pour suppression. La méthode `SaveChanges` doit émettre une instruction `DELETE`.
- `Detached`. L’entité n’est pas suivie par le contexte de base de données.

Dans une application de poste de travail, les changements d’état sont généralement définis automatiquement. Dans un type d’application de bureau, vous lisez une entité et apportez des modifications à certaines de ses valeurs de propriété. Son état passe alors automatiquement à `Modified`. Ensuite, lorsque vous appelez `SaveChanges`, le Entity Framework génère une instruction SQL `UPDATE` qui met à jour uniquement les propriétés réelles que vous avez modifiées.

La nature déconnectée des applications Web n’autorise pas cette séquence continue. Le [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) qui lit une entité est supprimé après le rendu d’une page. Lorsque le `HttpPost` `Edit` méthode d’action est appelée, une nouvelle demande est effectuée et que vous avez une nouvelle instance de [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx). vous devez donc définir manuellement l’état de l’entité sur `Modified.` puis, lorsque vous appelez `SaveChanges`, la Entity Framework met à jour toutes les colonnes de la ligne de base de données, car le contexte n’a aucun moyen de savoir quelles propriétés vous avez modifiées.

Si vous souhaitez que l’instruction SQL `Update` ne met à jour que les champs modifiés par l’utilisateur, vous pouvez enregistrer les valeurs d’origine d’une certaine façon (par exemple, les champs masqués) afin qu’ils soient disponibles lors de l’appel de la méthode `HttpPost` `Edit`. Vous pouvez ensuite créer une entité `Student` à l’aide des valeurs d’origine, appeler la méthode `Attach` avec cette version d’origine de l’entité, mettre à jour les valeurs de l’entité avec les nouvelles valeurs, puis appeler `SaveChanges.` pour plus d’informations, consultez [États d’entité et SaveChanges](https://msdn.microsoft.com/data/jj592676) et [données locales](https://msdn.microsoft.com/data/jj592872) dans le centre de développement de données MSDN.

Le code dans *Views\Student\Edit.cshtml* est similaire à ce que vous avez vu dans *Create. cshtml*et aucune modification n’est requise.

Exécutez la page en sélectionnant l’onglet **students** , puis en cliquant sur un lien hypertexte **Edit** .

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Changez quelques données et cliquez sur **Save**. Les données modifiées s’affichent dans la page index.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Mise à jour de la page de suppression

Dans *Controllers\StudentController.cs*, le code de modèle pour la méthode `HttpGet` `Delete` utilise la méthode `Find` pour récupérer l’entité `Student` sélectionnée, comme vous l’avez vu dans les méthodes `Details` et `Edit`. Cependant, pour implémenter un message d’erreur personnalisé quand l’appel à `SaveChanges` échoue, vous devez ajouter des fonctionnalités à cette méthode et à sa vue correspondante.

Comme vous l’avez vu pour les opérations de mise à jour et de création, les opérations de suppression nécessitent deux méthodes d’action. La méthode qui est appelée en réponse à une demande d’extraction affiche une vue qui donne à l’utilisateur la possibilité d’approuver ou d’annuler l’opération de suppression. Si l’utilisateur l’approuve, une demande POST est créée. Lorsque cela se produit, la méthode `Delete` `HttpPost` est appelée, puis cette méthode exécute l’opération de suppression.

Vous ajouterez un bloc de `try-catch` à la méthode `HttpPost` `Delete` pour gérer les erreurs qui peuvent se produire lorsque la base de données est mise à jour. Si une erreur se produit, le `HttpPost` `Delete` méthode appelle la méthode `Delete` `HttpGet`, en lui passant un paramètre qui indique qu’une erreur s’est produite. La méthode `HttpGet Delete` réaffiche ensuite la page de confirmation ainsi que le message d’erreur, ce qui donne à l’utilisateur la possibilité d’annuler ou de réessayer.

1. Remplacez le `HttpGet` `Delete` méthode d’action par le code suivant, qui gère le rapport d’erreurs : 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Ce code accepte un paramètre booléen [facultatif](https://msdn.microsoft.com/library/dd264739.aspx) qui indique s’il a été appelé après l’échec de l’enregistrement des modifications. Ce paramètre est `false` lorsque l' `HttpGet` `Delete` méthode est appelée sans échec précédent. Lorsqu’elle est appelée par la méthode `HttpPost` `Delete` en réponse à une erreur de mise à jour de la base de données, le paramètre est `true` et un message d’erreur est passé à la vue.
2. Remplacez le `HttpPost` `Delete` méthode d’action (nommée `DeleteConfirmed`) par le code suivant, qui exécute l’opération de suppression réelle et intercepte les erreurs de mise à jour de la base de données.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Ce code récupère l’entité sélectionnée, puis appelle la méthode [Remove](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) pour définir l’état de l’entité sur `Deleted`. Lorsque `SaveChanges` est appelée, une commande SQL `DELETE` est générée. Vous avez également changé le nom de la méthode d’action de `DeleteConfirmed` en `Delete`. Le code de génération de modèles automatique nommé `HttpPost` méthode `Delete` `DeleteConfirmed` pour attribuer une signature unique à la méthode `HttpPost`. (Le CLR exige que les méthodes surchargées aient des paramètres de méthode différents.) Maintenant que les signatures sont uniques, vous pouvez respecter la convention MVC et utiliser le même nom pour les `HttpPost` et `HttpGet` les méthodes Delete.

     Si l’amélioration des performances dans une application de volume élevé est une priorité, vous pouvez éviter une requête SQL inutile pour récupérer la ligne en remplaçant les lignes de code qui appellent les méthodes `Find` et `Remove` par le code suivant, comme indiqué en surbrillance jaune :

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Ce code instancie une entité `Student` en utilisant uniquement la valeur de clé primaire, puis définit l’état de l’entité sur `Deleted`. C’est tout ce dont a besoin Entity Framework pour pouvoir supprimer l’entité.

     Comme indiqué, la méthode `HttpGet` `Delete` ne supprime pas les données. L’exécution d’une opération de suppression en réponse à une demande d’extraction (ou, pour cette question, l’exécution d’une opération de modification, d’une opération de création ou toute autre opération qui modifie des données) crée un risque de sécurité. Pour plus d’informations, consultez [#46 Conseil MVC ASP.net — n’utilisez pas les liens de suppression, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) sur le blog de Stephen Walther.
3. Dans *Views\Student\Delete.cshtml*, ajoutez un message d’erreur entre l’en-tête `h2` et l’en-tête `h3`, comme indiqué dans l’exemple suivant :

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Exécutez la page en sélectionnant l’onglet **students** et en cliquant sur un lien hypertexte **Delete** :

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Cliquez sur **Delete**. La page Index s’affiche sans l’étudiant supprimé. (Vous verrez un exemple de code de gestion des erreurs dans action dans le didacticiel sur la gestion de l' [accès concurrentiel](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) plus loin dans cette série.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>S’assurer que les connexions de base de données ne restent pas ouvertes

Pour vous assurer que les connexions de base de données sont correctement fermées et que les ressources qu’elles contiennent sont libérées, vous devez voir que l’instance de contexte est supprimée. C’est la raison pour laquelle le code de génération de modèles automatique fournit une méthode [dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) à la fin de la classe `StudentController` dans *StudentController.cs*, comme illustré dans l’exemple suivant :

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

La classe de `Controller` de base implémente déjà l’interface `IDisposable`. ce code ajoute donc simplement une substitution à la méthode `Dispose(bool)` pour supprimer explicitement l’instance de contexte.

## <a name="summary"></a>Récapitulatif

Vous disposez maintenant d’un ensemble complet de pages qui effectuent des opérations CRUD simples pour les entités `Student`. Vous avez utilisé les applications d’assistance MVC pour générer des éléments d’interface utilisateur pour les champs de données. Pour plus d’informations sur les applications d’assistance MVC, consultez [rendu d’un formulaire à l’aide des applications auxiliaires html](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (la page est destinée à MVC 3 mais reste pertinente pour MVC 4).

Dans le didacticiel suivant, vous allez développer les fonctionnalités de la page d’index en ajoutant des fonctions de tri et de pagination.

Vous trouverez des liens vers d’autres ressources de Entity Framework dans le [plan de contenu d’accès aux données ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Suivant](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
