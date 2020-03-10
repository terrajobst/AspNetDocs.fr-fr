---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Ajout de contenu dynamique à une page mise enC#cache () | Microsoft Docs
author: microsoft
description: Découvrez comment mélanger du contenu dynamique et mis en cache dans la même page. La substitution postérieure au cache vous permet d’afficher du contenu dynamique, par exemple des annonces de bannières...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: be43712d3dd5235117558e991d9dd71aa30ec470
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601583"
---
# <a name="adding-dynamic-content-to-a-cached-page-c"></a>Ajout de contenu dynamique à une page mise en cache (C#)

par [Microsoft](https://github.com/microsoft)

> Découvrez comment mélanger du contenu dynamique et mis en cache dans la même page. La substitution postérieure au cache vous permet d’afficher du contenu dynamique, tel que des annonces de bannière ou des éléments d’actualité, dans une page qui a été mise en cache.

En tirant parti de la mise en cache de sortie, vous pouvez améliorer considérablement les performances d’une application ASP.NET MVC. Au lieu de régénérer une page chaque fois que la page est demandée, la page peut être générée une seule fois et mise en cache en mémoire pour plusieurs utilisateurs.

Mais il y a un problème. Que se passe-t-il si vous avez besoin d’afficher du contenu dynamique dans la page ? Par exemple, imaginez que vous souhaitez afficher une bannière dans la page. Vous ne souhaitez pas que la bannière bannière soit mise en cache afin que chaque utilisateur voit la même publication. Vous n’avez rien à faire !

Heureusement, il existe une solution simple. Vous pouvez tirer parti d’une fonctionnalité de l’infrastructure ASP.NET appelée *substitution après le cache*. La substitution postérieure au cache vous permet de substituer du contenu dynamique dans une page qui a été mise en cache en mémoire.

En règle générale, lorsque vous exportez une page en sortie à l’aide de l’attribut [OutputCache], la page est mise en cache sur le serveur et le client (le navigateur Web). Lorsque vous utilisez la substitution après le cache, une page est mise en cache uniquement sur le serveur.

#### <a name="using-post-cache-substitution"></a>Utilisation de la substitution postérieure au cache

L’utilisation de la substitution après le cache requiert deux étapes. Tout d’abord, vous devez définir une méthode qui retourne une chaîne qui représente le contenu dynamique que vous souhaitez afficher dans la page mise en cache. Ensuite, vous appelez la méthode HttpResponse. WriteSubstitution () pour injecter le contenu dynamique dans la page.

Imaginez, par exemple, que vous souhaitez afficher de manière aléatoire différents éléments d’actualité dans une page mise en cache. La classe de la liste 1 expose une méthode unique, nommée RenderNews (), qui retourne de manière aléatoire un élément d’actualité à partir d’une liste de trois éléments d’information.

**Liste 1 – Models\News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Pour tirer parti de la substitution postérieure au cache, vous appelez la méthode HttpResponse. WriteSubstitution (). La méthode WriteSubstitution () configure le code pour remplacer une région de la page mise en cache par du contenu dynamique. La méthode WriteSubstitution () est utilisée pour afficher l’élément d’actualité aléatoire dans la vue de la liste 2.

**Liste 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

La méthode RenderNews est transmise à la méthode WriteSubstitution (). Notez que la méthode RenderNews n’est pas appelée (il n’y a pas de parenthèses). Au lieu de cela, une référence à la méthode est passée à WriteSubstitution ().

La vue index est mise en cache. La vue est retournée par le contrôleur dans la liste 3. Notez que l’action index () est décorée avec un attribut [OutputCache] qui provoque la mise en cache de la vue d’index pendant 60 secondes.

**Liste 3 – Controllers\HomeController.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

Même si la vue d’index est mise en cache, des éléments de News aléatoires différents s’affichent lorsque vous demandez la page d’index. Lorsque vous demandez la page d’index, l’heure affichée par la page ne change pas pendant 60 secondes (voir figure 1). Le fait que l’heure ne change pas prouve que la page est mise en cache. Toutefois, le contenu injecté par la méthode WriteSubstitution (), l’élément d’actualité aléatoire, change avec chaque demande.

**Figure 1 : injection d’éléments de News dynamiques dans une page mise en cache**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Utilisation de la substitution postérieure au cache dans les méthodes d’assistance

Un moyen plus simple de tirer parti de la substitution après le cache consiste à encapsuler l’appel à la méthode WriteSubstitution () au sein d’une méthode d’assistance personnalisée. Cette approche est illustrée par la méthode d’assistance de la liste 4.

**Liste 4 – AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

La liste 4 contient une classe statique qui expose deux méthodes : RenderBanner () et RenderBannerInternal (). La méthode RenderBanner () représente la méthode d’assistance réelle. Cette méthode étend la classe ASP.NET MVC HtmlHelper standard afin que vous puissiez appeler html. RenderBanner () dans une vue comme n’importe quelle autre méthode d’assistance.

La méthode RenderBanner () appelle la méthode HttpResponse. WriteSubstitution () qui passe la méthode RenderBannerInternal () à la méthode WriteSubstitution ().

La méthode RenderBannerInternal () est une méthode privée. Cette méthode n’est pas exposée en tant que méthode d’assistance. La méthode RenderBannerInternal () renvoie de manière aléatoire une image de publicité de bannière à partir d’une liste de trois images de publicité de bannière.

La vue d’index modifiée dans la liste 5 illustre comment vous pouvez utiliser la méthode d’assistance RenderBanner (). Notez qu’une directive supplémentaire &lt;% @ Import%&gt; est incluse en haut de la vue pour importer l’espace de noms MvcApplication1. helpers. Si vous négligez d’importer cet espace de noms, la méthode RenderBanner () n’apparaîtra pas en tant que méthode sur la propriété html.

**Liste 5 – Views\Home\Index.aspx (avec la méthode RenderBanner ())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

Lorsque vous demandez la page rendue par la vue dans la liste 5, une publicité de bannière différente s’affiche avec chaque demande (voir figure 2). La page est mise en cache, mais la bannière est injectée dynamiquement par la méthode d’assistance RenderBanner ().

**Figure 2 : vue d’index affichant une bannière de bannière aléatoire**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Récapitulatif

Ce didacticiel a expliqué comment vous pouvez mettre à jour dynamiquement le contenu d’une page mise en cache. Vous avez appris à utiliser la méthode HttpResponse. WriteSubstitution () pour permettre l’injection de contenu dynamique dans une page mise en cache. Vous avez également appris à encapsuler l’appel à la méthode WriteSubstitution () au sein d’une méthode d’assistance HTML.

Tirez parti de la mise en cache dans la mesure du possible. elle peut avoir un impact considérable sur les performances de vos applications Web. Comme expliqué dans ce didacticiel, vous pouvez tirer parti de la mise en cache même lorsque vous avez besoin d’afficher du contenu dynamique dans vos pages.

> [!div class="step-by-step"]
> [Précédent](improving-performance-with-output-caching-cs.md)
> [Suivant](creating-a-controller-cs.md)
