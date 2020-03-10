---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Affichage des cartes dans un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cet article explique comment afficher des cartes interactives sur les pages d’un site Web pages Web ASP.NET (Razor) en fonction des services de mappage fournis par Bing, Google, ma...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 36f3b753cf312504892872ff54bef49854588990
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638676"
---
# <a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Affichage des cartes dans un site pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment afficher des cartes interactives sur les pages d’un site Web pages Web ASP.NET (Razor) en fonction des services de mappage fournis par Bing, Google, MapQuest et Yahoo.
> 
> Ce que vous allez apprendre :
> 
> - Comment générer un mappage basé sur une adresse.
> - Comment générer une carte en fonction des coordonnées de latitude et de longitude.
> - Comment inscrire un compte de développeur Bing Maps et obtenir une clé à utiliser avec Bing Maps.
> 
> Il s’agit de la fonctionnalité ASP.NET introduite dans l’article :
> 
> - Le programme d’assistance `Maps`.
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

Dans les pages Web, vous pouvez afficher les cartes sur une page à l’aide d' `Maps` Helper. Vous pouvez générer des mappages basés sur une adresse ou sur un ensemble de coordonnées de longitude et de latitude. La classe `Maps` vous permet d’appeler des moteurs de cartes populaires, notamment Bing, Google, MapQuest et Yahoo.

Les étapes d’ajout d’un mappage à une page sont les mêmes, quel que soit le moteur de carte que vous appelez. Il vous suffit d’ajouter une référence de fichier JavaScript qui met à disposition les méthodes permettant d’afficher le mappage, puis d’appeler les méthodes du programme d’assistance `Maps`.

Vous choisissez un service de mappage basé sur les `Maps` méthode d’assistance que vous utilisez. Vous pouvez utiliser l’une des opérations suivantes :

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Installation des composants dont vous avez besoin

Pour afficher les mappages, vous avez besoin des éléments suivants :

- Le programme d’assistance `Maps`. Cette application auxiliaire est dans la version 2 de la bibliothèque d’applications auxiliaires Web ASP.NET. Si vous n’avez pas encore ajouté la bibliothèque, vous pouvez l’installer dans votre site en tant que package NuGet. Pour plus d’informations, consultez [installation des applications d’assistance dans un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372). (Dans la Galerie, recherchez le package `microsoft-web-helpers`.)
- Bibliothèque jQuery. Plusieurs modèles de site WebMatrix incluent déjà des bibliothèques jQuery dans leurs dossiers de *script* . Si vous ne disposez pas de ces bibliothèques, vous pouvez télécharger la dernière bibliothèque jQuery directement à partir du site [jQuery.org](http://jQuery.org) . Ou vous pouvez créer un nouveau site à l’aide d’un modèle (par exemple, le modèle **Starter Site** ), puis copier les fichiers jQuery à partir de ce site vers votre site actuel.

Enfin, si vous souhaitez utiliser Bing Maps, vous devez d’abord créer un compte (gratuit) et obtenir une clé. Pour obtenir une clé, procédez comme suit :

1. Créez un compte sur le [compte de développeur Bing Maps](https://www.microsoft.com/maps/developers/web.aspx). Vous devez également disposer d’un compte Microsoft (Windows Live ID).

    Vous pouvez spécifier que vous souhaitez utiliser la clé à des fins d' **évaluation et de test**. Si vous testez la fonction de mappage sur votre propre ordinateur à l’aide de WebMatrix et IIS Express, accédez à l’espace de travail du **site** et notez l’URL de votre site (par exemple, `http://localhost:50408`, bien que votre numéro de port soit probablement différent). Vous pouvez utiliser cette adresse *localhost* comme site lorsque vous vous inscrivez.
2. Une fois que vous êtes inscrit à un compte, accédez au centre des comptes Bing Maps, puis cliquez sur **créer ou afficher les clés**:

    ![mappage-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Enregistrez la clé créée par Bing.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Création d’une carte basée sur une adresse (à l’aide de Google)

L’exemple suivant montre comment créer une page qui restitue un mappage basé sur une adresse. Cet exemple montre comment utiliser Google Maps.

1. Créez un fichier nommé *mapaddress. cshtml* à la racine du site. Cette page génère un mappage basé sur une adresse que vous lui transmettez.
2. Copiez le code suivant dans le fichier, en remplaçant le contenu existant.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Notez les fonctionnalités suivantes de la page :

    - Élément `<script>` dans l’élément `<head>`. Dans l’exemple, l’élément `<script>` fait référence au fichier *jQuery-1.6.4. min. js* , qui est une version minimisés (compressée) de la bibliothèque jQuery, version 1.6.4. Notez que la référence suppose que le fichier *. js* se trouve dans le dossier *scripts* de votre site. 

        > [!NOTE]
        > Si vous utilisez une autre version de la bibliothèque jQuery, assurez-vous simplement que vous pointez correctement sur cette version.
    - L’appel à la `@Maps.GetGoogleHtml` dans le corps de la page. Pour mapper une adresse, vous devez passer une chaîne d’adresse. Les méthodes pour les autres moteurs de mappage fonctionnent de la même façon (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Exécutez la page et entrez une adresse. La page affiche une carte, basée sur Google Maps, qui indique l’emplacement que vous avez spécifié.

     ![mappage-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Création d’une carte en fonction des coordonnées de latitude et de longitude (à l’aide de Bing)

Cet exemple montre comment créer un mappage basé sur des coordonnées. Cet exemple montre comment utiliser Bing Maps et comment inclure votre clé Bing. (Vous pouvez créer un mappage basé sur les coordonnées à l’aide des autres moteurs de mappage, sans utiliser de clé Bing.)

1. Créez un fichier nommé *MapCoordinates. cshtml* à la racine du site et remplacez le contenu existant par le code et le balisage suivants :

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Remplacez `your-key-here` par la clé Bing Maps que vous avez générée précédemment.
3. Exécutez la page *MapCoordinates. cshtml* , entrez les coordonnées de latitude et de longitude, puis cliquez sur la **carte** . disproportionnée. (Si vous ne connaissez aucune coordonnée, essayez ce qui suit. Il s’agit d’un emplacement sur le campus Microsoft Redmond.)

   - Latitude : 47.6781005859375
   - Longitude :-122.158317565918

     La page est affichée à l’aide des coordonnées que vous avez spécifiées.

     ![mappage-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[Informations de référence sur l’API Microsoft. Maps](https://msdn.microsoft.com/library/gg427611.aspx)
