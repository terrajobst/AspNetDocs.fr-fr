---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[Comment faire :] Choisir entre les méthodes des mises à jour de page AJAX ? | Microsoft Docs'
author: JoeStagner
description: Dans cette vidéo, Joe Stagner compare les deux principales méthodes d’exécution des mises à jour de page de style AJAX dans une application ASP.NET. La première méthode consiste à utiliser un UPD...
ms.author: riande
ms.date: 07/09/2007
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: 56e3ebfbe0b5af4234791136725de79e38171cc1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523666"
---
# <a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a>[Comment faire :] Choisir entre les méthodes des mises à jour de page AJAX ?

par [Joe Stagner](https://github.com/JoeStagner)

Dans cette vidéo, Joe Stagner compare les deux principales méthodes d’exécution des mises à jour de page de style AJAX dans une application ASP.NET. La première méthode consiste à utiliser un UpdatePanel, où aucun code supplémentaire ne doit être écrit côté client ou côté serveur. L’avantage de l’utilisation d’UpdatePanel est que tout fonctionne automatiquement. La pénalité est que, au niveau du client, elle nécessite une grande quantité de données à inclure dans la requête et la réponse AJAX, et au niveau du serveur, il est nécessaire d’exécuter un cycle de vie de page complet. La deuxième méthode consiste à utiliser des rappels réseau, où du code supplémentaire doit être écrit côté client et côté serveur. L’avantage de l’utilisation de rappels réseau est que, au niveau du client, il ne nécessite que très peu de données à inclure dans la requête et la réponse AJAX, et au niveau du serveur, seule la méthode de service appelée doit être exécutée. La pénalisation est le temps et les efforts qu’elle prend pour écrire le code nécessaire. Jean conclut la vidéo en évoquant ce que vous devez prendre en compte lorsque vous choisissez entre les deux méthodes principales des mises à jour de page de style AJAX. (Cette vidéo utilise le code de la vidéo [Comment prendre en main ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) et [Comment effectuer des rappels réseau côté client avec ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) Video.)

[&#9654;Regarder la vidéo (11 minutes)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> [Précédent](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [Suivant](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)
