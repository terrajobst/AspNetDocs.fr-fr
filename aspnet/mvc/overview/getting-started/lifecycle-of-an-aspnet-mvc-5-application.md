---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Cycle de vie d’une application ASP.NET MVC 5 | Microsoft Docs
author: cephalin
description: Téléchargez un document PDF qui représente le cycle de vie d’une application ASP.NET MVC 5. Ce document de cycle de vie fournit une vue d’ensemble du cycle de vie MVC...
ms.author: riande
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: f4a9b3fb61552b070db11fba617b5627fcd71cd5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582200"
---
# <a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Cycle de vie d’une application ASP.NET MVC 5

par [Cephas lin](https://github.com/cephalin)

[Télécharger le document PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Ici, vous pouvez télécharger un document PDF qui représente le cycle de vie de chaque application ASP.NET MVC 5, de la réception de la requête HTTP à l’envoi de la réponse HTTP au client. Il est conçu comme un outil éducatif pour ceux qui sont nouveaux dans ASP.NET MVC et également pour ceux qui ont besoin d’accéder à des aspects spécifiques de l’application. Le document PDF présente les fonctionnalités suivantes :

- Étapes [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) pertinentes pour vous aider à comprendre où MVC s’intègre au [cycle de vie de l’application ASP.net](https://msdn.microsoft.com/library/bb470252.aspx).
- Une vue d’ensemble du cycle de vie des applications MVC, où vous pouvez comprendre les principales étapes que chaque application MVC traverse dans le pipeline de traitement des requêtes.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Un affichage détaillé affiche des détails sur le pipeline de traitement des demandes. Vous pouvez comparer la vue détaillée et la vue détaillée pour voir comment les détails des cycles de vie sont collectés dans les différentes étapes. [Téléchargez le fichier PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) pour afficher une vue plus large.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Emplacement et objet de toutes les méthodes substituables sur l’objet [contrôleur](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) dans le pipeline de traitement des demandes. Vous pouvez ou non avoir besoin de remplacer une méthode, mais il est important que vous compreniez son rôle dans le cycle de vie de l’application afin que vous puissiez écrire du code à l’étape de cycle de vie appropriée pour l’effet que vous attendez.
- Diagrammes de relevé montrant comment chacun des types de filtre (authentification, autorisation, action et résultat) est appelé.
- Lien vers un article ou un blog utile à partir de chaque point d’intérêt dans l’affichage détaillé.

## <a name="next-steps"></a>Étapes suivantes

Ce document répond-il à vos besoins ? Nous apprécions vos commentaires. Si vous avez des questions sur le cycle de vie ASP.NET MVC dans votre application, [StackOverflow](http://stackoverflow.com/help) et les [Forums ASP.NET MVC](https://forums.asp.net/1146.aspx) sont des emplacements importants à poser. Suivez le didacticiel sur Twitter pour pouvoir [Télécharger des mises](https://twitter.com/Cephas_MSFT) à jour dans Mes derniers didacticiels.
