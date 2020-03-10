---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Partie 3 : vues et ViewModels | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 3 couvre les vues et les ViewModels.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3fcfc816cde22c697a78bab2c9ea7ace1bf68501
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559807"
---
# <a name="part-3-views-and-viewmodels"></a>Partie 3 : vues et ViewModels

par [Jon Galloway](https://github.com/jongalloway)

> Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.  
>   
> Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.  
>   
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 3 couvre les vues et les ViewModels.

Jusqu’à présent, nous venons de retourner des chaînes à partir d’actions de contrôleur. C’est un excellent moyen d’avoir une idée du fonctionnement des contrôleurs, mais il ne s’agit pas de la façon dont vous souhaiteriez créer une application Web réelle. Nous allons avoir besoin d’une meilleure façon de générer du code HTML dans les navigateurs qui visitent notre site, où nous pouvons utiliser des fichiers de modèle pour personnaliser plus facilement le contenu HTML renvoyé. C’est exactement ce que font les vues.

## <a name="adding-a-view-template"></a>Ajout d’un modèle de vue

Pour utiliser un modèle de vue, nous allons modifier la méthode de l’index HomeController pour retourner un ActionResult et le faire retourner View (), comme ci-dessous :

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

La modification ci-dessus indique que, au lieu de retourner une chaîne, nous souhaitons plutôt utiliser une « vue » pour générer un résultat en retour.

Nous allons maintenant ajouter un modèle de vue approprié à notre projet. Pour ce faire, nous allons positionner le curseur de texte dans la méthode d’action index, puis cliquer avec le bouton droit et sélectionner « Add View » (ajouter une vue). La boîte de dialogue Ajouter une vue s’affiche :

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

La boîte de dialogue « Ajouter une vue » nous permet de générer rapidement et facilement des fichiers de modèle de vue. Par défaut, la boîte de dialogue « Ajouter une vue » prérenseigne le nom du modèle de vue à créer afin qu’il corresponde à la méthode d’action qui va l’utiliser. Étant donné que nous avons utilisé le menu contextuel « ajouter une vue » dans la méthode d’action index () de notre HomeController, la boîte de dialogue « Ajouter une vue » ci-dessus contient « index » comme nom de vue prérempli par défaut. Nous n’avons pas besoin de modifier les options de cette boîte de dialogue. par conséquent, cliquez sur le bouton Ajouter.

Lorsque nous cliquons sur le bouton Ajouter, Visual Web Developer crée un modèle d’affichage index. cshtml pour nous dans le répertoire \Views\Home, en créant le dossier s’il n’existe pas déjà.

![](mvc-music-store-part-3/_static/image2.png)

Le nom et l’emplacement du dossier du fichier « index. cshtml » sont importants et suivent les conventions d’attribution de noms ASP.NET MVC par défaut. Le nom du répertoire, \Views\Home, correspond au contrôleur, appelé HomeController. Le nom du modèle de vue, index, correspond à la méthode d’action du contrôleur qui affiche la vue.

ASP.NET MVC nous permet d’éviter d’avoir à spécifier explicitement le nom ou l’emplacement d’un modèle de vue lorsque nous utilisons cette Convention d’affectation de noms pour retourner une vue. Par défaut, il affiche le modèle de vue \Views\Home\Index.cshtml quand nous écrivons du code comme ci-dessous dans le HomeController :

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer a créé et ouvert le modèle de vue « index. cshtml » après avoir cliqué sur le bouton « Ajouter » dans la boîte de dialogue « Ajouter une vue ». Le contenu de index. cshtml est indiqué ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Cette vue utilise le syntaxe Razor, qui est plus concis que le moteur d’affichage des Web Forms utilisé dans ASP.NET Web Forms et les versions précédentes de ASP.NET MVC. Le moteur d’affichage des Web Forms est toujours disponible dans ASP.NET MVC 3, mais de nombreux développeurs trouvent que le moteur d’affichage Razor est très bien adapté au développement ASP.NET MVC.

Les trois premières lignes définissent le titre de la page à l’aide de ViewBag. title. Nous allons voir comment cela fonctionne plus rapidement, mais nous allons tout d’abord mettre à jour le texte de l’en-tête de texte et afficher la page. Mettez à jour la balise &lt;H2&gt; pour indiquer « il s’agit de la page d’hébergement », comme indiqué ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

L’exécution de l’application montre que notre nouveau texte est visible sur la page d’hébergement.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Utilisation d’une disposition pour les éléments de site communs

La plupart des sites Web disposent de contenu partagé entre de nombreuses pages : navigation, pieds de page, images de logos, références de feuille de style, etc. Le moteur d’affichage Razor facilite la gestion à l’aide d’une page appelée \_Layout. cshtml, qui a été créée automatiquement pour nous dans le dossier/Views/Shared

![](mvc-music-store-part-3/_static/image4.png)

Double-cliquez sur ce dossier pour afficher son contenu, comme indiqué ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Le contenu de nos vues individuelles est affiché par la commande @RenderBody() et tout contenu courant que vous souhaitez voir apparaître en dehors de peut être ajouté au balisage \_Layout. cshtml. Nous souhaitons que notre magasin de musique MVC ait un en-tête commun avec des liens vers notre page d’hébergement et une zone de stockage sur toutes les pages du site. nous allons donc l’ajouter au modèle juste au-dessus de l’instruction @RenderBody().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Mise à jour de la feuille de style

Le modèle de projet vide comprend un fichier CSS très rationalisé qui comprend simplement les styles utilisés pour afficher les messages de validation. Notre concepteur a fourni des CSS et des images supplémentaires pour définir l’apparence de notre site. nous allons donc les ajouter maintenant.

Le fichier CSS et les images mis à jour sont inclus dans le répertoire de contenu de MvcMusicStore-Assets. zip, qui est disponible dans [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store). Nous allons les sélectionner dans l’Explorateur Windows et les déposer dans le dossier content de la solution dans Visual Web Developer, comme indiqué ci-dessous :

![](mvc-music-store-part-3/_static/image5.png)

Vous êtes invité à confirmer si vous souhaitez remplacer le fichier site. CSS existant. Cliquez sur Oui.

![](mvc-music-store-part-3/_static/image6.png)

Le dossier de contenu de votre application s’affiche à présent comme suit :

![](mvc-music-store-part-3/_static/image7.png)

À présent, nous allons exécuter l’application et voir comment nos modifications apparaissent sur la page d’hébergement.

![](mvc-music-store-part-3/_static/image8.png)

- Passons en revue ce qui a changé : la méthode d’action d’index du HomeController a trouvé et affiché le modèle \Views\Home\Index.cshtmlView, bien que notre code appelé « Return View () », parce que notre modèle de vue suivait la Convention d’affectation de noms standard.
- La page d’accueil affiche un message d’accueil simple défini dans le modèle de vue \Views\Home\Index.cshtml.
- La page d’accueil utilise notre modèle \_Layout. cshtml. le message d’accueil est donc contenu dans la disposition HTML du site standard.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Utilisation d’un modèle pour transmettre des informations à notre vue

Un modèle de vue qui affiche simplement du code HTML codé en dur n’est pas destiné à créer un site Web très intéressant. Pour créer un site Web dynamique, nous voulons plutôt transmettre des informations de nos actions de contrôleur à nos modèles de vue.

Dans le modèle Model-View-Controller, le terme modèle fait référence aux objets qui représentent les données de l’application. Souvent, les objets de modèle correspondent aux tables de votre base de données, mais ils n’en ont pas besoin.

Les méthodes d’action du contrôleur qui retournent un ActionResult peuvent passer un objet de modèle à la vue. Cela permet à un contrôleur de regrouper correctement toutes les informations nécessaires à la génération d’une réponse, puis de transmettre ces informations à un modèle de vue à utiliser pour générer la réponse HTML appropriée. Cela est plus facile à comprendre en l’regardant en action, donc commençons.

Tout d’abord, nous allons créer des classes de modèle pour représenter des genres et des albums au sein de notre magasin. Commençons par créer une classe genre. Cliquez avec le bouton droit sur le dossier « Models » dans votre projet, choisissez l’option « Ajouter une classe » et nommez le fichier « Genre.cs ».

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Ajoutez ensuite une propriété de nom de chaîne publique à la classe qui a été créée :

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Remarque : au cas où vous vous poseriez la question, la notation {obtient ; Set ;} C#utilise la fonctionnalité de propriétés implémentées automatiquement. Cela nous offre les avantages d’une propriété sans que nous puissions déclarer un champ de stockage.*

Ensuite, suivez les mêmes étapes pour créer une classe album (nommée Album.cs) qui a un titre et une propriété genre :

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Nous pouvons maintenant modifier le StoreController pour utiliser des vues qui affichent des informations dynamiques à partir de notre modèle. Si, à des fins de démonstration, nous avons nommé nos albums en fonction de l’ID de la demande, nous pourrions afficher ces informations comme dans l’affichage ci-dessous.

![](mvc-music-store-part-3/_static/image10.png)

Nous allons commencer par modifier l’action Détails du magasin pour afficher les informations relatives à un seul album. Ajoutez une instruction « using » en haut de la classe **StoreControllers** pour inclure l’espace de noms MvcMusicStore. Models. par conséquent, nous n’avons pas besoin de taper MvcMusicStore. Models. album chaque fois que nous souhaitons utiliser la classe album. La section « usings » de cette classe doit maintenant apparaître comme indiqué ci-dessous.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Ensuite, nous allons mettre à jour l’action du contrôleur des détails afin qu’elle renvoie un ActionResult plutôt qu’une chaîne, comme nous l’avons fait avec la méthode d’index du HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

À présent, nous pouvons modifier la logique pour retourner un objet album à la vue. Plus loin dans ce didacticiel, nous allons extraire les données d’une base de données. pour l’instant, nous allons utiliser des « données factices » pour commencer.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Remarque : Si vous n’êtes pas familiarisé C#avec, vous pouvez supposer que l’utilisation de var signifie que notre variable album est à liaison tardive. Ce n’est pas correct : C# le compilateur utilise l’inférence de type en fonction de ce que nous attribuons à la variable pour déterminer que cet album est de type album et la compilation de la variable album locale en tant que type d’album. nous obtenons donc la vérification au moment de la compilation et la prise en charge de l’éditeur de code Visual Studio.*

Créons maintenant un modèle de vue qui utilise notre album pour générer une réponse HTML. Avant cela, nous devons générer le projet afin que la boîte de dialogue Ajouter une vue sache la classe d’album nouvellement créée. Vous pouvez générer le projet en sélectionnant l’élément de menu Debug ⇨ Build MvcMusicStore (pour un crédit supplémentaire, vous pouvez utiliser le raccourci Ctrl-Shift-B pour générer le projet).

![](mvc-music-store-part-3/_static/image11.png)

Maintenant que nous avons configuré nos classes de prise en charge, nous sommes prêts à créer notre modèle de vue. Cliquez avec le bouton droit dans la méthode Details et sélectionnez « Ajouter une vue... » dans le menu contextuel.

![](mvc-music-store-part-3/_static/image12.png)

Nous allons créer un modèle de vue comme nous l’avons fait précédemment avec le HomeController. Comme nous l’avons créé à partir du StoreController, il sera généré par défaut dans un fichier \Views\Store\Index.cshtml.

Contrairement à ce qui précède, nous allons cocher la case « créer une vue fortement typée ». Nous allons ensuite sélectionner notre classe « album » dans la liste déroulante « Afficher la classe de données ». Dans ce cas, la boîte de dialogue « Ajouter une vue » crée un modèle de vue qui s’attend à ce qu’un objet album lui soit transmis pour être utilisé.

![](mvc-music-store-part-3/_static/image13.png)

Lorsque nous cliquons sur le bouton « Ajouter », notre modèle de vue \Views\Store\Details.cshtml sera créé, contenant le code suivant.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Notez la première ligne, qui indique que cette vue est fortement typée dans notre classe album. Le moteur de vue Razor comprend qu’il a été passé à un objet album. nous pouvons donc facilement accéder aux propriétés du modèle et même bénéficier de l’avantage d’IntelliSense dans l’éditeur de Visual Web Developer.

Mettez à jour la balise &lt;H2&gt; pour qu’elle affiche la propriété Title de l’album en modifiant cette ligne de manière à ce qu’elle apparaisse comme suit.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Notez qu’IntelliSense est déclenché lorsque vous entrez le point après le mot clé @Model, en indiquant les propriétés et les méthodes prises en charge par la classe album.

Réexécutez maintenant notre projet et visitez l’URL/Store/Details/5. Nous allons voir les détails d’un album comme ci-dessous.

![](mvc-music-store-part-3/_static/image14.png)

Nous allons maintenant effectuer une mise à jour similaire pour la méthode d’action Store Browse. Mettez à jour la méthode afin qu’elle retourne un ActionResult et modifiez la logique de la méthode afin qu’elle crée un nouvel objet genre et la retourne à la vue.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Cliquez avec le bouton droit dans la méthode browse et sélectionnez « Ajouter une vue... » dans le menu contextuel, ajoutez une vue fortement typée ajouter un fortement typé à la classe genre.

![](mvc-music-store-part-3/_static/image15.png)

Mettez à jour l’élément &lt;H2&gt; dans le code de vue (dans/Views/Store/Browse.cshtml) pour afficher les informations de genre.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Nous allons maintenant réexécuter notre projet et accéder au/Store/Browse ? Genre = URL Disco. La page parcourir s’affiche comme ci-dessous.

![](mvc-music-store-part-3/_static/image16.png)

Enfin, nous allons créer une mise à jour légèrement plus complexe de la méthode et de la vue d’action de l' **index Store** pour afficher une liste de tous les genres de notre magasin. Nous allons le faire en utilisant une liste de genres comme objet de modèle, plutôt qu’un simple genre.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Cliquez avec le bouton droit dans la méthode d’action stocker l’index et sélectionnez Ajouter une vue comme auparavant, sélectionnez genre comme classe de modèle, puis appuyez sur le bouton Ajouter.

![](mvc-music-store-part-3/_static/image17.png)

Tout d’abord, nous allons modifier la déclaration de @model pour indiquer que la vue attendra plusieurs objets de genre plutôt qu’un seul. Modifiez la première ligne de/Store/Index.cshtml comme suit :

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Cela indique au moteur de vue Razor qu’il utilisera un objet de modèle pouvant contenir plusieurs objets de genre. Nous utilisons une&gt; de genre IEnumerable&lt;plutôt qu’une liste&lt;&gt; de genre dans la mesure où elle est plus générique, ce qui nous permet de changer le type de modèle plus tard en tout type d’objet qui prend en charge l’interface IEnumerable.

Ensuite, nous allons parcourir les objets de genre dans le modèle, comme indiqué dans le code de vue terminé ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Notez que nous disposons d’une prise en charge complète d’IntelliSense, car nous entrons ce code, de sorte que lorsque vous tapez «@Model». Nous voyons toutes les méthodes et les propriétés prises en charge par un IEnumerable de type genre.

![](mvc-music-store-part-3/_static/image18.png)

Dans notre boucle « foreach », Visual Web Developer sait que chaque élément est de type genre. nous voyons donc IntelliSense pour chaque type de genre.

![](mvc-music-store-part-3/_static/image19.png)

Ensuite, la fonctionnalité de génération de modèles automatique a examiné l’objet de genre et déterminé que chaque aura une propriété Name, de sorte qu’il effectue une boucle et l’écrit. Elle génère également des liens de modification, de détails et de suppression pour chaque élément individuel. Nous allons tirer parti de cela plus tard dans notre responsable du magasin, mais pour l’instant, nous aimerions avoir une liste simple à la place.

Lorsque nous exécutons l’application et que vous accédez à/Store, nous voyons que le nombre et la liste de genres s’affichent.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Ajout de liens entre les pages

Notre URL/Store qui répertorie les genres répertorie actuellement les noms de genres simplement en texte brut. Nous allons modifier cela de sorte qu’au lieu de texte brut, nous ayons les noms de genres liés à l’URL/Store/Browse appropriée, afin que le fait de cliquer sur un genre musical comme « Disco » accède à l’URL/Store/Browse ? genre = Disco. Nous pourrions mettre à jour notre modèle de vue \Views\Store\Index.cshtml pour générer ces liens à l’aide d’un code comme ci-dessous **(ne tapez pas ceci dans, nous allons l’améliorer)** :

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Cela fonctionne, mais cela peut entraîner des problèmes ultérieurement, car il s’appuie sur une chaîne codée en dur. Par exemple, si nous voulions renommer le contrôleur, nous aurions besoin de parcourir notre code pour rechercher les liens qui doivent être mis à jour.

Une autre approche que nous pouvons utiliser est de tirer parti d’une méthode d’assistance HTML. ASP.NET MVC comprend des méthodes d’assistance HTML qui sont disponibles à partir de notre modèle de vue pour effectuer diverses tâches courantes, comme ceci. La méthode d’assistance HTML. ActionLink () est particulièrement utile et facilite la création de code HTML &lt;un&gt; des liens et prend en charge des détails ennuyeux, par exemple s’assurer que les chemins d’URL sont correctement encodés dans l’URL.

Html. ActionLink () a plusieurs surcharges différentes pour permettre de spécifier autant d’informations que nécessaire pour vos liens. Dans le cas le plus simple, vous fournissez uniquement le texte du lien et la méthode d’action à atteindre lorsque l’utilisateur clique sur le lien hypertexte sur le client. Par exemple, nous pouvons créer un lien vers la méthode « /store/ » index () sur la page Store Details avec le texte du lien « Go to the store index » à l’aide de l’appel suivant :

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Remarque : dans ce cas, nous n’avons pas besoin de spécifier le nom du contrôleur, car nous créons simplement une liaison à une autre action dans le même contrôleur que celui qui rend l’affichage actuel.*

Les liens vers la page de navigation doivent cependant passer un paramètre. nous allons donc utiliser une autre surcharge de la méthode html. ActionLink qui accepte trois paramètres :

- 1. Texte du lien qui affichera le nom du genre
- 2. Nom de l’action du contrôleur (Parcourir)
- 3. Router les valeurs des paramètres, en spécifiant à la fois le nom (genre) et la valeur (genre Name)

Pour ce faire, voici comment nous allons écrire ces liens dans la vue store index :

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Maintenant, lorsque nous exécutons à nouveau notre projet et que vous accédez à l’URL/Store/, une liste de genres s’affiche. Chaque genre est un lien hypertexte. quand vous cliquez dessus, nous nous remercions de notre URL/Store/Browse ? genre = *[genre]* .

![](mvc-music-store-part-3/_static/image3.jpg)

Le code HTML de la liste de genres ressemble à ceci :

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]

> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-2.md)
> [Suivant](mvc-music-store-part-4.md)
