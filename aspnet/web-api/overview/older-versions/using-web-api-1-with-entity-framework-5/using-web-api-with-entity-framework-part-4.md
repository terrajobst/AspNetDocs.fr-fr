---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Partie 4 : ajout d’une vue d’administration | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 664aeb33031e933322886a6d6bdd989277e9fda2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556013"
---
# <a name="part-4-adding-an-admin-view"></a>Partie 4 : ajout d’une vue d’administration

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Ajouter une vue d’administration

À présent, nous allons activer le côté client et ajouter une page qui peut consommer des données du contrôleur d’administration. La page permet aux utilisateurs de créer, modifier ou supprimer des produits, en envoyant des demandes AJAX au contrôleur.

Dans Explorateur de solutions, développez le dossier Controllers et ouvrez le fichier nommé HomeController.cs. Ce fichier contient un contrôleur MVC. Ajoutez une méthode nommée `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

La méthode **HttpRouteUrl** crée l’URI vers l’API Web, et nous la stockons dans le sac d’affichage pour une version ultérieure.

Ensuite, placez le curseur de texte à l’intérieur de la méthode d’action `Admin`, puis cliquez avec le bouton droit et sélectionnez **Ajouter une vue**. La boîte de dialogue **Ajouter une vue s’affiche** .

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

Dans la boîte de dialogue **Ajouter une vue** , nommez la vue « admin ». Activez la case à cocher **créer une vue fortement typée**. Sous **classe de modèle**, sélectionnez « produit (ProductStore. Models) ». Laissez toutes les autres options en tant que valeurs par défaut.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Cliquez sur **Ajouter** pour ajouter un fichier nommé Admin. cshtml sous affichages/page d’hébergement. Ouvrez ce fichier et ajoutez le code HTML suivant. Ce code HTML définit la structure de la page, mais aucune fonctionnalité n’est encore connectée.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Créer un lien vers la page d’administration

Dans Explorateur de solutions, développez le dossier views, puis développez le dossier Shared. Ouvrez le fichier nommé \_Layout. cshtml. Recherchez l’élément **UL** avec ID = "menu" et un lien d’action pour la vue d’administration :

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> Dans l’exemple de projet, j’ai apporté quelques modifications esthétiques, telles que le remplacement de la chaîne « Your logo here ». Celles-ci n’affectent pas les fonctionnalités de l’application. Vous pouvez télécharger le projet et comparer les fichiers.

Exécutez l’application et cliquez sur le lien « admin » qui apparaît en haut de la page d’hébergement. La page d’administration doit se présenter comme suit :

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Pour le moment, la page ne fait rien. Dans la section suivante, nous allons utiliser Knockout. js pour créer une interface utilisateur dynamique.

## <a name="add-authorization"></a>Ajouter une autorisation

La page d’administration est actuellement accessible à toute personne visitant le site. Modifions ceci pour restreindre l’autorisation aux administrateurs.

Commencez par ajouter un rôle « administrateur » et un utilisateur administrateur. Dans Explorateur de solutions, développez le dossier filtres et ouvrez le fichier nommé InitializeSimpleMembershipAttribute.cs. Recherchez le constructeur `SimpleMembershipInitializer`. Après l’appel à **WebSecurity. InitializeDatabaseConnection**, ajoutez le code suivant :

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Il s’agit d’une méthode rapide et incorrecte pour ajouter le rôle « administrateur » et créer un utilisateur pour le rôle.

Dans Explorateur de solutions, développez le dossier Controllers et ouvrez le fichier HomeController.cs. Ajoutez l’attribut **Authorize** à la méthode `Admin`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Ouvrez le fichier AdminController.cs et ajoutez l’attribut **Authorize** à l’ensemble de la classe `AdminController`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC et l’API Web définissent tous deux des attributs d' **autorisation** , dans différents espaces de noms. MVC utilise **System. Web. Mvc. AuthorizeAttribute**, tandis que l’API Web utilise **System. Web. http. AuthorizeAttribute**.

Désormais, seuls les administrateurs peuvent afficher la page d’administration. En outre, si vous envoyez une requête HTTP au contrôleur d’administration, la requête doit contenir un cookie d’authentification. Si ce n’est pas le cas, le serveur envoie une réponse HTTP 401 (non autorisée). Vous pouvez le voir dans Fiddler en envoyant une demande de récupération à `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Précédent](using-web-api-with-entity-framework-part-3.md)
> [Suivant](using-web-api-with-entity-framework-part-5.md)
