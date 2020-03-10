---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Partie 7 : appartenance et autorisation | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 7 couvre l’appartenance et l’autorisation.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a6a1a936e0ea29ea36721ba78f35845401f74c01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539199"
---
# <a name="part-7-membership-and-authorization"></a>Partie 7 : appartenance et autorisation

par [Jon Galloway](https://github.com/jongalloway)

> Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.  
>   
> Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.  
>   
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 7 couvre l’appartenance et l’autorisation.

Notre contrôleur Store Manager est actuellement accessible à toute personne visitant notre site. Modifions ceci pour restreindre l’autorisation aux administrateurs de site.

## <a name="adding-the-accountcontroller-and-views"></a>Ajout de AccountController et de vues

L’une des différences entre le modèle d’application Web ASP.NET MVC 3 complet et le modèle d’application Web vide ASP.NET MVC 3 est que le modèle vide n’inclut pas de contrôleur de compte. Nous allons ajouter un contrôleur de compte en copiant quelques fichiers à partir d’une nouvelle application ASP.NET MVC créée à partir du modèle d’application Web ASP.NET MVC 3 complet.

Créez une nouvelle application ASP.NET MVC à l’aide du modèle d’application Web ASP.NET MVC 3 complet et copiez les fichiers suivants dans les mêmes répertoires de notre projet :

1. Copier AccountController.cs dans le répertoire Controllers
2. Copier AccountModels dans le répertoire des modèles
3. Créer un répertoire de compte dans le répertoire views et copier les quatre affichages dans

Modifiez l’espace de noms pour les classes de contrôleur et de modèle afin qu’elles commencent par MvcMusicStore. La classe AccountController doit utiliser l’espace de noms MvcMusicStore. Controllers, et la classe AccountModels doit utiliser l’espace de noms MvcMusicStore. Models.

*Remarque : ces fichiers sont également disponibles dans le téléchargement MvcMusicStore-Assets. zip à partir duquel nous avons copié nos fichiers de conception de site au début du didacticiel. Les fichiers d’appartenance se trouvent dans le répertoire de code.*

La solution mise à jour doit ressembler à ce qui suit :

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Ajout d’un utilisateur administratif à l’aide du site de configuration ASP.NET

Avant de demander une autorisation sur notre site Web, nous devrons créer un utilisateur avec accès. Le moyen le plus simple de créer un utilisateur consiste à utiliser le site Web de configuration ASP.NET intégré.

Lancez le site Web de configuration ASP.NET en cliquant sur le suivi de l’icône dans la Explorateur de solutions.

![](mvc-music-store-part-7/_static/image2.png)

Un site Web de configuration est lancé. Cliquez sur l’onglet sécurité sur l’écran d’accueil, puis cliquez sur le lien activer les rôles au centre de l’écran.

![](mvc-music-store-part-7/_static/image3.png)

Cliquez sur le lien « créer ou gérer des rôles ».

![](mvc-music-store-part-7/_static/image4.png)

Entrez « administrateur » comme nom de rôle, puis cliquez sur le bouton Ajouter un rôle.

![](mvc-music-store-part-7/_static/image5.png)

Cliquez sur le bouton précédent, puis sur le lien créer un utilisateur sur le côté gauche.

![](mvc-music-store-part-7/_static/image6.png)

Renseignez les champs d’informations utilisateur sur la gauche à l’aide des informations suivantes :

| **Champ** | **Valeur** |
| --- | --- |
| **Nom d'utilisateur** | Administrateur |
| **Mot de passe** | password123! |
| **Confirmer le mot de passe** | password123! |
| **Message électronique** | (toute adresse de messagerie fonctionnera) |
| **Question de sécurité** | (tout ce que vous aimez) |
| **Réponse de sécurité** | (tout ce que vous aimez) |

*Remarque : vous pouvez bien sûr utiliser le mot de passe de votre choix. Les paramètres de sécurité de mot de passe par défaut requièrent un mot de passe de 7 caractères et contiennent un caractère non alphanumérique.*

Sélectionnez le rôle administrateur pour cet utilisateur, puis cliquez sur le bouton créer un utilisateur.

![](mvc-music-store-part-7/_static/image7.png)

À ce stade, vous devriez voir un message indiquant que l’utilisateur a été créé avec succès.

![](mvc-music-store-part-7/_static/image8.png)

Vous pouvez maintenant fermer la fenêtre du navigateur.

## <a name="role-based-authorization"></a>Autorisation basée sur les rôles

À présent, nous pouvons limiter l’accès à StoreManagerController à l’aide de l’attribut [Authorize], en spécifiant que l’utilisateur doit avoir le rôle d’administrateur pour accéder à n’importe quelle action de contrôleur dans la classe.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Remarque : l’attribut [Authorize] peut être placé sur des méthodes d’action spécifiques, ainsi que au niveau de la classe du contrôleur.*

À présent, la navigation vers/StoreManager affiche une boîte de dialogue de connexion :

![](mvc-music-store-part-7/_static/image9.png)

Une fois connecté avec notre nouveau compte d’administrateur, nous pouvons accéder à l’écran de modification de l’album comme auparavant.

> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-6.md)
> [Suivant](mvc-music-store-part-8.md)
