---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Utilisation d’images dans un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre vous montre comment ajouter, afficher et manipuler des images (redimensionner, retourner et ajouter des filigranes) dans votre site Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 53514b3c314fc182a43c82974ffcfa8158a636a1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631858"
---
# <a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Utilisation d’images dans un site pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment ajouter, afficher et manipuler des images (redimensionner, retourner et ajouter des filigranes) sur un site Web pages Web ASP.NET (Razor).
> 
> Ce que vous allez apprendre :
> 
> - Comment ajouter dynamiquement une image à une page.
> - Comment permettre aux utilisateurs de télécharger une image.
> - Comment redimensionner une image.
> - Comment retourner ou faire pivoter une image.
> - Comment ajouter un filigrane à une image.
> - Comment utiliser une image en tant que filigrane.
> 
> Voici les fonctionnalités de programmation ASP.NET présentées dans l’article :
> 
> - Le programme d’assistance `WebImage`.
> - Objet `Path`, qui fournit des méthodes qui vous permettent de manipuler les noms de chemin d’accès et de fichier.
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

<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Ajout dynamique d’une image à une page Web

Vous pouvez ajouter des images à votre site Web et à des pages individuelles pendant que vous développez le site Web. Vous pouvez également permettre aux utilisateurs de charger des images, ce qui peut être utile pour des tâches telles que l’ajout d’une photo de profil.

Si une image est déjà disponible sur votre site et que vous souhaitez simplement l’afficher sur une page, vous utilisez un élément de `<img>` HTML comme suit :

[!code-html[Main](9-working-with-images/samples/sample1.html)]

Parfois, cependant, vous devez être en mesure d’afficher des images de &#8212; manière dynamique, mais vous ne savez pas quelle image afficher tant que la page n’est pas en cours d’exécution.

La procédure décrite dans cette section montre comment afficher une image à la volée dans laquelle les utilisateurs spécifient le nom du fichier image à partir d’une liste de noms d’images. Ils sélectionnent le nom de l’image dans une liste déroulante, et lorsqu’ils envoient la page, l’image qu’ils ont sélectionnée s’affiche.

![image](9-working-with-images/_static/image1.jpg "ch9images-1. jpg")

1. Dans WebMatrix, créez un nouveau site Web.
2. Ajoutez une nouvelle page nommée *DynamicImage. cshtml*.
3. Dans le dossier racine du site Web, ajoutez un nouveau dossier et nommez-le *images*.
4. Ajoutez quatre images au dossier *images* que vous venez de créer. (Toutes les images que vous avez pratiques peuvent faire, mais elles doivent tenir sur une page.) Renommez les images *PHOTO1. jpg*, *PHOTO2. jpg*, *Photo3. jpg*et *Photo4. jpg*. (Vous n’allez pas utiliser *Photo4. jpg* dans cette procédure, mais vous l’utiliserez plus loin dans cet article.)
5. Vérifiez que les quatre images ne sont pas marquées en lecture seule.
6. Remplacez le contenu existant de la page par ce qui suit :

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Le corps de la page contient une liste déroulante (élément `<select>`) nommée `photoChoice`. La liste comporte trois options, et l’attribut `value` de chaque option de liste a le nom de l’une des images que vous placez dans le dossier *images* . En fait, la liste permet à l’utilisateur de sélectionner un nom convivial tel que &quot;photo 1&quot;, et il transmet ensuite le nom du fichier *. jpg* lorsque la page est envoyée.

    Dans le code, vous pouvez récupérer la sélection de l’utilisateur (en d’autres termes, le nom du fichier image) à partir de la liste en lisant `Request["photoChoice"]`. Vous voyez tout d’abord s’il existe une sélection. Si c’est le cas, vous construisez un chemin d’accès pour l’image qui se compose du nom du dossier pour les images et du nom du fichier image de l’utilisateur. (Si vous avez essayé de créer un chemin d’accès mais qu’il n’y a rien dans `Request["photoChoice"]`, vous obtenez une erreur.) Cela aboutit à un chemin d’accès relatif comme celui-ci :

    *images/PHOTO1. jpg*

    Le chemin d’accès est stocké dans une variable nommée `imagePath` dont vous aurez besoin plus tard dans la page.

    Dans le corps, il y a également un élément `<img>` qui est utilisé pour afficher l’image choisie par l’utilisateur. L’attribut `src` n’est pas défini sur un nom de fichier ou une URL, comme vous le feriez pour afficher un élément statique. Au lieu de cela, il est défini sur `@imagePath`, ce qui signifie qu’il obtient sa valeur à partir du chemin d’accès que vous avez défini dans le code.

    La première fois que la page s’exécute, cependant, il n’y a aucune image à afficher, car l’utilisateur n’a rien sélectionné. Cela signifie généralement que l’attribut `src` est vide et que l’image s’affiche comme une&quot; rouge &quot;x (ou tout ce que le navigateur restitue lorsqu’il ne peut pas trouver d’image). Pour éviter cela, placez l’élément `<img>` dans un bloc `if` qui teste si la variable `imagePath` contient des éléments. Si l’utilisateur a effectué une sélection, `imagePath` contient le chemin d’accès. Si l’utilisateur n’a pas sélectionné d’image ou s’il s’agit de la première fois que la page est affichée, l’élément `<img>` n’est même pas restitué.
7. Enregistrez le fichier et exécutez la page dans un navigateur. (Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.)
8. Sélectionnez une image dans la liste déroulante, puis cliquez sur **exemple d’image**. Assurez-vous que vous voyez différentes images pour différents choix.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Chargement d’une image

L’exemple précédent vous a montré comment afficher une image de manière dynamique, mais elle fonctionnait uniquement avec des images qui figuraient déjà sur votre site Web. Cette procédure montre comment permettre aux utilisateurs de charger une image, qui est ensuite affichée sur la page. Dans ASP.NET, vous pouvez manipuler les images à la volée à l’aide de l’application auxiliaire `WebImage`, qui contient des méthodes qui vous permettent de créer, manipuler et enregistrer des images. Le `WebImage` Helper prend en charge tous les types de fichiers d’image Web courants, y compris *. jpg*, *. png*et *. bmp*. Tout au long de cet article, vous allez utiliser des images *. jpg* , mais vous pouvez utiliser n’importe quel type d’image.

![image](9-working-with-images/_static/image2.jpg "ch9images-2. jpg")

1. Ajoutez une nouvelle page et nommez-la *UploadImage. cshtml*.
2. Remplacez le contenu existant de la page par ce qui suit : 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Le corps du texte a un élément `<input type="file">`, qui permet aux utilisateurs de sélectionner un fichier à télécharger. Lorsqu’ils cliquent sur **Envoyer**, le fichier qu’ils ont choisi est soumis en même temps que le formulaire.

    Pour obtenir l’image chargée, vous utilisez le programme d’assistance `WebImage`, qui présente toutes sortes de méthodes utiles pour travailler avec des images. Plus précisément, vous utilisez `WebImage.GetImageFromRequest` pour obtenir l’image chargée (le cas échéant) et la stocker dans une variable nommée `photo`.

    Dans cet exemple, une grande partie du travail implique l’obtention et la définition des noms de fichier et de chemin d’accès. Le problème est que vous souhaitez obtenir le nom (et simplement le nom) de l’image que l’utilisateur a téléchargée, puis créer un nouveau chemin d’accès pour l’emplacement où vous allez stocker l’image. Étant donné que les utilisateurs peuvent télécharger plusieurs images portant le même nom, vous utilisez un peu de code supplémentaire pour créer des noms uniques et vous assurer que les utilisateurs ne remplacent pas les images existantes.

    Si une image a réellement été chargée (le test `if (photo != null)`), vous récupérez le nom de l’image à partir de la propriété de `FileName` de l’image. Lorsque l’utilisateur charge l’image, `FileName` contient le nom d’origine de l’utilisateur, qui inclut le chemin d’accès à partir de l’ordinateur de l’utilisateur. Elle peut se présenter comme suit :

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Vous ne souhaitez pas toutes ces informations de chemin &#8212; d’accès, bien que vous souhaitiez simplement le nom de fichier réel (*SamplePhoto1. jpg*). Vous pouvez supprimer uniquement le fichier d’un chemin d’accès à l’aide de la méthode `Path.GetFileName`, comme suit :

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Vous créez ensuite un nom de fichier unique en ajoutant un GUID au nom d’origine. (Pour plus d’informations sur les GUID, consultez [à propos des GUID](#SB_AboutGUIDs) plus loin dans cet article.) Ensuite, vous créez un chemin d’accès complet que vous pouvez utiliser pour enregistrer l’image. Le chemin d’enregistrement est constitué du nouveau nom de fichier, du dossier (images) et de l’emplacement du site Web actuel.

    > [!NOTE]
    > Pour que votre code enregistre des fichiers dans le dossier *images* , l’application a besoin d’autorisations en lecture-écriture pour ce dossier. Sur votre ordinateur de développement, ce n’est généralement pas un problème. Toutefois, lorsque vous publiez votre site sur le serveur Web d’un fournisseur d’hébergement, vous devrez peut-être définir explicitement ces autorisations. Si vous exécutez ce code sur le serveur d’un fournisseur d’hébergement et que vous obtenez des erreurs, contactez le fournisseur d’hébergement pour savoir comment définir ces autorisations.

    Enfin, vous transmettez le chemin d’enregistrement à la méthode `Save` de l’application auxiliaire `WebImage`. Cela stocke l’image chargée sous son nouveau nom. La méthode Save ressemble à ceci : `photo.Save(@"~\" + imagePath)`. Le chemin d’accès complet est ajouté à `@"~\"`, qui correspond à l’emplacement du site Web actuel. (Pour plus d’informations sur l’opérateur `~`, consultez [Introduction à la programmation Web ASP.net à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Comme dans l’exemple précédent, le corps de la page contient un élément `<img>` pour afficher l’image. Si `imagePath` a été défini, l’élément `<img>` est restitué et son attribut `src` est défini sur la valeur `imagePath`.
3. Exécutez la page dans un navigateur.
4. Téléchargez une image et assurez-vous qu’elle est affichée dans la page.
5. Dans votre site, ouvrez le dossier *images* . Vous voyez qu’un nouveau fichier a été ajouté, dont le nom de fichier ressemble à ceci :: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto. png*

    Il s’agit de l’image que vous avez téléchargée avec un GUID préfixé au nom. (Votre propre fichier aura un GUID différent et est probablement nommé autre chose que *myphoto. png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>À propos des GUID
> 
> Un GUID (identificateur global unique) est un identificateur qui est généralement rendu dans un format comme suit : `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Les chiffres et les lettres (à partir de A-F) diffèrent pour chaque GUID, mais ils suivent tous le modèle d’utilisation de groupes de 8-4-4-4-12 caractères. (Techniquement, un GUID est un nombre de 16 octets/128 bits.) Lorsque vous avez besoin d’un GUID, vous pouvez appeler du code spécialisé qui génère un GUID pour vous. L’idée derrière les GUID est qu’entre l’énorme taille du nombre (3,4 x 10<sup>38</sup>) et l’algorithme pour sa génération, le nombre résultant est pratiquement garanti être l’un de ces types. Les GUID sont donc un bon moyen de générer des noms pour les choses quand vous devez vous assurer que vous n’utiliserez pas deux fois le même nom. L’inconvénient, bien sûr, est que les GUID ne sont pas particulièrement conviviaux. ils ont donc tendance à être utilisés lorsque le nom est utilisé uniquement dans le code.

<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Redimensionnement d'une image

Si votre site Web accepte les images d’un utilisateur, vous souhaiterez peut-être redimensionner les images avant de les afficher ou de les enregistrer. Vous pouvez à nouveau utiliser le programme d’assistance `WebImage` pour ce.

Cette procédure montre comment redimensionner une image chargée pour créer une miniature, puis enregistrer la miniature et l’image d’origine dans le site Web. Vous affichez la miniature sur la page et utilisez un lien hypertexte pour rediriger les utilisateurs vers l’image en taille réelle.

![image](9-working-with-images/_static/image3.jpg "ch9images-3. jpg")

1. Ajoutez une nouvelle page nommée *thumbnail. cshtml*.
2. Dans le dossier *images* , créez un sous-dossier nommé *thumbs*.
3. Remplacez le contenu existant de la page par ce qui suit : 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Ce code est similaire au code de l’exemple précédent. La différence est que ce code enregistre l’image deux fois, une fois normalement et une fois que vous avez créé une copie miniature de l’image. Tout d’abord, vous récupérez l’image téléchargée et vous l’enregistrez dans le dossier *images* . Vous construisez ensuite un nouveau chemin d’accès pour l’image miniature. Pour créer réellement la miniature, vous appelez la méthode `Resize` de l’application d’assistance `WebImage` pour créer une image de 60 pixels par 60 pixels. L’exemple montre comment conserver les proportions et comment empêcher l’agrandissement de l’image (si la nouvelle taille rend l’image plus grande). L’image redimensionnée est ensuite enregistrée dans le sous-dossier *thumbs* .

    À la fin du balisage, vous utilisez le même `<img>` élément avec l’attribut `src` dynamique que vous avez vu dans les exemples précédents pour afficher de manière conditionnelle l’image. Dans ce cas, vous affichez la miniature. Vous utilisez également un élément `<a>` pour créer un lien hypertexte vers la grande version de l’image. Comme avec l’attribut `src` de l’élément `<img>`, vous définissez l’attribut `href` de l’élément `<a>` de manière dynamique sur le contenu de `imagePath`. Pour vous assurer que le chemin d’accès peut fonctionner en tant qu’URL, vous passez `imagePath` à la méthode `Html.AttributeEncode`, qui convertit les caractères réservés dans le chemin d’accès en caractères OK dans une URL.
4. Exécutez la page dans un navigateur.
5. Téléchargez une photo et vérifiez que la miniature est affichée.
6. Cliquez sur la miniature pour afficher l’image en taille réelle.
7. Dans les *images* et les *images/thumbs*, Notez que de nouveaux fichiers ont été ajoutés.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Rotation et retournement d’une image

Le `WebImage` Helper vous permet également de retourner et de faire pivoter des images. Cette procédure montre comment obtenir une image à partir du serveur, retourner l’image à l’envers (verticalement), l’enregistrer, puis afficher l’image retournée sur la page. Dans cet exemple, vous utilisez simplement un fichier déjà présent sur le serveur (*PHOTO2. jpg*). Dans une application réelle, vous pouvez probablement retourner une image dont vous obtenez le nom dynamique, comme vous l’avez fait dans les exemples précédents.

![image](9-working-with-images/_static/image4.jpg "ch9images-4. jpg")

1. Ajoutez une nouvelle page nommée *FlipImage. cshtml*.
2. Remplacez le contenu existant de la page par ce qui suit : 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    Le code utilise le programme d’assistance `WebImage` pour obtenir une image à partir du serveur. Vous créez le chemin d’accès à l’image à l’aide de la même technique que celle utilisée dans les exemples précédents pour enregistrer des images, et vous transmettez ce chemin lorsque vous créez une image à l’aide de `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Si une image est trouvée, vous construisez un nouveau chemin d’accès et un nouveau nom de fichier, comme vous l’avez fait dans des exemples précédents. Pour retourner l’image, vous appelez la méthode `FlipVertical`, puis vous enregistrez à nouveau l’image.

    L’image est de nouveau affichée sur la page à l’aide de l’élément `<img>` avec l’attribut `src` défini sur `imagePath`.
3. Exécutez la page dans un navigateur. L’image de *PHOTO2. jpg* est affichée à l’envers.
4. Actualisez la page ou demandez à nouveau la page pour voir que l’image est retournée vers le haut.

Pour faire pivoter une image, vous utilisez le même code, mais au lieu d’appeler le `FlipVertical` ou `FlipHorizontal`, vous appelez `RotateLeft` ou `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Ajout d’un filigrane à une image

Lorsque vous ajoutez des images à votre site Web, vous pouvez ajouter un filigrane à l’image avant de l’enregistrer ou de l’afficher sur une page. Les gens utilisent souvent des filigranes pour ajouter des informations de Copyright à une image ou pour publier leur nom professionnel.

![image](9-working-with-images/_static/image5.jpg "ch9images-5. jpg")

1. Ajoutez une nouvelle page nommée *Watermark. cshtml*.
2. Remplacez le contenu existant de la page par ce qui suit : 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Ce code est similaire au code de la page *FlipImage. cshtml* de la version antérieure (même si cette fois-ci, il utilise le fichier *Photo3. jpg* ). Pour ajouter le filigrane, vous devez appeler la méthode `AddTextWatermark` de l' `WebImage` Helper avant d’enregistrer l’image. Dans l’appel à `AddTextWatermark`, vous transmettez le texte &quot;mon filigrane&quot;, définissez la couleur de la police sur jaune et définissez la famille de polices sur Arial. (Bien que cela ne soit pas illustré ici, le `WebImage` Helper vous permet également de spécifier l’opacité, la famille de polices et la taille de police, ainsi que la position du texte de filigrane.) Lorsque vous enregistrez l’image, elle ne doit pas être en lecture seule.

    Comme vous l’avez vu précédemment, l’image est affichée sur la page à l’aide de l’élément `<img>` avec l’attribut SRC défini sur `@imagePath`.
3. Exécutez la page dans un navigateur. Notez le texte « mon filigrane » dans le coin inférieur droit de l’image.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Utilisation d’une image en tant que filigrane

Au lieu d’utiliser du texte pour un filigrane, vous pouvez utiliser une autre image. Les gens utilisent parfois des images comme un logo de société comme filigrane, ou ils utilisent une image en filigrane au lieu de texte pour les informations de copyright.

![image](9-working-with-images/_static/image6.jpg "ch9images-6. jpg")

1. Ajoutez une nouvelle page nommée *ImageWatermark. cshtml*.
2. Ajoutez une image au dossier *images* que vous pouvez utiliser comme logo, puis renommez l’image *MyCompanyLogo. jpg*. Cette image doit être une image que vous pouvez voir clairement lorsqu’elle est définie sur 80 pixels de large et 20 pixels de haut.
3. Remplacez le contenu existant de la page par ce qui suit : 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Il s’agit d’une autre variante du code des exemples précédents. Dans ce cas, vous appelez `AddImageWatermark` pour ajouter l’image en filigrane à l’image cible (*Photo3. jpg*) avant d’enregistrer l’image. Lorsque vous appelez `AddImageWatermark`, vous définissez sa largeur sur 80 pixels et la hauteur sur 20 pixels. L’image *MyCompanyLogo. jpg* est alignée horizontalement au centre et alignée verticalement en bas de l’image cible. L’opacité est définie sur 100% et la marge intérieure est définie sur 10 pixels. Si l’image en filigrane est plus grande que l’image cible, rien ne se produit. Si l’image en filigrane est plus grande que l’image cible et que vous définissez le remplissage de l’image en filigrane sur zéro, le filigrane est ignoré.

    Comme précédemment, vous affichez l’image à l’aide de l’élément `<img>` et d’un attribut `src` dynamique.
4. Exécutez la page dans un navigateur. Notez que l’image de filigrane s’affiche au bas de l’image principale.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[Utilisation de fichiers dans un site pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202896)

[Introduction à la programmation de pages Web ASP.NET à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=251587)
