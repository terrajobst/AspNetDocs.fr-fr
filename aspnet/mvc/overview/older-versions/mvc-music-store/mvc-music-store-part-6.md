---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Partie 6 : utilisation d’annotations de données pour la validation de modèle | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 6 couvre l’utilisation des annotations de données pour le modèle V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: bc031dd5be61cc6707c522f85f6af77a420c8b31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539276"
---
# <a name="part-6-using-data-annotations-for-model-validation"></a>Partie 6 : utilisation d’annotations de données pour la validation de modèle

par [Jon Galloway](https://github.com/jongalloway)

> Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.  
>   
> Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.  
>   
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 6 couvre l’utilisation des annotations de données pour la validation de modèle.

Nous avons un problème majeur avec nos formulaires de création et de modification : ils ne font pas de validation. Nous pouvons effectuer des opérations telles que laisser les champs obligatoires vides ou taper des lettres dans le champ Price, et la première erreur que nous allons voir provient de la base de données.

Nous pouvons facilement ajouter la validation à notre application en ajoutant des annotations de données à nos classes de modèle. Les annotations de données nous permettent de décrire les règles que nous voulons appliquer à nos propriétés de modèle, et ASP.NET MVC s’occupera de leur application et de l’affichage des messages appropriés à nos utilisateurs.

## <a name="adding-validation-to-our-album-forms"></a>Ajout de la validation à nos formulaires d’album

Nous allons utiliser les attributs d’annotation de données suivants :

- **Required** : indique que la propriété est un champ obligatoire
- **DisplayName** : définit le texte que nous souhaitons utiliser sur les champs de formulaire et les messages de validation
- **StringLength** : définit une longueur maximale pour un champ de chaîne.
- **Plage** : donne une valeur maximale et minimale pour un champ numérique
- **Bind** : répertorie les champs à exclure ou inclure lors de la liaison de valeurs de paramètre ou de formulaire à des propriétés de modèle
- **ScaffoldColumn** : permet de masquer les champs des formulaires de l’éditeur

*Remarque : pour plus d’informations sur la validation de modèle à l’aide des attributs d’annotation de données, consultez la documentation MSDN à l’adresse* [`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Ouvrez la classe album et ajoutez les instructions *using* suivantes en haut.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Ensuite, mettez à jour les propriétés pour ajouter des attributs d’affichage et de validation comme indiqué ci-dessous.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Alors que nous y sommes, nous avons également modifié le genre et l’artiste en propriétés virtuelles. Cela permet à Entity Framework de les charger en différé en fonction des besoins.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Une fois que vous avez ajouté ces attributs à notre modèle d’album, notre écran de création et de modification commence immédiatement à valider les champs et à utiliser les noms d’affichage que nous avons choisis (par exemple, URL de la pochette de l’album au lieu de AlbumArtUrl). Exécutez l’application et accédez à/StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Ensuite, nous allons rompre certaines règles de validation. Entrez un prix de 0 et laissez le titre vide. Quand vous cliquez sur le bouton créer, le formulaire s’affiche avec des messages d’erreur de validation indiquant les champs qui ne respectent pas les règles de validation que nous avons définies.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Test de la validation côté client

La validation côté serveur est très importante du point de vue de l’application, car les utilisateurs peuvent contourner la validation côté client. Toutefois, les formulaires de page Web qui implémentent uniquement la validation côté serveur présentent trois problèmes significatifs.

1. L’utilisateur doit attendre que le formulaire soit publié, validé sur le serveur et que la réponse soit envoyée à son navigateur.
2. L’utilisateur ne recevra pas de commentaires immédiats lorsqu’il corrigera un champ afin qu’il transmette maintenant les règles de validation.
3. Nous gaspillons les ressources du serveur pour exécuter la logique de validation au lieu de tirer parti du navigateur de l’utilisateur.

Heureusement, les modèles de l’échafaudage ASP.NET MVC 3 intègrent la validation côté client, ce qui ne nécessite aucun travail supplémentaire.

Si vous tapez une seule lettre dans le champ titre conforme aux exigences de validation, le message de validation est immédiatement supprimé.

![](mvc-music-store-part-6/_static/image3.png)

> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-5.md)
> [Suivant](mvc-music-store-part-7.md)
