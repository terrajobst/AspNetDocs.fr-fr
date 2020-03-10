---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Utiliser AJAX pour implémenter des scénarios de mappage | Microsoft Docs
author: microsoft
description: L’étape 11 montre comment intégrer la prise en charge du mappage AJAX dans notre application NerdDinner, ce qui permet aux utilisateurs qui créent, modifient ou visualisent des dîners de voir le l...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 7fc90f978b9f9eca511feca70a3c0d02ec69b940
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580114"
---
# <a name="use-ajax-to-implement-mapping-scenarios"></a>Utiliser AJAX pour implémenter des scénarios de mappage

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 11 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.
> 
> L’étape 11 montre comment intégrer la prise en charge du mappage AJAX dans notre application NerdDinner, ce qui permet aux utilisateurs qui créent, modifient ou visualisent des dîners d’afficher l’emplacement du dîner de manière graphique.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner étape 11 : intégration d’une carte AJAX

Nous allons maintenant rendre notre application un peu plus attrayante en intégrant la prise en charge du mappage AJAX. Cela permet aux utilisateurs qui créent, modifient ou visualisent des dîners d’afficher l’emplacement du dîner de manière graphique.

### <a name="creating-a-map-partial-view"></a>Création d’une vue partielle de la carte

Nous allons utiliser la fonctionnalité de mappage à plusieurs endroits de notre application. Pour conserver notre code à sec, nous encapsulerons la fonctionnalité de carte commune dans un modèle partiel unique que nous pouvons réutiliser dans plusieurs actions et vues de contrôleur. Nous allons nommer cette vue partielle « map. ascx » et la créer dans le répertoire \Views\Dinners

Nous pouvons créer le mappage map. ascx partiellement en cliquant avec le bouton droit sur le répertoire \Views\Dinners et en choisissant la commande de menu Add-&gt;View. Nous allons nommer la vue « map. ascx », la vérifier en tant que vue partielle et indiquer que nous allons la transmettre à une classe de modèle « dîner » fortement typée :

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Lorsque nous cliquons sur le bouton « Ajouter », notre modèle partiel sera créé. Nous allons ensuite mettre à jour le fichier Map. ascx pour obtenir le contenu suivant :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

La première &lt;script&gt; référence pointe vers la bibliothèque de mappage Microsoft Virtual Earth 6,2. La deuxième &lt;script&gt; référence pointe vers un fichier Map. js que nous allons bientôt créer, ce qui encapsule notre logique de mappage JavaScript courante. L’élément &lt;div id = "theMap"&gt; est le conteneur HTML qui sera utilisé par Virtual Earth pour héberger le mappage.

Nous disposons ensuite d’un bloc de&gt; de script &lt;incorporé qui contient deux fonctions JavaScript spécifiques à cette vue. La première fonction utilise jQuery pour associer une fonction qui s’exécute lorsque la page est prête à exécuter le script côté client. Elle appelle une fonction d’assistance LoadMap () que nous allons définir dans notre fichier de script map. js pour charger le contrôle de carte Virtual Earth. La deuxième fonction est un gestionnaire d’événements de rappel qui ajoute un code confidentiel à la carte qui identifie un emplacement.

Notez que nous utilisons un bloc &lt;% =%&gt; côté serveur dans le bloc de script côté client pour incorporer la latitude et la longitude du dîner que nous voulons mapper dans le JavaScript. Il s’agit d’une technique utile pour générer des valeurs dynamiques qui peuvent être utilisées par un script côté client (sans nécessiter un rappel AJAX distinct sur le serveur pour récupérer les valeurs, ce qui le rend plus rapide). Les blocs &lt;% =%&gt; s’exécutent lors du rendu de la vue sur le serveur, et la sortie du code HTML finit simplement par des valeurs JavaScript incorporées (par exemple : var latitude = 47,64312 ;).

### <a name="creating-a-mapjs-utility-library"></a>Création d’une bibliothèque d’utilitaires map. js

Nous allons maintenant créer le fichier Map. js que nous pouvons utiliser pour encapsuler la fonctionnalité JavaScript pour notre carte (et implémenter les méthodes LoadMap et LoadPin ci-dessus). Pour ce faire, cliquez avec le bouton droit sur le répertoire \Scripts au sein de notre projet, puis choisissez la commande de menu « Add-&gt;New Item », sélectionnez l’élément JScript et nommez-le « map. js ».

Voici le code JavaScript que nous ajouterons au fichier Map. js qui interagit avec Virtual Earth pour afficher notre carte et y ajouter des codes PIN pour nos dîners :

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Intégration de la carte avec les formulaires créer et modifier

Nous allons maintenant intégrer la prise en charge de la carte à nos scénarios de création et de modification existants. La bonne nouvelle, c’est que c’est assez facile à faire et ne nécessite pas de modifier le code de votre contrôleur. Étant donné que nos vues Create et Edit partagent une vue partielle « DinnerForm » commune pour implémenter l’interface utilisateur du dîner, nous pouvons ajouter la carte à un emplacement et faire en sorte que nos deux scénarios de création et de modification l’utilisent.

Tout ce qu’il nous faut faire, c’est d’ouvrir la vue partielle \Views\Dinners\DinnerForm.ascx et de la mettre à jour pour inclure notre nouvelle carte partielle. Voici à quoi ressemblera le DinnerForm mis à jour une fois le mappage ajouté (Remarque : les éléments de formulaire HTML sont omis dans l’extrait de code ci-dessous, par souci de concision) :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Le DinnerForm partiel ci-dessus prend un objet de type « DinnerFormViewModel » comme type de modèle (parce qu’il a besoin à la fois d’un objet dîner, ainsi que d’un SelectList pour remplir le DropDownList des pays). Notre mappage partiel a juste besoin d’un objet de type « dîner » comme type de modèle, et par conséquent, lorsque nous rendons le mappage partiel, nous transmettons uniquement la sous-propriété de dîner de DinnerFormViewModel :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

La fonction JavaScript que nous avons ajoutée au partiel utilise jQuery pour attacher un événement « Blur » à la zone de texte HTML « Address ». Vous avez probablement entendu parler des événements de « focus » qui se déclenchent lorsqu’un utilisateur clique sur des tabulations ou dans une zone de texte. L’inverse est un événement de « flou » qui se déclenche quand un utilisateur quitte une zone de texte. Le gestionnaire d’événements ci-dessus efface les valeurs de zone de texte de latitude et de longitude lorsque cela se produit, puis trace le nouvel emplacement d’adresse sur notre carte. Un gestionnaire d’événements de rappel que nous avons défini dans le fichier Map. js met à jour les zones de texte longitude et latitude de notre formulaire à l’aide des valeurs renvoyées par Virtual Earth en fonction de l’adresse que nous lui avons attribuée.

Et maintenant, lorsque nous exécutons à nouveau notre application et cliquons sur l’onglet « dîner de l’hôte », une carte par défaut s’affiche avec nos éléments de formulaire dîner standard :

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Lorsque nous entrons une adresse et que vous appuyez ensuite sur TAB, le mappage est mis à jour dynamiquement pour afficher l’emplacement et notre gestionnaire d’événements remplit les zones de texte Latitude/Longitude avec les valeurs d’emplacement :

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Si nous enregistrons le nouveau dîner, puis le rouvrez pour le modifier, nous constatons que l’emplacement de la carte s’affiche lorsque la page se charge :

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Chaque fois que le champ d’adresse est modifié, la carte et les coordonnées de latitude/longitude sont mises à jour.

Maintenant que la carte affiche l’emplacement de dîner, nous pouvons également modifier les champs de formulaire de latitude et de longitude pour qu’ils soient des zones de texte visibles en tant qu’éléments masqués (puisque le mappage est automatiquement mis à jour chaque fois qu’une adresse est entrée). Pour ce faire, nous allons passer de l’utilisation du programme d’assistance HTML HTML. TextBox () à l’utilisation de la méthode d’assistance HTML. Hidden () :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Désormais, nos formulaires sont un peu plus conviviaux et évitent d’afficher la latitude/longitude brute (tout en les stockant avec chaque dîner dans la base de données) :

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Intégration de la carte avec le mode Détails

Maintenant que nous disposons d’une carte intégrée à nos scénarios de création et de modification, nous allons également l’intégrer dans notre scénario de détails. Tout ce que nous devons faire, c’est d’appeler &lt;% html. RenderPartial (« Map »); %&gt; dans le mode Détails.

Voici ce que le code source de la vue complète des détails (avec intégration de la carte) ressemble à ceci :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Désormais, lorsqu’un utilisateur accède à une URL/Dinners/Details/[id], il voit des détails sur le dîner, l’emplacement du dîner sur la carte (avec un code confidentiel qui, lorsqu’il est placé sur, affiche le titre du dîner et de l’adresse) et a un lien AJAX vers RSVP pour celui-ci :

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implémentation de la recherche d’emplacement dans la base de données et le référentiel

Pour terminer la mise en œuvre d’AJAX, nous allons ajouter une carte à la page d’hébergement de l’application, qui permet aux utilisateurs de rechercher des dîners proches.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Nous commencerons par implémenter la prise en charge au sein de notre couche de base de données et de référentiel de données pour effectuer efficacement une recherche RADIUS basée sur l’emplacement pour les dîners. Nous pourrions utiliser les nouvelles [fonctionnalités géospatiales de SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) pour implémenter cela, ou vous pouvez également utiliser une approche de fonction SQL traitée par Gary Dryden dans l’article : [http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) et Rob cône blogué sur l’utilisation de avec LINQ to SQL ici : [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Pour implémenter cette technique, nous allons ouvrir le « Explorateur de serveurs » dans Visual Studio, sélectionner la base de données NerdDinner, puis cliquer avec le bouton droit sur le sous-nœud « fonctions » sous celui-ci et choisir de créer une nouvelle « fonction scalaire » :

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Nous allons ensuite coller la fonction DistanceBetween suivante :

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Nous allons ensuite créer une fonction table dans SQL Server que nous appelons « NearestDinners » :

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Cette fonction de table « NearestDinners » utilise la fonction d’assistance DistanceBetween pour retourner tous les dîners dans un rayon de 100 kilomètres de la latitude et de la longitude que nous fournissons :

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Pour appeler cette fonction, nous allons d’abord ouvrir le concepteur LINQ to SQL en double-cliquant sur le fichier NerdDinner. dbml dans le répertoire \Models :

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Nous allons ensuite faire glisser les fonctions NearestDinners et DistanceBetween sur le concepteur de LINQ to SQL, ce qui entraîne leur ajout en tant que méthodes sur notre classe de NerdDinnerDataContext LINQ to SQL :

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Nous pouvons ensuite exposer une méthode de requête « FindByLocation » sur notre classe DinnerRepository qui utilise la fonction NearestDinner pour retourner les dîners à venir qui se trouvent dans 100 kilomètres de l’emplacement spécifié :

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implémentation d’une méthode d’action de recherche AJAX basée sur JSON

Nous allons maintenant implémenter une méthode d’action de contrôleur qui tire parti de la nouvelle méthode de dépôt FindByLocation () pour retourner une liste de données de dîner qui peut être utilisée pour remplir un mappage. Cette méthode d’action renverra les données du dîner au format JSON (JavaScript Object Notation) afin qu’elles puissent être facilement manipulées à l’aide de JavaScript sur le client.

Pour l’implémenter, nous allons créer une nouvelle classe « SearchController » en cliquant avec le bouton droit sur le répertoire \Controllers et en choisissant la commande de menu Add-&gt;Controller. Nous allons ensuite implémenter une méthode d’action « SearchByLocation » dans la nouvelle classe SearchController, comme ci-dessous :

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

La méthode d’action SearchByLocation de SearchController appelle en interne la méthode FindByLocation sur DinnerRepository pour obtenir une liste de dîners proches. Au lieu de retourner directement les objets du dîner au client, il retourne à la place les objets JsonDinner. La classe JsonDinner expose un sous-ensemble de propriétés de dîner (par exemple : pour des raisons de sécurité, elle ne divulgue pas les noms des personnes qui ont RSVP pour un dîner). Il comprend également une propriété RSVPCount qui n’existe pas sur le dîner, et qui est calculée dynamiquement en comptant le nombre d’objets RSVP associés à un dîner particulier.

Nous utilisons ensuite la méthode d’assistance JSON () sur la classe de base du contrôleur pour retourner la séquence de dîners à l’aide d’un format de câble JSON. JSON est un format de texte standard pour représenter des structures de données simples. Voici un exemple de ce à quoi ressemble une liste JSON de deux objets JsonDinner lorsqu’elle est retournée à partir de notre méthode d’action :

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Appel de la méthode AJAX basée sur JSON à l’aide de jQuery

Nous sommes maintenant prêts à mettre à jour la page d’hébergement de l’application NerdDinner pour utiliser la méthode d’action SearchByLocation de SearchController. Pour ce faire, nous allons ouvrir le modèle de vue/Views/Home/Index.aspx et le mettre à jour pour qu’il dispose d’une zone de texte, d’un bouton de recherche, de notre carte et d’un &lt;élément&gt; div nommé dinnerList :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Nous pouvons ensuite ajouter deux fonctions JavaScript à la page :

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

La première fonction JavaScript charge la carte lorsque la page se charge pour la première fois. La deuxième fonction JavaScript associe un gestionnaire d’événements Click JavaScript sur le bouton de recherche. Lorsque le bouton est enfoncé, il appelle la fonction JavaScript FindDinnersGivenLocation () que nous ajouterons à notre fichier Map. js :

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Cette fonction FindDinnersGivenLocation () appelle map. Recherchez () sur le contrôle Virtual Earth pour le centrer à l’emplacement indiqué. Lorsque le service de carte Virtual Earth retourne, la carte. La méthode Find () appelle la méthode de rappel callbackUpdateMapDinners que nous lui avons passée comme argument final.

La méthode callbackUpdateMapDinners () est l’endroit où le travail réel est effectué. Elle utilise la méthode d’assistance $. poster () de jQuery pour effectuer un appel AJAX à la méthode d’action SearchByLocation () de SearchController, en lui transmettant la latitude et la longitude de la carte qui vient d’être centrée. Il définit une fonction inline qui est appelée lorsque la méthode d’assistance $. postérieure () se termine, et les résultats des dîners au format JSON retournés par la méthode d’action SearchByLocation () lui sont passés à l’aide d’une variable appelée « dîners ». Il effectue ensuite une boucle sur chaque dîner renvoyé et utilise la latitude et la longitude du dîner, ainsi que d’autres propriétés pour ajouter un nouveau code confidentiel sur la carte. Il ajoute également une entrée de dîner à la liste HTML des dîners à droite de la carte. Il associe ensuite un événement de pointage à la liste des clics-infos et à la liste HTML afin que les détails du dîner s’affichent quand un utilisateur pointe dessus :

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

Et maintenant, lorsque nous exécutons l’application et que nous visitons la page d’hébergement, une carte s’affiche. Lorsque vous entrez le nom d’une ville, la carte affiche les dîners à venir à proximité :

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Le fait de pointer sur un dîner affiche des détails à son sujet.

En cliquant sur le titre de dîner dans la bulle ou sur le côté droit de la liste HTML, nous nous rendons au dîner, que nous pouvons ensuite éventuellement RSVP pour :

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Étape suivante

Nous avons maintenant implémenté toutes les fonctionnalités de l’application de notre application NerdDinner. Voyons maintenant comment nous pouvons activer les tests unitaires automatisés de l’informatique.

> [!div class="step-by-step"]
> [Précédent](use-ajax-to-deliver-dynamic-updates.md)
> [Suivant](enable-automated-unit-testing.md)
