---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Affichage de vidéos dans un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre explique comment afficher une vidéo dans un pages Web ASP.NET avec syntaxe Razor page.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 516d46f38ce8910209f4207c474b0404bf012950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628946"
---
# <a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Affichage de vidéos dans un site pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment utiliser un lecteur vidéo (multimédia) dans un site Web pages Web ASP.NET (Razor) pour permettre aux utilisateurs d’afficher les vidéos stockées sur le site. Pages Web ASP.NET avec syntaxe Razor vous permet de lire des vidéos flash ( *. swf*), Media Player ( *. wmv*) et Silverlight ( *. xap*).
> 
> Ce que vous allez apprendre :
> 
> - Comment choisir un lecteur vidéo.
> - Comment ajouter une vidéo à une page Web.
> - Comment définir les attributs d’un lecteur vidéo.
> 
> Voici les fonctionnalités des pages Razor ASP.NET présentées dans l’article :
> 
> - Le programme d’assistance `Video`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> Ce didacticiel fonctionne également avec WebMatrix 3.

## <a name="introduction"></a>Introduction

Vous souhaiterez peut-être afficher une vidéo sur votre site. Pour ce faire, vous pouvez créer un lien vers un site qui a déjà la vidéo, par exemple YouTube. Si vous souhaitez incorporer une vidéo à partir de ces sites directement dans vos propres pages, vous pouvez généralement obtenir le balisage HTML à partir du site, puis le copier dans votre page. Par exemple, l’exemple suivant montre comment incorporer une vidéo YouTube :

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Si vous souhaitez lire une vidéo qui se trouve sur votre propre site Web (et non sur un site de partage vidéo public), vous ne pouvez pas le lier directement à l’aide d’un balisage incorporé comme celui-ci. Toutefois, vous pouvez lire des vidéos à partir de votre site à l’aide du programme d’assistance `Video`, qui restitue un lecteur multimédia directement dans une page.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Choix d’un lecteur vidéo

Il existe un grand nombre de formats pour les fichiers vidéo, et chaque format requiert généralement un joueur différent et une autre façon de configurer le lecteur. Dans les pages Razor ASP.NET, vous pouvez lire une vidéo dans une page Web à l’aide du programme d’assistance `Video`. Le `Video` Helper simplifie le processus d’incorporation de vidéos dans une page Web, car il génère automatiquement les éléments HTML `object` et `embed` qui sont normalement utilisés pour ajouter de la vidéo à la page.

Le programme d’assistance `Video` prend en charge les lecteurs multimédias suivants :

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>Le lecteur flash

Le lecteur `Flash` du programme d’assistance `Video` vous permet de lire des vidéos flash (fichiers *. swf* ) dans une page Web. Au minimum, vous devez fournir un chemin d’accès au fichier vidéo. Si vous ne spécifiez rien, à l’exception du chemin d’accès, le lecteur utilise les valeurs par défaut définies par la version actuelle de Flash. Les paramètres par défaut sont les suivants :

- La vidéo est affichée à l’aide de sa largeur et de sa hauteur par défaut et sans couleur d’arrière-plan.
- La vidéo est lue automatiquement lors du chargement de la page.
- La vidéo s’exécute en continu jusqu’à ce qu’elle soit explicitement arrêtée.
- La vidéo est mise à l’échelle pour afficher toute la vidéo, au lieu de la rogner pour l’adapter à une taille spécifique.
- La vidéo est lue dans une fenêtre.

### <a name="the-mediaplayer-player"></a>Le lecteur MediaPlayer

Le lecteur `MediaPlayer` du programme d’assistance `Video` vous permet de lire des vidéos Windows Media (fichiers *. wmv* ), des fichiers audio Windows Media (fichiers *. WMA* ) et des fichiers MP3 ( *. mp3* ) dans une page Web. Vous devez inclure le chemin d’accès du fichier multimédia à lire ; tous les autres paramètres sont facultatifs. Si vous spécifiez uniquement un chemin d’accès, le lecteur utilise les paramètres par défaut définis par la version actuelle de MediaPlayer, par exemple :

- La vidéo est affichée en utilisant sa largeur et sa hauteur par défaut.
- La vidéo est lue automatiquement lors du chargement de la page.
- La vidéo est lue une seule fois (elle n’est pas en boucle).
- Le lecteur affiche le jeu complet de contrôles dans l’interface utilisateur.
- La vidéo est lue dans une fenêtre.

### <a name="the-silverlight-player"></a>Le lecteur Silverlight

Le lecteur `Silverlight` du programme d’assistance `Video` vous permet de lire des Windows Media Video (fichiers *. wmv* ), des Windows Media audio (fichiers *. WMA* ) et des fichiers MP3 ( *. mp3* ). Vous devez définir le paramètre path pour qu’il pointe vers un package d’application Silverlight (fichier *. xap* ). Vous devez également définir les paramètres de largeur et de hauteur. Tous les autres paramètres sont facultatifs. Quand vous utilisez le lecteur Silverlight pour la vidéo, si vous définissez uniquement les paramètres requis, le lecteur Silverlight affiche la vidéo sans couleur d’arrière-plan.

> [!NOTE]
> Si vous ne connaissez pas encore Silverlight : le fichier *. xap* est un fichier compressé qui contient des instructions de mise en page dans un fichier *. Xaml* , du code managé dans les assemblys et des ressources facultatives. Vous pouvez créer un fichier *. xap* dans Visual Studio en tant que projet d’application Silverlight.

Le lecteur vidéo `Silverlight` utilise à la fois les paramètres que vous fournissez pour le lecteur et les paramètres fournis dans le fichier *. xap* .

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Types MIME
> 
> Quand un navigateur télécharge un fichier, le navigateur s’assure que le type de fichier correspond au type MIME spécifié pour le document en cours de rendu. Le type MIME est le type de contenu ou le type de média d’un fichier. Le programme d’assistance `Video` utilise les types MIME suivants :
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`

<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Vidéos flash (. swf)

Cette procédure vous montre comment lire une vidéo Flash nommée *Sample. swf*. La procédure suppose que vous disposez d’un dossier nommé *Media* sur votre site et que le fichier *. swf* se trouve dans ce dossier.

1. Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas encore fait.
2. Dans le site Web, ajoutez une page et nommez-la *FlashVideo. cshtml*.
3. Ajoutez le balisage suivant à la page : 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Exécutez la page dans un navigateur. (Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.) La page s’affiche et la vidéo est lue automatiquement. 

    ![image](10-working-with-video/_static/image1.jpg "ch08_video -1. jpg")

Vous pouvez définir le paramètre `quality` pour une vidéo Flash sur `low`, `autolow`, `autohigh`, `medium`, `high`et `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Vous pouvez modifier la vidéo Flash pour qu’elle s’exécute à une taille spécifique à l’aide du paramètre `scale`, que vous pouvez définir comme suit :

- `showall`. Cela rend l’intégralité de la vidéo visible tout en conservant les proportions d’origine. Toutefois, vous pouvez vous retrouver avec des bordures de chaque côté.
- `noorder`. Cela met à l’échelle la vidéo tout en conservant les proportions d’origine, mais elle peut être rognée.
- `exactfit`. Cela rend l’intégralité de la vidéo visible sans conserver les proportions d’origine, mais la distorsion peut se produire.

Si vous ne spécifiez pas de paramètre `scale`, la vidéo entière est visible et les proportions d’origine sont conservées sans rognage. L’exemple suivant montre comment utiliser le paramètre `scale` :

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Le lecteur Flash Player prend en charge un paramètre de mode vidéo nommé `windowMode`. Vous pouvez définir cette valeur sur `window`, `opaque`et `transparent`. Par défaut, le `windowMode` est défini sur `window`, qui affiche la vidéo dans une fenêtre distincte sur la page Web. Le paramètre `opaque` masque tout ce qui se trouve derrière la vidéo sur la page Web. Le paramètre `transparent` permet à l’arrière-plan de la page Web de s’afficher dans la vidéo, en supposant qu’une partie de la vidéo est transparente.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Vidéos MediaPlayer ( *. wmv*)

La procédure suivante montre comment lire une vidéo multimédia Windows nommée *Sample. wmv* qui se trouve dans le dossier *Media* .

1. Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas déjà fait.
2. Créez une nouvelle page nommée *MediaPlayerVideo. cshtml*.
3. Ajoutez le balisage suivant à la page : 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Exécutez la page dans un navigateur. La vidéo se charge et se lance automatiquement. 

    ![image](10-working-with-video/_static/image2.jpg "ch08_video-2. jpg")

Vous pouvez définir `playCount` sur un entier qui indique le nombre de fois que la vidéo est automatiquement lue :

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

Le paramètre `uiMode` vous permet de spécifier les contrôles qui apparaissent dans l’interface utilisateur. Vous pouvez définir `uiMode` sur `invisible`, `none`, `mini`ou `full`. Si vous ne spécifiez pas de paramètre `uiMode`, la vidéo sera affichée à l’aide de la fenêtre d’État, de la barre de recherche, des boutons de contrôle et des contrôles de volume, en plus de la fenêtre vidéo. Ces contrôles s’affichent également si vous utilisez le lecteur pour lire un fichier audio. Voici un exemple d’utilisation du paramètre `uiMode` :

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Par défaut, l’audio est activé lors de la lecture de la vidéo. Vous pouvez désactiver l’audio en affectant au paramètre `mute` la valeur true :

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Vous pouvez contrôler le niveau audio de la vidéo de MediaPlayer en définissant le paramètre `volume` sur une valeur comprise entre 0 et 100. La valeur par défaut est 50. Voici un exemple :

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Vidéos Silverlight

Cette procédure vous montre comment lire la vidéo contenue dans une page Silverlight *. xap* qui se trouve dans un dossier nommé *Media*.

1. Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas déjà fait.
2. Créez une nouvelle page nommée *SilverlightVideo. cshtml*.
3. Ajoutez le balisage suivant à la page : 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Exécutez la page dans un navigateur. 

    ![image](10-working-with-video/_static/image3.jpg "ch08_video-3. jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[Présentation de Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Attributs d’objet Flash et de balise EMBED](http://kb2.adobe.com/cps/127/tn_12701.html)

[Balises PARAM du SDK Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
