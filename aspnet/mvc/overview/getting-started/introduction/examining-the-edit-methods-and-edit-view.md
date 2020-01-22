---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Examen des méthodes de modification et de la vue Edit | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/06/2019
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 946c88d2b337e3bf634f815c7f1ce045f29d9d84
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76518740"
---
# <a name="examining-the-edit-methods-and-edit-view"></a>Examen des méthodes de modification et de la vue de modification

par [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

Dans cette section, vous allez examiner les méthodes et les vues d’action générées par le `Edit` pour le contrôleur de films. Mais tout d’abord, nous allons utiliser une version abrégée pour améliorer l’apparence de la date de publication. Ouvrez le fichier *Models\Movie.cs* et ajoutez les lignes en surbrillance, comme indiqué ci-dessous :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Vous pouvez également définir la culture de date comme suit :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Nous aborderons [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dans le prochain didacticiel. L’attribut [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) spécifie les éléments à afficher pour le nom d’un champ (dans le cas présent, « Release Date » au lieu de « ReleaseDate »). L’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) spécifie le type des données. dans le cas présent, il s’agit d’une date, l’information d’heure stockée dans le champ n’est donc pas affichée. L’attribut [displayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) est nécessaire pour un bogue dans le navigateur Chrome qui restitue des formats de date de manière incorrecte.

Exécutez l’application et accédez au contrôleur `Movies`. Maintenez le pointeur de la souris sur un lien **modifier** pour afficher l’URL à laquelle il est lié.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Le lien **Edit** a été généré par la méthode `Html.ActionLink` dans la vue *Views\Movies\Index.cshtml* :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

L’objet `Html` est une application d’assistance exposée à l’aide d’une propriété sur la classe de base [System. Web. Mvc. WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) . La méthode `ActionLink` de l’application auxiliaire facilite la génération dynamique de liens hypertexte HTML liés aux méthodes d’action sur les contrôleurs. Le premier argument de la méthode `ActionLink` est le texte du lien à afficher (par exemple, `<a>Edit Me</a>`). Le deuxième argument est le nom de la méthode d’action à appeler (dans ce cas, l’action `Edit`). Le dernier argument est un [objet anonyme](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) qui génère les données d’itinéraire (dans le cas présent, l’ID 4).

Le lien généré affiché dans l’image précédente est `http://localhost:1234/Movies/Edit/4`. L’itinéraire par défaut (défini dans l' *application\_Start\RouteConfig.cs*) prend le modèle d’URL `{controller}/{action}/{id}`. Par conséquent, ASP.NET convertit `http://localhost:1234/Movies/Edit/4` en une demande à la méthode d’action `Edit` du contrôleur de `Movies` avec le paramètre `ID` égal à 4. Examinez le code suivant à partir du fichier *App\_Start\RouteConfig.cs* . La méthode [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) est utilisée pour acheminer les requêtes http vers le contrôleur et la méthode d’action appropriés et fournir le paramètre d’ID facultatif. La méthode [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) est également utilisée par les [HtmlHelper](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) , par exemple `ActionLink` pour générer des URL en fonction du contrôleur, de la méthode d’action et de toutes les données d’itinéraire.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Vous pouvez également passer des paramètres de méthode d’action à l’aide d’une chaîne de requête. Par exemple, l’URL `http://localhost:1234/Movies/Edit?ID=3` passe également le paramètre `ID` de 3 à l' `Edit` méthode d’action du contrôleur de `Movies`.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Ouvrez le contrôleur `Movies`. Les deux méthodes d’action `Edit` sont présentées ci-dessous.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Notez que la deuxième méthode d’action `Edit` est précédée de l’attribut `HttpPost`. Cet attribut spécifie que la surcharge de la méthode `Edit` peut être appelée uniquement pour les requêtes de publication. Vous pouvez appliquer l’attribut `HttpGet` à la première méthode Edit, mais cela n’est pas nécessaire, car il s’agit de la valeur par défaut. (Nous reviendrons sur les méthodes d’action qui reçoivent implicitement l’attribut `HttpGet` en tant que méthodes `HttpGet`.) L’attribut [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) est un autre mécanisme de sécurité important qui empêche les pirates de dépublier des données sur votre modèle. Vous devez inclure uniquement les propriétés de l’attribut de liaison que vous souhaitez modifier. Vous pouvez en savoir plus sur la survalidation et l’attribut de liaison dans mon [note de sécurité de survalidation](https://go.microsoft.com/fwlink/?LinkId=317598). Dans le modèle simple utilisé dans ce didacticiel, nous allons lier toutes les données du modèle. L’attribut [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) est utilisé pour empêcher la falsification d’une demande et est associé à `@Html.AntiForgeryToken()` dans le fichier de vue de modification (*Views\Movies\Edit.cshtml*), une partie est indiquée ci-dessous :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` génère un jeton anti-contrefaçon de formulaire masqué qui doit correspondre à la méthode `Edit` du contrôleur de `Movies`. Pour plus d’informations sur la falsification de requête intersites (également appelée XSRF ou CSRF), consultez le didacticiel [XSRF/CSRF Prevention dans MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

La méthode `HttpGet` `Edit` prend le paramètre Movie ID, recherche le film à l’aide de la méthode `Find` Entity Framework et retourne le film sélectionné à la vue Edit. Si un film est introuvable, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) est retourné. Quand le système de génération de modèles automatique a créé la vue Edit, il a examiné la classe `Movie` et a créé le code pour restituer les éléments `<label>` et `<input>` de chaque propriété de la classe. L’exemple suivant illustre la vue Edit qui a été générée par le système de génération de modèles automatique de Visual Studio :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Notez que le modèle de vue a une instruction `@model MvcMovie.Models.Movie` en haut du fichier, ce qui indique que la vue s’attend à ce que le modèle du modèle de vue soit de type `Movie`.

Le code de génération de modèles automatique utilise plusieurs *méthodes d’assistance* pour simplifier le balisage HTML. Le [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Helper affiche le nom du champ (&quot;titre&quot;, &quot;version finale&quot;, &quot;genre&quot;ou &quot;cours&quot;). Le programme d’assistance [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) restitue un élément HTML `<input>`. Le programme d’assistance [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) affiche tous les messages de validation associés à cette propriété.

Exécutez l’application et accédez à l’URL */movies* . Cliquez sur un lien **Edit**. Dans le navigateur, affichez la source de la page. Le code HTML de l’élément Form est indiqué ci-dessous.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

Les éléments de `<input>` se trouvent dans un élément de `<form>` HTML dont l’attribut `action` est défini pour une publication sur l’URL */movies/Edit* . Les données du formulaire sont publiées sur le serveur lorsque l’utilisateur clique sur le bouton **Enregistrer** . La deuxième ligne affiche le jeton [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) masqué généré par l’appel `@Html.AntiForgeryToken()`.

## <a name="processing-the-post-request"></a>Traitement de la requête POST

Le code suivant montre la version `HttpPost` de la méthode d’action `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

L’attribut [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) valide le jeton [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) généré par l’appel `@Html.AntiForgeryToken()` dans la vue.

Le [Binder de modèle ASP.NET MVC](https://msdn.microsoft.com/library/dd410405.aspx) prend les valeurs de formulaire publiées et crée un objet `Movie` passé comme paramètre `movie`. Le `ModelState.IsValid` vérifie que les données envoyées dans le formulaire peuvent être utilisées pour modifier (modifier ou mettre à jour) un objet `Movie`. Si les données sont valides, les données de film sont enregistrées dans la collection `Movies` du `db`(`MovieDBContext` instance). Les nouvelles données de film sont enregistrées dans la base de données en appelant la méthode `SaveChanges` de `MovieDBContext`. Après avoir enregistré les données, le code redirige l’utilisateur vers la méthode d’action `Index` de la classe `MoviesController`, qui affiche la collection de films, avec notamment les modifications qui viennent d’être apportées.

Dès que la validation côté client détermine que la valeur d’un champ n’est pas valide, un message d’erreur s’affiche. Si JavaScript est désactivé, la validation côté client est désactivée. Toutefois, le serveur détecte que les valeurs publiées ne sont pas valides et les valeurs de formulaire sont réaffichées avec des messages d’erreur.

La validation est examinée plus en détail plus loin dans le didacticiel.

Les `Html.ValidationMessageFor` helpers dans le modèle de vue *Edit. cshtml* s’occupent de l’affichage des messages d’erreur appropriés.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Toutes les méthodes `HttpGet` suivent un modèle similaire. Ils obtiennent un objet Movie (ou une liste d’objets, dans le cas de `Index`) et transmettent le modèle à la vue. La méthode `Create` passe un objet Movie vide à la vue Create. Toutes les méthodes qui créent, modifient, suppriment ou changent d’une quelconque manière des données le font dans la surcharge `HttpPost` de la méthode. La modification des données dans une méthode HTTP obtient est un risque pour la sécurité, comme décrit dans le billet de blog entrée [ASP.net Conseil MVC #46 – ne pas utiliser les liens de suppression, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). La modification des données dans une méthode d’extraction viole également les meilleures pratiques HTTP et le modèle architectural [Rest](http://en.wikipedia.org/wiki/Representational_State_Transfer) , qui spécifie que les demandes d’extraction ne doivent pas modifier l’état de votre application. En d’autres termes, une opération GET doit être sûre, ne doit avoir aucun effet secondaire et ne doit pas modifier vos données persistantes.

## <a name="jquery-validation-for-non-english-locales"></a>validation jQuery pour les paramètres régionaux autres que l’anglais

Si vous utilisez un ordinateur US-English, vous pouvez ignorer cette section et passer au didacticiel suivant. Vous pouvez télécharger la version globale de ce didacticiel [ici](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Pour obtenir un didacticiel en deux parties sur l’internationalisation, consultez [internationalisation ASP.NET MVC 5 Nadeem](http://afana.me/post/aspnet-mvc-internationalization.aspx).

> [!NOTE]
> pour prendre en charge la validation jQuery pour les paramètres régionaux autres que l’anglais qui utilisent une virgule (&quot;,&quot;) pour une virgule décimale et des formats de date autres que l’anglais des États-Unis, vous devez inclure *global. js* et votre fichier *cultures/globaliser. cultures. js* spécifique (à partir de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) et JavaScript pour utiliser `Globalize.parseFloat`. Vous pouvez récupérer la validation jQuery non anglaise à partir de NuGet. (Ne pas installer global si vous utilisez des paramètres régionaux anglais.)

1. Dans le menu **Outils** , cliquez sur **Gestionnaire de package NuGet**, puis sur **gérer les packages NuGet pour la solution**.

    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. Dans le volet gauche, sélectionnez <strong>Parcourir *.</strong>*  (Voir l’image ci-dessous.)
3. Dans la zone d’entrée, tapez *Globalize* \*.

    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) choisissez `jQuery.Validation.Globalize`, choisissez `MvcMovie`, puis cliquez sur **installer**. Le fichier *Scripts\jquery.globalize\globalize.js* sera ajouté à votre projet. Le dossier * Scripts\jquery.globalize\cultures\* contiendra de nombreux fichiers JavaScript de culture. Notez que l’installation de ce package peut prendre cinq minutes.

   Le code suivant montre les modifications apportées au fichier Views\Movies\Edit.cshtml :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Pour éviter de répéter ce code dans chaque vue d’édition, vous pouvez le déplacer dans le fichier de disposition. Pour optimiser le téléchargement du script, consultez mon didacticiel [et la minimisation](../../performance/bundling-and-minification.md).

Pour plus d’informations, consultez [internationalisation ASP.NET MVC 3](http://afana.me/post/aspnet-mvc-internationalization.aspx) et [ASP.NET MVC 3 internationalisation-partie 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

En guise de correctif temporaire, si la validation ne fonctionne pas dans vos paramètres régionaux, vous pouvez forcer votre ordinateur à utiliser l’anglais des États-Unis ou vous pouvez désactiver JavaScript dans votre navigateur. Pour forcer votre ordinateur à utiliser l’anglais des États-Unis, vous pouvez ajouter l’élément Globalization au fichier *Web. config* racine des projets. Le code suivant montre l’élément Globalization dont la culture est définie sur États-Unis anglais.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a>Dans le didacticiel suivant, nous allons implémenter la fonctionnalité de recherche.

> [!div class="step-by-step"]
> [Précédent](accessing-your-models-data-from-a-controller.md)
> [Suivant](adding-search.md)
