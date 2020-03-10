---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Comment faire :] Utilisez la propriété REPONSE. Filter pour remplacer du code HTML dans une page ASP.NET | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo, Chris pixels montre comment utiliser la propriété REPONSE. Filter pour intercepter et modifier le code HTML envoyé à une page. Tout d’abord, un exemple de page est créé...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 2ebd9162f81f5270c92c6b8d55e2d2dad4660701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602815"
---
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Comment faire :] Utiliser la propriété REPONSE. Filter pour remplacer du code HTML dans une page ASP.NET

par [Chris pixels](https://twitter.com/chrispels)

Dans cette vidéo, Chris pixels montre comment utiliser la propriété REPONSE. Filter pour intercepter et modifier le code HTML envoyé à une page. Tout d’abord, vous créez un exemple de page avec du texte simple. Ensuite, une classe de flux personnalisée est créée, qui sert de flux de remplacement pour le flux actuel envoyé au navigateur de l’utilisateur. Dans cette classe de flux personnalisée, le contenu de la page est récupéré à partir du flux, modifié, puis écrit dans le flux de réponse. Dans cette classe de flux personnalisée, la méthode Write est personnalisée pour remplacer le code HTML dans le flux de réponse de base, ce qui a pour effet de modifier ce qui est envoyé au navigateur de l’utilisateur. Enfin, la nouvelle classe Stream est assignée à la propriété Response. Filter dans la page\_événement Load, ce qui fournit le mécanisme de modification du contenu de la page.

[&#9654;Regarder la vidéo (13 minutes)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
