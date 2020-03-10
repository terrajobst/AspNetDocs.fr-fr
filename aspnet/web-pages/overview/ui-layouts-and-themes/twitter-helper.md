---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Application auxiliaire Twitter avec pages Web ASP.NET | Microsoft Docs
author: Rick-Anderson
description: Cette rubrique et cette application montrent comment ajouter un programme d’assistance Twitter à votre projet WebMatrix 3. Il contient le code d’assistance Twitter et montre comment appeler le programme d’assistance...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638557"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a>Helper Twitter avec ASP.NET Web Pages

par [Tom FitzMacken](https://github.com/tfitzmac)

> [!IMPORTANT]
> Les applications d’assistance Twitter sont obsolètes. Pour obtenir les derniers outils d’engagement de Twitter pour les sites Web, consultez [vue d’ensemble de Twitter pour les sites Web](https://developer.twitter.com/en/docs/twitter-for-websites/overview).

> Cette rubrique et cette application montrent comment ajouter un programme d’assistance Twitter à votre projet WebMatrix 3. Il contient le code d’assistance Twitter et montre comment appeler les méthodes d’assistance.
> 
> Ce code pour le fichier Twitter. cshtml a été développé par **Tian Pan** de Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2.

## <a name="introduction"></a>Introduction

Cette rubrique montre comment ajouter un programme d’assistance Twitter à votre application et utiliser syntaxe Razor pour appeler les méthodes d’assistance. Le programme d’assistance Twitter facilite l’incorporation de boutons et de widgets Twitter dans votre application. Pour utiliser un widget Twitter, tel que la chronologie d’un utilisateur ou les résultats de la recherche pour un mot-dièse, vous devez d’abord créer le [widget sur Twitter](https://twitter.com/settings/widgets). Après avoir créé votre widget, vous recevrez un ID de widget. Vous transmettez cet ID de widget en tant que paramètre lors de l’appel des méthodes d’assistance qui affichent le widget.

Cette rubrique a été écrite pour la version 1,1 de l’API Twitter. En ajoutant directement le code d’assistance Twitter à votre projet, vous pouvez mettre à jour le code d’assistance Si l’API Twitter change.

Pour plus d’informations sur l’installation de WebMatrix, consultez [Présentation de pages Web ASP.NET 2-prise en main](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Ajouter l’assistance Twitter à votre projet

Pour ajouter le programme d’assistance Twitter, commencez par ajouter un dossier nommé **App\_code** à votre projet. Créez ensuite un fichier nommé **Twitter. cshtml**.

![Dossier App_Code](twitter-helper/_static/image1.png)

Remplacez le code par défaut dans Twitter. cshtml par le code suivant.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Appeler des méthodes Twitter à partir de vos pages Web

L’exemple suivant montre comment utiliser les méthodes d’assistance Twitter à partir d’une page de votre projet. Dans votre projet, vous souhaiterez remplacer les valeurs de paramètre par des valeurs qui correspondent à vos besoins. Vous pouvez utiliser les ID de widget fournis pour explorer le fonctionnement des méthodes, mais vous souhaiterez générer vos propres widgets pour votre projet.

Tous les paramètres indiqués ci-dessous ne sont pas requis. Les paramètres facultatifs sont utilisés pour personnaliser l’affichage du bouton ou du widget. Par exemple, le bouton suivant nécessite uniquement le nom d’utilisateur, mais l’exemple montre comment inclure le nombre d’abonnés et comment spécifier la taille du bouton et de la langue.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Consulter les résultats

Le code ci-dessus produit les boutons et les widgets suivants. Ces boutons et widgets sont des captures d’écran entièrement fonctionnelles. Le bouton suivant est affiché en espagnol, car le paramètre de langue était défini sur **es**.

### <a name="follow-button"></a>Bouton suivant

[Suivez @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Bouton Tweet

`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>` [Tweet](https://twitter.com/share)

### <a name="user-timeline-profile"></a>Chronologie de l’utilisateur (profil)

[Tweets par @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Favoris

Les [tweets préférés par @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>List

[Tweets à partir de @Microsoft/MS\_des bandes\_de consommateurs](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Rechercher

[Tweets sur &quot;#asp&quot;.net](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
