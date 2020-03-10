---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Partie 9 : inscription et extraction | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 9 couvre l’inscription et l’extraction.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 040bc0ccef889fb9a7c3d9b5ce88c75b7b754248
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559534"
---
# <a name="part-9-registration-and-checkout"></a>Partie 9 : inscription et extraction

par [Jon Galloway](https://github.com/jongalloway)

> Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.  
>   
> Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.  
>   
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 9 couvre l’inscription et l’extraction.

Dans cette section, nous allons créer un CheckoutController qui collecte les informations relatives à l’adresse et au paiement du client. Nous aurons besoin que les utilisateurs s’inscrivent auprès de notre site avant de procéder à l’extraction, de sorte que ce contrôleur nécessite une autorisation.

Les utilisateurs accèdent au processus de validation à partir de leur panier en cliquant sur le bouton « Checkout ».

![](mvc-music-store-part-9/_static/image1.jpg)

Si l’utilisateur n’est pas connecté, il est invité à le faire.

![](mvc-music-store-part-9/_static/image1.png)

Une fois la connexion établie, l’utilisateur affiche l’affichage adresse et paiement.

![](mvc-music-store-part-9/_static/image2.png)

Une fois qu’ils ont rempli le formulaire et envoyé la commande, l’écran de confirmation de commande s’affiche.

![](mvc-music-store-part-9/_static/image3.png)

Si vous tentez d’afficher un ordre inexistant ou une commande qui n’appartient pas à vous, l’affichage des erreurs s’affiche.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migration du panier d’achat

Lorsque le processus d’achat est anonyme, lorsque l’utilisateur clique sur le bouton Checkout, il doit s’inscrire et se connecter. Les utilisateurs s’attendent à conserver leurs informations sur le panier d’achat entre les visites. nous devons donc associer les informations du panier d’achat à un utilisateur lors de l’inscription ou de la connexion.

C’est en fait très simple à faire, car notre classe ShoppingCart a déjà une méthode qui associe tous les éléments du panier en cours à un nom d’utilisateur. Il vous suffit d’appeler cette méthode lorsqu’un utilisateur termine l’inscription ou la connexion.

Ouvrez la classe **AccountController** que nous avons ajoutée lors de la configuration de l’appartenance et de l’autorisation. Ajoutez une instruction using référençant MvcMusicStore. Models, puis ajoutez la méthode MigrateShoppingCart suivante :

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Modifiez ensuite l’action de publication d’ouverture de session pour appeler MigrateShoppingCart une fois que l’utilisateur a été validé, comme indiqué ci-dessous :

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Apportez la même modification à l’action de publication du Registre, juste après la création du compte d’utilisateur :

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

Voilà, un panier d’achat anonyme sera automatiquement transféré vers un compte d’utilisateur lors de l’inscription ou de la connexion.

## <a name="creating-the-checkoutcontroller"></a>Création du CheckoutController

Cliquez avec le bouton droit sur le dossier Controllers et ajoutez un nouveau contrôleur au projet nommé CheckoutController à l’aide du modèle de contrôleur vide.

![](mvc-music-store-part-9/_static/image5.png)

Tout d’abord, ajoutez l’attribut Authorize au-dessus de la déclaration de classe du contrôleur pour demander aux utilisateurs de s’inscrire avant d’effectuer l’extraction :

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Remarque : cela est similaire à la modification que nous avons apportée précédemment à StoreManagerController, mais dans ce cas, l’attribut Authorize nécessite que l’utilisateur soit dans un rôle d’administrateur. Dans le contrôleur d’extraction, nous exigeons que l’utilisateur soit connecté, mais qu’il ne soit pas obligé d’être administrateur.*

Par souci de simplicité, nous ne traiterons pas les informations de paiement dans ce didacticiel. Au lieu de cela, nous autorisons les utilisateurs à extraire à l’aide d’un code promotionnel. Nous allons stocker ce code promotionnel à l’aide d’une constante nommée code promo.

Comme dans le StoreController, nous allons déclarer un champ qui contiendra une instance de la classe MusicStoreEntities, nommée storeDB. Pour utiliser la classe MusicStoreEntities, vous devez ajouter une instruction using pour l’espace de noms MvcMusicStore. Models. La partie supérieure de notre contrôleur de validation apparaît ci-dessous.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

Le CheckoutController aura les actions de contrôleur suivantes :

**AddressAndPayment (méthode d’extraction)** affiche un formulaire pour permettre à l’utilisateur d’entrer ses informations.

**AddressAndPayment (méthode de publication)** valide l’entrée et traite la commande.

**Terminer** s’affiche une fois que l’utilisateur a terminé le processus d’extraction. Cette vue inclut le numéro de commande de l’utilisateur comme confirmation.

Tout d’abord, nous allons renommer l’action du contrôleur d’index (générée lors de la création du contrôleur) sur AddressAndPayment. Cette action de contrôleur affiche simplement le formulaire d’extraction, de sorte qu’elle ne nécessite aucune information de modèle.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Notre méthode de publication AddressAndPayment suit le même modèle que celui que nous avons utilisé dans le StoreManagerController : il essaiera d’accepter l’envoi du formulaire et de compléter la commande, puis d’afficher de nouveau le formulaire en cas d’échec.

Une fois que la validation de l’entrée de formulaire est conforme à nos exigences de validation pour une commande, nous vérifions directement la valeur du formulaire code promo. En supposant que tout est correct, nous enregistrons les informations mises à jour dans la commande, indiquons à l’objet ShoppingCart de terminer le processus de commande et redirigent vers l’action terminer.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Une fois le processus d’extraction terminé, les utilisateurs sont redirigés vers l’action de contrôleur complète. Cette action effectue une vérification simple pour confirmer que la commande appartient effectivement à l’utilisateur connecté avant d’afficher le numéro de commande en tant que confirmation.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Remarque : l’affichage des erreurs a été créé automatiquement pour nous dans le dossier/Views/Shared lorsque nous avons commencé le projet.*

Le code CheckoutController complet est le suivant :

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Ajout de la vue AddressAndPayment

Créons maintenant la vue AddressAndPayment. Cliquez avec le bouton droit sur l’une des actions du contrôleur AddressAndPayment et ajoutez une vue nommée AddressAndPayment, qui est fortement typée comme un ordre et utilise le modèle de modification, comme indiqué ci-dessous.

![](mvc-music-store-part-9/_static/image6.png)

Cette vue utilise deux des techniques que nous avons examinées lors de la création de la vue StoreManagerEdit :

- Nous utiliserons html. EditorForModel () pour afficher les champs de formulaire pour le modèle de commande
- Nous allons utiliser des règles de validation à l’aide d’une classe Order avec des attributs de validation

Nous allons commencer par mettre à jour le code du formulaire pour utiliser HTML. EditorForModel (), suivi d’une zone de texte supplémentaire pour le code de promotion. Le code complet de la vue AddressAndPayment est illustré ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Définition des règles de validation pour la commande

Maintenant que notre vue est configurée, nous allons configurer les règles de validation pour notre modèle de commande, comme nous l’avons fait précédemment pour le modèle d’album. Cliquez avec le bouton droit sur le dossier Models et ajoutez une classe nommée Order. En plus des attributs de validation que nous avons utilisés précédemment pour l’album, nous utiliserons également une expression régulière pour valider l’adresse de messagerie de l’utilisateur.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Si vous tentez d’envoyer le formulaire avec des informations manquantes ou non valides, un message d’erreur s’affiche à l’aide de la validation côté client.

![](mvc-music-store-part-9/_static/image7.png)

Bien, nous avons effectué la plupart des tâches difficiles pour le processus de validation. Nous avons juste quelques chances et se terminent. Nous devons ajouter deux vues simples et nous devons prendre en charge le transfert des informations du panier pendant le processus de connexion.

## <a name="adding-the-checkout-complete-view"></a>Ajout de la vue d’extraction complète

La vue extraction terminée est assez simple, car elle doit simplement afficher l’ID de commande. Cliquez avec le bouton droit sur l’action terminer le contrôleur et ajoutez une vue nommée Complete, qui est fortement typée comme int.

![](mvc-music-store-part-9/_static/image8.png)

À présent, nous allons mettre à jour le code de vue pour afficher l’ID de commande, comme indiqué ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Mise à jour de l’affichage des erreurs

Le modèle par défaut comprend un affichage des erreurs dans le dossier vues partagées afin qu’il puisse être réutilisé ailleurs dans le site. Cet affichage des erreurs contient une erreur très simple et n’utilise pas notre disposition de site. nous allons donc le mettre à jour.

Étant donné qu’il s’agit d’une page d’erreur générique, le contenu est très simple. Nous inclurons un message et un lien pour accéder à la page précédente dans l’historique si l’utilisateur souhaite réessayer son action.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]

> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-8.md)
> [Suivant](mvc-music-store-part-10.md)
