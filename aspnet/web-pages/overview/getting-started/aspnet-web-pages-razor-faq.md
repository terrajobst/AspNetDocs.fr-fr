---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: FAQ sur les pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cet article répertorie les questions fréquemment posées sur pages Web ASP.NET (Razor) et WebMatrix. Versions logicielles utilisées dans le didacticiel pages Web ASP.NET (R...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 6fa1b668e915af856a693e79eafb7180ed504dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640930"
---
# <a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages - Questions fréquentes (FAQ) (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix n’est plus recommandé en tant qu’environnement de développement intégré pour pages Web ASP.NET. Utilisez [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) ou [Visual Studio code](https://code.visualstudio.com/).
>
> Cet article répertorie les questions fréquemment posées sur pages Web ASP.NET (Razor) et WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2, WebMatrix 2 et Visual Studio 2012.

- [Quelle est la différence entre pages Web ASP.NET, ASP.NET Web Forms et ASP.NET MVC ?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Ai-je besoin de WebMatrix pour pouvoir travailler avec des pages Web ?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Puis-je utiliser des contrôles de Web Forms ASP.NET sur une page Web pages ?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Puis-je déployer un site pages Web ASP.NET sans utiliser WebMatrix ?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Dois-je utiliser le programme d’assistance WebSecurity pour prendre en charge les connexions ?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Pages Web ASP.NET prend-il en charge HTML5 ?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Puis-je utiliser JavaScript et jQuery avec les pages Web ?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Ressources supplémentaires pour MSBuild](#AdditionalResources)

Pour toute question sur les erreurs et autres problèmes, consultez le [Guide de résolution des problèmes de pages Web ASP.net (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Quelle est la différence entre pages Web ASP.NET, ASP.NET Web Forms et ASP.NET MVC ?

Les trois technologies ASP.NET pour la création d’applications Web dynamiques :

- Pages Web ASP.NET se concentre sur l’ajout de code dynamique (côté serveur) et l’accès aux pages HTML, et présente une syntaxe simple et légère.
- ASP.NET Web Forms est basé sur un modèle objet page et des contrôles de type fenêtre traditionnels (boutons, listes, etc.). Web Forms utilise un modèle basé sur les événements qui est familier à ceux qui ont travaillé avec le développement basé sur le client (Windows Forms).
- ASP.NET MVC implémente le modèle MVC (Model-View-Controller) pour ASP.NET. L’accent est mis sur la « séparation des préoccupations » (couches de traitement, de données et d’interface utilisateur).

Les trois frameworks sont entièrement pris en charge et continuent à être développés par l’équipe ASP.NET. En général, le choix de l’infrastructure à utiliser dépend de votre expérience et de l’expérience utilisateur avec ASP.NET.

En particulier, pages Web ASP.NET a été conçu pour faciliter la tâche des personnes qui connaissent déjà HTML pour ajouter le traitement du serveur à leurs pages. Il s’agit d’un bon choix pour les élèves, les amateurs, les personnes en général qui commencent à programmer. Il peut également être un bon choix pour les développeurs qui ont une expérience des technologies Web non-ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Ai-je besoin de WebMatrix pour pouvoir travailler avec des pages Web ?

Non. WebMatrix n’est plus recommandé en tant qu’environnement de développement intégré pour pages Web ASP.NET. Utilisez [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) ou [Visual Studio code](https://code.visualstudio.com/).

Si vous ne souhaitez pas utiliser Visual Studio ou Visual Studio Code, vous pouvez installer les produits composants individuellement à l’aide de [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Vous avez besoin des produits suivants :

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (qui installe également le pages Web ASP.NET Framework)
- IIS Express (serveur Web)
- Microsoft SQL Server Compact 4,0 (la base de données)

Vous pouvez utiliser un éditeur de texte pour modifier les pages *. cshtml* (ou *. vbhtml*).

La gestion des bases de données SQL Server Compact (fichiers *. sdf* ) sans outil est un peu plus difficile. Outils containds de Visual Studio pour la gestion des bases de données *. sdf* . Vous pouvez également exécuter des commandes SQL dans le code pour effectuer de nombreuses tâches de gestion de SQL Server.

Pour tester les pages *. cshtml* sans utiliser un environnement de développement intégré (IDE), vous pouvez les déployer sur un serveur actif. (Consultez Comment [déployer un site pages Web ASP.net sans utiliser WebMatrix ?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Exécution de IIS Express sans utiliser d’IDE

Si vous installez IIS Express sur votre ordinateur en tant que serveur Web, vous pouvez l’utiliser pour tester les pages. Vous pouvez exécuter IIS Express à partir de la ligne de commande et l’associer à un numéro de port spécifique. Vous spécifiez ensuite ce port lorsque vous demandez des fichiers *. cshtml* dans votre navigateur.

Dans Windows, ouvrez une invite de commandes avec des privilèges d’administrateur et accédez à *C:\Program Files\IIS Express.* (Pour les systèmes 64 bits, utilisez le dossier *C:\Program Files (x86) \IIS Express.)* Entrez ensuite la commande suivante, en utilisant le chemin d’accès réel à votre site :

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Vous pouvez utiliser n’importe quel numéro de port qui n’est pas déjà réservé par un autre processus. (Les numéros de port supérieurs à 1024 sont généralement gratuits.) Pour la valeur `path`, utilisez le chemin d’accès du dossier du site Web où se trouvent les fichiers *. cshtml* .

Après avoir exécuté cette commande pour configurer IIS Express pour servir vos pages, vous pouvez ouvrir un navigateur et accéder à un fichier *. cshtml* . Utilisez une URL semblable à la suivante :

`http://localhost:35896/default.cshtml`

Pour obtenir de l’aide sur IIS Express options de ligne de commande, entrez `iisexpress.exe /?` sur la ligne de commande.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Puis-je utiliser des contrôles de Web Forms ASP.NET sur une page Web pages ?

Non. Web Forms contrôles tels que le contrôle [CheckBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) , les [contrôles de validation](https://msdn.microsoft.com/library/bwd43d0x)et le contrôle [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) fonctionnent uniquement dans les pages Web Forms (fichiers *. aspx* ). Ces contrôles requièrent l’infrastructure de page Web Forms.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Puis-je déployer un site pages Web ASP.NET sans utiliser WebMatrix ?

Oui. Vous pouvez copier manuellement les fichiers du site Web sur un serveur (généralement à l’aide de FTP). Si vous effectuez une copie manuelle, vous devez également copier les fichiers qui prennent en charge SQL Server Compact (la base de données). Pour plus d’informations, consultez l’entrée de blog [déploiement d’applications Web pages sans outil](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Dois-je utiliser le programme d’assistance WebSecurity pour prendre en charge les connexions ?

Non. Le fournisseur `SimpleMembership` qui fait partie de pages Web ASP.NET est une option. Les fournisseurs de sécurité qui font partie de ASP.NET (que vous pouvez utiliser avec dans Web Forms) sont également disponibles. Par exemple, vous pouvez utiliser l’authentification par formulaire dans pages Web ASP.NET de la même façon que vous le feriez dans Web Forms. Pour obtenir un exemple d’utilisation de l’authentification par formulaire, consultez l’article Support Microsoft [comment implémenter l’authentification basée sur les formulaires dans votre C#application ASP.net à l’aide de .net](https://support.microsoft.com/kb/301240). Pour télécharger un exemple simple, consultez [ASP.NET version de «Login &amp; Password](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Pour plus d’informations sur l’utilisation de l’authentification Windows, consultez le billet [de blog utilisation de l’authentification Windows dans pages Web ASP.net](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Pages Web ASP.NET prend-il en charge HTML5 ?

Oui. Les pages que vous créez avec les pages Web ASP.NET (pages *. cshtml* ou *. vbhtml* ) sont essentiellement des pages HTML qui contiennent également du code qui s’exécute sur le serveur, avant le rendu de la page. Tant que le navigateur de l’utilisateur prend en charge HTML5, vous pouvez utiliser des éléments HTML5 dans une page *. cshtml* ou *. vbhtml* .

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Puis-je utiliser JavaScript et jQuery avec les pages Web ?

Absolument. Les pages que vous créez avec les pages Web ASP.NET (pages *. cshtml* ou *. vbhtml* ) sont simplement des pages HTML contenant du code serveur. Par conséquent, tout ce que vous pouvez faire dans une page HTML normale à l’aide de JavaScript ou de jQuery, vous pouvez également le faire dans une page *. cshtml* ou *. vbhtml* .

Le modèle **Starter Site** dans WebMatrix contient un certain nombre de bibliothèques jQuery. Si vous créez un site à l’aide de ce modèle, le dossier *scripts* contient une bibliothèque jQuery Core (*jQuery-1.6.2. js)* et des bibliothèques pour la validation jQuery (*jQuery. Validate. js*, etc.).

Voici quelques billets de blog qui illustrent les différentes façons d’utiliser jQuery avec pages Web ASP.NET :

- [Ajout de jQuery Goody à pages Web ASP.net à l’aide de WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) par Rachel appel
- [5 min : WebMatrix + jQuery UI + JSON + modèles jQuery](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) par Jonas Eriksson
- [WebMatrix et les formulaires jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) par Mike salé

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[ASP.NET Web Pages (Razor) - Guide de résolution des problèmes](https://go.microsoft.com/fwlink/?LinkId=253001)

[WebMatrix et pages Web ASP.net forum](https://forums.asp.net/1224.aspx/1?WebMatrix) sur le site Web ASP.net
