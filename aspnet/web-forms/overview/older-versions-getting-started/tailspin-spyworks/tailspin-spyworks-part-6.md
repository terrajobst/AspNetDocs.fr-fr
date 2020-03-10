---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Partie 6 : ASP.NET Membership | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 6 ajoute l’appartenance ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: b0caa89dc9ffb5bb7451fa2d9d346c7db2bf1466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564182"
---
# <a name="part-6-aspnet-membership"></a>Partie 6 : appartenance à ASP.NET

par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.
> 
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 6 ajoute l’appartenance ASP.NET.

## <a id="_Toc260221672"></a>Utilisation de l’appartenance ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Cliquer sur sécurité

![](tailspin-spyworks-part-6/_static/image1.jpg)

Assurez-vous que nous utilisons l’authentification par formulaire.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Utilisez le lien « créer un utilisateur » pour créer deux utilisateurs.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Lorsque vous avez terminé, reportez-vous à la fenêtre de Explorateur de solutions et actualisez la vue.

![](tailspin-spyworks-part-6/_static/image2.png)

Notez que ASPNETDB. MDF fine a été créé. Ce fichier contient les tables pour prendre en charge les services ASP.NET principaux comme l’appartenance.

Nous pouvons maintenant commencer à implémenter le processus d’extraction.

Commencez par créer une page CheckOut. aspx.

La page CheckOut. aspx ne doit être accessible qu’aux utilisateurs qui se sont connectés. nous allons donc restreindre l’accès aux utilisateurs connectés et rediriger les utilisateurs qui ne sont pas connectés à la page de connexion.

Pour ce faire, nous allons ajouter le code suivant à la section de configuration de notre fichier Web. config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Le modèle pour ASP.NET Web Forms applications a automatiquement ajouté une section d’authentification à notre fichier Web. config et a établi la page de connexion par défaut.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Nous devons modifier le fichier code-behind login. aspx pour migrer un panier d’achat anonyme lorsque l’utilisateur se connecte. Modifiez la page\_événement de chargement comme suit.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Ajoutez ensuite un gestionnaire d’événements « journalisé » comme celui-ci pour définir le nom de session sur l’utilisateur qui vient d’être connecté, puis remplacez l’ID de session temporaire du panier d’achat par celui de l’utilisateur en appelant la méthode MigrateCart dans notre classe MyShoppingCart. (Implémenté dans le fichier. cs)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implémentez la méthode MigrateCart () comme suit.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

Dans Checkout. aspx, nous allons utiliser un contrôle EntityDataSource et un GridView dans notre page d’extraction de la même façon que nous l’avons fait dans la page du panier d’achat.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Notez que notre contrôle GridView spécifie un gestionnaire d’événements « OnDataBound » nommé MyList\_RowDataBound donc implémenter ce gestionnaire d’événements comme celui-ci.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Cette méthode conserve un total cumulé du panier d’achat à mesure que chaque ligne est liée et met à jour la ligne inférieure du contrôle GridView.

À ce niveau, nous avons mis en œuvre une présentation « Review » (révision) de la commande à placer.

Nous allons gérer un scénario de panier vide en ajoutant quelques lignes de code à notre page\_événement de chargement :

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Quand l’utilisateur clique sur le bouton « envoyer », nous allons exécuter le code suivant dans le gestionnaire d’événements de clic sur le bouton Envoyer.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

La « viande » du processus de soumission de commande doit être implémentée dans la méthode SubmitOrder () de notre classe MyShoppingCart.

SubmitOrder :

- Prenez toutes les lignes du panier d’achat et utilisez-les pour créer un nouvel enregistrement de commande et les enregistrements OrderDetails associés.
- Calculez la date d’expédition.
- Effacez le panier d’achat.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Dans le cadre de cet exemple d’application, nous calculerons une date d’expédition en ajoutant simplement deux jours à la date actuelle.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

L’exécution de l’application nous permettra de tester le processus d’achat du début à la fin.

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-5.md)
> [Suivant](tailspin-spyworks-part-7.md)
