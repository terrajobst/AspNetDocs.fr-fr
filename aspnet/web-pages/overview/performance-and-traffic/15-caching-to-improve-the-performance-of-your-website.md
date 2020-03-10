---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Mise en cache des données dans un site pages Web ASP.NET (Razor) pour de meilleures performances | Microsoft Docs
author: Rick-Anderson
description: Vous pouvez accélérer votre site Web en le stockant, c’est-à-dire le cache-les résultats des données qui, normalement, prendront beaucoup de temps pour récupérer ou traiter un...
ms.author: riande
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 01796d3ca699a6af5d9162b22a926551435c2040
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641518"
---
# <a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Mise en cache des données dans un site pages Web ASP.NET (Razor) pour de meilleures performances

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment utiliser une application d’assistance pour mettre en cache des informations pour accélérer les performances d’un site Web pages Web ASP.NET (Razor). Vous pouvez accélérer votre site Web en faisant en sorte &#8212; qu’il stocke &#8212; les résultats de données qui, en général, prendront beaucoup de temps pour la récupération ou le traitement et qui ne change pas souvent.
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment utiliser la mise en cache pour améliorer la réactivité de votre site Web.
> 
> Voici les fonctionnalités ASP.NET présentées dans l’article :
> 
> - Le programme d’assistance `WebCache`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2.

Chaque fois qu’une personne demande une page de votre site, le serveur Web doit effectuer un travail afin de traiter la demande. Pour certaines de vos pages, le serveur devra peut-être effectuer des tâches qui prennent un certain temps (en comparaison), telles que la récupération des données d’une base de données. Même si ces tâches ne prennent pas beaucoup de temps en termes absolus, si votre site subit beaucoup de trafic, toute une série de demandes individuelles qui obligent le serveur Web à exécuter la tâche complexe ou lente peut s’ajouter à beaucoup de travail. Cela peut finalement avoir une incidence sur les performances du site.

Une façon d’améliorer les performances de votre site Web dans des cas comme celui-ci consiste à mettre en cache des données. Si votre site reçoit des demandes répétées pour les mêmes informations et que les informations ne doivent pas être modifiées pour chaque personne et qu’il n’est pas sensible au temps, au lieu de le réextraire ou de le recalculer, vous pouvez extraire les données une seule fois, puis stocker les résultats. La prochaine fois qu’une requête arrive pour cette information, vous la récupérez simplement dans le cache.

En général, vous mettez en cache des informations qui ne changent pas fréquemment. Lorsque vous placez des informations dans le cache, elles sont stockées en mémoire sur le serveur Web. Vous pouvez spécifier la durée de mise en cache, de secondes à jours. Lorsque la période de mise en cache expire, les informations sont automatiquement supprimées du cache.

> [!NOTE]
> Les entrées dans le cache peuvent être supprimées pour des raisons autres que celles qui ont expiré. Par exemple, le serveur Web risque de manquer temporairement de mémoire, et une manière de récupérer de la mémoire consiste à lever des entrées hors du cache. Comme vous le verrez, même si vous avez placé des informations dans le cache, vous devez vérifier qu’elles sont toujours là lorsque vous en avez besoin.

Imaginez que votre site Web dispose d’une page affichant les prévisions de température et de météo actuelles. Pour obtenir ce type d’informations, vous pouvez envoyer une demande à un service externe. Étant donné que ces informations ne changent pas très (au cours d’une période de deux heures, par exemple) et que les appels externes nécessitent du temps et de la bande passante, il s’agit d’un bon candidat pour la mise en cache.

## <a name="adding-caching-to-a-page"></a>Ajout de la mise en cache à une page

ASP.NET comprend un programme d’assistance `WebCache` qui facilite l’ajout de la mise en cache à votre site et l’ajout de données au cache. Dans cette procédure, vous allez créer une page qui met en cache l’heure actuelle. Ce n’est pas un exemple concret, puisque l’heure actuelle est un changement fréquent et qu’il n’est pas non plus complexe à calculer. Toutefois, il s’agit d’un bon moyen d’illustrer la mise en cache en action.

1. Ajoutez une nouvelle page nommée *WEBCACHE. cshtml* au site Web.
2. Ajoutez le code et le balisage suivants à la page :

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Lorsque vous mettez en cache des données, vous les placez dans le cache à l’aide d’un nom unique sur le site Web. Dans ce cas, vous utiliserez une entrée de cache nommée `CachedTime`. Il s’agit de la `cacheItemKey` présentée dans l’exemple de code.

    Le code commence par lire l’entrée de cache `CachedTime`. Si une valeur est retournée (autrement dit, si l’entrée du cache n’est pas null), le code définit simplement la valeur de la variable d’heure sur les données du cache.

    Toutefois, si l’entrée du cache n’existe pas (c’est-à-dire qu’elle est null), le code définit la valeur d’heure, l’ajoute au cache et définit une valeur d’expiration à une minute. Après une minute, l’entrée du cache est ignorée. (La valeur d’expiration par défaut pour un élément dans le cache est de 20 minutes.) La `WebCache.Set(cacheItemKey, time, 1, false)` de commande montre comment ajouter la valeur d’heure actuelle au cache et définir son expiration sur 1 minute. Si vous définissez le paramètre *slidingExpiration* sur `false`, le délai d’expiration n’est pas renouvelé chaque fois qu’il est demandé. Il expire exactement 1 minute après son ajout à l’origine au cache. Si vous définissez cette valeur sur `true` le délai d’expiration de 1 minute est réinitialisé chaque fois que la valeur est demandée à partir du cache.

    Ce code illustre le modèle que vous devez toujours utiliser lorsque vous mettez en cache des données. Avant d’extraire un peu du cache, vérifiez toujours d’abord si la méthode `WebCache.Get` a retourné la valeur null. N’oubliez pas que l’entrée de cache a peut-être expiré ou a peut-être été supprimée pour une raison quelconque, de sorte que toute entrée donnée n’est jamais garantie dans le cache.
3. Exécutez *WEBCACHE. cshtml* dans un navigateur. (Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.) La première fois que vous demandez la page, les données d’heure ne sont pas dans le cache et le code doit ajouter la valeur d’heure au cache.

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Actualisez *WEBCACHE. cshtml* dans le navigateur. Cette fois-ci, les données de temps se trouvent dans le cache. Notez que l’heure n’a pas changé depuis la dernière fois que vous avez consulté la page.

    ![cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Patientez une minute avant que le cache soit vidé, puis actualisez la page. La page indique de nouveau que les données d’heure n’ont pas été trouvées dans le cache, et l’heure mise à jour est ajoutée au cache.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Affichage de données dans un graphique](https://go.microsoft.com/fwlink/?LinkId=202895)
- Informations de référence sur l' [API WEBCACHE](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
