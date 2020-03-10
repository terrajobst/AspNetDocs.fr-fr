---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Fournir une prise en charge de l’entrée de formulaire de données CRUD (créer, lire, mettre à jour, supprimer) | Microsoft Docs
author: microsoft
description: L’étape 5 montre comment reprendre la classe DinnersController en prenant en charge la modification, la création et la suppression des dîners également.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: b3123af9a1477bc496a0d229d628510fc202b6d2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580555"
---
# <a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Fournir une prise en charge des entrées dans les formulaires de données CRUD (créer, lire, mettre à jour, supprimer)

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 5 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.
> 
> L’étape 5 montre comment reprendre la classe DinnersController en prenant en charge la modification, la création et la suppression des dîners également.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner étape 5 : créer, mettre à jour, supprimer des scénarios de formulaire

Nous avons introduit des contrôleurs et des vues, et expliqué comment les utiliser pour implémenter une expérience de liste/détails pour les dîners sur site. L’étape suivante consiste à mettre à niveau notre classe DinnersController et à activer la prise en charge de la modification, de la création et de la suppression des dîners également.

### <a name="urls-handled-by-dinnerscontroller"></a>URL gérées par DinnersController

Nous avons ajouté précédemment des méthodes d’action à DinnersController qui ont implémenté la prise en charge de deux URL : */dinners* et */dinners/Details/[id]* .

| **URL** | **DOVERB** | **Fonction** |
| --- | --- | --- |
| */Dinners/* | GET | Affichez une liste HTML des dîners à venir. |
| */Dinners/Details/[id]* | GET | Affichez les détails d’un dîner spécifique. |

Nous allons maintenant ajouter des méthodes d’action pour implémenter trois URL supplémentaires : */dinners/Edit/[id]* , */dinners/Create*et */dinners/delete/[id]* . Ces URL permettront la prise en charge de la modification des dîners existants, de la création de nouveaux dîners et de la suppression des dîners.

Nous allons prendre en charge les interactions de verbe http et http après ces nouvelles URL. Les demandes HTTP d’accès à ces URL affichent la vue HTML initiale des données (un formulaire rempli avec les données du dîner dans le cas de « modifier », un formulaire vide dans le cas de « créer » et un écran de confirmation de suppression dans le cas de « supprimer »). Les requêtes HTTP envoyées à ces URL vont enregistrer/mettre à jour/supprimer les données du dîner dans notre DinnerRepository (et à partir de là, dans la base de données).

| **URL** | **DOVERB** | **Fonction** |
| --- | --- | --- |
| */Dinners/Edit/[id]* | GET | Affichez un formulaire HTML modifiable rempli avec les données du dîner. |
| POST | Enregistrez les modifications apportées au formulaire pour un dîner particulier dans la base de données. |
| */Dinners/Create* | GET | Affichez un formulaire HTML vide qui permet aux utilisateurs de définir de nouveaux dîners. |
| POST | Créez un nouveau dîner et enregistrez-le dans la base de données. |
| */Dinners/Delete/[id]* | GET | Affichez l’écran de confirmation de suppression. |
| POST | Supprime le dîner spécifié de la base de données. |

### <a name="edit-support"></a>Modifier la prise en charge

Commençons par implémenter le scénario « modifier ».

#### <a name="the-http-get-edit-action-method"></a>Méthode d’action de modification HTTP-obtenue

Nous allons commencer par implémenter le comportement HTTP « récupérer » de notre méthode d’action de modification. Cette méthode est appelée lorsque l’URL */dinners/Edit/[id]* est demandée. Notre implémentation se présente comme suit :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Le code ci-dessus utilise DinnerRepository pour récupérer un objet dinner. Il restitue ensuite un modèle de vue à l’aide de l’objet dîner. Étant donné que nous n’avons pas passé explicitement un nom de modèle à la méthode d’assistance *View ()* , il utilisera le chemin d’accès par défaut basé sur la Convention pour résoudre le modèle de vue:/views/dinners/Edit.aspx.

Créons maintenant ce modèle de vue. Pour ce faire, vous devez cliquer avec le bouton droit dans la méthode Edit et sélectionner la commande de menu contextuel « Add View » (ajouter une vue) :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

Dans la boîte de dialogue « Add View » (ajouter une vue), nous indiquons que nous transmettons un objet dîner à notre modèle de vue en tant que modèle, et que vous choisissez de générer automatiquement un modèle « Edit » (modifier) :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio ajoute un nouveau fichier de modèle de vue « Edit. aspx » pour nous dans le répertoire « \Views\Dinners ». Elle ouvre également le nouveau modèle de vue « Edit. aspx » dans l’éditeur de code, renseigné avec une implémentation d’échafaudage « Edit » initiale comme ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Nous allons apporter quelques modifications à la structure de « modification » par défaut, puis mettre à jour le modèle de vue Edit pour afficher le contenu ci-dessous (ce qui supprime quelques-unes des propriétés que nous ne souhaitons pas exposer) :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Lorsque nous exécutons l’application et demandez l’URL *« /dinners/Edit/1 »* , la page suivante s’affiche :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Le balisage HTML généré par notre vue ressemble à ce qui suit. Il s’agit d’un code HTML standard, avec un &lt;formulaire&gt; élément qui exécute une requête HTTP sur l’URL */dinners/Edit/1* lorsque le bouton « enregistrer » &lt;type d’entrée = « envoyer »/&gt; est envoyé. Un élément HTML &lt;input type = "Text"/&gt; élément a été généré pour chaque propriété modifiable :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Méthodes d’assistance HTML HTML. BeginForm () et html. TextBox ()

Notre modèle de vue « Edit. aspx » utilise plusieurs méthodes « html Helper » : html. ValidationSummary (), html. BeginForm (), html. TextBox () et html. ValidationMessage (). En plus de générer un balisage HTML pour nous, ces méthodes d’assistance offrent une prise en charge intégrée de la gestion des erreurs et de la validation.

##### <a name="htmlbeginform-helper-method"></a>Méthode d’assistance HTML. BeginForm ()

La méthode d’assistance HTML. BeginForm () est celle qui génère l’élément HTML &lt;Form&gt; dans notre balisage. Dans notre modèle de vue Edit. aspx, vous remarquerez que nous C# appliquons une instruction « using » lors de l’utilisation de cette méthode. L’accolade ouvrante indique le début de la &lt;formulaire&gt; le contenu, et l’accolade fermante est ce qui indique la fin de l’élément &lt;/formulaire&gt; :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Si vous trouvez l’approche d’instruction « using » non naturelle pour un scénario de ce type, vous pouvez également utiliser une combinaison html. BeginForm () et html. EndForm () (ce qui fait la même chose) :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

L’appel de html. BeginForm () sans aucun paramètre entraîne la sortie d’un élément de formulaire qui effectue une publication HTTP sur l’URL de la requête actuelle. C’est la raison pour laquelle notre vue Edit génère une *&lt;Form action = "/dinners/Edit/1" Method = "poster"&gt;* élément. Nous aurions pu également passer des paramètres explicites à html. BeginForm () si nous voulions poster vers une URL différente.

##### <a name="htmltextbox-helper-method"></a>Méthode d’assistance HTML. TextBox ()

Notre vue Edit. aspx utilise la méthode d’assistance HTML. TextBox () pour générer &lt;type d’entrée = "Text"/&gt; éléments :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

La méthode html. TextBox () ci-dessus accepte un seul paramètre, qui est utilisé pour spécifier à la fois les attributs ID/Name du &lt;type d’entrée = "Text"/&gt; élément à la sortie, ainsi que la propriété de modèle à partir de laquelle remplir la valeur de la zone de texte. Par exemple, l’objet dîner que nous avons passé à la vue Edit avait une valeur de propriété « Title » de « .NET futures » et, par conséquent, la sortie de l’appel de la méthode html. TextBox (« title ») : *&lt;d’entrée ID = "title" Name = "title" type = "Text" value = ". net futures"/&gt;* .

Vous pouvez également utiliser le premier paramètre html. TextBox () pour spécifier l’ID/nom de l’élément, puis transmettre explicitement la valeur à utiliser comme deuxième paramètre :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Il est souvent préférable d’effectuer une mise en forme personnalisée de la valeur qui est sortie. La méthode statique String. format () intégrée à .NET est utile pour ces scénarios. Notre modèle de vue Edit. aspx utilise cette valeur pour mettre en forme la valeur EventDate (qui est de type DateTime) afin qu’il n’affiche pas les secondes de l’heure :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Un troisième paramètre de html. TextBox () peut éventuellement être utilisé pour générer des attributs HTML supplémentaires. L’extrait de code ci-dessous montre comment restituer un attribut Size = "30" supplémentaire et un attribut Class = "mycssclass" sur le &lt;type d’entrée = "Text"/&gt; élément. Notez la manière dont nous échappons le nom de l’attribut de classe à l’aide d’une « classe@" character because "» C#est un mot clé réservé dans :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implémentation de la méthode d’action HTTP-après modification

Nous avons maintenant la version HTTP-obtenir de notre méthode d’action de modification implémentée. Lorsqu’un utilisateur demande l’URL */dinners/Edit/1* , il reçoit une page html comme suit :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Le fait d’appuyer sur le bouton « enregistrer » entraîne la publication d’un formulaire sur l’URL */dinners/Edit/1* et envoie le code html &lt;les valeurs d’entrée&gt; de formulaire à l’aide du verbe http. Nous allons maintenant implémenter le comportement HTTP Après notre méthode d’action de modification, qui va gérer l’enregistrement du dîner.

Nous allons commencer par ajouter une méthode d’action « Edit » surchargée à notre DinnersController avec un attribut « AcceptVerbs » qui indique qu’elle gère les scénarios HTTP HTTP :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Lorsque l’attribut [AcceptVerbs] est appliqué aux méthodes d’action surchargées, ASP.NET MVC gère automatiquement la distribution des requêtes à la méthode d’action appropriée en fonction du verbe HTTP entrant. Les requêtes HTTP HTTP envoyées à */dinners/Edit/[id]* URL seront dirigées vers la méthode Edit ci-dessus, tandis que toutes les autres requêtes de verbe http envoyées aux URL */dinners/Edit/[id]* seront dirigées vers la première méthode Edit implémentée (qui n’avait pas d’attribut `[AcceptVerbs]`).

| **Rubrique latérale : pourquoi distinguer par le biais des verbes HTTP ?** |
| --- |
| Vous vous demandez peut-être pourquoi utiliser une URL unique et différencier son comportement via le verbe HTTP ? Pourquoi ne pas simplement avoir deux URL distinctes pour gérer le chargement et l’enregistrement des modifications de modification ? Par exemple:/Dinners/Edit/[id] pour afficher le formulaire initial et/Dinners/Save/[id] pour gérer la publication de formulaire et l’enregistrer ? L’inconvénient de la publication de deux URL distinctes est que dans les cas où nous publions vers/Dinners/Save/2, puis que vous deviez Réafficher le formulaire HTML en raison d’une erreur d’entrée, l’utilisateur final finit par avoir l’URL/Dinners/Save/2 dans la barre d’adresse du navigateur (puisque c’était l’URL vers laquelle le formulaire a été publié). Si l’utilisateur final insère cette page réaffichée dans la liste des favoris de son navigateur, ou copie/colle l’URL et l’envoie par courrier électronique à un ami, elle finit par enregistrer une URL qui ne fonctionnera pas dans le futur (puisque cette URL dépend des valeurs de publication). En exposant une URL unique (par exemple,/Dinners/Edit/[id]) et en différenciant son traitement par le verbe HTTP, il est possible pour les utilisateurs finaux de créer un signet pour la page de modification et/ou d’envoyer l’URL à d’autres utilisateurs. |

#### <a name="retrieving-form-post-values"></a>Récupération de valeurs de publication de formulaire

Il existe plusieurs façons d’accéder aux paramètres de formulaire publiés dans notre méthode HTTP POST « Edit ». Une approche simple consiste à utiliser simplement la propriété Request sur la classe de base Controller pour accéder à la collection de formulaires et récupérer directement les valeurs publiées :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

L’approche ci-dessus est un peu plus détaillée, mais surtout une fois que nous ajoutons une logique de gestion des erreurs.

Une meilleure approche pour ce scénario consiste à tirer parti de la méthode d’assistance *UpdateModel ()* intégrée sur la classe de base du contrôleur. Il prend en charge la mise à jour des propriétés d’un objet que nous passons à l’aide des paramètres de formulaire entrants. Elle utilise la réflexion pour déterminer les noms de propriété sur l’objet, puis les convertit automatiquement et leur assigne des valeurs en fonction des valeurs d’entrée soumises par le client.

Nous pourrions utiliser la méthode UpdateModel () pour simplifier notre action HTTP-après modification à l’aide de ce code :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Nous pouvons maintenant accéder à l’URL */dinners/Edit/1* et modifier le titre de notre dîner :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Lorsque nous cliquons sur le bouton « enregistrer », nous allons effectuer une publication de formulaire sur notre action de modification, et les valeurs mises à jour sont conservées dans la base de données. Nous serez ensuite redirigé vers l’URL des détails du dîner (qui affichera les valeurs nouvellement enregistrées) :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Gestion des erreurs de modification

Notre implémentation HTTP-poster actuelle fonctionne bien, sauf en cas d’erreur.

Lorsqu’un utilisateur fait une erreur en modifiant un formulaire, nous devons nous assurer que le formulaire est réaffiché avec un message d’erreur informatif qui l’aide à le résoudre. Cela inclut les cas où un utilisateur final publie une entrée incorrecte (par exemple, une chaîne de date incorrecte), ainsi que des cas où le format d’entrée est valide, mais qu’il existe une violation de règle d’entreprise. Lorsque des erreurs se produisent, le formulaire doit conserver les données d’entrée que l’utilisateur a entrées à l’origine afin qu’il n’ait pas à recharger manuellement les modifications. Ce processus doit être répété autant de fois que nécessaire jusqu’à ce que le formulaire se termine correctement.

ASP.NET MVC intègre des fonctionnalités intéressantes qui facilitent la gestion des erreurs et le réaffichage des formulaires. Pour afficher ces fonctionnalités en action, nous allons mettre à jour notre méthode d’action de modification avec le code suivant :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Le code ci-dessus est similaire à notre implémentation précédente, sauf que nous envoyons maintenant un bloc de gestion des erreurs Try/Catch autour de notre travail. Si une exception se produit lors de l’appel de UpdateModel (), ou lorsque vous essayez d’enregistrer le DinnerRepository (qui lève une exception si l’objet Dinner que nous tentons d’enregistrer n’est pas valide en raison d’une violation de règle dans notre modèle), notre bloc de gestion des erreurs catch effectue. Au sein de celui-ci, nous faisons une boucle sur les violations de règle qui existent dans l’objet Dinner et les ajoutons à un objet ModelState (que nous aborderons bientôt). Nous affichons ensuite la vue.

Pour voir comment cela fonctionne, réexécutez l’application, modifiez un dîner, modifiez-le pour qu’il ait un titre vide, un EventDate de « faux » et utilisez un numéro de téléphone au Royaume-Uni avec une valeur de pays américaine. Lorsque vous appuyez sur le bouton « enregistrer », notre méthode HTTP après modification ne pourra pas enregistrer le dîner (en raison d’erreurs) et affichera le formulaire :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Notre application a une expérience d’erreur correcte. Les éléments de texte avec l’entrée non valide sont mis en surbrillance en rouge et les messages d’erreur de validation sont affichés à l’utilisateur final. Le formulaire conserve également les données d’entrée que l’utilisateur a entrées à l’origine, de sorte qu’il n’est pas nécessaire de reremplir quoi que ce soit.

Que se passe-t-il ? Comment les zones de texte Title, EventDate et ContactPhone se mettent-elles en surbrillance en rouge et savent-elles en sortie les valeurs utilisateur initialement entrées ? Comment les messages d’erreur s’affichent-ils dans la liste en haut ? La bonne nouvelle, c’est que cela n’a pas été le cas par magie, car nous avons utilisé certaines des fonctionnalités ASP.NET MVC intégrées qui facilitent la validation des entrées et les scénarios de gestion des erreurs.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Fonctionnement de ModelState et des méthodes d’assistance HTML de validation

Les classes de contrôleur ont une collection de propriétés « ModelState » qui fournit un moyen d’indiquer que des erreurs existent avec un objet de modèle passé à une vue. Les entrées d’erreur dans la collection ModelState identifient le nom de la propriété de modèle avec le problème (par exemple, « title », « EventDate » ou « ContactPhone ») et autorisent la spécification d’un message d’erreur convivial (par exemple : « title is required »).

La méthode d’assistance *UpdateModel ()* remplit automatiquement la collection ModelState lorsqu’elle rencontre des erreurs lors de la tentative d’assigner des valeurs de formulaire aux propriétés de l’objet de modèle. Par exemple, la propriété EventDate de notre objet Dinner est de type DateTime. Lorsque la méthode UpdateModel () n’a pas pu assigner la valeur de chaîne « factice » dans le scénario ci-dessus, la méthode UpdateModel () a ajouté une entrée à la collection ModelState indiquant qu’une erreur d’assignation s’est produite avec cette propriété.

Les développeurs peuvent également écrire du code pour ajouter explicitement des entrées d’erreur dans la collection ModelState comme nous le faisons ci-dessous dans notre bloc de gestion des erreurs « catch », qui remplit la collection ModelState avec des entrées basées sur les violations de règle actives dans le Objet dîner :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Intégration de l’assistance HTML avec ModelState

Méthodes d’assistance HTML, telles que html. TextBox ()-Vérifiez la collection ModelState lors du rendu de la sortie. Si une erreur existe pour l’élément, elles restituent la valeur entrée par l’utilisateur et une classe d’erreur CSS.

Par exemple, dans notre vue « Edit », nous utilisons la méthode d’assistance HTML. TextBox () pour afficher le EventDate de notre objet dîner :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Lorsque la vue a été rendue dans le scénario d’erreur, la méthode html. TextBox () a vérifié la collection ModelState pour voir si des erreurs ont été associées à la propriété « EventDate » de notre objet dinner. Lorsqu’il a déterminé qu’une erreur s’est produite, il a rendu l’entrée utilisateur envoyée (« BOGUe ») comme valeur, et a ajouté une classe d’erreur CSS au &lt;type d’entrée = « TextBox »/&gt; balisage qu’il a généré :

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Vous pouvez personnaliser l’apparence de la classe d’erreur CSS de votre choix. La classe d’erreur CSS par défaut – « entrée-validation-erreur », est définie dans la feuille de style *\content\site.CSS* et ressemble à ceci :

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Cette règle CSS est ce qui a provoqué la mise en surbrillance de nos éléments d’entrée non valides comme ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Méthode d’assistance HTML. ValidationMessage ()

La méthode d’assistance HTML. ValidationMessage () peut être utilisée pour générer le message d’erreur ModelState associé à une propriété de modèle particulière :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Le code ci-dessus génère les sorties suivantes : *&lt;span class = "Field-validation-Error"&gt; la valeur’incorrecte’n’est pas valide&lt;/span&gt;*

La méthode d’assistance HTML. ValidationMessage () prend également en charge un deuxième paramètre qui permet aux développeurs de remplacer le message d’erreur affiché :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Le code ci-dessus génère : *&lt;span class = "Field-validation-Error"&gt;\*&lt;/span&gt;* au lieu du texte d’erreur par défaut lorsqu’une erreur est présente pour la propriété EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Méthode d’assistance HTML. ValidationSummary ()

La méthode d’assistance HTML. ValidationSummary () peut être utilisée pour afficher un message d’erreur résumé, accompagné d’un &lt;UL&gt;&lt;Li/&gt;&lt;/UL&gt; liste de tous les messages d’erreur détaillés dans la collection ModelState :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

La méthode d’assistance HTML. ValidationSummary () prend un paramètre de chaîne facultatif, qui définit un message d’erreur Résumé à afficher au-dessus de la liste des erreurs détaillées :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Vous pouvez éventuellement utiliser CSS pour remplacer l’aspect de la liste d’erreurs.

#### <a name="using-a-addruleviolations-helper-method"></a>Utilisation d’une méthode d’assistance AddRuleViolations

Notre implémentation initiale de la modification HTTP-Après a utilisé une instruction foreach dans son bloc catch pour effectuer une boucle sur les violations de règle de l’objet Dinner et les ajouter à la collection ModelState du contrôleur :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Nous pouvons rendre ce code un peu plus propre en ajoutant une classe « ControllerHelpers » au projet NerdDinner, et implémenter une méthode d’extension « AddRuleViolations » dans celui-ci, qui ajoute une méthode d’assistance à la classe ASP.NET MVC ModelStateDictionary. Cette méthode d’extension peut encapsuler la logique nécessaire pour remplir le ModelStateDictionary avec une liste d’erreurs RuleViolation :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Nous pouvons ensuite mettre à jour notre méthode d’action HTTP-après modification pour utiliser cette méthode d’extension pour remplir la collection ModelState avec nos violations de règle de dîner.

#### <a name="complete-edit-action-method-implementations"></a>Exécuter les implémentations de la méthode d’action de modification

Le code ci-dessous implémente l’ensemble de la logique de contrôleur nécessaire pour notre scénario de modification :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

L’avantage de notre implémentation de la modification est que ni notre classe de contrôleur, ni notre modèle de vue ne doivent savoir quoi que ce soit sur la validation ou les règles métier spécifiques appliquées par notre dîner. Nous pouvons ajouter des règles supplémentaires à notre modèle à l’avenir et n’auront pas à apporter de modifications de code à notre contrôleur ou vue afin qu’elles soient prises en charge. Cela nous permet de faire évoluer facilement nos exigences en matière d’applications à l’avenir, avec un minimum de modifications de code.

### <a name="create-support"></a>Créer un support

Nous avons terminé l’implémentation du comportement « Edit » de notre classe DinnersController. Passons maintenant à l’implémentation de la prise en charge de la « création », qui permettra aux utilisateurs d’ajouter de nouveaux dîners.

#### <a name="the-http-get-create-action-method"></a>Méthode d’action de création HTTP-obtenue

Nous allons commencer par implémenter le comportement HTTP « récupérer » de notre méthode d’action de création. Cette méthode est appelée quand une personne visite l’URL */dinners/Create* . Notre implémentation ressemble à ceci :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Le code ci-dessus crée un nouvel objet dîner et affecte sa propriété EventDate à une semaine à l’avenir. Il affiche ensuite une vue basée sur le nouvel objet dîner. Étant donné que nous n’avons pas passé explicitement de nom à la méthode d’assistance *View ()* , il utilisera le chemin d’accès par défaut basé sur la Convention pour résoudre le modèle de vue:/views/dinners/Create.aspx.

Créons maintenant ce modèle de vue. Pour ce faire, cliquez avec le bouton droit dans la méthode Create action et sélectionnez la commande de menu contextuel « Add View » (ajouter une vue). Dans la boîte de dialogue « Add View » (ajouter une vue), nous indiquons que nous transmettons un objet dîner au modèle de vue et que vous choisissez de générer automatiquement la structure d’un modèle « Create » :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio enregistre une nouvelle vue « Create. aspx » basée sur une génération de modèles automatique dans le répertoire « \Views\Dinners » et l’ouvre dans l’IDE :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Nous allons apporter quelques modifications au fichier de génération de modèles par défaut « créer » qui a été généré pour nous et le modifier pour qu’il ressemble à ce qui suit :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Et maintenant, lorsque nous exécutons notre application et que vous accédez à l’URL *« /dinners/Create »* dans le navigateur, elle affiche l’interface utilisateur comme ci-dessous à partir de notre implémentation d’action de création :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implémentation de la méthode d’action de création HTTP-Après

Nous avons implémenté la version HTTP-obtenir de notre méthode d’action Create. Lorsqu’un utilisateur clique sur le bouton « enregistrer », il effectue une publication de formulaire sur l’URL */dinners/Create* et envoie le code html &lt;entrée&gt; formulaire à l’aide du verbe http.

Nous allons maintenant implémenter le comportement HTTP de la méthode Create action. Nous allons commencer par ajouter une méthode d’action « Create » surchargée à notre DinnersController avec un attribut « AcceptVerbs » qui indique qu’elle gère les scénarios HTTP HTTP :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Il existe plusieurs façons d’accéder aux paramètres de formulaire publiés dans notre méthode « Create » activée pour HTTP-POST.

Une approche consiste à créer un nouvel objet Dinner, puis à utiliser la méthode d’assistance *UpdateModel ()* (comme nous l’avons fait avec l’action Edit) pour le remplir avec les valeurs de formulaire publiées. Nous pouvons ensuite l’ajouter à notre DinnerRepository, le rendre persistant dans la base de données et rediriger l’utilisateur vers notre action détails pour afficher le dîner nouvellement créé à l’aide du code ci-dessous :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Vous pouvez également utiliser une approche dans laquelle notre méthode d’action Create () prend un objet Dinner comme paramètre de méthode. ASP.NET MVC instancie ensuite automatiquement un nouvel objet dîner pour nous, remplit ses propriétés à l’aide des entrées de formulaire et le transmet à notre méthode d’action :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Notre méthode d’action ci-dessus vérifie que l’objet dîner a été correctement rempli avec les valeurs de publication de formulaire en vérifiant la propriété ModelState. IsValid. La valeur false est renvoyée en cas de problème de conversion d’entrée (par exemple, une chaîne de « faux » pour la propriété EventDate) et, en cas de problème, notre méthode d’action réaffiche le formulaire.

Si les valeurs d’entrée sont valides, la méthode d’action tente d’ajouter et d’enregistrer le nouveau dîner dans le DinnerRepository. Il encapsule ce travail dans un bloc try/catch et réaffiche le formulaire s’il existe des violations de règle d’entreprise (ce qui amènerait la méthode dinnerRepository. Save () à lever une exception).

Pour voir ce comportement de gestion des erreurs en action, nous pouvons demander l’URL */dinners/Create* et renseigner les détails sur un nouveau dîner. Une entrée ou des valeurs incorrectes entraînent le réaffichage du formulaire créer avec les erreurs mises en surbrillance comme ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Notez que notre formulaire de création respecte exactement les mêmes règles de validation et d’entreprise que notre formulaire de modification. Cela est dû au fait que nos règles de validation et d’entreprise ont été définies dans le modèle et qu’elles n’ont pas été incorporées dans l’interface utilisateur ou le contrôleur de l’application. Cela signifie que nous pouvons modifier/faire évoluer nos règles de validation ou d’entreprise dans un emplacement unique et les faire appliquer à l’ensemble de notre application. Nous n’aurons pas besoin de modifier le code dans nos méthodes d’action de modification ou de création pour honorer automatiquement toutes les nouvelles règles ou modifications apportées à celles-ci.

Lorsque nous corrigeons les valeurs d’entrée et que vous cliquez à nouveau sur le bouton « enregistrer », l’ajout à DinnerRepository échoue et un nouveau dîner est ajouté à la base de données. Nous sommes ensuite redirigés vers l’URL */dinners/Details/[id]* , où nous verrons les détails sur le dîner nouvellement créé :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Supprimer la prise en charge

Nous allons maintenant ajouter la prise en charge de la « suppression » à notre DinnersController.

#### <a name="the-http-get-delete-action-method"></a>Méthode d’action HTTP-récupérer la suppression

Nous allons commencer par implémenter le comportement HTTP d’extraction de notre méthode d’action de suppression. Cette méthode sera appelée lorsqu’un utilisateur visitera l’URL */dinners/delete/[id]* . Voici l’implémentation :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

La méthode d’action tente de récupérer le dîner à supprimer. Si le dîner existe, il restitue une vue basée sur l’objet dîner. Si l’objet n’existe pas (ou a déjà été supprimé), il retourne une vue qui restitue le modèle de vue « NotFound » que nous avons créé précédemment pour notre méthode d’action « Details ».

Nous pouvons créer le modèle de vue « supprimer » en cliquant avec le bouton droit dans la méthode d’action de suppression et en sélectionnant la commande de menu contextuel « ajouter une vue ». Dans la boîte de dialogue « Ajouter une vue », nous indiquons que nous transmettons un objet dîner à notre modèle de vue en tant que modèle, et que vous choisissez de créer un modèle vide :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio ajoute un nouveau fichier de modèle de vue « Delete. aspx » pour nous dans notre répertoire « \Views\Dinners ». Nous allons ajouter du code HTML et du code au modèle pour implémenter un écran de confirmation de suppression comme ci-dessous :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Le code ci-dessus affiche le titre du dîner à supprimer, et génère une &lt;formulaire&gt; élément qui effectue une publication sur l’URL/Dinners/Delete/[id] si l’utilisateur final clique sur le bouton « supprimer » dans celui-ci.

Lorsque nous exécutons notre application et que vous accédez à l’URL *« /dinners/delete/[id] »* pour un objet dîner valide, elle affiche l’interface utilisateur comme ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Rubrique latérale : Pourquoi est-ce que nous effectuons une publication ?** |
| --- |
| Vous pouvez vous demander : Pourquoi avons-nous fait l’effort de création d’un &lt;formulaire&gt; dans notre écran de confirmation de suppression ? Pourquoi ne pas simplement utiliser un lien hypertexte standard pour établir un lien vers une méthode d’action qui effectue l’opération de suppression réelle ? Cela est dû au fait que nous souhaitons être vigilants pour la protection contre les analyseurs Web et les moteurs de recherche qui découvrent nos URL et qui entraînent la suppression par inadvertance des données lorsqu’ils suivent les liens. Les URL HTTP-obtenir sont considérées comme « sûres » pour qu’elles puissent accéder/à l’analyse, et elles sont supposées ne pas suivre les publications HTTP. Une bonne règle consiste à veiller à toujours placer les opérations destructrices ou de modification des données en arrière-plan des requêtes HTTP-POSTCONNEXION. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implémentation de la méthode d’action HTTP-après suppression

Nous avons maintenant la version HTTP-obtenir de notre méthode d’action de suppression implémentée qui affiche un écran de confirmation de suppression. Lorsqu’un utilisateur final clique sur le bouton « supprimer », il exécute une publication de formulaire sur l’URL */dinners/Dinner/[id]* .

Nous allons maintenant implémenter le comportement HTTP « poster » de la méthode d’action de suppression à l’aide du code ci-dessous :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

La version HTTP-poster de notre méthode d’action de suppression tente de récupérer l’objet dîner à supprimer. S’il ne peut pas le trouver (parce qu’il a déjà été supprimé), il affiche notre modèle « NotFound ». S’il trouve le dîner, il le supprime de la DinnerRepository. Il affiche ensuite un modèle « supprimé ».

Pour implémenter le modèle « supprimé », cliquez avec le bouton droit dans la méthode d’action et choisissez le menu contextuel « ajouter une vue ». Nous allons nommer notre vue « Deleted » (supprimer) et être un modèle vide (et ne pas accepter un objet de modèle fortement typé). Nous allons ensuite y ajouter du contenu HTML :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Et maintenant, lorsque nous exécutons notre application et que nous avons accès à l’URL *« /dinners/delete/[id] »* pour un objet dîner valide, l’écran de confirmation de la suppression de dîner s’affiche comme ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Lorsque nous cliquons sur le bouton « supprimer », un message HTTP-poster est exécuté sur l’URL */dinners/delete/[id]* , ce qui supprime le dîner de notre base de données et affiche notre modèle de vue « supprimé » :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Sécurité de la liaison de modèle

Nous avons abordé deux façons différentes d’utiliser les fonctionnalités de liaison de modèle intégrées de ASP.NET MVC. La première utilise la méthode UpdateModel () pour mettre à jour les propriétés d’un objet de modèle existant, et la deuxième à l’aide de la prise en charge de ASP.NET MVC pour passer des objets de modèle dans en tant que paramètres de méthode d’action. Ces deux techniques sont très puissantes et extrêmement utiles.

Cette puissance apporte également sa responsabilité en informatique. Il est important de toujours être paranoïaques sur la sécurité lors de l’acceptation d’une entrée d’utilisateur, et cela est également vrai lorsque vous liez des objets à une entrée de formulaire. Veillez à toujours encoder en HTML toute valeur entrée par l’utilisateur pour éviter les attaques par injection de code HTML et JavaScript, et faites attention aux attaques par injection SQL (Remarque : nous utilisons LINQ to SQL pour notre application, qui encode automatiquement les paramètres pour éviter ces types d’attaques). Vous ne devez jamais vous appuyer sur la validation côté client seule et toujours utiliser la validation côté serveur pour vous protéger contre les pirates qui essaient de vous envoyer des valeurs erronées.

L’un des éléments de sécurité supplémentaires à prendre en compte lors de l’utilisation des fonctionnalités de liaison de ASP.NET MVC est l’étendue des objets que vous liez. En particulier, vous souhaitez vous assurer que vous comprenez les implications en matière de sécurité des propriétés que vous autorisez à être liées, et que vous autorisez uniquement les propriétés qui doivent être mises à jour par un utilisateur final.

Par défaut, la méthode UpdateModel () tente de mettre à jour toutes les propriétés de l’objet de modèle qui correspondent aux valeurs de paramètre de formulaire entrantes. De même, les objets passés en tant que paramètres de méthode d’action par défaut peuvent également avoir toutes leurs propriétés définies via des paramètres de formulaire.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Verrouillage de la liaison sur la base de l’utilisation

Vous pouvez verrouiller la stratégie de liaison à la base de l’utilisation en fournissant une « liste d’inclusion » explicite des propriétés qui peuvent être mises à jour. Pour ce faire, vous pouvez passer un paramètre de tableau de chaînes supplémentaire à la méthode UpdateModel (), comme ci-dessous :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Les objets passés en tant que paramètres de méthode d’action prennent également en charge un attribut [Bind] qui permet de spécifier une « liste d’inclusion » de propriétés autorisées comme ci-dessous :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Verrouillage de la liaison sur une base de type

Vous pouvez également verrouiller les règles de liaison en fonction du type. Cela vous permet de spécifier les règles de liaison une seule fois, puis de les appliquer dans tous les scénarios (y compris les UpdateModel et les scénarios de paramètres de méthode d’action) sur tous les contrôleurs et méthodes d’action.

Vous pouvez personnaliser les règles de liaison par type en ajoutant un attribut [Bind] à un type, ou en l’inscrivant dans le fichier global. asax de l’application (utile pour les scénarios où vous ne possédez pas le type). Vous pouvez ensuite utiliser les propriétés include et Exclude de l’attribut de liaison pour contrôler les propriétés qui peuvent être liées pour une classe ou une interface particulière.

Nous allons utiliser cette technique pour la classe dîner dans notre application NerdDinner et ajouter un attribut [Bind] à celle-ci, qui limite la liste des propriétés pouvant être liées à ce qui suit :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Notez que nous n’autorisons pas la collection de RSVPs à être manipulés par liaison et que nous n’autorisons pas les propriétés DinnerID ou HostedBy à être définies via une liaison. Pour des raisons de sécurité, nous allons uniquement manipuler ces propriétés particulières à l’aide de code explicite dans nos méthodes d’action.

### <a name="crud-wrap-up"></a>Habillage CRUD

ASP.NET MVC comprend un certain nombre de fonctionnalités intégrées qui facilitent l’implémentation de scénarios de publication de formulaires. Nous avons utilisé une variété de ces fonctionnalités pour fournir une prise en charge de l’interface utilisateur CRUD en plus de notre DinnerRepository.

Nous utilisons une approche axée sur les modèles pour implémenter notre application. Cela signifie que toutes nos logiques de validation et de règle d’entreprise sont définies dans notre couche de modèle, et non dans nos contrôleurs ou vues. Ni notre classe de contrôleur, ni nos modèles de vue ne savent quoi que ce soit sur les règles d’entreprise spécifiques appliquées par notre classe de modèle dîner.

Cela permet de nettoyer l’architecture de l’application et de la rendre plus facile à tester. Nous pouvons ajouter des règles d’entreprise supplémentaires à notre couche de modèle à l’avenir et *ne pas avoir à apporter de modifications au code* de notre contrôleur ou de cette vue pour qu’elles soient prises en charge. Cela nous permettra de faire évoluer et de changer l’application à l’avenir pour évoluer.

Notre DinnersController permet désormais de dîner des listes/détails, ainsi qu’une prise en charge de la création, de la modification et de la suppression. Vous trouverez le code complet de la classe ci-dessous :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Étape suivante

Nous disposons désormais d’une prise en charge CRUD (création, lecture, mise à jour et suppression) de base dans notre classe DinnersController.

Voyons maintenant comment nous pouvons utiliser les classes ViewData et ViewModel pour activer une interface utilisateur encore plus riche sur nos formulaires.

> [!div class="step-by-step"]
> [Précédent](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Suivant](use-viewdata-and-implement-viewmodel-classes.md)
