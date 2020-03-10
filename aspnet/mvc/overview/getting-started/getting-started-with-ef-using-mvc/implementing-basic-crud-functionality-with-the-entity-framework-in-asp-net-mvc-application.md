---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: 'Didacticiel : implémenter la fonctionnalité CRUD avec l’Entity Framework dans ASP.NET MVC | Microsoft Docs'
description: Examinez et personnalisez le code de création, de lecture, de mise à jour, de suppression (CRUD) créé automatiquement par la génération de modèles automatique MVC dans les contrôleurs et les vues.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9ed388543dd54d209ff2a0b92df4f7659962582c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583159"
---
# <a name="tutorial-implement-crud-functionality-with-the-entity-framework-in-aspnet-mvc"></a>Didacticiel : implémenter la fonctionnalité CRUD avec l’Entity Framework dans ASP.NET MVC

Dans le [didacticiel précédent](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), vous avez créé une application MVC qui stocke et affiche des données à l’aide du Entity Framework (EF) 6 et SQL Server de la base de données locale. Dans ce didacticiel, vous allez examiner et personnaliser le code CRUD (création, lecture, mise à jour, suppression) que la génération de modèles automatique MVC crée automatiquement pour vous dans les contrôleurs et les vues.

> [!NOTE]
> Il est courant d’implémenter le modèle de référentiel pour créer une couche d’abstraction entre votre contrôleur et la couche d’accès aux données. Pour simplifier ces didacticiels et vous apprendre à utiliser EF 6 lui-même, ils n’utilisent pas de référentiels. Pour plus d’informations sur la façon d’implémenter des référentiels, consultez le [mappage de contenu d’accès aux données ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

Voici des exemples de pages Web que vous créez :

![Capture d’écran de la page des détails de l’étudiant.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Capture d’écran de la page de création de Student.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Capture d’écran de la page de suppression des étudiants.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créer une page de détails
> * Mettre à jour la page Create
> * Mettre à jour la méthode de modification HttpPost
> * Mettre à jour la page Delete
> * Fermer les connexions de base de données
> * Gérer les transactions

## <a name="prerequisites"></a>Conditions préalables requises

* [Créer le modèle de données Entity Framework](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

## <a name="create-a-details-page"></a>Créer une page de détails

Le code généré automatiquement pour la page `Index` des élèves a quitté la propriété `Enrollments`, car cette propriété contient une collection. Dans la page `Details`, vous affichez le contenu de la collection dans un tableau HTML.

Dans *Controllers\StudentController.cs*, la méthode d’action pour la vue `Details` utilise la méthode [Find](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) pour récupérer une seule entité `Student`.

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

La valeur de clé est passée à la méthode en tant que paramètre `id` et provient de *données d’itinéraire* dans le lien hypertexte **Détails** sur la page d’index.

### <a name="tip-route-data"></a>Conseil : **acheminer les données**

Les données de routage sont des données que le Binder de modèle a trouvées dans un segment d’URL spécifié dans la table de routage. Par exemple, l’itinéraire par défaut spécifie les segments `controller`, `action`et `id` :

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

Dans l’URL suivante, l’itinéraire par défaut mappe `Instructor` comme `controller`, `Index` comme `action` et 1 comme `id`; Il s’agit des valeurs de données d’itinéraire.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

`?courseID=2021` est une valeur de chaîne de requête. Le Binder de modèle fonctionne également si vous transmettez le `id` en tant que valeur de chaîne de requête :

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Les URL sont créées par des instructions `ActionLink` dans la vue Razor. Dans le code suivant, le paramètre `id` correspond à l’itinéraire par défaut, de sorte que `id` est ajouté aux données d’itinéraire.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

Dans le code suivant, `courseID` ne correspond pas à un paramètre de l’itinéraire par défaut, il est donc ajouté en tant que chaîne de requête.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]

### <a name="to-create-the-details-page"></a>Pour créer la page de détails

1. Ouvrez *Views\Student\Details.cshtml*.

   Chaque champ est affiché à l’aide d’un programme d’assistance `DisplayFor`, comme indiqué dans l’exemple suivant :

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]

2. Après l' `EnrollmentDate` champ et immédiatement avant la balise de `</dl>` de fermeture, ajoutez le code en surbrillance pour afficher une liste d’inscriptions, comme indiqué dans l’exemple suivant :

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Si la mise en retrait du code est incorrecte après le collage du code, appuyez sur **ctrl**+**K**, **CTRL**+**D** pour le mettre en forme.

    Ce code parcourt en boucle les entités dans la propriété de navigation `Enrollments`. Pour chaque `Enrollment` entité dans la propriété, elle affiche le titre du cours et le grade. Le titre du cours est extrait de l’entité `Course` stockée dans la propriété de navigation `Course` de l’entité `Enrollments`. Toutes ces données sont récupérées automatiquement à partir de la base de données lorsqu’elles sont nécessaires. En d’autres termes, vous utilisez le chargement différé ici. Vous n’avez pas spécifié le *chargement hâtif* pour la propriété de navigation `Courses`, donc les inscriptions n’ont pas été récupérées dans la même requête qui a obtenu les élèves. Au lieu de cela, la première fois que vous tentez d’accéder à la propriété de navigation `Enrollments`, une nouvelle requête est envoyée à la base de données pour récupérer les données. Plus loin dans cette série, vous pouvez en savoir plus sur le chargement différé et le chargement hâtif dans le didacticiel sur la [lecture des données associées](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .

3. Ouvrez la page de détails en démarrant le programme (**Ctrl**+**F5**), en sélectionnant l’onglet **students** , puis en cliquant sur le lien **Détails** pour Alexander Carson. (Si vous appuyez sur **Ctrl**+**F5** alors que le fichier *Details. cshtml* est ouvert, vous recevez une erreur HTTP 400. Cela est dû au fait que Visual Studio tente d’exécuter la page de détails, mais qu’il n’a pas été atteint à partir d’un lien qui spécifie le Student à afficher. Si cela se produit, supprimez « Student/details » de l’URL, puis réessayez, ou fermez le navigateur, cliquez avec le bouton droit sur le projet et cliquez sur **afficher** > **afficher dans le navigateur**.)

    La liste des cours et des notes de l’étudiant sélectionné s’affiche.

4. Fermez le navigateur.

## <a name="update-the-create-page"></a>Mettre à jour la page Create

1. Dans *Controllers\StudentController.cs*, remplacez le <xref:System.Web.Mvc.HttpPostAttribute> `Create` méthode d’action par le code suivant. Ce code ajoute un bloc `try-catch` et supprime `ID` de l’attribut <xref:System.Web.Mvc.BindAttribute> pour la méthode de génération de modèles automatique :

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Ce code ajoute l’entité `Student` créée par le Binder de modèle ASP.NET MVC au jeu d’entités `Students`, puis enregistre les modifications dans la base de données. Le *classeur de modèles* fait référence à la fonctionnalité MVC ASP.net qui vous permet d’utiliser plus facilement les données envoyées par un formulaire. un Binder de modèle convertit les valeurs de formulaire publiées en types CLR et les passe à la méthode d’action dans les paramètres. Dans ce cas, le Binder de modèle instancie une entité `Student` pour vous en utilisant les valeurs de propriété de la collection `Form`.

    Vous avez supprimé `ID` de l’attribut bind, car `ID` est la valeur de clé primaire que SQL Server définira automatiquement lorsque la ligne sera insérée. L’entrée de l’utilisateur ne définit pas la valeur `ID`.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgery-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Avertissement de sécurité : l’attribut `ValidateAntiForgeryToken` permet d’éviter les attaques [par falsification de requête intersites](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) . Elle nécessite une instruction `Html.AntiForgeryToken()` correspondante dans la vue, que vous verrez plus tard.

    L’attribut `Bind` est un moyen de se protéger contre la *survalidation dans les* scénarios de création. Par exemple, supposons que l’entité `Student` comporte une propriété `Secret` que vous ne souhaitez pas définir pour cette page Web.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Même si vous n’avez pas de champ `Secret` sur la page Web, un pirate peut utiliser un outil tel que [Fiddler](http://fiddler2.com/home)ou écrire du code JavaScript pour envoyer une valeur de formulaire `Secret`. Sans l’attribut <xref:System.Web.Mvc.BindAttribute> qui limite les champs que le Binder de modèle utilise lorsqu’il crée une instance de `Student`<em>,</em> le Binder de modèle sélectionne cette valeur de formulaire `Secret` et l’utilise pour créer l’instance d’entité `Student`. Ensuite, la valeur spécifiée par le hacker pour le champ de formulaire `Secret`, quelle qu’elle soit, est mise à jour dans la base de données. L’illustration suivante montre l’outil Fiddler ajoutant le champ `Secret` (avec la valeur « surpost ») aux valeurs de formulaire publiées.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

    La valeur « OverPost » serait correctement ajoutée à la propriété `Secret` de la ligne insérée, même si vous n’aviez jamais prévu que la page web puisse définir cette propriété.

    Il est préférable d’utiliser le paramètre `Include` avec l’attribut `Bind` pour les champs d' *autorisation* . Il est également possible d’utiliser le paramètre `Exclude` pour les champs de *liste rouge* que vous souhaitez exclure. La raison pour laquelle `Include` est plus sécurisée est que lorsque vous ajoutez une nouvelle propriété à l’entité, le nouveau champ n’est pas automatiquement protégé par une liste de `Exclude`.

    Vous pouvez empêcher la survalidation dans les scénarios de modification en lisant d’abord l’entité de la base de données, puis en appelant `TryUpdateModel`, en transmettant une liste de propriétés autorisées explicite. Il s’agit de la méthode utilisée dans ces didacticiels.

    Une autre façon d’empêcher la survalidation recommandée par de nombreux développeurs consiste à utiliser des modèles de vue plutôt que des classes d’entité avec la liaison de modèle. Incluez seulement les propriétés que vous voulez mettre à jour dans le modèle de vue. Une fois le Binder de modèle MVC terminé, copiez les propriétés du modèle de vue dans l’instance d’entité, en utilisant éventuellement un outil tel que [AutoMapper](http://automapper.org/). Utilisez DB. Entrée sur l’instance d’entité pour définir son état sur inchangé, puis définissez la propriété ("PropertyName"). IsModified sur true pour chaque propriété d’entité incluse dans le modèle de vue. Cette méthode fonctionne à la fois dans les scénarios de modification et de création.

    À part l’attribut `Bind`, le bloc `try-catch` est la seule modification que vous avez apportée au code généré automatiquement. Si une exception qui dérive de <xref:System.Data.DataException> est interceptée lors de l’enregistrement des modifications, un message d’erreur générique est affiché. Les exceptions <xref:System.Data.DataException> sont parfois dues à quelque chose d’externe à l’application et non pas à une erreur de programmation : il est donc conseillé à l’utilisateur de réessayer. Bien que ceci ne soit pas implémenté dans cet exemple, une application destinée à la production doit consigner l’exception. Pour plus d’informations, consultez la section **Journal pour obtenir un aperçu** de [Surveillance et télémétrie (génération d’applications Cloud du monde réel avec Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Le code dans *Views\Student\Create.cshtml* est similaire à ce que vous avez vu dans *Details. cshtml*, à la différence près que les `EditorFor` et `ValidationMessageFor` sont utilisés pour chaque champ au lieu de `DisplayFor`. Voici le code correspondant :

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create. cshtml* comprend également `@Html.AntiForgeryToken()`, qui fonctionne avec l’attribut `ValidateAntiForgeryToken` dans le contrôleur afin d’éviter les attaques par [falsification de requête intersites](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) .

    Aucune modification n’est requise dans *Create. cshtml*.

2. Exécutez la page en démarrant le programme, en sélectionnant l’onglet **students** , puis en cliquant sur **créer nouveau**.

3. Entrez les noms et une date non valide, puis cliquez sur **créer** pour afficher le message d’erreur.

    Il s’agit de la validation côté serveur que vous recevez par défaut. Dans un didacticiel ultérieur, vous verrez comment ajouter des attributs qui génèrent du code pour la validation côté client. Le code en surbrillance suivant montre le contrôle de validation de modèle dans la méthode **Create** .

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]

4. Changez la date en une valeur valide, puis cliquez sur **Create** pour voir apparaître le nouvel étudiant dans la page **Index**.

5. Fermez le navigateur.

## <a name="update-httppost-edit-method"></a>Mettre à jour la méthode de modification HttpPost

1. Remplacez le <xref:System.Web.Mvc.HttpPostAttribute> `Edit` méthode d’action par le code suivant :

   [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

   > [!NOTE]
   > Dans *Controllers\StudentController.cs*, la méthode `HttpGet Edit` (celle sans l’attribut `HttpPost`) utilise la méthode `Find` pour récupérer l’entité `Student` sélectionnée, comme vous l’avez vu dans la méthode `Details`. Vous n’avez pas besoin de modifier cette méthode.

   Ces modifications implémentent une meilleure pratique de sécurité pour empêcher la [survalidation](#overpost), l’échafaudage a généré un attribut `Bind` et ajouté l’entité créée par le Binder de modèle au jeu d’entités avec un indicateur modifié. Ce code n’est plus recommandé, car l’attribut `Bind` efface toutes les données préexistantes dans les champs non listés dans le paramètre `Include`. À l’avenir, le générateur de modèles automatique de contrôleur MVC sera mis à jour afin qu’il ne génère pas d’attributs de `Bind` pour les méthodes de modification.

   Le nouveau code lit l’entité existante et appelle <xref:System.Web.Mvc.Controller.TryUpdateModel%2A> pour mettre à jour les champs de l’entrée d’utilisateur dans les données de formulaire publiées. Le suivi automatique des modifications de Entity Framework définit l’indicateur [EntityState. Modified](<xref:System.Data.EntityState.Modified>) sur l’entité. Lorsque la méthode [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) est appelée, l’indicateur <xref:System.Data.EntityState.Modified> oblige l’Entity Framework à créer des instructions SQL pour mettre à jour la ligne de la base de données. Les [conflits d’accès concurrentiel](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) sont ignorés et toutes les colonnes de la ligne de base de données sont mises à jour, y compris celles que l’utilisateur n’a pas modifiées. (Un didacticiel ultérieur montre comment gérer les conflits d’accès concurrentiel et, si vous souhaitez uniquement que des champs individuels soient mis à jour dans la base de données, vous pouvez définir l’entité sur [EntityState. Unchanged](<xref:System.Data.EntityState.Unchanged>) et définir des champs individuels sur [EntityState. Modified](<xref:System.Data.EntityState.Modified>).)

   Pour empêcher la survalidation, les champs que vous souhaitez mettre à jour par la page de modification sont inclus dans la liste verte dans les paramètres `TryUpdateModel`. Actuellement, vous ne protégez aucun champ supplémentaire, mais le fait de répertorier les champs que vous voulez que le classeur de modèles lie garantit que si vous ajoutez ultérieurement des champs au modèle de données, ils seront automatiquement protégés jusqu’à ce que vous les ajoutiez explicitement ici.

   Suite à ces modifications, la signature de méthode de la méthode d’édition HttpPost est la même que la méthode d’édition HttpGet ; par conséquent, vous avez renommé la méthode EditPost.

   > [!TIP]
   >
   > **États d’entité et méthodes Attach et SaveChanges**
   >
   > Le contexte de base de données effectue le suivi de la synchronisation ou non des entités en mémoire avec leurs lignes correspondantes dans la base de données, et ces informations déterminent ce qui se passe quand vous appelez la méthode `SaveChanges`. Par exemple, lorsque vous transmettez une nouvelle entité à la méthode [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) , l’état de cette entité est défini sur `Added`. Ensuite, lorsque vous appelez la méthode [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , le contexte de base de données émet une commande SQL `INSERT`.
   >
   > Une entité peut être dans l’un des [États](xref:System.Data.EntityState)suivants :
   >
   > - `Added`. L’entité n’existe pas encore dans la base de données. La méthode `SaveChanges` doit émettre une instruction `INSERT`.
   > - `Unchanged`. La méthode `SaveChanges` ne doit rien faire avec cette entité. Quand vous lisez une entité dans la base de données, l’entité a d’abord cet état.
   > - `Modified`. Tout ou partie des valeurs de propriété de l’entité ont été modifiées. La méthode `SaveChanges` doit émettre une instruction `UPDATE`.
   > - `Deleted`. L’entité a été marquée pour suppression. La méthode `SaveChanges` doit émettre une instruction `DELETE`.
   > - `Detached`. L’entité n’est pas suivie par le contexte de base de données.
   >
   > Dans une application de poste de travail, les changements d’état sont généralement définis automatiquement. Dans un type d’application de bureau, vous lisez une entité et apportez des modifications à certaines de ses valeurs de propriété. Son état passe alors automatiquement à `Modified`. Ensuite, lorsque vous appelez `SaveChanges`, le Entity Framework génère une instruction SQL `UPDATE` qui met à jour uniquement les propriétés réelles que vous avez modifiées.
   >
   > La nature déconnectée des applications Web n’autorise pas cette séquence continue. Le [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) qui lit une entité est supprimé après le rendu d’une page. Lorsque le `HttpPost` `Edit` méthode d’action est appelée, une nouvelle demande est effectuée et que vous avez une nouvelle instance de [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx). vous devez donc définir manuellement l’état de l’entité sur `Modified.` puis, lorsque vous appelez `SaveChanges`, la Entity Framework met à jour toutes les colonnes de la ligne de base de données, car le contexte n’a aucun moyen de savoir quelles propriétés vous avez modifiées.
   >
   > Si vous souhaitez que l’instruction SQL `Update` ne met à jour que les champs modifiés par l’utilisateur, vous pouvez enregistrer les valeurs d’origine d’une certaine façon (par exemple, les champs masqués) afin qu’ils soient disponibles lors de l’appel de la méthode `HttpPost` `Edit`. Vous pouvez ensuite créer une entité `Student` à l’aide des valeurs d’origine, appeler la méthode `Attach` avec cette version d’origine de l’entité, mettre à jour les valeurs de l’entité avec les nouvelles valeurs, puis appeler `SaveChanges.` pour plus d’informations, consultez [États d’entité et SaveChanges](/ef/ef6/saving/change-tracking/entity-state) et [données locales](/ef/ef6/querying/local-data).

   Le code HTML et Razor dans *Views\Student\Edit.cshtml* est similaire à ce que vous avez vu dans *Create. cshtml*et aucune modification n’est requise.

2. Exécutez la page en démarrant le programme, en sélectionnant l’onglet **students** , puis en cliquant sur un lien hypertexte **Edit** .

3. Changez quelques données et cliquez sur **Save**. Les données modifiées s’affichent dans la page index.

4. Fermez le navigateur.

## <a name="update-the-delete-page"></a>Mettre à jour la page Delete

Dans *Controllers\StudentController.cs*, le code de modèle pour la méthode <xref:System.Web.Mvc.HttpGetAttribute> `Delete` utilise la méthode `Find` pour récupérer l’entité `Student` sélectionnée, comme vous l’avez vu dans les méthodes `Details` et `Edit`. Cependant, pour implémenter un message d’erreur personnalisé quand l’appel à `SaveChanges` échoue, vous devez ajouter des fonctionnalités à cette méthode et à sa vue correspondante.

Comme vous l’avez vu pour les opérations de mise à jour et de création, les opérations de suppression nécessitent deux méthodes d’action. La méthode qui est appelée en réponse à une demande d’extraction affiche une vue qui donne à l’utilisateur la possibilité d’approuver ou d’annuler l’opération de suppression. Si l’utilisateur l’approuve, une demande POST est créée. Lorsque cela se produit, la méthode `Delete` `HttpPost` est appelée, puis cette méthode exécute l’opération de suppression.

Vous ajouterez un bloc de `try-catch` à la méthode <xref:System.Web.Mvc.HttpPostAttribute> `Delete` pour gérer les erreurs qui peuvent se produire lorsque la base de données est mise à jour. Si une erreur se produit, le <xref:System.Web.Mvc.HttpPostAttribute> `Delete` méthode appelle la méthode `Delete` <xref:System.Web.Mvc.HttpGetAttribute>, en lui passant un paramètre qui indique qu’une erreur s’est produite. La méthode <xref:System.Web.Mvc.HttpGetAttribute> `Delete` réaffiche ensuite la page de confirmation avec le message d’erreur, ce qui donne à l’utilisateur la possibilité d’annuler ou de réessayer.

1. Remplacez le <xref:System.Web.Mvc.HttpGetAttribute> `Delete` méthode d’action par le code suivant, qui gère le rapport d’erreurs :

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Ce code accepte un [paramètre facultatif](https://msdn.microsoft.com/library/dd264739.aspx) qui indique si la méthode a été appelée après l’échec de l’enregistrement des modifications. Ce paramètre est `false` lorsque l' `HttpGet` `Delete` méthode est appelée sans échec précédent. Lorsqu’elle est appelée par la méthode `HttpPost` `Delete` en réponse à une erreur de mise à jour de la base de données, le paramètre est `true` et un message d’erreur est passé à la vue.

2. Remplacez le <xref:System.Web.Mvc.HttpPostAttribute> `Delete` méthode d’action (nommée `DeleteConfirmed`) par le code suivant, qui exécute l’opération de suppression réelle et intercepte les erreurs de mise à jour de la base de données.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Ce code récupère l’entité sélectionnée, puis appelle la méthode [Remove](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) pour définir l’état de l’entité sur `Deleted`. Lorsque `SaveChanges` est appelée, une commande SQL `DELETE` est générée. Vous avez également changé le nom de la méthode d’action de `DeleteConfirmed` en `Delete`. Le code de génération de modèles automatique nommé `HttpPost` méthode `Delete` `DeleteConfirmed` pour attribuer une signature unique à la méthode `HttpPost`. (Le CLR exige que les méthodes surchargées aient des paramètres de méthode différents.) Maintenant que les signatures sont uniques, vous pouvez respecter la convention MVC et utiliser le même nom pour les `HttpPost` et `HttpGet` les méthodes Delete.

    Si l’amélioration des performances dans une application de volume élevé est une priorité, vous pouvez éviter une requête SQL inutile pour récupérer la ligne en remplaçant les lignes de code qui appellent les méthodes `Find` et `Remove` par le code suivant :

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Ce code instancie une entité `Student` en utilisant uniquement la valeur de clé primaire, puis définit l’état de l’entité sur `Deleted`. C’est tout ce dont a besoin Entity Framework pour pouvoir supprimer l’entité.

    Comme indiqué, la méthode `HttpGet` `Delete` ne supprime pas les données. L’exécution d’une opération de suppression en réponse à une demande d’extraction (ou, pour cette question, l’exécution d’une opération de modification, d’une opération de création ou toute autre opération qui modifie des données) crée un risque de sécurité. Pour plus d’informations, consultez [#46 Conseil MVC ASP.net — n’utilisez pas les liens de suppression, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) sur le blog de Stephen Walther.

3. Dans *Views\Student\Delete.cshtml*, ajoutez un message d’erreur entre l’en-tête `h2` et l’en-tête `h3`, comme indiqué dans l’exemple suivant :

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

4. Exécutez la page en démarrant le programme, en sélectionnant l’onglet **students** , puis en cliquant sur un lien hypertexte **Delete** .

5. Choisissez **supprimer** dans la page qui indique que **vous voulez vraiment supprimer ce message ?** .

    La page index s’affiche sans l’étudiant supprimé. (Vous verrez un exemple de code de gestion des erreurs dans action dans le didacticiel sur la [concurrence](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="close-database-connections"></a>Fermer les connexions de base de données

Pour fermer les connexions de base de données et libérer les ressources qu’elles détiennent le plus rapidement possible, supprimez l’instance de contexte lorsque vous en avez terminé avec celle-ci. C’est la raison pour laquelle le code de génération de modèles automatique fournit une méthode [dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) à la fin de la classe `StudentController` dans *StudentController.cs*, comme illustré dans l’exemple suivant :

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

La classe de `Controller` de base implémente déjà l’interface `IDisposable`. ce code ajoute donc simplement une substitution à la méthode `Dispose(bool)` pour supprimer explicitement l’instance de contexte.

## <a name="handle-transactions"></a>Gérer les transactions

Par défaut, Entity Framework implémente implicitement les transactions. Dans les scénarios où vous apportez des modifications à plusieurs lignes ou tables, puis appelez `SaveChanges`, le Entity Framework s’assure automatiquement que toutes vos modifications ont réussi ou échoué. Si certaines modifications sont effectuées en premier puis qu’une erreur se produit, ces modifications sont automatiquement annulées. Pour les scénarios où vous avez besoin de davantage de contrôle&mdash;par exemple, si vous souhaitez inclure des opérations effectuées en dehors de Entity Framework dans une transaction&mdash;consultez [utilisation des transactions](/ef/ef6/saving/transactions).

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Vous disposez maintenant d’un ensemble complet de pages qui effectuent des opérations CRUD simples pour les entités `Student`. Vous avez utilisé les applications d’assistance MVC pour générer des éléments d’interface utilisateur pour les champs de données. Pour plus d’informations sur les applications d’assistance MVC, consultez [rendu d’un formulaire à l’aide des applications auxiliaires html](/previous-versions/aspnet/dd410596(v=vs.98)) (l’article s’applique à MVC 3 mais est toujours pertinent pour MVC 5).

Des liens vers d’autres ressources EF 6 se trouvent dans [ASP.NET Data Access-Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Création d’une page de détails
> * Mettre à jour la page Create
> * Mise à jour de la méthode de modification HttpPost
> * Page Delete mise à jour
> * Fermer les connexions de base de données
> * Transactions gérées

Passez à l’article suivant pour apprendre à ajouter le tri, le filtrage et la pagination au projet.
> [!div class="nextstepaction"]
> [Tri, filtrage et pagination](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
