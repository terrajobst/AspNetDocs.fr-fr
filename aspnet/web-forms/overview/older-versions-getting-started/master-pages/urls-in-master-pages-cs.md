---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: URL dans les pages maîtresC#() | Microsoft Docs
author: rick-anderson
description: Résout le risque de rupture des URL dans la page maître en raison du fait que le fichier de la page maître se trouve dans un répertoire relatif différent de la page de contenu. Recherche la relocalisation...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 2551a5361256234883bb37e46e794037284445a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78585847"
---
# <a name="urls-in-master-pages-c"></a>URL dans les pages maîtres (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)

> Résout le risque de rupture des URL dans la page maître en raison du fait que le fichier de la page maître se trouve dans un répertoire relatif différent de la page de contenu. Examine la relocalisation des URL via ~ dans la syntaxe déclarative et l’utilisation de ResolveUrl et ResolveClientUrl par programmation. (Consultez également

## <a name="introduction"></a>Introduction

Dans tous les exemples que nous avons vus jusqu’à présent, les pages maître et de contenu se trouvent dans le même dossier (le dossier racine du site Web). Mais il n’y a aucune raison pour laquelle les pages maîtres et de contenu doivent se trouver dans le même dossier. Vous pouvez certainement créer des pages de contenu dans des sous-dossiers. De même, vous pouvez créer un dossier `~/MasterPages/` dans lequel vous placez les pages maîtres de votre site.

L’un des problèmes potentiels liés au placement de pages maîtres et de contenu dans des dossiers distincts implique des URL rompues. Si la page maître contient des URL relatives dans des liens hypertexte, des images ou d’autres éléments, le lien n’est pas valide pour les pages de contenu qui se trouvent dans un dossier différent. Dans ce didacticiel, nous examinons la source de ce problème, ainsi que les solutions de contournement.

## <a name="the-problem-with-relative-urls"></a>Le problème avec les URL relatives

Une URL sur une page Web est dite *URL relative* si l’emplacement de la ressource vers laquelle elle pointe est relatif à l’emplacement de la page Web dans la structure de dossiers du site Web. Les URL qui ne commencent pas par une barre oblique de début (`/`) ou un protocole (tel que `http://`) sont relatives, car elles sont résolues par le navigateur en fonction de l’emplacement de la page Web qui contient l’URL.

Par exemple, notre site Web contient un dossier `~/Images/` avec un fichier image unique, `PoweredByASPNET.gif`. Le fichier de page maître `Site.master` a un élément `<img>` dans la région `footerContent` avec le balisage suivant :

[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

La valeur d’attribut `src` dans l’élément `<img>` est une URL relative, car elle ne commence pas par `/` ou `http://`. En résumé, la valeur d’attribut `src` indique au navigateur d’examiner le sous-dossier `Images` pour un fichier nommé `PoweredByASPNET.gif`.

Quand vous visitez une page de contenu, le balisage ci-dessus est envoyé directement au navigateur. Prenez un moment pour visiter `About.aspx` et afficher la source HTML envoyée au navigateur. Vous constaterez que le même balisage dans la page maître a été envoyée au navigateur.

[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

Si la page de contenu se trouve dans le dossier racine (comme c’est le cas `About.aspx`), tout fonctionne comme prévu, car il existe un sous-dossier `Images` relatif au dossier racine. Toutefois, les choses s’interrompent si la page de contenu se trouve dans un dossier différent de celui de la page maître. Pour illustrer cela, créez un sous-dossier nommé `Admin`. Ensuite, ajoutez une page de contenu nommée `Default.aspx` au dossier `Admin`, en veillant à lier la nouvelle page à la page maître `Site.master`.

> [!NOTE]
> Dans le didacticiel [*spécification du titre, des balises meta et d’autres en-têtes HTML du didacticiel de la page maître,* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) nous avons créé une classe de page de base personnalisée nommée `BasePage` qui définit automatiquement le titre de la page de contenu (si elle n’a pas été explicitement assignée). N’oubliez pas que la classe code-behind de la page nouvellement créée dérive de `BasePage` pour pouvoir utiliser cette fonctionnalité.

Une fois que vous avez créé cette page de contenu, votre Explorateur de solutions doit ressembler à la figure 1.

![Un nouveau dossier et une nouvelle page ASP.NET ont été ajoutés au projet](urls-in-master-pages-cs/_static/image1.png)

**Figure 01**: un nouveau dossier et une nouvelle page ASP.net ont été ajoutés au projet

Ensuite, mettez à jour le fichier `Web.sitemap` pour inclure une nouvelle entrée `<siteMapNode>` pour cette leçon. Le code XML suivant montre le balisage complet `Web.sitemap`, qui comprend désormais l’ajout d’un troisième élément `<siteMapNode>`.

[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

La page de `Default.aspx` nouvellement créée doit avoir quatre contrôles de contenu correspondant aux quatre ContentPlaceHolders dans `Site.master`. Ajoutez du texte au contrôle de contenu référençant le `MainContent` ContentPlaceHolder, puis accédez à la page via un navigateur. Comme le montre la figure 2, le navigateur ne peut pas trouver le fichier image `PoweredByASPNET.gif`. Comment cela se fait-il ?

La page de contenu `~/Admin/Default.aspx` reçoit le même code HTML pour la région `footerContent` que la page `About.aspx` :

[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

Étant donné que l’attribut `src` de l’élément `<img>` est une URL relative, le navigateur tente de rechercher un dossier `Images` relatif à l’emplacement du dossier de la page Web. En d’autres termes, le navigateur recherche le fichier image `Admin/Images/PoweredByASPNET.gif`.

[![le fichier image PoweredByASPNET. gif est introuvable](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)

**Figure 02**: le fichier image `PoweredByASPNET.gif` est introuvable ([cliquez pour afficher l’image en taille réelle](urls-in-master-pages-cs/_static/image4.png))

### <a name="replacing-relative-urls-with-absolute-urls"></a>Remplacement des URL relatives par des URL absolues

L’inverse d’une URL relative est une *URL absolue*, qui est une URL qui commence par une barre oblique (`/`) ou un protocole tel que `http://`. Étant donné qu’une URL absolue spécifie l’emplacement d’une ressource à partir d’un point fixe connu, la même URL absolue est valide dans n’importe quelle page Web, quel que soit l’emplacement de la page Web dans la structure de dossiers du site Web.

Pour remédier à l’image cassée illustrée dans la figure 2, nous devons mettre à jour l’attribut `src` de l’élément `<img>` afin qu’il utilise une URL absolue au lieu d’une URL relative. Pour déterminer l’URL absolue correcte, accédez à l’une des pages Web de votre site Web, puis examinez la barre d’adresses. Comme le montre la barre d’adresses de la figure 2, le chemin d’accès complet à l’application Web est `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`. Par conséquent, nous pourrions mettre à jour l’attribut `src` de l’élément `<img>` avec l’une ou l’autre des deux URL absolues suivantes :

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

Prenez un moment pour mettre à jour l’attribut `src` de l’élément `<img>` sur une URL absolue à l’aide de l’un des formulaires ci-dessus, puis accédez à la page `~/Admin/Default.aspx` via un navigateur. Cette fois, le navigateur trouvera et affichera correctement le fichier image `PoweredByASPNET.gif` (voir figure 3).

[![l’image PoweredByASPNET. gif est maintenant affichée](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)

**Figure 03**: l’image `PoweredByASPNET.gif` s’affiche à présent ([cliquez pour afficher l’image en taille réelle](urls-in-master-pages-cs/_static/image7.png))

Bien que le codage en dur dans l’URL absolue fonctionne, il associe étroitement votre code HTML à l’emplacement du serveur et du dossier du site Web, ce qui peut changer. L’utilisation d’une URL absolue de la forme `http://localhost:3908/...` est fragile car le numéro de port qui précède `localhost` est automatiquement sélectionné chaque fois que le serveur Web de développement ASP.NET intégré de Visual Studio est démarré. De même, la partie `http://localhost` est valide uniquement lors du test local. Une fois que le code est déployé sur un serveur de production, la base de l’URL passe à autre chose, comme `http://www.yourserver.com`. L’URL absolue au format `/ASPNET_MasterPages_Tutorial_04_CS/...` souffre également d’une même fragilité, car ce chemin d’application diffère souvent entre les serveurs de développement et de production.

La bonne nouvelle, c’est que ASP.NET offre une méthode permettant de générer une URL relative valide au moment de l’exécution.

## <a name="usingandresolveclienturl"></a>Utilisation de`~`et`ResolveClientUrl`

Plutôt que de coder en dur une URL absolue, ASP.NET permet aux développeurs de pages d’utiliser le tilde (`~`) pour indiquer la racine de l’application Web. Par exemple, précédemment dans ce didacticiel, j’ai utilisé la notation `~/Admin/Default.aspx` dans le texte pour faire référence à la page `Default.aspx` dans le dossier `Admin`. Le `~` indique que le dossier `Admin` est un sous-dossier de la racine de l’application Web.

La [méthode`ResolveClientUrl`](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) de la classe `Control` prend une URL et la modifie en une URL relative appropriée pour la page Web sur laquelle le contrôle réside. Par exemple, l’appel de `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` à partir de `About.aspx` retourne `Images/PoweredByASPNET.gif`. Toutefois, son appel à partir de `~/Admin/Default.aspx`retourne `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Étant donné que tous les contrôles serveur ASP.NET dérivent de la classe `Control`, tous les contrôles serveur ont accès à la méthode `ResolveClientUrl`. Même la classe `Page` dérive de la classe `Control`, ce qui signifie que vous pouvez utiliser cette méthode directement à partir des classes code-behind de vos pages ASP.NET.

### <a name="usingin-the-declarative-markup"></a>Utilisation de`~`dans le balisage déclaratif

Plusieurs contrôles Web ASP.NET incluent des propriétés associées à l’URL : le contrôle de lien hypertexte a une propriété `NavigateUrl` ; le contrôle image a une propriété `ImageUrl` ; et ainsi de suite. Lorsqu’ils sont rendus, ces contrôles passent leurs valeurs de propriété associées à l’URL à `ResolveClientUrl`. Par conséquent, si ces propriétés contiennent un `~` pour indiquer la racine de l’application Web, l’URL sera modifiée en une URL relative valide.

N’oubliez pas que seuls les contrôles serveur ASP.NET transforment les `~` dans leurs propriétés associées à l’URL. Si un `~` apparaît dans le balisage HTML statique, comme `<img src="~/Images/PoweredByASPNET.gif" />`, le moteur ASP.NET envoie l' `~` au navigateur avec le reste du contenu HTML. Le navigateur suppose que le `~` fait partie de l’URL. Par exemple, si le navigateur reçoit le balisage `<img src="~/Images/PoweredByASPNET.gif" />` il part du principe qu’il existe un sous-dossier nommé `~` avec un sous-dossier `Images` contenant le fichier image `PoweredByASPNET.gif`.

Pour corriger le balisage de l’image dans `Site.master`, remplacez l’élément `<img>` existant par un contrôle Web d’image ASP.NET. Définissez la `ID` du contrôle Web de l’image sur `PoweredByImage`, sa propriété `ImageUrl` sur `~/Images/PoweredByASPNET.gif`, et sa propriété `AlternateText` sur « Powered by ASP.NET ! ».

[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

Après avoir apporté cette modification à la page maître, réexaminez la page de `~/Admin/Default.aspx`. Cette fois, le `PoweredByASPNET.gif` fichier image s’affiche dans la page (voir figure 3). Lorsque le contrôle Web de l’image est affiché, il utilise la méthode `ResolveClientUrl` pour résoudre sa valeur de propriété `ImageUrl`. Dans `~/Admin/Default.aspx` la `ImageUrl` est convertie en URL relative appropriée, comme le montre l’extrait de code suivant de la source HTML :

[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> En plus d’être utilisés dans les propriétés de contrôle Web basée sur une URL, le `~` peut également être utilisé lors de l’appel des méthodes `Response.Redirect` et `Server.MapPath`, entre autres. En outre, la méthode `ResolveClientUrl` peut être appelée directement à partir d’un ASP.NET ou d’un balisage déclaratif d’une page maître, si nécessaire ; consultez l’entrée de blog de [Fritz oignon](https://www.pluralsight.com/blogs/fritz/) [en utilisant `ResolveClientUrl` dans le balisage](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).

## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Correction des URL relatives restantes de la page maître

En plus de l’élément `<img>` dans le `footerContent` que nous venons de corriger, la page maître contient une URL plus relative qui requiert votre attention. La région `topContent` comprend le lien « didacticiels sur les pages maîtres », qui pointe vers `Default.aspx`.

[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

Étant donné que cette URL est relative, elle envoie l’utilisateur à la page `Default.aspx` dans le dossier de la page de contenu visitée. Pour que ce lien pointe toujours vers `Default.aspx` dans le dossier racine, nous devons remplacer l’élément `<a>` par un contrôle HyperLink Web afin de pouvoir utiliser la notation `~`.

Supprimez le balisage de l’élément `<a>` et ajoutez un contrôle de lien hypertexte à la place. Définissez la `ID` du lien hypertexte sur `lnkHome`, sa propriété `NavigateUrl` sur `~/Default.aspx`, et sa propriété `Text` sur « didacticiels sur les pages maîtres ».

[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

C’est tout ! À ce stade, toutes les URL de notre page maître sont correctement basées sur le rendu d’une page de contenu, quels que soient les dossiers dans lesquels se trouvent la page maître et la page de contenu.

### <a name="automatic-url-resolution-in-theheadsection"></a>Résolution automatique des URL dans la section`<head>`

Dans le didacticiel [*création d’une disposition à l’ensemble du site à l’aide de pages maîtres*](creating-a-site-wide-layout-using-master-pages-cs.md) , nous avons ajouté un `<link>` au fichier `Styles.css` dans la région `<head>` :

[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

Tandis que l’attribut `href` de l’élément `<link>` est relatif, il est automatiquement converti en chemin d’accès approprié au moment de l’exécution. Comme nous l’avons vu dans la section [*spécification du titre, des balises meta et d’autres en-têtes HTML du didacticiel page maître*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) , la `<head>` région est en fait un contrôle côté serveur, ce qui lui permet de modifier le contenu de ses contrôles internes lorsqu’il est rendu.

Pour vérifier cela, revisitez la page de `~/Admin/Default.aspx` et affichez la source HTML envoyée au navigateur. Comme le montre l’extrait de code ci-dessous, l’attribut `href` de l’élément `<link>` a été automatiquement modifié en URL relative appropriée, `../Styles.css`.

[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a>Récapitulatif

Les pages maîtres incluent très souvent des liens, des images et d’autres ressources externes qui doivent être spécifiées via une URL. Étant donné que la page maître et les pages de contenu peuvent ne pas exister dans le même dossier, il est important de s’abstenir d’utiliser des URL relatives. Bien qu’il soit possible d’utiliser des URL absolues codées en dur, cela permet de coupler étroitement l’URL absolue à l’application Web. Si l’URL absolue change, comme c’est souvent le cas lors du déplacement ou du déploiement d’une application Web, vous devez vous souvenir de revenir en arrière et de mettre à jour les URL absolues.

L’approche idéale consiste à utiliser le tilde (`~`) pour indiquer la racine de l’application. Les contrôles Web ASP.NET qui contiennent des propriétés liées à l’URL mappent les `~` à la racine de l’application au moment de l’exécution. En interne, les contrôles Web utilisent la méthode `ResolveClientUrl` de la classe `Control` pour générer une URL relative valide. Cette méthode est publique et disponible à partir de chaque contrôle serveur (y compris la classe `Page`). vous pouvez donc l’utiliser par programmation à partir de vos classes code-behind, si nécessaire.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Pages maîtres dans ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Relocalisation d’URL dans une page maître](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Utilisation de `ResolveClientUrl` dans le balisage](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs ouvrages ASP/ASP. net et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 3,5 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott peut être contacté au [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
> [Suivant](control-id-naming-in-content-pages-cs.md)
