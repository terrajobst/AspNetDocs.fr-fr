---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Partie 2 : contrôleurs | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 2 couvre les contrôleurs.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9dc2226f4951d4bed122df37d35bbb94730a00ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559877"
---
# <a name="part-2-controllers"></a>Partie 2 : contrôleurs

par [Jon Galloway](https://github.com/jongalloway)

> Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.  
>   
> Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.  
>   
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 2 couvre les contrôleurs.

Avec les infrastructures Web traditionnelles, les URL entrantes sont généralement mappées à des fichiers sur disque. Par exemple : une requête pour une URL telle que « /Products.aspx » ou « /Products.php » peut être traitée par un fichier « Products. aspx » ou « Products. php ».

Les frameworks MVC basés sur le Web mappent les URL au code du serveur d’une manière légèrement différente. Au lieu de mapper les URL entrantes aux fichiers, elles mappent les URL aux méthodes sur les classes. Ces classes sont appelées « contrôleurs » et sont responsables du traitement des demandes HTTP entrantes, de la gestion des entrées d’utilisateur, de la récupération et de l’enregistrement des données et de la détermination de la réponse à renvoyer au client (afficher le code HTML, télécharger un fichier, rediriger vers un autre URL, etc.).

## <a name="adding-a-homecontroller"></a>Ajout d’un HomeController

Nous allons commencer notre application MVC Music Store en ajoutant une classe de contrôleur qui traitera les URL vers la page d’hébergement de notre site. Nous allons suivre les conventions d’affectation de noms par défaut de ASP.NET MVC et l’appeler HomeController.

Cliquez avec le bouton droit sur le dossier « Controllers » dans le Explorateur de solutions et sélectionnez « Ajouter », puis le « contrôleur... » commande

![](mvc-music-store-part-2/_static/image1.jpg)

La boîte de dialogue « Ajouter un contrôleur » s’affiche. Nommez le contrôleur « HomeController » et appuyez sur le bouton Ajouter.

![](mvc-music-store-part-2/_static/image1.png)

Cela créera un nouveau fichier, HomeController.cs, avec le code suivant :

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Pour démarrer le plus simplement possible, remplacez la méthode d’index par une méthode simple qui retourne simplement une chaîne. Nous allons apporter deux modifications :

- Modifiez la méthode pour retourner une chaîne au lieu d’un ActionResult
- Modifiez l’instruction return pour retourner « hello from the Accueil »

La méthode doit maintenant ressembler à ceci :

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Exécution de l'application

À présent, nous allons exécuter le site. Nous pouvons démarrer notre serveur Web et essayer le site en utilisant l’un des éléments suivants :

- Choisissez l’élément de menu Déboguer ⇨ démarrer le débogage
- Cliquez sur le bouton représentant une flèche verte dans la barre d’outils ![](mvc-music-store-part-2/_static/image2.jpg)
- Utilisez le raccourci clavier F5.

À l’aide de l’une des étapes ci-dessus, nous allons compiler notre projet, puis provoquer le démarrage de la Serveur de développement ASP.NET intégrée à Visual Web Developer. Une notification s’affiche dans le coin inférieur de l’écran pour indiquer que le Serveur de développement ASP.NET a démarré et indique le numéro de port sous lequel il s’exécute.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer ouvre automatiquement une fenêtre de navigateur dont l’URL pointe vers notre serveur Web. Cela nous permettra de tester rapidement notre application Web :

![](mvc-music-store-part-2/_static/image3.png)

Bien, c’était assez rapide : nous avons créé un nouveau site Web, ajouté une fonction à trois lignes, et nous obtenons du texte dans un navigateur. Pas de fusée science, mais c’est un début.

*Remarque : Visual Web Developer comprend le Serveur de développement ASP.NET, qui exécutera votre site Web sur un numéro de « port » gratuit. Dans la capture d’écran ci-dessus, le site s’exécute sur `http://localhost:26641/`, donc il utilise le port 26641. Le numéro de votre port sera différent. Lorsque nous parlerons des URL comme/Store/Browse dans ce didacticiel, celles-ci seront placées après le numéro de port. En supposant qu’il s’agit d’un numéro de port de 26641, la navigation vers/Store/Browse signifie `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Ajout d’un StoreController

Nous avons ajouté un contrôle HomeController simple qui implémente la page d’hébergement de notre site. Nous allons maintenant ajouter un autre contrôleur que nous allons utiliser pour implémenter la fonctionnalité de navigation de notre magasin musical. Notre contrôleur de magasin prend en charge trois scénarios :

- Page de liste des genres musicaux dans notre magasin de musique
- Une page de navigation qui répertorie tous les albums musicaux dans un genre particulier
- Page de détails qui affiche des informations sur un album de musique spécifique

Nous allons commencer par ajouter une nouvelle classe StoreController. Si vous ne l’avez pas encore fait, arrêtez l’exécution de l’application en fermant le navigateur ou en sélectionnant l’élément de menu Déboguer ⇨ arrêter le débogage.

Ajoutez maintenant un nouveau StoreController. Comme nous l’avons fait avec HomeController, nous allons le faire en cliquant avec le bouton droit sur le dossier « Controllers » dans le Explorateur de solutions et en choisissant l’élément de menu Add-&gt;Controller.

![](mvc-music-store-part-2/_static/image4.png)

Notre nouvelle StoreController a déjà une méthode « index ». Nous allons utiliser cette méthode « index » pour implémenter notre page de listing qui répertorie tous les genres de notre magasin musical. Nous allons également ajouter deux méthodes supplémentaires pour implémenter les deux autres scénarios que nous souhaitons que notre StoreController gère : parcourir et détails.

Ces méthodes (index, navigation et détails) au sein de notre contrôleur sont appelées « actions de contrôleur », et comme vous l’avez déjà vu avec la méthode d’action HomeController. index (), leur travail est de répondre aux demandes d’URL et (en général) de déterminer le contenu doit être renvoyé au navigateur ou à l’utilisateur qui a appelé l’URL.

Nous allons démarrer notre implémentation de StoreController en modifiant la méthode theIndex () pour retourner la chaîne « Hello from Store. index () » et nous ajouterons des méthodes similaires pour Browse () et Details () :

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Réexécutez le projet et parcourez les URL suivantes :

- /Store
- /Store/Browse
- /Store/Details

L’accès à ces URL appellera les méthodes d’action dans notre contrôleur et renverra les réponses de chaîne :

![](mvc-music-store-part-2/_static/image5.png)

C’est parfait, mais il s’agit simplement de chaînes constantes. Nous allons les rendre dynamiques, afin qu’ils prennent les informations de l’URL et les affichent dans la sortie de la page.

Tout d’abord, nous allons modifier la méthode d’action Browse pour récupérer une valeur QueryString à partir de l’URL. Pour ce faire, nous pouvons ajouter un paramètre « genre » à notre méthode d’action. Lorsque nous procédons ainsi, ASP.NET MVC passe automatiquement tout paramètre de publication QueryString ou de formulaire nommé « genre » à notre méthode d’action lorsqu’il est appelé.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Remarque : nous utilisons la méthode de l’utilitaire HttpUtility. HtmlEncode pour assainir les entrées utilisateur. Cela empêche les utilisateurs d’injecter JavaScript dans notre vue avec un lien tel que/Store/Browse ? Genre =&lt;script&gt;Window. Location = 'http://hackersite.com'&lt;/script&gt;.*

Nous allons maintenant accéder à/Store/Browse ? Genre = Disco

![](mvc-music-store-part-2/_static/image6.png)

Nous allons ensuite modifier l’action détails pour lire et afficher un paramètre d’entrée nommé ID. Contrairement à la méthode précédente, nous n’incorporerons pas la valeur d’ID en tant que paramètre QueryString. Au lieu de cela, nous l’incorporons directement dans l’URL. Par exemple:/Store/Details/5.

ASP.NET MVC nous permet de le faire facilement sans avoir à configurer quoi que ce soit. La Convention de routage par défaut de ASP.NET MVC consiste à traiter le segment d’une URL après le nom de la méthode d’action en tant que paramètre nommé « ID ». Si votre méthode d’action a un paramètre nommé ID, ASP.NET MVC passe automatiquement le segment d’URL à vous en tant que paramètre.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Exécutez l’application et accédez à/Store/Details/5 :

![](mvc-music-store-part-2/_static/image7.png)

Récapitulons ce que nous avons fait jusqu’à présent :

- Nous avons créé un nouveau projet MVC ASP.NET dans Visual Web Developer
- Nous avons abordé la structure de dossiers de base d’une application ASP.NET MVC.
- Nous avons appris à exécuter notre site Web à l’aide de l’Serveur de développement ASP.NET
- Nous avons créé deux classes de contrôleur : un HomeController et un StoreController
- Nous avons ajouté des méthodes d’action à nos contrôleurs qui répondent aux demandes d’URL et renvoient du texte au navigateur.

> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-1.md)
> [Suivant](mvc-music-store-part-3.md)
