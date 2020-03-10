---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Ajout de réseaux sociaux à des sites pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre explique comment intégrer votre site aux services de réseau social. Dans ce chapitre, vous allez apprendre à permettre aux utilisateurs de créer un signet/lien vers votre site Web...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 1637464b0473bba8133acbbf8918d92b4f552701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526935"
---
# <a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Ajout de réseaux sociaux à des sites pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment ajouter des liens de réseaux sociaux pour Facebook, Twitter, Reddit et Explorer aux pages d’un site Web pages Web ASP.NET (Razor), et comment inclure des flux Twitter, des cartes Xbox Gamer et des images Gravatar.
> 
> Ce que vous allez apprendre :
> 
> - Comment permettre aux utilisateurs de créer un signet sur votre site.
> - Comment ajouter un flux Twitter.
> - Comment ajouter un bouton de **type** Facebook à des pages.
> - Comment afficher les images Gravatar.com.
> - Comment afficher une carte Xbox Gamer sur votre site.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 2
> - Bibliothèque d’assistance Web ASP.NET (package NuGet)
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 3, à l’exception des composants qui utilisent la bibliothèque d’assistance Web ASP.NET.

<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Liaison de votre site Web sur les sites de réseau social

Si vous êtes un utilisateur de votre site, il souhaite souvent le partager avec des amis. Vous pouvez faciliter cette opération en affichant des glyphes (icônes) sur lesquels les utilisateurs peuvent cliquer pour partager une page sur des sites Reddit, Facebook, Twitter ou similaires.

Pour afficher ces glyphes, ajoutez le programme d’assistance `LinkSharecode` à une page. Les personnes qui visitent votre page peuvent cliquer sur un glyphe individuel. S’ils disposent d’un compte avec ce site de réseaux sociaux, ils peuvent alors poster un lien vers votre page sur ce site.

![Image 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas encore ajoutée.-Créez une page nommée *ListLinkShare. cshtml* et ajoutez le balisage suivant :

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    Dans cet exemple, lorsque le programme d’assistance `LinkShare` s’exécute, le titre de la page est passé en tant que paramètre, qui à son tour transmet le titre de la page au site de réseau social. Toutefois, vous pouvez transmettre n’importe quelle chaîne de votre choix. Cet exemple spécifie également les sites de réseau social à inclure dans la liste. Vous pouvez spécifier les sites de réseau social qui sont pertinents pour votre site.
2. Exécutez la page *ListLinkShare. cshtml* dans un navigateur. (Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.)
3. Cliquez sur un glyphe pour l’un des sites pour lesquels vous êtes inscrit. Le lien vous amène à la page sur le site de réseau social sélectionné où vous pouvez partager un lien. Par exemple, si vous cliquez sur le lien Reddit, vous êtes dirigé vers la page `submit to reddit` sur le site Web Reddit.

     ![Image 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Ajout d’un flux Twitter

Pour plus d’informations sur l’utilisation d’un programme d’assistance Twitter qui est compatible avec la version actuelle de l’API Twitter, consultez [l’aide de Twitter](../ui-layouts-and-themes/twitter-helper.md). Cet exemple montre comment écrire votre propre application d’assistance afin de pouvoir facilement réutiliser le code de nombreuses pages.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Affichage d’un bouton Facebook &quot;comme&quot;

Dans certains cas, la meilleure solution consiste à obtenir le code directement auprès du fournisseur de réseaux sociaux plutôt que de s’appuyer sur une application auxiliaire. Cela est particulièrement vrai si le fournisseur de réseaux sociaux met à jour ses options plus rapidement que l’application auxiliaire n’est mise à jour.

Pour ajouter des fonctionnalités Facebook (telles que le bouton like) à votre site, vous pouvez récupérer des extraits de code à partir du site [Developers.Facebook.com](https://developers.facebook.com/) . Sur le site Facebook, vous utilisez leurs outils pour générer un extrait de code adapté à votre site.

Le code en surbrillance suivant est le code qui a été récupéré à partir de l’outil de bouton like sur le site developers.facebook.com. Vous devez fournir votre propre ID d’application.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Rendu d’une image Gravatar

Un *Gravatar* (un &quot;avatar&quot;internationalement reconnu) est une image qui peut être utilisée sur plusieurs sites Web comme avatar &#8212; , une image qui vous représente. Par exemple, un Gravatar peut identifier une personne dans un billet de forum, dans un commentaire de blog, et ainsi de suite. (Vous pouvez inscrire votre propre Gravatar sur le site Web Gravatar sur [http://www.gravatar.com/](http://www.gravatar.com/).) Si vous souhaitez afficher des images à côté des noms de personnes ou des adresses de messagerie sur votre site Web, vous pouvez utiliser le programme d’assistance Gravatar.

Dans cet exemple, vous utilisez un seul Gravatar qui représente vous-même. Une autre façon d’utiliser un gravatar est de permettre aux utilisateurs de spécifier leur adresse Gravatar lorsqu’ils s’inscrivent sur votre site. (Vous pouvez apprendre comment permettre aux utilisateurs de s’inscrire [pour ajouter la sécurité et l’appartenance à un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202904).) Ensuite, chaque fois que vous affichez des informations pour cet utilisateur, vous pouvez simplement ajouter le gravatar à l’emplacement où vous affichez le nom de l’utilisateur.

1. Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas déjà fait.
2. Créez une nouvelle page Web nommée *Gravatar. cshtml*.
3. Ajoutez le balisage suivant au fichier : 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    La méthode `Gravatar.GetHtml` affiche l’image Gravatar sur la page. Pour modifier la taille de l’image, vous pouvez inclure un nombre en tant que second paramètre. La taille par défaut est 80. Les nombres inférieurs à 80 réduisent la taille de l’image. Les nombres supérieurs à 80 rendent l’image plus grande.
4. Dans les méthodes de `Gravatar.GetHtml`, remplacez `<Your Gravatar account here>` par l’adresse de messagerie que vous utilisez pour votre compte gravatar. (Si vous n’avez pas de compte gravatar, vous pouvez utiliser l’adresse de messagerie d’une personne qui fait.)
5. Exécutez la page dans votre navigateur. La page affiche deux images Gravatar pour l’adresse e-mail que vous avez spécifiée. La deuxième image est plus petite que la première. 

    ![Image 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Affichage d’une carte Xbox Gamer

Quand des personnes lisent des jeux Microsoft Xbox en ligne, chaque utilisateur possède un ID unique. Les statistiques sont conservées pour chaque joueur sous la forme d’une carte de joueur, qui affiche leur réputation, leur score et les jeux récemment joués. Si vous êtes un passionné de Xbox, vous pouvez afficher votre carte de joueur sur les pages de votre site à l’aide du programme d’assistance `GamerCard`.

1. Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas déjà fait.
2. Créez une nouvelle page nommée *XboxGamer. cshtml* et ajoutez le balisage suivant.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Utilisez la propriété `GamerCard.GetHtml` pour spécifier l’alias de la carte du joueur à afficher.
3. Exécutez la page dans votre navigateur. La page affiche la carte Xbox Gamer que vous avez spécifiée.

    ![Image 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
