---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Partie 5 : modifier les formulaires et les modèles | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 5 couvre les formulaires de modification et la création de modèles.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20b99cbe57b5dfa623205838a5929733a6c2d70d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559548"
---
# <a name="part-5-edit-forms-and-templating"></a>Partie 5 : modifier les formulaires et les modèles

par [Jon Galloway](https://github.com/jongalloway)

> Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.  
>   
> Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.
> 
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 5 couvre les formulaires de modification et la création de modèles.

Dans le chapitre précédent, nous avons chargé des données à partir de notre base de données et nous les affichions. Dans ce chapitre, nous allons également activer la modification des données.

## <a name="creating-the-storemanagercontroller"></a>Création du StoreManagerController

Nous allons commencer par créer un nouveau contrôleur appelé **StoreManagerController**. Pour ce contrôleur, nous allons tirer parti des fonctionnalités de génération de modèles automatique disponibles dans la mise à jour des outils ASP.NET MVC 3. Définissez les options de la boîte de dialogue Ajouter un contrôleur comme indiqué ci-dessous.

![](mvc-music-store-part-5/_static/image1.png)

Lorsque vous cliquez sur le bouton Ajouter, vous constatez que le mécanisme de génération de modèles automatique ASP.NET MVC 3 effectue une bonne quantité de travail pour vous :

- Elle crée le nouveau StoreManagerController avec une variable Entity Framework locale
- Il ajoute un dossier StoreManager au dossier views du projet
- Elle ajoute les vues Create. cshtml, DELETE. cshtml, Details. cshtml, Edit. cshtml et index. cshtml, fortement typées dans la classe album.

![](mvc-music-store-part-5/_static/image2.png)

La nouvelle classe de contrôleur StoreManager comprend des actions de contrôleur CRUD (créer, lire, mettre à jour, supprimer) qui savent comment utiliser la classe de modèle d’album et utiliser notre contexte de Entity Framework pour l’accès aux bases de données.

## <a name="modifying-a-scaffolded-view"></a>Modification d’une vue échafaudée

Il est important de se souvenir que, même si ce code a été généré pour nous, il s’agit d’un code ASP.NET MVC standard, tout comme nous avons écrit dans ce didacticiel. Il est conçu pour vous faire gagner du temps lors de l’écriture d’un code de contrôleur réutilisable et de la création manuelle des vues fortement typées, mais ce n’est pas le type de code généré que vous avez pu constater par des avertissements de manière importante dans les commentaires sur la façon dont vous ne doit pas modifier le code. Il s’agit de votre code et vous êtes censé le changer.

Commençons par une modification rapide de la vue d’index StoreManager (/Views/StoreManager/Index.cshtml). Cette vue affiche une table qui répertorie les albums dans notre magasin avec des liens modifier/détails/supprimer, et comprend les propriétés publiques de l’album. Nous allons supprimer le champ AlbumArtUrl, car il n’est pas très utile dans cet affichage. Dans &lt;section&gt; de la table de la vue de code, supprimez les éléments &lt;&gt; et &lt;TD&gt; entourant les références AlbumArtUrl, comme indiqué par les lignes en surbrillance ci-dessous :

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Le code d’affichage modifié s’affiche comme suit :

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Un premier aperçu du responsable du magasin

Exécutez maintenant l’application et accédez à/StoreManager/. Cela permet d’afficher l’index du responsable du magasin que nous venons de modifier, en affichant une liste des albums dans le magasin avec des liens pour modifier, détails et supprimer.

![](mvc-music-store-part-5/_static/image3.png)

Cliquer sur le lien modifier affiche un formulaire de modification avec les champs de l’album, y compris les listes déroulantes pour le genre et l’artiste.

![](mvc-music-store-part-5/_static/image4.png)

Cliquez sur le lien « retour à la liste » en bas, puis cliquez sur le lien détails d’un album. Cela permet d’afficher les informations détaillées d’un album individuel.

![](mvc-music-store-part-5/_static/image5.png)

Là encore, cliquez sur le lien retour à la liste, puis cliquez sur un lien supprimer. Une boîte de dialogue de confirmation s’affiche, indiquant les détails de l’album et demandant si nous sommes sûrs de le supprimer.

![](mvc-music-store-part-5/_static/image6.png)

En cliquant sur le bouton supprimer en bas, vous allez supprimer l’album et revenir à la page index, qui affiche l’album supprimé.

Nous n’avons pas terminé avec le responsable du magasin, mais nous avons le contrôleur de travail et le code de vue pour les opérations CRUD à partir desquelles démarrer.

## <a name="looking-at-the-store-manager-controller-code"></a>Examen du code du contrôleur Store Manager

Le contrôleur du responsable du magasin contient une bonne quantité de code. Passons en revue ce point de haut en bas. Le contrôleur comprend des espaces de noms standard pour un contrôleur MVC, ainsi qu’une référence à l’espace de noms de nos modèles. Le contrôleur a une instance privée de MusicStoreEntities, utilisée par chacune des actions de contrôleur pour l’accès aux données.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Actions des détails et de l’index du gestionnaire de magasin

La vue index récupère une liste d’albums, y compris les informations relatives au genre et à l’artiste référencées de chaque album, comme nous l’avons vu précédemment lors de l’utilisation de la méthode browse Store. La vue index suit les références aux objets liés afin de pouvoir afficher le nom de genre et le nom de l’artiste de chaque album, de sorte que le contrôleur est efficace et interrogeant ces informations dans la requête d’origine.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

L’action du contrôleur des détails du contrôleur StoreManager fonctionne exactement de la même façon que l’action Détails du contrôleur de magasin que nous avons écrite précédemment pour l’album par ID à l’aide de la méthode Find (), puis le renvoie à la vue.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Méthodes d’action de création

Les méthodes d’action Create sont légèrement différentes de celles que nous avons vu jusqu’à présent, car elles gèrent l’entrée de formulaire. Lorsqu’un utilisateur visite/StoreManager/Create/, il affiche un formulaire vide. Cette page HTML contient une &lt;formulaire&gt; élément qui contient des éléments d’entrée de zone de liste déroulante et de zone de texte où ils peuvent entrer les détails de l’album.

Une fois que l’utilisateur a renseigné les valeurs de formulaire de l’album, il peut appuyer sur le bouton « enregistrer » pour soumettre ces modifications à notre application afin de l’enregistrer dans la base de données. Quand l’utilisateur appuie sur le bouton « enregistrer », le formulaire de &lt;&gt; effectue une publication HTTP sur l’URL/StoreManager/Create/et envoie les valeurs de&gt; du formulaire &lt;dans le cadre de la requête HTTP-HTTP.

ASP.NET MVC nous permet de fractionner facilement la logique de ces deux scénarios d’invocation d’URL en nous permettant d’implémenter deux méthodes d’action « Create » distinctes au sein de notre classe StoreManagerController : une pour gérer l’URL HTTP-aller au/StoreManager/Create/et l’autre pour gérer le HTTP-poster des modifications soumises.

### <a name="passing-information-to-a-view-using-viewbag"></a>Passage d’informations à une vue à l’aide de ViewBag

Nous avons utilisé le ViewBag précédemment dans ce didacticiel, mais je n’en ai pas parlé. ViewBag nous permet de transmettre des informations à la vue sans utiliser d’objet de modèle fortement typé. Dans ce cas, notre action modifier le contrôleur HTTP-obtenir doit transmettre une liste de genres et d’artistes au formulaire pour remplir les listes déroulantes, et le moyen le plus simple de le faire consiste à les retourner en tant qu’éléments ViewBag.

ViewBag est un objet dynamique, ce qui signifie que vous pouvez taper ViewBag. foo ou ViewBag. YourNameHere sans écrire de code pour définir ces propriétés. Dans ce cas, le code du contrôleur utilise ViewBag. GenreId et ViewBag. ArtistId afin que les valeurs de liste déroulante soumises avec le formulaire soient GenreId et ArtistId, qui sont les propriétés de l’album qui seront configurées.

Ces valeurs de liste déroulante sont retournées au formulaire à l’aide de l’objet SelectList, qui est généré uniquement à cet effet. Pour ce faire, utilisez un code similaire à celui-ci :

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Comme vous pouvez le voir dans le code de la méthode d’action, trois paramètres sont utilisés pour créer cet objet :

- Liste des éléments que la liste déroulante affichera. Notez qu’il ne s’agit pas simplement d’une chaîne. nous passons une liste de genres.
- Le paramètre suivant passé à SelectList est la valeur sélectionnée. Cela explique comment le SelectList sait comment présélectionner un élément dans la liste. Cela sera plus facile à comprendre lorsque nous examinerons le formulaire de modification, ce qui est assez similaire.
- Le dernier paramètre est la propriété à afficher. Dans ce cas, cela indique que la propriété Genre.Name est celle qui sera affichée à l’utilisateur.

Dans ce souci, l’action de création HTTP-is est assez simple-deux SelectLists sont ajoutés au ViewBag, et aucun objet de modèle n’est transmis au formulaire (car il n’a pas encore été créé).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Applications auxiliaires HTML pour afficher les listes déroulantes dans la vue Create

Étant donné que nous avons parlé de la façon dont les valeurs de liste déroulante sont passées à la vue, examinons rapidement la vue pour voir comment ces valeurs sont affichées. Dans le code de vue (/Views/StoreManager/Create.cshtml), vous verrez que l’appel suivant est effectué pour afficher la liste déroulante de genres.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

C’est ce qu’on appelle un programme d’assistance HTML, une méthode utilitaire qui effectue une tâche d’affichage courante. Les applications auxiliaires HTML sont très utiles pour maintenir le code de vue concis et lisible. Le programme d’assistance HTML. DropDownList est fourni par ASP.NET MVC, mais comme nous le verrons ultérieurement, il est possible de créer nos propres aide pour le code de vue que nous allons réutiliser dans notre application.

L’appel HTML. DropDownList doit simplement être indiqué deux choses : où obtenir la liste à afficher et la valeur (le cas échéant) doit être présélectionnée. Le premier paramètre, GenreId, indique à DropDownList de rechercher une valeur nommée GenreId dans le modèle ou ViewBag. Le deuxième paramètre est utilisé pour indiquer la valeur à afficher comme étant initialement sélectionné dans la liste déroulante. Dans la mesure où ce formulaire est un formulaire de création, il n’y a aucune valeur à présélectionner et String. Empty est passé.

### <a name="handling-the-posted-form-values"></a>Gestion des valeurs de formulaire publiées

Comme nous l’avons vu précédemment, il existe deux méthodes d’action associées à chaque formulaire. La première gère la requête HTTP-obtenir et affiche le formulaire. La seconde gère la requête HTTP-POSTCONNEXION, qui contient les valeurs de formulaire soumises. Notez que l’action du contrôleur possède un attribut [HttpPost], qui indique à ASP.NET MVC qu’il doit répondre uniquement aux requêtes HTTP-POSTCONNEXION.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Cette action a quatre responsabilités :

- 1. Lire les valeurs de formulaire
- 2. Vérifier si les valeurs de formulaire passent des règles de validation
- 3. Si l’envoi du formulaire est valide, enregistrez les données et affichez la liste mise à jour.
- 4. Si l’envoi du formulaire n’est pas valide, réaffichez le formulaire avec des erreurs de validation

#### <a name="reading-form-values-with-model-binding"></a>Lecture des valeurs de formulaire avec la liaison de modèle

L’action du contrôleur traite une soumission de formulaire qui comprend des valeurs pour GenreId et ArtistId (dans la liste déroulante) et des valeurs de zone de texte pour titre, Price et AlbumArtUrl. Bien qu’il soit possible d’accéder directement aux valeurs de formulaire, une meilleure approche consiste à utiliser les fonctionnalités de liaison de modèle intégrées à ASP.NET MVC. Lorsqu’une action de contrôleur accepte un type de modèle en tant que paramètre, ASP.NET MVC tente de remplir un objet de ce type à l’aide des entrées de formulaire (ainsi que des valeurs de routage et QueryString). Pour ce faire, il recherche des valeurs dont les noms correspondent aux propriétés de l’objet de modèle, par exemple lors de la définition de la valeur GenreId de l’objet album, il recherche une entrée portant le nom GenreId. Lorsque vous créez des vues à l’aide des méthodes standard dans ASP.NET MVC, les formulaires sont toujours rendus à l’aide de noms de propriété en tant que noms de champ d’entrée, ce qui signifie que les noms de champs correspondent simplement.

#### <a name="validating-the-model"></a>Validation du modèle

Le modèle est validé avec un simple appel à ModelState. IsValid. Nous n’avons pas encore ajouté de règles de validation à notre classe album. nous allons le faire dans un instant, à ce stade, ce contrôle n’a pas grand-chose à faire. Ce qui est important, c’est que ce contrôle ModelStat. IsValid s’adaptera aux règles de validation que nous plaçons dans notre modèle. par conséquent, les modifications ultérieures apportées aux règles de validation ne nécessitent aucune mise à jour du code d’action du contrôleur.

#### <a name="saving-the-submitted-values"></a>Enregistrement des valeurs soumises

Si l’envoi du formulaire réussit la validation, il est temps d’enregistrer les valeurs dans la base de données. Avec Entity Framework, il suffit d’ajouter le modèle à la collection d’albums et d’appeler SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework génère les commandes SQL appropriées pour rendre la valeur persistante. Une fois les données enregistrées, nous redirigeons vers la liste des albums pour voir notre mise à jour. Pour ce faire, vous devez retourner RedirectToAction avec le nom de l’action de contrôleur que nous souhaitons afficher. Dans ce cas, il s’agit de la méthode d’index.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Affichage des envois de formulaire non valides avec des erreurs de validation

Dans le cas d’une entrée de formulaire non valide, les valeurs de liste déroulante sont ajoutées au ViewBag (comme dans le cas HTTP-alobtenu) et les valeurs de modèle liées sont retournées à la vue pour l’affichage. Les erreurs de validation s’affichent automatiquement à l’aide du programme d’assistance HTML @Html.ValidationMessageFor.

#### <a name="testing-the-create-form"></a>Test du formulaire de création

Pour tester cela, exécutez l’application et accédez à/StoreManager/Create/. cette opération affiche le formulaire vide qui a été retourné par la méthode StoreController Create HTTP-obten.

Renseignez certaines valeurs et cliquez sur le bouton créer pour envoyer le formulaire.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Gestion des modifications

La paire d’actions de modification (HTTP-obten et HTTP-poster) est très similaire aux méthodes d’action de création que nous venons de regarder. Étant donné que le scénario de modification implique l’utilisation d’un album existant, la méthode Edit HTTP-obten charge l’album en fonction du paramètre « ID », passé par le biais de l’itinéraire. Ce code pour récupérer un album par AlbumId est identique à celui que nous avons précédemment consulté dans l’action Détails Controller. Comme avec la méthode Create/HTTP-obten, les valeurs de liste déroulante sont retournées via ViewBag. Cela nous permet de retourner un album en tant qu’objet de modèle à la vue (qui est fortement typée dans la classe album) tout en passant des données supplémentaires (par exemple, une liste de genres) via ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

L’action modifier le protocole HTTP-poster est très similaire à l’action créer HTTP-poster. La seule différence est qu’au lieu d’ajouter un nouvel album à la base de la base de la. Collection d’albums, nous avons trouvé l’instance actuelle de l’album à l’aide de DB. Entrée (album) et définition de son état sur modifié. Cela indique Entity Framework que nous modifions un album existant au lieu d’en créer un nouveau.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Nous pouvons tester cela en exécutant l’application et en accédant à/StoreManger/, puis en cliquant sur le lien Modifier pour un album.

![](mvc-music-store-part-5/_static/image9.png)

Cela permet d’afficher le formulaire de modification affiché par la méthode de modification HTTP-obtient. Renseignez certaines valeurs et cliquez sur le bouton enregistrer.

![](mvc-music-store-part-5/_static/image10.png)

Cette opération publie le formulaire, enregistre les valeurs et nous renvoie à la liste d’albums, indiquant que les valeurs ont été mises à jour.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Gestion de la suppression

La suppression suit le même modèle que modifier et créer, à l’aide d’une action de contrôleur pour afficher le formulaire de confirmation, et d’une autre action de contrôleur pour gérer l’envoi de formulaire.

L’action HTTP-récupérer la suppression du contrôleur est exactement la même que celle de l’action précédente du contrôleur Store Manager.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Nous affichons un formulaire fortement typé dans un type d’album, à l’aide du modèle de contenu supprimer la vue.

![](mvc-music-store-part-5/_static/image12.png)

Le modèle de suppression affiche tous les champs du modèle, mais nous pouvons le simplifier un peu plus longtemps. Modifiez le code de vue dans/Views/StoreManager/Delete.cshtml comme suit.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Cela permet d’afficher une confirmation de suppression simplifiée.

![](mvc-music-store-part-5/_static/image13.png)

Si vous cliquez sur le bouton supprimer, le formulaire est publié sur le serveur, ce qui entraîne l’exécution de l’action DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Notre action de contrôleur HTTP-après suppression effectue les actions suivantes :

- 1. Charge l’album par ID
- 2. Supprime l’album et enregistre les modifications
- 3. Redirige vers l’index, indiquant que l’album a été supprimé de la liste

Pour tester cela, exécutez l’application et accédez à/StoreManager. Sélectionnez un album dans la liste et cliquez sur le lien supprimer.

![](mvc-music-store-part-5/_static/image14.png)

L’écran de confirmation de la suppression s’affiche.

![](mvc-music-store-part-5/_static/image15.png)

Le fait de cliquer sur le bouton Supprimer permet de supprimer l’album et de revenir à la page index du responsable du magasin, qui indique que l’album a été supprimé.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Utilisation d’un programme d’assistance HTML personnalisé pour tronquer du texte

Nous avons rencontré un problème potentiel avec notre page d’index Store Manager. Les propriétés titre de l’album et nom de l’artiste peuvent être suffisamment longues pour pouvoir lever la mise en forme de la table. Nous allons créer une application auxiliaire HTML personnalisée pour nous permettre de tronquer facilement ces propriétés et d’autres dans nos vues.

![](mvc-music-store-part-5/_static/image17.png)

La syntaxe de @helper de Razor a rendu assez facile de créer vos propres fonctions d’assistance pour une utilisation dans vos vues. Ouvrez la vue/Views/StoreManager/Index.cshtml et ajoutez le code suivant directement après la ligne de @model.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Cette méthode d’assistance prend une chaîne et une longueur maximale à autoriser. Si le texte fourni est plus petit que la longueur spécifiée, le programme d’assistance le renvoie tel quel. Si elle est plus longue, elle tronque le texte et affiche « ... » pour le reste.

Nous pouvons maintenant utiliser notre application auxiliaire TRUNCATE pour vous assurer que les propriétés titre de l’album et nom de l’artiste sont inférieures à 25 caractères. Le code complet de la vue à l’aide de notre nouvelle application d’assistance TRUNCATE apparaît ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Désormais, lorsque nous parcourons l’URL/StoreManager/, les albums et les titres sont conservés en dessous de nos longueurs maximales.

![](mvc-music-store-part-5/_static/image18.png)

Remarque : cela montre le cas simple de création et d’utilisation d’une application d’assistance dans une vue. Pour en savoir plus sur la création d’applications d’assistance que vous pouvez utiliser dans votre site, consultez mon billet de blog : [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)

> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-4.md)
> [Suivant](mvc-music-store-part-6.md)
