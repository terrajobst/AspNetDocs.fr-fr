---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Partie 2 : couche d’accès aux données | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 2 couvre l’ajout de la couche d’accès aux données.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 342d2c54dfba5d052570e890f85dcf9739f9884f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573247"
---
# <a name="part-2-data-access-layer"></a>Partie 2 : couche d’accès aux données

par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.
> 
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 2 couvre l’ajout de la couche d’accès aux données.

## <a id="_Toc260221668"></a>Ajout de la couche d’accès aux données

Notre application de commerce électronique dépend de deux bases de données.

Pour obtenir des informations sur le client, nous allons utiliser la base de données d’appartenance ASP.NET standard. Pour notre panier d’achat et catalogue de produits, nous allons implémenter une base de données SQL Express comme suit.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Après avoir créé la base de données (commerce. mdf) dans le dossier application\_des données de l’application, nous pouvons procéder à la création de notre couche d’accès aux données à l’aide du Entity Framework .NET.

Nous allons créer un dossier nommé « Data\_Access » et cliquer avec le bouton droit sur ce dossier et sélectionner « Ajouter un nouvel élément ».

Dans l’élément « modèles installés », puis sélectionnez « ADO.NET Entity Data Model », entrez EDM\_commerce. edmx comme nom et cliquez sur le bouton « Ajouter ».

![](tailspin-spyworks-part-2/_static/image2.jpg)

Choisissez « générer à partir de la base de données ».

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Enregistrez et générez.

Nous sommes maintenant prêts à ajouter notre première fonctionnalité : un menu de catégorie de produit.

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-1.md)
> [Suivant](tailspin-spyworks-part-3.md)
