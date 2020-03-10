---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Rendu des sites pages Web ASP.NET (Razor) pour les appareils mobiles | Microsoft Docs
author: Rick-Anderson
description: 'Cet article explique comment créer des pages dans un site pages Web ASP.NET (Razor) qui s’affichent correctement sur les périphériques mobiles. Ce que vous allez apprendre : comment...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: c012348d65e48a275cb0e4808fef2a7f31e5fb33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563566"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Rendu des sites pages Web ASP.NET (Razor) pour les appareils mobiles

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment créer des pages dans un site pages Web ASP.NET (Razor) qui s’affichent correctement sur les périphériques mobiles.
> 
> Ce que vous allez apprendre :
> 
> - Comment utiliser une convention d’affectation de noms pour spécifier qu’une page est conçue spécifiquement pour les périphériques mobiles.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2.

Pages Web ASP.NET vous permet de créer des affichages personnalisés pour le rendu du contenu sur des appareils mobiles ou autres.

La façon la plus simple de créer une page spécifique à un appareil dans un site pages Web ASP.NET consiste à utiliser un modèle de nom de fichier comme celui-ci : *nom_fichier. mobile. cshtml*. Vous pouvez créer deux versions d’une page (par exemple, l’une nommée *MyFile. cshtml* et l’autre nommée *MyFile. mobile. cshtml*). Au moment de l’exécution, quand un appareil mobile demande *MyFile. cshtml*, ASP.NET restitue le contenu à partir de *MyFile. mobile. cshtml*. Dans le cas contraire, *MyFile. cshtml* est restitué.

L’exemple suivant montre comment activer le rendu mobile en ajoutant une page de contenu pour les périphériques mobiles. *Page1. cshtml* contient du contenu plus un encadré de navigation. *Page1. mobile. cshtml* contient le même contenu, mais omet la barre latérale.

1. Dans un site pages Web ASP.NET, créez un fichier nommé *Page1. cshtml* et remplacez le contenu actuel par le balisage suivant.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Créez un fichier nommé *Page1. mobile. cshtml* et remplacez le contenu existant par le balisage suivant. Notez que la version mobile de la page omet la section de navigation pour un meilleur rendu sur un écran plus petit.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Exécutez un navigateur de bureau et accédez à *Page1. cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Exécutez un navigateur mobile (ou un émulateur d’appareil mobile) et accédez à *Page1. cshtml*. (Notez que vous n’incluez pas *. mobile.* dans le cadre de l’URL.) Même si la requête est dans *Page1. cshtml*, ASP.NET affiche *Page1. mobile. cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Pour tester les pages mobiles, vous pouvez utiliser un simulateur d’appareil mobile qui s’exécute sur un ordinateur de bureau. Cet outil vous permet de tester les pages Web telles qu’elles apparaissent sur les périphériques mobiles (c’est-à-dire, généralement avec une zone d’affichage bien plus petite). L’un des exemples d’un simulateur est le [Sélecteur d’agent utilisateur](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) pour Mozilla Firefox, qui vous permet d’émuler différents navigateurs mobiles à partir d’une version de bureau de Firefox.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[Émulateur Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
