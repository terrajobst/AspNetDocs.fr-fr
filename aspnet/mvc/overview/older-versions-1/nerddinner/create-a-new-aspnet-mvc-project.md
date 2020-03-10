---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Créer un nouveau projet MVC ASP.NET | Microsoft Docs
author: microsoft
description: L’étape 1 montre comment mettre en place la structure d’application NerdDinner de base.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 189ddc187fc83db14106b2da199ba12a70a32b45
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580933"
---
# <a name="create-a-new-aspnet-mvc-project"></a>Créer un projet ASP.NET MVC

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 1 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.
> 
> L’étape 1 montre comment mettre en place la structure d’application NerdDinner de base.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner étape 1 : fichier-&gt;nouveau projet

Nous allons commencer notre application NerdDinner en sélectionnant l’élément de menu **fichier-&gt;nouveau projet** dans visual Studio 2008 ou la version gratuite de Visual Web developer 2008 Express.

La boîte de dialogue « Nouveau projet » s’affiche. Pour créer une application ASP.NET MVC, nous allons sélectionner le nœud « Web » sur le côté gauche de la boîte de dialogue, puis choisir le modèle de projet « application Web ASP.NET MVC » sur la droite :

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Important : Assurez-vous que vous avez téléchargé et installé ASP.NET MVC ; sinon, il n’apparaîtra pas dans la boîte de dialogue Nouveau projet. Vous pouvez utiliser v2 de l' [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) si vous ne l’avez pas encore installé (ASP.NET MVC est disponible dans la section «plateformes et runtimes de&gt;Web).*

Nous allons nommer le nouveau projet que nous allons créer « NerdDinner », puis cliquer sur le bouton « OK » pour le créer.

Lorsque nous cliquons sur « OK », Visual Studio affiche une boîte de dialogue supplémentaire qui nous invite à créer également un projet de test unitaire pour la nouvelle application. Ce projet de test unitaire nous permet de créer des tests automatisés qui vérifient les fonctionnalités et le comportement de notre application (nous aborderons la procédure à suivre plus loin dans ce didacticiel).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

La liste déroulante infrastructure de test dans la boîte de dialogue ci-dessus est remplie avec tous les modèles de projet de test unitaire ASP.NET MVC disponibles installés sur l’ordinateur. Les versions peuvent être téléchargées pour NUnit, MBUnit et XUnit. L’infrastructure de test unitaire Visual Studio intégrée est également prise en charge.

*Remarque : l’infrastructure de tests unitaires Visual Studio est uniquement disponible avec Visual Studio 2008 Professional et versions ultérieures. Si vous utilisez VS 2008 Standard Edition ou Visual Web Developer 2008 Express, vous devez télécharger et installer les extensions NUnit, MBUnit ou XUnit pour ASP.NET MVC afin de pouvoir afficher cette boîte de dialogue. La boîte de dialogue ne s’affiche pas si aucun Framework de test n’est installé.*

Nous allons utiliser le nom par défaut « NerdDinner. tests » pour le projet de test que nous créons, et utiliser l’option d’infrastructure « test unitaire Visual Studio ». Lorsque nous cliquons sur le bouton « OK », Visual Studio crée une solution pour nous avec deux projets en informatique : un pour notre application Web et un pour nos tests unitaires :

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Examen de la structure de répertoires NerdDinner

Quand vous créez une application MVC ASP.NET avec Visual Studio, elle ajoute automatiquement un certain nombre de fichiers et de répertoires au projet :

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Par défaut, les projets MVC ASP.NET ont six répertoires de niveau supérieur :

| **Répertoire** | **Fonction** |
| --- | --- |
| **/Controllers** | Où vous placez les classes de contrôleur qui gèrent les demandes d’URL |
| **/Models** | Où vous placez des classes qui représentent et manipulent des données |
| **/Views** | Où vous placez les fichiers de modèle d’interface utilisateur chargés du rendu de la sortie |
| **/Scripts** | Où placer les scripts et les fichiers de la bibliothèque JavaScript (. js) |
| **/Content** | Où vous placez les fichiers CSS et image, ainsi que d’autres contenus non dynamiques/non-JavaScript |
| **/App\_données** | Où vous stockez les fichiers de données que vous souhaitez lire/écrire. |

ASP.NET MVC ne nécessite pas cette structure. En fait, les développeurs qui travaillent sur des applications volumineuses partitionnent généralement l’application entre plusieurs projets pour faciliter leur gestion (par exemple : les classes de modèle de données sont souvent placées dans un projet de bibliothèque de classes distinct de l’application Web). Toutefois, la structure de projet par défaut fournit une bonne Convention de répertoire par défaut que nous pouvons utiliser pour assurer le bon fonctionnement de notre application.

Lorsque nous développons le répertoire/Controllers, vous constaterez que Visual Studio a ajouté deux classes de contrôleur – HomeController et AccountController – par défaut au projet :

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Lorsque nous développons le répertoire/views, vous trouverez trois sous-répertoires (/Home,/Account et/Shared), ainsi que plusieurs fichiers de modèles qu’ils contiennent, qui ont également été ajoutés au projet par défaut :

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Lorsque nous développons les répertoires/content et/scripts, nous trouvons un fichier site. CSS qui est utilisé pour appliquer un style à tous les codes HTML sur le site, ainsi que des bibliothèques JavaScript capables d’activer la prise en charge d’AJAX et de jQuery dans l’application :

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Lorsque nous développons le projet NerdDinner. tests, vous trouverez deux classes qui contiennent des tests unitaires pour nos classes de contrôleur :

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Ces fichiers par défaut ajoutés par Visual Studio fournissent une structure de base pour une application opérationnelle, avec la page d’installation, à propos de la page, des pages de connexion de compte/de déconnexion/inscription et une page d’erreur non gérée (tout en étant connecté et prêt à l’emploi).

### <a name="running-the-nerddinner-application"></a>Exécution de l’application NerdDinner

Pour exécuter le projet, vous pouvez choisir les éléments de menu Déboguer **-&gt;démarrer le débogage** ou **Déboguer-&gt;exécuter sans débogage** :

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Cette opération lance le serveur Web ASP.NET intégré qui est fourni avec Visual Studio et exécute notre application :

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Vous trouverez ci-dessous la page d’hébergement de notre nouveau projet (URL : « / ») lors de son exécution :

![](create-a-new-aspnet-mvc-project/_static/image11.png)

En cliquant sur l’onglet « à propos de », vous affichez une page à propos de (URL : « /Home/About ») :

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Cliquer sur le lien « connexion » en haut à droite nous amène à une page de connexion (URL : « /Account/LogOn »)

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Si nous n’avons pas de compte de connexion, nous pouvons cliquer sur le lien register (URL : « /Account/Register ») pour en créer un :

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Le code permettant d’implémenter la fonctionnalité de démarrage, d’informations et de déconnexion/inscription ci-dessus a été ajouté par défaut lors de la création de notre nouveau projet. Nous l’utiliserons comme point de départ de notre application.

### <a name="testing-the-nerddinner-application"></a>Test de l’application NerdDinner

Si vous utilisez l’édition Professional ou une version ultérieure de Visual Studio 2008, nous pouvons utiliser la prise en charge intégrée de l’IDE de test unitaire dans Visual Studio pour tester le projet :

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Le choix de l’une des options ci-dessus permet d’ouvrir le volet « Résultats des tests » dans l’IDE et de nous fournir un état de réussite/échec sur les 27 tests unitaires inclus dans le nouveau projet, qui couvrent les fonctionnalités intégrées :

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Plus loin dans ce didacticiel, nous aborderons les tests automatisés et ajoutons des tests unitaires supplémentaires qui couvrent les fonctionnalités d’application que nous implémentons.

### <a name="next-step"></a>Étape suivante

Nous disposons désormais d’une structure d’application de base en place. Créons maintenant [une base de données pour stocker les données](create-a-database.md)de l’application.

> [!div class="step-by-step"]
> [Précédent](introducing-the-nerddinner-tutorial.md)
> [Suivant](create-a-database.md)
