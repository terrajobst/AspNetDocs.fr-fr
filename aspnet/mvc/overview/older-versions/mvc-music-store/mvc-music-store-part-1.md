---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Partie 1 : vue d’ensemble et > de fichier nouveau projet | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La première partie couvre la vue d’ensemble et le > de fichiers de nouveau projet.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 48428ff4ab5888253ed93ac41e79006eec823ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559982"
---
# <a name="part-1-overview-and-file-new-project"></a>Partie 1 : vue d’ensemble et > un nouveau projet de fichier

par [Jon Galloway](https://github.com/jongalloway)

> Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.  
>   
> Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.  
>   
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La première partie couvre la vue d’ensemble et le&gt;de fichiers de nouveau projet.

## <a name="overview"></a>Présentation

Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Web Developer pour le développement Web. Nous allons démarrer lentement, de sorte que l’expérience de développement Web au niveau débutant est correcte.

L’application que nous allons créer est un magasin de musique simple. L’application comprend trois parties principales : achat, extraction et administration.

![](mvc-music-store-part-1/_static/image1.jpg)

Les visiteurs peuvent parcourir les albums par genre :

![](mvc-music-store-part-1/_static/image2.jpg)

Ils peuvent afficher un seul album et l’ajouter à leur panier :

![](mvc-music-store-part-1/_static/image3.jpg)

Ils peuvent examiner leur panier, en supprimant tous les éléments qu’ils ne souhaitent plus :

![](mvc-music-store-part-1/_static/image4.jpg)

En poursuivant l’extraction, vous êtes invité à vous connecter ou à s’inscrire pour obtenir un compte d’utilisateur.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Après avoir créé un compte, il peut terminer la commande en remplissant les informations d’expédition et de paiement. Pour simplifier les choses, nous faisons une promotion exceptionnelle : tout est gratuit s’ils entrent dans le code de promotion « gratuit » !

![](mvc-music-store-part-1/_static/image5.jpg)

Après l’ordonnancement, un écran de confirmation simple s’affiche :

![](mvc-music-store-part-1/_static/image6.jpg)

En plus des pages orientées client, nous allons également créer une section administrateur qui affiche la liste des albums à partir desquels les administrateurs peuvent créer, modifier et supprimer des albums :

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. fichier&gt; nouveau projet

### <a name="installing-the-software"></a>Installation du logiciel

Ce didacticiel commence par créer un nouveau projet ASP.NET MVC 3 à l’aide de la version gratuite de Visual Web Developer 2010 Express (qui est gratuite), puis nous ajouterons de manière incrémentielle des fonctionnalités pour créer une application fonctionnelle complète. En cours de route, nous allons aborder l’accès aux bases de données, les scénarios de publication de formulaire, la validation des données, l’utilisation de pages maîtres pour une mise en page cohérente, l’utilisation d’AJAX pour les mises à jour et la validation des pages, la connexion des utilisateurs, etc.

Vous pouvez suivre la procédure pas à pas, ou vous pouvez télécharger l’application terminée à partir de [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

Vous pouvez utiliser Visual Studio 2010 SP1 ou Visual Web Developer 2010 Express SP1 (une version gratuite de Visual Studio 2010) pour générer l’application. Nous utiliserons le SQL Server Compact (également gratuit) pour héberger la base de données. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous.

- [Composants requis pour Visual Studio Web Developer Express SP1]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4,0]-inclut la prise en charge du runtime et des outils

### <a name="creating-a-new-aspnet-mvc-3-project"></a>Création d’un nouveau projet ASP.NET MVC 3

Nous allons commencer par sélectionner « nouveau projet » dans le menu fichier de Visual Web Developer. La boîte de dialogue Nouveau projet s’affiche.

![](mvc-music-store-part-1/_static/image5.png)

Nous allons sélectionner le groupe C# de modèles Web&gt; Visual sur la gauche, puis choisir le modèle « Application Web ASP.NET MVC 3 » dans la colonne centrale. Nommez votre projet MvcMusicStore et appuyez sur le bouton OK.

![](mvc-music-store-part-1/_static/image8.jpg)

Cette opération affiche une boîte de dialogue secondaire qui nous permet de définir des paramètres spécifiques à MVC pour notre projet. Sélectionnez les éléments suivants :

Modèle de projet-sélectionnez vide

Moteur d’affichage-sélectionner Razor

Utiliser la balise sémantique HTML5-activée

Vérifiez que vos paramètres sont comme indiqué ci-dessous, puis appuyez sur le bouton OK.

![](mvc-music-store-part-1/_static/image9.jpg)

Cela va créer notre projet. Jetons un coup d’œil aux dossiers qui ont été ajoutés à notre application dans le Explorateur de solutions sur le côté droit.

![](mvc-music-store-part-1/_static/image10.jpg)

Le modèle MVC 3 vide n’est pas complètement vide : il ajoute une structure de dossier de base :

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC utilise des conventions d’affectation de noms de base pour les noms de dossiers :

| **Dossier** | **Fonction** |
| --- | --- |
| **/Controllers** | Les contrôleurs répondent aux entrées du navigateur, décident de la marche à suivre et retournent une réponse à l’utilisateur. |
| **/Views** | Les vues contiennent nos modèles d’interface utilisateur |
| **/Models** | Les modèles contiennent et manipulent des données |
| **/Content** | Ce dossier contient nos images, CSS et tout autre contenu statique |
| **/Scripts** | Ce dossier contient nos fichiers JavaScript |

Ces dossiers sont inclus même dans une application ASP.NET MVC vide, car l’infrastructure ASP.NET MVC utilise par défaut une approche « Convention sur la configuration » et effectue certaines hypothèses par défaut en fonction des conventions d’affectation des noms de dossiers. Par exemple, les contrôleurs recherchent des vues dans le dossier vues par défaut sans avoir à les spécifier explicitement dans votre code. L’utilisation des conventions par défaut permet de réduire la quantité de code que vous devez écrire et peut également faciliter la compréhension de votre projet par d’autres développeurs. Nous expliquerons ces conventions plus en détail lors de la création de notre application.

> [!div class="step-by-step"]
> [Next](mvc-music-store-part-2.md)
