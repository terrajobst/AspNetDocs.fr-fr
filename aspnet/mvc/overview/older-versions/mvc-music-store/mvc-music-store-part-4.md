---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Partie 4 : accès aux modèles et aux données | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 4 couvre les modèles et l’accès aux données.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 402be340f1ea3344675e7b859cea8c5130cfc8ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559674"
---
# <a name="part-4-models-and-data-access"></a>Partie 4 : modèles et accès aux données

par [Jon Galloway](https://github.com/jongalloway)

> Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.  
>   
> Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.
> 
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 4 couvre les modèles et l’accès aux données.

Jusqu’à présent, nous venons de transmettre des « données factices » de nos contrôleurs à nos modèles de vue. Nous sommes maintenant prêts à raccorder une véritable base de données. Dans ce didacticiel, nous allons aborder l’utilisation de SQL Server Compact édition (souvent appelée SQL CE) comme moteur de base de données. SQL CE est une base de données gratuite, incorporée et basée sur des fichiers, qui ne nécessite aucune installation ni configuration, ce qui en fait la solution la plus pratique pour le développement local.

## <a name="database-access-with-entity-framework-code-first"></a>Accès à la base de données avec Entity Framework code-First

Nous allons utiliser la prise en charge Entity Framework (EF) qui est incluse dans les projets ASP.NET MVC 3 pour interroger et mettre à jour la base de données. EF est une API de données de mappage relationnel objet (ORM) flexible qui permet aux développeurs d’interroger et de mettre à jour les données stockées dans une base de données d’une manière orientée objet.

Entity Framework version 4 prend en charge un paradigme de développement appelé code-First. Code-First vous permet de créer un objet de modèle en écrivant des classes simples (également appelées POCO à partir d’objets CLR « Plain-Old ») et peut même créer la base de données à la volée à partir de vos classes.

### <a name="changes-to-our-model-classes"></a>Modifications apportées à nos classes de modèle

Nous allons tirer parti de la fonctionnalité de création de base de données dans Entity Framework dans ce didacticiel. Avant cela, cependant, nous allons apporter quelques modifications mineures à nos classes de modèle pour ajouter des éléments que nous utiliserons plus tard.

#### <a name="adding-the-artist-model-classes"></a>Ajout des classes du modèle Artist

Nos albums seront associés à des artistes. nous allons donc ajouter une classe de modèle simple pour décrire un artiste. Ajoutez une nouvelle classe au dossier Models nommé Artist.cs à l’aide du code indiqué ci-dessous.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Mise à jour de nos classes de modèle

Mettez à jour la classe album comme indiqué ci-dessous.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Ensuite, effectuez les mises à jour suivantes de la classe genre.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-app_data-folder"></a>Ajout du dossier de données de l’application\_

Nous allons ajouter une application\_répertoire de données à notre projet pour stocker les fichiers de base de données SQL Server Express. Les données d'\_d’application sont un répertoire spécial dans ASP.NET qui possède déjà les autorisations d’accès de sécurité appropriées pour l’accès aux bases de données. Dans le menu projet, sélectionnez Ajouter un dossier ASP.NET, puis application\_données.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Création d’une chaîne de connexion dans le fichier Web. config

Nous allons ajouter quelques lignes au fichier de configuration du site Web afin que Entity Framework sache comment se connecter à notre base de données. Double-cliquez sur le fichier Web. config situé à la racine du projet.

![](mvc-music-store-part-4/_static/image2.png)

Faites défiler vers le bas de ce fichier et ajoutez une section &lt;connectionStrings&gt; juste au-dessus de la dernière ligne, comme indiqué ci-dessous.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Ajout d’une classe de contexte

Cliquez avec le bouton droit sur le dossier Models et ajoutez une nouvelle classe nommée MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Cette classe représente le contexte de base de données Entity Framework et gère nos opérations de création, lecture, mise à jour et suppression pour nous. Le code de cette classe est indiqué ci-dessous.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

C’est tout ! il n’y a pas d’autres configurations, interfaces spéciales, etc. En étendant la classe de base DbContext, notre classe MusicStoreEntities est en mesure de gérer nos opérations de base de données pour nous. Maintenant que nous avons raccroché, nous allons ajouter quelques propriétés à nos classes de modèle pour tirer parti de certaines des informations supplémentaires de notre base de données.

### <a name="adding-our-store-catalog-data"></a>Ajout de données de catalogue du magasin

Nous allons tirer parti d’une fonctionnalité de Entity Framework qui ajoute des données de valeur initiale à une base de données nouvellement créée. Cette opération préremplira le catalogue du magasin avec une liste de genres, d’artistes et d’albums. Le téléchargement MvcMusicStore-Assets. zip, qui comprenait nos fichiers de conception de site utilisés précédemment dans ce didacticiel, contient un fichier de classe avec ces données de départ, situé dans un dossier nommé code.

Dans le dossier code/Models, localisez le fichier SampleData.cs et déposez-le dans le dossier Models de notre projet, comme indiqué ci-dessous.

![](mvc-music-store-part-4/_static/image4.png)

À présent, nous devons ajouter une ligne de code pour indiquer à Entity Framework sur cette classe SampleData. Double-cliquez sur le fichier global. asax à la racine du projet pour l’ouvrir et ajoutez la ligne suivante au début de la méthode de démarrage de l’application\_.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

À ce stade, nous avons terminé le travail nécessaire pour configurer Entity Framework pour notre projet.

## <a name="querying-the-database"></a>Interrogation de la base de données

Nous allons maintenant mettre à jour notre StoreController afin qu’au lieu d’utiliser des « données factices », elle appelle la base de données pour interroger toutes ses informations. Nous allons commencer par déclarer un champ sur le **StoreController** pour contenir une instance de la classe MusicStoreEntities, nommée storeDB :

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Mise à jour de l’index du magasin pour interroger la base de données

La classe MusicStoreEntities est gérée par le Entity Framework et expose une propriété de collection pour chaque table de notre base de données. Nous allons mettre à jour l’action d’index de StoreController pour récupérer tous les genres de notre base de données. Auparavant, nous avons effectué ce codage en dur des données de chaîne. À présent, nous pouvons simplement utiliser le contexte de Entity Framework de la collection :

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Aucune modification ne doit être apportée à notre modèle de vue, car nous revenons toujours à la même StoreIndexViewModel que celle que nous avons retournée avant-nous nous contentons de renvoyer des données en direct à partir de notre base de données maintenant.

Lorsque nous exécutons à nouveau le projet et que vous accédez à l’URL « /Store », nous voyons maintenant une liste de tous les genres dans notre base de données :

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Mise à jour de la navigation et des détails du magasin pour utiliser des données actives

Avec la méthode d’action/Store/Browse ? genre = *[some-genre]* , nous recherchons un genre par nom. Nous attendons un seul résultat, puisque nous ne devrions jamais avoir deux entrées pour le même nom de genre, et donc nous pouvons utiliser. Extension unique () dans LINQ pour interroger l’objet genre approprié comme celui-ci (ne le tapez pas encore) :

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

La méthode unique prend une expression lambda en tant que paramètre, qui spécifie que nous souhaitons un objet de genre unique, de sorte que son nom corresponde à la valeur que nous avons définie. Dans le cas ci-dessus, nous chargeons un objet de genre unique avec une valeur de nom correspondant à Disco.

Nous allons tirer parti d’une fonctionnalité de Entity Framework qui nous permet d’indiquer d’autres entités associées que nous voulons charger également lorsque l’objet genre est récupéré. Cette fonctionnalité est appelée mise en forme des résultats de la requête et nous permet de réduire le nombre de fois où nous avons besoin d’accéder à la base de données pour récupérer toutes les informations dont nous avons besoin. Nous souhaitons prérécupérer les albums pour le genre que nous récupérons. nous allons donc mettre à jour notre requête pour inclure from genres. include ("Albums") pour indiquer que nous voulons également les albums associés. Cette opération est plus efficace, car elle récupère les données de genre et d’album dans une demande de base de données unique.

Avec les explications, voici à quoi ressemble notre action de contrôleur de navigation mise à jour :

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Nous pouvons maintenant mettre à jour la vue de navigation dans le magasin pour afficher les albums disponibles dans chaque genre. Ouvrez le modèle de vue (situé dans/Views/Store/Browse.cshtml) et ajoutez une liste à puces d’albums comme indiqué ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

L’exécution de notre application et l’exploration de/Store/Browse ? genre = jazz montrent que nos résultats sont désormais extraits de la base de données, affichant tous les albums dans le genre sélectionné.

![](mvc-music-store-part-4/_static/image2.jpg)

Nous allons apporter la même modification à notre URL/Store/Details/[id] et remplacer nos données fictives par une requête de base de données qui charge un album dont l’ID correspond à la valeur du paramètre.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

L’exécution de notre application et l’exploration de/Store/Details/1 montrent que nos résultats sont désormais extraits de la base de données.

![](mvc-music-store-part-4/_static/image5.png)

Maintenant que la page de détails du magasin est configurée pour afficher un album par l’ID de l’album, nous allons mettre à jour la vue **Parcourir** pour établir un lien vers le mode Détails. Nous allons utiliser HTML. ActionLink, exactement comme nous l’avons fait pour établir une liaison à partir de l’index de Store à la fin de la section précédente. La source complète de la vue Parcourir s’affiche ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Nous sommes maintenant en mesure de naviguer à partir de la page de votre boutique vers une page de genre, qui répertorie les albums disponibles, et en cliquant sur un album, vous pouvez afficher les détails de cet album.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-3.md)
> [Suivant](mvc-music-store-part-5.md)
