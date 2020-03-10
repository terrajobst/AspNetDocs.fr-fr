---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Création d’URL lisibles dans des sites pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cet article décrit le routage dans un site Web pages Web ASP.NET (Razor) et explique comment cela vous permet d’utiliser des URL plus lisibles et mieux adaptées à SEO. Ce que vous allez faire...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 832db8e144cab730f16c78f67c12feb9b7c92c7c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628393"
---
# <a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Création d’URL lisibles dans des sites pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article décrit le routage dans un site Web pages Web ASP.NET (Razor) et explique comment cela vous permet d’utiliser des URL plus lisibles et mieux adaptées à SEO.
> 
> Ce que vous allez apprendre :
> 
> - Comment ASP.NET utilise le routage pour vous permettre d’utiliser des URL plus lisibles et pouvant faire l’objet d’une recherche.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2.

## <a name="about-routing"></a>À propos du routage

Les URL des pages de votre site peuvent avoir un impact sur le fonctionnement du site. Une URL &quot;&quot; conviviale peut faciliter l’utilisation du site par les personnes. Il peut également être utile avec l’optimisation du moteur de recherche (SEO) pour le site. Les sites Web ASP.NET incluent la possibilité d’utiliser automatiquement des URL conviviales.

ASP.NET vous permet de créer des URL explicites qui décrivent les actions des utilisateurs au lieu de simplement pointer vers un fichier sur le serveur. Considérez ces URL pour un blog fictif :

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Comparez ces URL aux éléments suivants :

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

Dans la première paire, un utilisateur doit savoir que le blog s’affiche à l’aide de la page *blog. cshtml* , puis créer une chaîne de requête qui obtient la bonne catégorie ou plage de dates. Le deuxième ensemble d’exemples est beaucoup plus facile à comprendre et à créer.

Les URL du premier exemple pointent également directement vers un fichier spécifique (*blog. cshtml*). Si, pour une raison quelconque, le blog a été déplacé vers un autre dossier sur le serveur, ou si le blog a été réécrit pour utiliser une autre page, les liens seraient erronés. Le deuxième ensemble d’URL ne pointe pas vers une page spécifique. ainsi, même si l’implémentation du blog ou l’emplacement change, les URL sont toujours valides.

Dans pages Web ASP.NET, vous pouvez créer des URL plus conviviales comme celles des exemples ci-dessus, car ASP.NET utilise le *routage*. Le routage crée un mappage logique à partir d’une URL vers une page (ou des pages) qui peut traiter la demande. Étant donné que le mappage est logique (pas physique, vers un fichier spécifique), le routage offre une grande flexibilité dans la façon dont vous définissez les URL de votre site.

## <a name="how-routing-works"></a>Fonctionnement du routage

Quand ASP.NET traite une requête, il lit l’URL pour déterminer comment l’acheminer. ASP.NET tente de faire correspondre des segments individuels de l’URL à des fichiers sur le disque, en allant de gauche à droite. S’il y a une correspondance, tout ce qui reste dans l’URL est passé à la page en tant qu' *informations de chemin d’accès*.

Imaginez qu’une personne effectue une demande à l’aide de cette URL :

`http://www.contoso.com/a/b/c`

La recherche se présente comme suit :

1. Existe-t-il un fichier avec le chemin d’accès et le nom de */a/b/c.cshtml*? Dans ce cas, exécutez cette page et ne lui transmettez aucune information. Sinon...
2. Existe-t-il un fichier avec le chemin d’accès et le nom de */a/b.cshtml*? Dans ce cas, exécutez cette page et transmettez la valeur `c` à celle-ci. Sinon...
3. Existe-t-il un fichier avec le chemin d’accès et le nom de */a.cshtml*? Dans ce cas, exécutez cette page et transmettez la valeur `b/c` à celle-ci.

Si la recherche n’a trouvé aucune correspondance exacte pour les fichiers *. cshtml* dans leurs dossiers spécifiés, ASP.NET continue à rechercher ces fichiers à tour de fonction :

1. */a/b/c/default.cshtml* (aucune information sur le chemin d’accès).
2. */a/b/c/index.cshtml* (aucune information sur le chemin d’accès).

> [!NOTE]
> Pour être clair, les demandes de pages spécifiques (autrement dit, les demandes incluant l’extension de nom de fichier *. cshtml* ) fonctionnent comme prévu. Une demande comme `http://www.contoso.com/a/b.cshtml` exécutera la page *b. cshtml* .

À l’intérieur d’une page, vous pouvez obtenir les informations de chemin d’accès via la propriété `UrlData` de la page, qui est un dictionnaire. Imaginez que vous avez un fichier nommé *ViewCustomers. cshtml* et que votre site reçoit cette demande :

`http://mysite.com/myWebSite/ViewCustomers/1000`

Comme décrit dans les règles ci-dessus, la demande est envoyée à votre page. À l’intérieur de la page, vous pouvez utiliser un code semblable au suivant pour obtenir et afficher les informations relatives au chemin (dans ce cas, la valeur &quot;1000&quot;) :

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Étant donné que le routage n’implique pas de noms de fichiers complets, il peut y avoir une ambiguïté si vous avez des pages qui ont le même nom mais des extensions de nom de fichier différentes (par exemple, *MyPage. cshtml* et *MyPage. html*). Afin d’éviter les problèmes de routage, il est préférable de s’assurer que vous n’avez pas de pages dans votre site dont les noms diffèrent uniquement par leur extension.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[WebMatrix : URL, URLData et routage pour SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Cette entrée de blog de Mike salée fournit des informations supplémentaires sur le fonctionnement du routage dans pages Web ASP.NET.
