---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Fonctionnalités mobiles ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Il existe désormais une version MVC 5 de ce didacticiel avec des exemples de code dans déployer une application Web mobile MVC 5 ASP.NET sur sites Web Azure.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 907a16946c93761cd543135b0b226c8696b041f0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594635"
---
# <a name="aspnet-mvc-4-mobile-features"></a>Fonctionnalités mobiles d'ASP.NET MVC 4

par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Il existe désormais une version MVC 5 de ce didacticiel avec des exemples de code dans [déployer une application Web mobile MVC 5 ASP.net sur sites Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).

Ce didacticiel vous apprend les bases de l’utilisation des fonctionnalités mobiles dans une application Web ASP.NET MVC 4. Pour ce didacticiel, vous pouvez utiliser [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) ou Visual Web developer 2010 Express Service Pack 1 (&quot;Visual Web Developer ou VWD existant&quot;). Vous pouvez utiliser la version professionnelle de Visual Studio si vous en avez déjà besoin.

Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (recommandé) ou Visual Studio Web Developer Express SP1. Visual Studio 2012 contient ASP.NET MVC 4. Si vous utilisez Visual Web Developer 2010, vous devez installer [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Vous aurez également besoin d’un émulateur de navigateur mobile. L’une des opérations suivantes fonctionne :

- [Émulateur Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Il s’agit de l’émulateur utilisé dans la plupart des captures d’écran de ce didacticiel.)
- Modifiez la chaîne de l’agent utilisateur pour émuler un iPhone. Consultez [cette](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) entrée de blog.
- [Émulateur mobile Opera](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) avec l’agent utilisateur défini sur iPhone. Pour obtenir des instructions sur la façon de définir l’agent utilisateur dans Safari sur « iPhone », consultez Comment faire savoir [à Safari qu’il s’agit d’IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) sur le blog de David Alison.

Les projets Visual Studio C# avec le code source sont disponibles pour accompagner cette rubrique :

- [Téléchargement du projet de démarrage](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Téléchargement du projet terminé](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Ce que vous allez générer

Pour ce didacticiel, vous allez ajouter des fonctionnalités mobiles à l’application simple Listing Conference fournie dans le [projet de démarrage](https://go.microsoft.com/fwlink/?LinkId=228307). La capture d’écran suivante montre la page balises de l’application terminée, telle qu’elle apparaît dans l' [émulateur Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Pour plus d’informations sur l’entrée au clavier, consultez [mappage du clavier pour Windows Phone émulateur](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) .

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Vous pouvez utiliser Internet Explorer version 9 ou 10, FireFox ou chrome pour développer votre application mobile en définissant la chaîne de l' [agent utilisateur](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). L’illustration suivante montre le didacticiel terminé à l’aide d’Internet Explorer émulant un iPhone. Vous pouvez utiliser les outils de développement Internet Explorer F-12 et l' [outil Fiddler](http://www.fiddler2.com/fiddler2/) pour vous aider à déboguer votre application.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Compétences que vous allez apprendre

Voici ce que vous allez apprendre :

- Comment les modèles ASP.NET MVC 4 utilisent l’attribut `viewport` HTML5 et le rendu adaptatif pour améliorer l’affichage sur les appareils mobiles.
- Comment créer des affichages spécifiques aux appareils mobiles.
- Comment créer un sélecteur de vue qui permet aux utilisateurs de basculer entre une vue mobile et une vue de bureau de l’application.

### <a name="getting-started"></a>Mise en route

Téléchargez l’application de liste de conférences pour le projet de démarrage en utilisant le lien suivant : [Télécharger](https://go.microsoft.com/fwlink/?LinkId=228307). Ensuite, dans l’Explorateur Windows, cliquez avec le bouton droit sur le fichier *MvcMobile. zip* , puis sélectionnez **Propriétés**. Dans la boîte de dialogue **Propriétés de MvcMobile. zip** , choisissez le bouton **débloquer** . (Le déblocage empêche un avertissement de sécurité qui se produit lorsque vous essayez d’utiliser un fichier *. zip* que vous avez téléchargé à partir du Web.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Cliquez avec le bouton droit sur le fichier *MvcMobile. zip* et sélectionnez **extraire tout** pour décompresser le fichier. Dans Visual Studio, ouvrez le fichier *MvcMobile. sln* .

Appuyez sur CTRL + F5 pour exécuter l’application, ce qui l’affichera dans votre navigateur de bureau. Démarrez votre émulateur de navigateur mobile, copiez l’URL de l’application de conférence dans l’émulateur, puis cliquez sur le lien **Parcourir par balise** . Si vous utilisez l’émulateur de Windows Phone, cliquez dans la barre d’URL et appuyez sur la touche pause pour accéder au clavier. L’image ci-dessous montre la vue *AllTags* (à partir de la sélection de l’option **Parcourir par balise**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

L’affichage est très lisible sur un appareil mobile. Choisissez le lien ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

La vue de la balise ASP.NET est très encombrée. Par exemple, la colonne de **Date** est très difficile à lire. Plus loin dans ce didacticiel, vous allez créer une version de la vue *AllTags* spécifiquement pour les navigateurs mobiles et qui rendra l’affichage lisible.

Remarque : actuellement, il existe un bogue dans le moteur de mise en cache mobile. Pour les applications de production, vous devez installer le package [DisplayModes pépite fixe](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) . Pour plus d’informations sur le correctif, consultez [bogue ASP.NET MVC 4 mobile Caching corrigé](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) .

## <a name="css-media-queries"></a>Requêtes de média CSS

Les [requêtes de média CSS](http://www.w3.org/TR/css3-mediaqueries/) sont une extension de CSS pour les types de média. Elles vous permettent de créer des règles qui remplacent les règles CSS par défaut pour des navigateurs spécifiques (agents utilisateur). Une règle courante pour CSS qui cible les navigateurs mobiles définit la taille maximale de l’écran. Le fichier *Content\Site.CSS* créé lors de la création d’un nouveau projet Internet ASP.NET MVC 4 contient la requête multimédia suivante :

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Si la fenêtre du navigateur a une largeur de 850 pixels ou moins, elle utilisera les règles CSS à l’intérieur de ce bloc multimédia. Vous pouvez utiliser des requêtes de média CSS de ce type pour fournir un meilleur affichage du contenu HTML sur les petits navigateurs (tels que les navigateurs mobiles) que les règles CSS par défaut qui sont conçues pour des affichages plus larges des navigateurs de bureau.

## <a name="the-viewport-meta-tag"></a>La balise Meta Viewport

La plupart des navigateurs mobiles définissent une largeur de fenêtre de navigateur virtuel (la fenêtre d' *affichage*) qui est bien plus grande que la largeur réelle du périphérique mobile. Cela permet aux navigateurs mobiles d’ajuster l’intégralité de la page Web dans l’affichage virtuel. Les utilisateurs peuvent ensuite effectuer un zoom avant sur du contenu intéressant. Toutefois, si vous définissez la largeur de la fenêtre d’affichage sur la largeur réelle du périphérique, aucun zoom n’est nécessaire, car le contenu s’adapte au navigateur mobile.

La balise de `<meta>` de la fenêtre d’affichage dans le fichier de disposition ASP.NET MVC 4 définit la fenêtre d’affichage sur la largeur de l’appareil. La ligne suivante affiche la balise de `<meta>` de la fenêtre d’affichage dans le fichier de disposition ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Examen de l’effet des requêtes de média CSS et de la balise Meta Viewport

Ouvrez le fichier *Views\Shared\\_Layout. cshtml* dans l’éditeur et commentez la balise de `<meta>` de la fenêtre d’affichage. Le balisage suivant montre la ligne commentée.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Ouvrez le fichier *MvcMobile\Content\Site.CSS* dans l’éditeur et définissez la largeur maximale de la requête de média sur zéro pixel. Cela empêchera l’utilisation des règles CSS dans les navigateurs mobiles. La ligne suivante affiche la requête de média modifiée :

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Enregistrez vos modifications et accédez à l’application de conférence dans un émulateur de navigateur mobile. Le texte minuscule dans l’image suivante est le résultat de la suppression de la balise de `<meta>` de la fenêtre d’affichage. Sans balise de `<meta>` de fenêtre d’affichage, le navigateur effectue un zoom arrière sur la largeur de la fenêtre d’affichage par défaut (850 pixels ou plus large pour la plupart des navigateurs mobiles).

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Annuler vos modifications : supprimez les marques de commentaire de la balise de `<meta>` de la fenêtre d’affichage dans le fichier de disposition et restaurez la requête de média sur 850 pixels dans le fichier *site. CSS* . Enregistrez vos modifications et actualisez le navigateur mobile pour vérifier que l’affichage convivial des appareils mobiles a été restauré.

La balise de `<meta>` Viewport et la requête de média CSS ne sont pas spécifiques à ASP.NET MVC 4, et vous pouvez tirer parti de ces fonctionnalités dans n’importe quelle application Web. Mais elles sont désormais intégrées aux fichiers générés lorsque vous créez un nouveau projet ASP.NET MVC 4.

Pour plus d’informations sur la balise de `<meta>` Viewport, reportez-vous à l' [un des deux Viewports, deuxième partie](http://www.quirksmode.org/mobile/viewports2.html).

Dans la section suivante, vous allez apprendre à fournir des affichages spécifiques à un navigateur mobile.

## <a name="overriding-views-layouts-and-partial-views"></a>Remplacement des vues, des dispositions et des vues partielles

Une nouvelle fonctionnalité importante dans ASP.NET MVC 4 est un mécanisme simple qui vous permet de remplacer n’importe quelle vue (notamment les dispositions et les vues partielles) pour les navigateurs mobiles en général, pour un navigateur mobile individuel ou pour un navigateur spécifique. Pour fournir une vue mobile spécifique, vous pouvez copier un fichier de vue et ajouter *. Mobile* vers le nom de fichier. Par exemple, pour créer une vue de l' *index* mobile, copiez *Views\Home\Index.cshtml* dans *Views\Home\Index.mobile.cshtml*.

Dans cette section, vous allez créer un fichier de disposition mobile spécifique.

Pour commencer, copiez *Views\Shared\\_Layout. cshtml* vers *Views\Shared\\_Layout. mobile. cshtml*. Ouvrez *\_Layout. mobile. cshtml* et remplacez le titre **MVC4 Conference** par **Conference (mobile)** .

Dans chaque appel de `Html.ActionLink`, supprimez « parcourir par » dans chaque lien *ActionLink*. Le code suivant montre la section du corps terminé du fichier de disposition mobile.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Copiez le fichier *Views\Home\AllTags.cshtml* dans *Views\Home\AllTags.mobile.cshtml*. Ouvrez le nouveau fichier et remplacez l’élément `<h2>` « Tags » par « Tags (M) » :

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Accédez à la page balises à l’aide d’un navigateur de bureau et à l’aide de l’émulateur de navigateur mobile. L’émulateur de navigateur mobile affiche les deux modifications que vous avez apportées.

[![p2m_layoutTags. mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

En revanche, l’affichage du Bureau n’a pas changé.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Affichages spécifiques au navigateur

Outre les affichages spécifiques aux appareils mobiles et aux postes de travail, vous pouvez créer des affichages pour un navigateur individuel. Par exemple, vous pouvez créer des affichages spécifiquement pour le navigateur iPhone. Dans cette section, vous allez créer une disposition pour le navigateur iPhone et une version iPhone de la vue *AllTags* .

Ouvrez le fichier *global. asax* et ajoutez le code suivant à la méthode `Application_Start`.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Ce code définit un nouveau mode d’affichage nommé « iPhone » qui sera comparé à chaque demande entrante. Si la demande entrante correspond à la condition que vous avez définie (c’est-à-dire, si l’agent utilisateur contient la chaîne « iPhone »), ASP.NET MVC recherche les vues dont le nom contient le suffixe « iPhone ».

Dans le code, cliquez avec le bouton droit sur `DefaultDisplayMode`, choisissez **résoudre**, puis choisissez `using System.Web.WebPages;`. Cela ajoute une référence à l’espace de noms `System.Web.WebPages`, où les types `DisplayModes` et `DefaultDisplayMode` sont définis.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Vous pouvez également ajouter manuellement la ligne suivante à la section `using` du fichier.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Le contenu complet du fichier *global. asax* est présenté ci-dessous.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Enregistrez les modifications. Copiez le fichier *MvcMobile\Views\Shared\\_Layout. mobile. cshtml* sur *MvcMobile\Views\Shared\\_Layout. iPhone. cshtml*. Ouvrez le nouveau fichier, puis modifiez l’en-tête `h1` de `Conference (Mobile)` en `Conference (iPhone)`.

Copiez le fichier *MvcMobile\Views\Home\AllTags.mobile.cshtml* dans *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. Dans le nouveau fichier, remplacez l’élément `<h2>` « Tags (M) » par « Tags (iPhone) ».

Exécutez l'application. Exécutez un émulateur de navigateur mobile, assurez-vous que son agent utilisateur est défini sur « iPhone », puis accédez à la vue *AllTags* . La capture d’écran suivante montre la vue *AllTags* affichée dans le navigateur [Safari](http://www.apple.com/safari/download/) . Vous pouvez télécharger Safari pour Windows [ici](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

Dans cette section, nous avons vu comment créer des dispositions et des vues mobiles et comment créer des dispositions et des vues pour des appareils spécifiques tels que l’iPhone. Dans la section suivante, vous verrez comment tirer parti de jQuery mobile pour obtenir des affichages mobiles plus attrayants.

## <a name="using-jquery-mobile"></a>Utilisation de jQuery mobile

La bibliothèque [jQuery mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) Library fournit une infrastructure d’interface utilisateur qui fonctionne sur tous les principaux navigateurs mobiles. jQuery Mobile applique une *amélioration progressive* aux navigateurs mobiles qui prennent en charge CSS et JavaScript. L’amélioration progressive permet à tous les navigateurs d’afficher le contenu de base d’une page Web, tout en permettant à des navigateurs et appareils plus puissants d’avoir un affichage plus riche. Les fichiers JavaScript et CSS inclus avec jQuery mobile style de nombreux éléments pour s’adapter aux navigateurs mobiles sans apporter de modifications au balisage.

Dans cette section, vous allez installer le package NuGet *jQuery. mobile. Mvc* , qui installe jQuery mobile et un widget View-Switcher.

Pour commencer, supprimez le *\\partagé _Layout. mobile. cshtml* et les fichiers *\\_Layout. iPhone. cshtml partagés* que vous avez créés précédemment.

Renommez les fichiers *Views\Home\AllTags.mobile.cshtml* et *Views\Home\AllTags.iPhone.cshtml* en *Views\Home\AllTags.iPhone.cshtml.Hide* et *Views\Home\AllTags.mobile.cshtml.Hide*. Comme les fichiers n’ont plus d’extension *. cshtml* , ils ne sont pas utilisés par le runtime MVC ASP.net pour restituer la vue *AllTags* .

Installez le package NuGet *jQuery. mobile. Mvc* en procédant comme suit :

1. Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. Dans la **console du gestionnaire de package**, entrez `Install-Package jQuery.Mobile.MVC -version 1.0.0`

L’illustration suivante montre les fichiers ajoutés et modifiés au projet MvcMobile par le package NuGet jQuery. mobile. MVC. Les fichiers ajoutés ont le suffixe [Add] après le nom de fichier. L’image n’affiche pas les fichiers GIF et PNG ajoutés au dossier *Content\images* .

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Le package NuGet jQuery. mobile. MVC installe les éléments suivants :

- L' *application\_fichier Start\BundleMobileConfig.cs* , qui est nécessaire pour référencer les fichiers JavaScript et CSS jQuery ajoutés. Vous devez suivre les instructions ci-dessous et faire référence au Bundle mobile défini dans ce fichier.
- fichiers CSS mobiles jQuery.
- Un widget de contrôleur `ViewSwitcher` (*Controllers\ViewSwitcherController.cs*).
- fichiers JavaScript jQuery mobile.
- Un fichier de disposition de style mobile jQuery (*Views\Shared\\_Layout. mobile. cshtml*).
- Vue partielle du sélecteur d’affichage *(MvcMobile\Views\Shared\\_ViewSwitcher. cshtml*) qui fournit un lien en haut de chaque page pour basculer de la vue du bureau vers la vue mobile, et vice versa.
- Plusieurs fichiers image<em>. png</em> et <em>. gif</em> dans le dossier <em>Content\images</em> .

Ouvrez le fichier *global. asax* et ajoutez le code suivant comme dernière ligne de la méthode `Application_Start`.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Le code suivant montre le fichier *global. asax* complet.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Si vous utilisez Internet Explorer 9 et que vous ne voyez pas la `BundleMobileConfig` ligne au-dessus en jaune en surbrillance, cliquez [sur l'](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![image du bouton Affichage de compatibilité du bouton Affichage de compatibilité (désactivé)](https://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Image du bouton Affichage de compatibilité (désactivé)") dans IE pour faire passer l’icône d’une ![image de contour du bouton Affichage de compatibilité (désactivé)](https://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Image du bouton Affichage de compatibilité (désactivé)") à une image couleur unie ![du bouton Affichage de compatibilité (activé)](https://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "Image du bouton Affichage de compatibilité (activé)"). Vous pouvez également consulter ce didacticiel dans FireFox ou chrome.

Ouvrez le fichier *MvcMobile\Views\Shared\\_Layout. mobile. cshtml* et ajoutez le balisage suivant directement après l’appel de `Html.Partial` :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Le fichier *MvcMobile\Views\Shared\\_Layout. mobile. cshtml* complet est indiqué ci-dessous :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Générez l’application, puis dans votre émulateur de navigateur mobile, accédez à la vue *AllTags* . Les éléments suivants s’affichent :

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Vous pouvez déboguer le code mobile spécifique en [définissant la chaîne de l’agent utilisateur](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) pour Internet Explorer ou chrome sur iPhone, puis en utilisant les outils de développement F-12. Si votre navigateur mobile n’affiche pas **les liens**à l’écran, à l' **orateur**, à la **balise**et **à la date** sous forme de boutons, les références aux scripts et fichiers CSS de jQuery mobile ne sont probablement pas correctes.

En plus des modifications de style, vous pouvez afficher l’affichage **mobile** et un lien qui vous permet de basculer de la vue mobile vers la vue bureau. Choisissez le lien **affichage du Bureau** , et la vue Bureau s’affiche.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

La vue Bureau n’offre aucun moyen de revenir directement à l’affichage mobile. Vous le corrigerez maintenant. Ouvrez le fichier *Views\Shared\\_Layout. cshtml* . Juste sous la page `body` élément, ajoutez le code suivant, qui restitue le widget de commutateur de vue :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Actualisez la vue *AllTags* dans le navigateur mobile. Vous pouvez maintenant naviguer entre les affichages de bureau et mobiles.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Remarque de débogage : vous pouvez ajouter le code suivant à la fin de l'\\Views\Shared _ViewSwitcher. cshtml pour vous aider à déboguer des vues lors de l’utilisation d’un navigateur dont la chaîne de l’agent utilisateur est définie sur un appareil mobile.
>
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
>
> et en ajoutant l’en-tête suivant au fichier *Views\Shared\\_Layout. cshtml* .
>
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]

Accédez à la page *AllTags* dans un navigateur de bureau. Le widget de sélecteur d’affichage n’est pas affiché dans un navigateur de bureau, car il est ajouté uniquement à la page de disposition mobile. Plus loin dans ce didacticiel, vous verrez comment vous pouvez ajouter le widget vue-sélecteur à la vue bureau.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Amélioration de la liste des intervenants

Dans le navigateur mobile, sélectionnez le lien **Speakers** . Étant donné qu’il n’y a pas d’affichage mobile (*AllSpeakers. mobile. cshtml*), les haut-parleurs par défaut (*AllSpeakers. cshtml*) sont affichés à l’aide de la vue mobile Layout ( *\_Layout. mobile. cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Vous pouvez désactiver globalement une vue (non mobile) par défaut à partir du rendu dans une disposition mobile en définissant `RequireConsistentDisplayMode` sur `true` dans les *vues\\fichier _ViewStart. cshtml* , comme suit :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Lorsque `RequireConsistentDisplayMode` a la valeur `true`, la disposition mobile (<em>\_Layout. mobile. cshtml</em>) est utilisée uniquement pour les vues mobiles. (Autrement dit, le fichier de vue se présente sous la forme <em>* * viewName</em><em>. Mobile. cshtml</em>.) vous souhaiterez peut-être définir `RequireConsistentDisplayMode` sur `true` si votre disposition mobile ne fonctionne pas correctement avec vos vues non mobiles. La capture d’écran ci-dessous montre comment la page <em>Speakers</em> s’affiche lorsque `RequireConsistentDisplayMode` est défini sur `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Vous pouvez désactiver le mode d’affichage cohérent dans une vue en définissant `RequireConsistentDisplayMode` sur `false` dans le fichier de vue. Le balisage suivant dans le fichier *Views\Home\AllSpeakers.cshtml* définit `RequireConsistentDisplayMode` sur `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Création d’une vue de haut-parleurs mobiles

Comme vous venez de le constater, l’affichage des *haut-parleurs* est lisible, mais les liens sont petits et difficiles à toucher sur un appareil mobile. Dans cette section, vous allez créer une vue de *haut-parleurs* mobile qui ressemble à une application mobile moderne : elle affiche des liens volumineux et faciles à utiliser et contient une zone de recherche permettant de trouver rapidement les intervenants.

Copiez *AllSpeakers. cshtml* dans *AllSpeakers. mobile. cshtml*. Ouvrez le fichier *AllSpeakers. mobile. cshtml* et supprimez l’élément d’en-tête `<h2>`.

Dans la balise `<ul>`, ajoutez l’attribut `data-role` et définissez sa valeur sur `listview`. Comme d’autres [attributs de`data-*`](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` facilite le robinet des éléments de liste volumineux. Voici à quoi ressemble le balisage complet :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Actualisez le navigateur mobile. La vue mise à jour ressemble à ceci :

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Bien que l’affichage mobile s’ait amélioré, il est difficile de parcourir la longue liste des intervenants. Pour résoudre ce problème, dans la balise `<ul>`, ajoutez l’attribut `data-filter` et affectez-lui la valeur `true`. Le code ci-dessous montre le balisage `ul`.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

L’illustration suivante montre la zone de filtre de recherche en haut de la page qui résulte de l’attribut `data-filter`.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Lorsque vous tapez chaque lettre dans la zone de recherche, jQuery mobile filtre la liste affichée comme indiqué dans l’image ci-dessous.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Amélioration de la liste des balises

Comme pour l’affichage par défaut des *haut-parleurs* , la vue des *balises* est lisible, mais les liens sont petits et difficiles à taper sur un appareil mobile. Dans cette section, vous allez corriger la vue des *balises* de la même façon que vous avez résolu la vue des *intervenants* .

Supprimez le &quot;masquer&quot; suffixe dans le fichier *Views\Home\AllTags.mobile.cshtml.Hide* afin que le nom soit *Views\Home\AllTags.mobile.cshtml*. Ouvrez le fichier renommé et supprimez l’élément `<h2>`.

Ajoutez les attributs `data-role` et `data-filter` à la balise `<ul>`, comme illustré ici :

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

L’image ci-dessous montre le filtrage de page de balises sur la lettre `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Amélioration de la liste dates

Vous pouvez améliorer l’affichage des *dates* comme si vous avez amélioré les vues des *haut-parleurs* et des *balises* , afin qu’elles soient plus faciles à utiliser sur un appareil mobile.

Copiez le fichier *Views\Home\AllDates.cshtml* dans *Views\Home\AllDates.mobile.cshtml*. Ouvrez le nouveau fichier et supprimez l’élément `<h2>`.

Ajoutez `data-role="listview"` à la balise `<ul>`, comme suit :

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

L’image ci-dessous montre à quoi ressemble la page de **Date** avec l’attribut `data-role`.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) Remplacez le contenu du fichier *Views\Home\AllDates.mobile.cshtml* par le code suivant :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Ce code regroupe toutes les sessions par jours. Il crée un séparateur de liste pour chaque nouveau jour. il répertorie toutes les sessions pour chaque jour sous un séparateur. Voici à quoi elle ressemble lorsque ce code s’exécute :

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Amélioration de la vue SessionsTable

Dans cette section, vous allez créer une vue de sessions spécifique à mobile. Les modifications que nous apporterons sont plus étendues que dans les autres vues que nous avons créées.

Dans le navigateur mobile, appuyez sur le bouton **haut-parleur** , puis entrez `Sc` dans la zone de recherche.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Appuyez sur le lien **Scott Hanselman** .

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Comme vous pouvez le voir, l’affichage est difficile à lire sur un navigateur mobile. La colonne de date est difficile à lire et la colonne de balises est en dehors de la vue. Pour résoudre ce problème, copiez *Views\Home\SessionsTable.cshtml* vers *Views\Home\SessionsTable.mobile.cshtml*, puis remplacez le contenu du fichier par le code suivant :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Le code supprime les colonnes Room et Tags et met en forme le titre, l’intervenant et la date verticalement, afin que toutes ces informations soient lisibles sur un navigateur mobile. L’image ci-dessous reflète les modifications de code.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Amélioration de la vue SessionByCode

Enfin, vous allez créer une vue spécifique mobile de la vue *SessionByCode* . Dans le navigateur mobile, appuyez sur le bouton **haut-parleur** , puis entrez `Sc` dans la zone de recherche.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Appuyez sur le lien **Scott Hanselman** . Les sessions de Scott Hanselman s’affichent.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Choisissez **une vue d’ensemble du lien MS Web Stack d’amour** .

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

L’affichage du bureau par défaut est parfait, mais vous pouvez l’améliorer.

Copiez *Views\Home\SessionByCode.cshtml* sur *Views\Home\SessionByCode.mobile.cshtml* et remplacez le contenu du fichier *Views\Home\SessionByCode.mobile.cshtml* par le balisage suivant :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Le nouveau balisage utilise l’attribut `data-role` pour améliorer la disposition de la vue.

Actualisez le navigateur mobile. L’image suivante reflète les modifications de code que vous venez de faire :

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup et révision

Ce didacticiel a introduit les nouvelles fonctionnalités mobiles de ASP.NET MVC 4 developer preview. Les fonctionnalités mobiles sont les suivantes :

- Possibilité de remplacer la disposition, les vues et les vues partielles, à la fois globalement et pour une vue individuelle.
- Contrôle de la disposition et substitution partielle de la mise en œuvre à l’aide de la propriété `RequireConsistentDisplayMode`.
- Widget de sélecteur d’affichage pour les vues mobiles qui peut également être affiché dans les vues de bureau.
- Prise en charge de la prise en charge de navigateurs spécifiques, tels que le navigateur iPhone.

## <a name="see-also"></a>Voir aussi

- site [mobile jQuery](http://jquerymobile.com) .
- [Vue d’ensemble de jQuery mobile](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Recommandation du W3C sur les meilleures pratiques pour les applications Web mobiles](http://www.w3.org/TR/mwabp/)
- [Recommandation du W3C sur les candidats pour les requêtes de média](http://www.w3.org/TR/css3-mediaqueries/)
