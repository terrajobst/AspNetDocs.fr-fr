---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Partie 1 : fichier-> nouveau projet | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 1 couvre la vue d’ensemble et le fichier/nouveau projet.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 05a3ace3d8fef9c1f3593f7948e42b4725d70134
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636128"
---
# <a name="part-1-file--new-project"></a>Partie 1 : fichier > nouveau projet

par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.
> 
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 1 couvre la vue d’ensemble et le fichier/nouveau projet.

## <a id="_Toc260221666"></a>Vue

Ce didacticiel est une introduction à ASP.NET WebForms. Nous allons démarrer lentement, de sorte que l’expérience de développement Web au niveau débutant est correcte.

L’application que nous allons créer est un magasin en ligne simple.

![](tailspin-spyworks-part-1/_static/image1.jpg)

Les visiteurs peuvent parcourir les produits par catégorie :

![](tailspin-spyworks-part-1/_static/image2.jpg)

Ils peuvent afficher un seul produit et l’ajouter à leur panier :

![](tailspin-spyworks-part-1/_static/image3.jpg)

Ils peuvent examiner leur panier, en supprimant tous les éléments qu’ils ne souhaitent plus :

![](tailspin-spyworks-part-1/_static/image4.jpg)

En poursuivant l’extraction, vous êtes invité à

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Après l’ordonnancement, un écran de confirmation simple s’affiche :

![](tailspin-spyworks-part-1/_static/image7.jpg)

Nous allons commencer par créer un nouveau projet ASP.NET WebForms dans Visual Studio 2010, et nous ajouterons de manière incrémentielle des fonctionnalités pour créer une application fonctionnelle complète. En cours de route, nous allons aborder l’accès aux bases de données, les affichages de listes et de grilles, les pages de mise à jour des données, la validation des données, l’utilisation de pages maîtres pour une mise en page cohérente, AJAX, la validation, l’appartenance des utilisateurs

Vous pouvez suivre la procédure pas à pas, ou vous pouvez télécharger l’application terminée à partir de [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Vous pouvez utiliser Visual Studio 2010 ou la version gratuite de Visual Web Developer 2010 de [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/). Pour générer l’application, vous pouvez utiliser l’une ou l’autre SQL Server SQL Server Express libre pour héberger la base de données.

## <a id="_Toc260221667"></a>Fichier/nouveau projet

Nous allons commencer par sélectionner le nouveau projet dans le menu fichier de Visual Studio. La boîte de dialogue Nouveau projet s’affiche.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Nous allons sélectionner le groupe C# Visual/Web Templates sur la gauche, puis choisir le modèle « Application Web ASP.net » dans la colonne centrale. Nommez votre projet TailspinSpyworks et appuyez sur le bouton OK.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Cela va créer notre projet. Jetons un coup d’œil aux dossiers inclus dans notre application dans le Explorateur de solutions sur le côté droit.

![](tailspin-spyworks-part-1/_static/image10.jpg)

La solution vide n’est pas complètement vide : elle ajoute une structure de dossier de base :

![](tailspin-spyworks-part-1/_static/image1.png)

Notez les conventions implémentées par le modèle de projet par défaut ASP.NET 4.

- Le dossier « Account » implémente une interface utilisateur de base pour ASP. Sous-système d’appartenance du réseau.
- Le dossier « scripts » sert de référentiel pour les fichiers JavaScript côté client et les fichiers jQuery. js de base sont mis à disposition par défaut.
- Le dossier « styles » est utilisé pour organiser les visuels de sites Web (feuilles de style CSS)

Quand nous appuyons sur F5 pour exécuter notre application et afficher la page default. aspx, nous voyons ce qui suit.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Notre première amélioration de l’application consiste à remplacer le fichier style. CSS du modèle WebForms par défaut par les classes CSS et les fichiers image associés qui rendront le asthetics visuel que nous souhaitons pour notre application Tailspin SpyWorks.

Après cela, notre page default. aspx s’affiche comme suit.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Notez les liens d’image situés en haut à droite de la page, ainsi que les éléments de menu qui ont été ajoutés à la page maître. Seuls les liens « connexion » et « compte » pointent vers les pages qui existent (générées par le modèle par défaut) et les autres pages que nous allons implémenter lors de la création de notre application.

Nous allons également déplacer la page maître vers le répertoire styles. Bien qu’il ne s’agisse que d’une préférence, il peut faciliter un peu les choses si nous décidons de rendre notre application « peau » à l’avenir.

Après cela, nous devrons modifier les références de page maître dans tous les fichiers. aspx générés par les pages Web Forms ASP.NET par défaut.

> [!div class="step-by-step"]
> [Next](tailspin-spyworks-part-2.md)
