---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Présentation du didacticiel NerdDinner | Microsoft Docs
author: shanselman
description: La meilleure façon d’apprendre une nouvelle infrastructure consiste à en créer un autre. Ce didacticiel explique comment créer une application de petite taille, mais complète, à l’aide de ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 154cfe6694cf723c0a1f8e33bfdb42c97594518f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580576"
---
# <a name="introducing-the-nerddinner-tutorial"></a>Introduction au didacticiel NerdDinner

par [Scott Hanselman](https://github.com/shanselman)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> La meilleure façon d’apprendre une nouvelle infrastructure consiste à en créer un autre. Ce didacticiel vous guide dans la création d’une application de petite taille, mais complète, à l’aide de ASP.NET MVC 1, et présente quelques-uns des principaux concepts qui l’appuient.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-tutorial"></a>Didacticiel NerdDinner

La meilleure façon d’apprendre une nouvelle infrastructure consiste à en créer un autre. Ce didacticiel explique comment créer une application de petite taille, mais complète, à l’aide de ASP.NET MVC, et présente quelques-uns des principaux concepts qui l’appuient.

L’application que nous allons générer est appelée « NerdDinner ». NerdDinner permet aux utilisateurs de trouver et d’organiser facilement les dîners en ligne :

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner permet aux utilisateurs inscrits de créer, de modifier et de supprimer des dîners. Il applique un ensemble cohérent de règles de validation et d’entreprise à l’ensemble de l’application :

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Les visiteurs peuvent utiliser un mappage basé sur AJAX pour rechercher les dîners à venir à proximité :

![](introducing-the-nerddinner-tutorial/_static/image3.png)

En cliquant sur un dîner, vous les trouverez dans une page de détails où ils pourront en savoir plus :

![](introducing-the-nerddinner-tutorial/_static/image4.png)

S’ils souhaitent assister au dîner, ils peuvent se connecter ou s’inscrire sur le site :

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Ils peuvent ensuite cliquer sur un lien RSVP basé sur AJAX pour assister à l’événement :

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implémentation de NerdDinner

Nous allons commencer notre application NerdDinner à l’aide de la commande de nouveau projet de fichier&gt;dans Visual Studio pour créer un nouveau projet MVC ASP.NET. Nous ajouterons ensuite de manière incrémentielle des fonctionnalités et des fonctionnalités. Nous allons aborder les éléments suivants :

1. [Comment créer un nouveau projet MVC ASP.NET](create-a-new-aspnet-mvc-project.md)
2. [Comment créer une base de données](create-a-database.md)
3. [Comment créer un modèle avec des validations de règles d’entreprise](build-a-model-with-business-rule-validations.md)
4. [Comment utiliser des contrôleurs et des vues pour implémenter une interface utilisateur de liste/détails](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [Comment fournir une prise en charge de l’entrée de formulaire CRUD (créer, lire, mettre à jour, supprimer)](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [Comment utiliser ViewData et implémenter des classes ViewModel](use-viewdata-and-implement-viewmodel-classes.md)
7. [Réutilisation de l’interface utilisateur à l’aide des pages maîtres et des parties partielles](re-use-ui-using-master-pages-and-partials.md)
8. [Comment implémenter une pagination des données efficace](implement-efficient-data-paging.md)
9. [Comment sécuriser des applications à l’aide de l’authentification et de l’autorisation](secure-applications-using-authentication-and-authorization.md)
10. [Comment utiliser AJAX pour fournir des mises à jour dynamiques](use-ajax-to-deliver-dynamic-updates.md)
11. [Comment utiliser AJAX pour implémenter des scénarios de mappage](use-ajax-to-implement-mapping-scenarios.md)
12. [Comment activer les tests unitaires automatisés](enable-automated-unit-testing.md)

Vous pouvez créer votre propre copie de NerdDinner à partir de zéro en effectuant chaque étape de la procédure pas à pas dans ce chapitre. Vous pouvez également télécharger une version complète du code source ici : [NerdDinner sur GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Vous pouvez également [Télécharger une version PDF gratuite de ce didacticiel](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) si vous souhaitez lire le didacticiel en mode hors connexion.

Pour générer l’application, vous pouvez utiliser Visual Studio 2008 ou la version gratuite de Visual Web Developer 2008 Express. Vous pouvez utiliser SQL Server ou la SQL Server Express libre pour la base de données.

Vous pouvez installer ASP.NET MVC, Visual Web Developer 2008 Express et SQL Server Express (tout gratuitement) à l’aide de V2 du [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Commençons...

Maintenant que nous avons abordé le NerdDinner, nous allons nous reporter à nos manches et écrire du code.

Nous allons commencer par utiliser file-&gt;nouveau projet dans Visual Studio pour créer l’application NerdDinner.

> [!div class="step-by-step"]
> [Next](create-a-new-aspnet-mvc-project.md)
