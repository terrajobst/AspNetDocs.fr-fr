---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Création d’une disposition cohérente dans les sites pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Pour améliorer l’efficacité de la création de pages Web pour votre site, vous pouvez créer des blocs de contenu réutilisables (tels que des en-têtes et des pieds de page) pour votre site Web et c...
ms.author: riande
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 3f63ce68ae4c13970ac0df196167ace0b22b592c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627448"
---
# <a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Création d’une disposition cohérente dans les sites pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment vous pouvez utiliser les pages de disposition d’un site Web pages Web ASP.NET (Razor) pour créer des blocs de contenu réutilisables (tels que des en-têtes et des pieds de page) et créer une apparence cohérente pour toutes les pages du site.
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment créer des blocs de contenu réutilisables comme des en-têtes et des pieds de page.
> - Comment créer une apparence cohérente pour toutes les pages de votre site à l’aide d’une disposition.
> - Comment passer des données au moment de l’exécution à une page de disposition.
> 
> Voici les fonctionnalités ASP.NET présentées dans l’article :
> 
> - Blocs de contenu, qui sont des fichiers contenant du contenu au format HTML à insérer dans plusieurs pages.
> - Les pages de disposition, qui sont des pages qui contiennent du contenu au format HTML qui peuvent être partagées par des pages du site Web.
> - Les méthodes `RenderPage`, `RenderBody`et `RenderSection`, qui indiquent à ASP.NET où insérer les éléments de page.
> - Le dictionnaire `PageData` qui vous permet de partager des données entre des blocs de contenu et des pages de disposition.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2.

## <a name="about-layout-pages"></a>À propos des pages de disposition

De nombreux sites Web contiennent du contenu qui s’affiche sur chaque page, comme un en-tête et un pied de page, ou une zone qui indique aux utilisateurs qu’ils sont connectés. ASP.NET vous permet de créer un fichier distinct avec un bloc de contenu qui peut contenir du texte, du balisage et du code, tout comme une page Web standard. Vous pouvez ensuite insérer le bloc de contenu dans d’autres pages du site où vous souhaitez afficher les informations. De cette façon, vous n’êtes pas obligé de copier et coller le même contenu dans chaque page. La création d’un contenu courant comme celui-ci facilite également la mise à jour de votre site. Si vous avez besoin de modifier le contenu, vous pouvez simplement mettre à jour un seul fichier, et les modifications sont reflétées partout où le contenu a été inséré.

Le diagramme suivant illustre le fonctionnement des blocs de contenu. Quand un navigateur demande une page du serveur Web, ASP.NET insère les blocs de contenu au point où la méthode `RenderPage` est appelée dans la page principale. La page terminé (fusionnée) est ensuite envoyée au navigateur.

![Diagramme conceptuel montrant comment la méthode RenderPage insère une page référencée dans la page active.](3-creating-a-consistent-look/_static/image1.jpg)

Dans cette procédure, vous allez créer une page qui référence deux blocs de contenu (un en-tête et un pied de page) situés dans des fichiers distincts. Vous pouvez utiliser ces mêmes blocs de contenu dans n’importe quelle page de votre site. Une fois que vous avez terminé, vous obtenez une page semblable à celle-ci :

![Capture d’écran montrant une page dans le navigateur qui résulte de l’exécution d’une page qui comprend des appels à la méthode RenderPage.](3-creating-a-consistent-look/_static/image2.png)

1. Dans le dossier racine de votre site Web, créez un fichier nommé *index. cshtml*.
2. Remplacez le balisage existant par le code suivant :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. Dans le dossier racine, créez un dossier nommé *Shared*.

    > [!NOTE]
    > Il est courant de stocker les fichiers partagés entre les pages Web dans un dossier nommé *Shared*.
4. Dans le dossier *partagé* , créez un fichier nommé *\_Header. cshtml*.
5. Remplacez tout contenu existant par ce qui suit :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Notez que le nom de fichier est *\_Header. cshtml*, avec un trait de soulignement (\_) comme préfixe. ASP.NET n’envoie pas de page au navigateur si son nom commence par un trait de soulignement. Cela empêche les personnes de demander (par inadvertance ou autrement) ces pages directement. Il est judicieux d’utiliser un trait de soulignement pour nommer les pages qui contiennent des blocs de contenu, car vous ne souhaitez pas vraiment que les utilisateurs soient en &#8212; mesure de demander l’insertion de ces pages dans d’autres pages.
6. Dans le dossier *partagé* , créez un fichier nommé *\_footer. cshtml* et remplacez le contenu par ce qui suit :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. Dans la page *index. cshtml* , ajoutez deux appels à la méthode `RenderPage`, comme illustré ici :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Cela montre comment insérer un bloc de contenu dans une page Web. Vous appelez la méthode `RenderPage` et lui transmettez le nom du fichier dont vous souhaitez insérer le contenu à ce stade. Ici, vous insérez le contenu des fichiers *\_Header. cshtml* et *\_footer. cshtml* dans le fichier *index. cshtml* .
8. Exécutez la page *index. cshtml* dans un navigateur. (Dans WebMatrix, dans l’espace de travail **fichiers** , cliquez avec le bouton droit sur le fichier, puis sélectionnez **lancer dans le navigateur**.)
9. Dans le navigateur, affichez la page source. (Par exemple, dans Internet Explorer, cliquez avec le bouton droit sur la page, puis cliquez sur **afficher la source**.)

    Cela vous permet de voir le balisage de la page Web qui est envoyé au navigateur, qui combine le balisage de la page d’index avec les blocs de contenu. L’exemple suivant illustre la source de la page rendue pour *index. cshtml*. Les appels à `RenderPage` que vous avez insérés dans *index. cshtml* ont été remplacés par le contenu réel des fichiers d’en-tête et de pied de page.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Création d’une apparence cohérente à l’aide des pages de disposition

Jusqu’à présent, vous savez qu’il est facile d’inclure le même contenu sur plusieurs pages. Une approche plus structurée de la création d’une apparence cohérente pour un site consiste à utiliser des pages de disposition. Une page de disposition définit la structure d’une page Web, mais ne contient pas de contenu réel. Une fois que vous avez créé une page de disposition, vous pouvez créer des pages Web qui contiennent le contenu, puis les lier à la page de disposition. Lorsque ces pages sont affichées, elles sont formatées en fonction de la page de disposition. (Dans ce sens, une page de disposition agit comme un type de modèle pour le contenu qui est défini dans d’autres pages.)

La page de disposition est similaire à une page HTML, à la différence qu’elle contient un appel à la méthode `RenderBody`. La position de la méthode `RenderBody` dans la page de disposition détermine l’emplacement où les informations de la page de contenu seront incluses.

Le diagramme suivant montre comment les pages de contenu et les pages de disposition sont combinées au moment de l’exécution pour produire la page Web terminée. Le navigateur demande une page de contenu. La page de contenu contient du code qui spécifie la page de disposition à utiliser pour la structure de la page. Dans la page disposition, le contenu est inséré au point où la méthode `RenderBody` est appelée. Les blocs de contenu peuvent également être insérés dans la page de disposition en appelant la méthode `RenderPage`, comme vous l’avez fait dans la section précédente. Une fois la page Web terminée, elle est envoyée au navigateur.

![Capture d’écran montrant une page dans le navigateur qui résulte de l’exécution d’une page qui comprend des appels à la méthode RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

La procédure suivante montre comment créer une page de disposition et lier des pages de contenu à celle-ci.

1. Dans le dossier *partagé* de votre site Web, créez un fichier nommé *\_layout1. cshtml*.
2. Remplacez tout contenu existant par ce qui suit :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Vous utilisez la méthode `RenderPage` dans une page de disposition pour insérer des blocs de contenu. Une page de disposition ne peut contenir qu’un seul appel à la méthode `RenderBody`.
3. Dans le dossier *partagé* , créez un fichier nommé *\_Header2. cshtml* et remplacez le contenu existant par ce qui suit :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. Dans le dossier racine, créez un nouveau dossier et nommez-le *styles*.
5. Dans le dossier *styles* , créez un fichier nommé *site. CSS* et ajoutez les définitions de style suivantes :

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Ces définitions de style sont ici uniquement pour montrer comment les feuilles de style peuvent être utilisées avec les pages de disposition. Si vous le souhaitez, vous pouvez définir vos propres styles pour ces éléments.
6. Dans le dossier racine, créez un fichier nommé *Content1. cshtml* et remplacez le contenu existant par ce qui suit :

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Il s’agit d’une page qui utilise une page de disposition. Le bloc de code en haut de la page indique la page de disposition à utiliser pour mettre en forme ce contenu.
7. Exécutez *Content1. cshtml* dans un navigateur. La page rendue utilise le format et la feuille de style définis dans *\_layout1. cshtml* et le texte (contenu) défini dans *Content1. cshtml*.

    ![[image]](3-creating-a-consistent-look/_static/image4.png)

    Vous pouvez répéter l’étape 6 pour créer des pages de contenu supplémentaires qui peuvent ensuite partager la même page de disposition.

    > [!NOTE]
    > Vous pouvez configurer votre site pour que vous puissiez utiliser automatiquement la même page de disposition pour toutes les pages de contenu d’un dossier. Pour plus d’informations, consultez [Personnalisation du comportement à l’ensemble du site pour pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Conception de pages de disposition comportant plusieurs sections de contenu

Une page de contenu peut avoir plusieurs sections, ce qui est utile si vous souhaitez utiliser des dispositions qui ont plusieurs zones avec du contenu remplaçable. Dans la page contenu, attribuez un nom unique à chaque section. (La section par défaut n’a pas de nom.) Dans la page disposition, vous ajoutez une méthode `RenderBody` pour spécifier l’emplacement où la section sans nom (par défaut) doit apparaître. Vous ajoutez ensuite des méthodes `RenderSection` distinctes afin de rendre les sections nommées individuellement.

Le diagramme suivant montre comment ASP.NET gère le contenu qui est divisé en plusieurs sections. Chaque section nommée est contenue dans un bloc de section de la page de contenu. (Ils sont nommés `Header` et `List` dans l’exemple.) Le Framework insère la section de contenu dans la page de disposition à l’endroit où la méthode `RenderSection` est appelée. La section sans nom (valeur par défaut) est insérée au point où la méthode `RenderBody` est appelée, comme vous l’avez vu précédemment.

![Diagramme conceptuel montrant comment la méthode RenderSection insère des sections de références dans la page actuelle.](3-creating-a-consistent-look/_static/image5.jpg)

Cette procédure indique comment créer une page de contenu qui contient plusieurs sections de contenu et comment la restituer à l’aide d’une page de disposition qui prend en charge plusieurs sections de contenu.

1. Dans le dossier *partagé* , créez un fichier nommé *\_Layout2. cshtml*.
2. Remplacez tout contenu existant par ce qui suit :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Vous utilisez la méthode `RenderSection` pour restituer les sections d’en-tête et de liste.
3. Dans le dossier racine, créez un fichier nommé *Content2. cshtml* et remplacez le contenu existant par ce qui suit :

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Cette page de contenu contient un bloc de code en haut de la page. Chaque section nommée est contenue dans un bloc de section. Le reste de la page contient la section de contenu (sans nom) par défaut.
4. Exécutez *Content2. cshtml* dans un navigateur.

    ![Capture d’écran montrant une page dans le navigateur qui résulte de l’exécution d’une page qui comprend des appels à la méthode RenderSection.](3-creating-a-consistent-look/_static/image6.png)

## <a name="making-content-sections-optional"></a>Rendre les sections de contenu facultatives

Normalement, les sections que vous créez dans une page de contenu doivent correspondre aux sections définies dans la page de disposition. Vous pouvez recevoir des erreurs si l’un des éléments suivants se produit :

- La page de contenu contient une section qui n’a pas de section correspondante dans la page de disposition.
- La page de disposition contient une section pour laquelle il n’existe aucun contenu.
- La page de disposition comprend des appels de méthode qui essaient d’afficher plusieurs fois la même section.

Toutefois, vous pouvez substituer ce comportement pour une section nommée en déclarant la section comme étant facultative dans la page de disposition. Cela vous permet de définir plusieurs pages de contenu qui peuvent partager une page de disposition, mais qui peuvent avoir ou non du contenu pour une section spécifique.

1. Ouvrez *Content2. cshtml* et supprimez la section suivante :

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Enregistrez la page, puis exécutez-la dans un navigateur. Un message d’erreur s’affiche, car la page de contenu ne fournit pas de contenu pour une section définie dans la page de disposition, à savoir la section d’en-tête.

    ![Capture d’écran qui montre l’erreur qui se produit si vous exécutez une page qui appelle la méthode RenderSection, mais que la section correspondante n’est pas fournie.](3-creating-a-consistent-look/_static/image7.png)
3. Dans le dossier *partagé* , ouvrez la page *\_Layout2. cshtml* et remplacez la ligne suivante :

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    par le code suivant :

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Vous pouvez également remplacer la ligne de code précédente par le bloc de code suivant, qui produit les mêmes résultats :

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Exécutez à nouveau la page *Content2. cshtml* dans un navigateur. (Si cette page est toujours ouverte dans le navigateur, il vous suffit de l’actualiser.) Cette fois, la page s’affiche sans erreur, même si elle n’a pas d’en-tête.

## <a name="passing-data-to-layout-pages"></a>Passage de données à des pages de disposition

Vous pouvez avoir des données définies dans la page de contenu à laquelle vous devez faire référence dans une page de disposition. Si c’est le cas, vous devez passer les données de la page de contenu à la page de disposition. Par exemple, vous souhaiterez peut-être afficher l’état de la connexion d’un utilisateur, ou vous pouvez afficher ou masquer les zones de contenu en fonction de l’entrée de l’utilisateur.

Pour passer des données d’une page de contenu à une page de disposition, vous pouvez placer des valeurs dans la propriété `PageData` de la page de contenu. La propriété `PageData` est une collection de paires nom/valeur qui contiennent les données que vous souhaitez passer entre les pages. Dans la page disposition, vous pouvez ensuite lire les valeurs en dehors de la propriété `PageData`.

Voici un autre diagramme. Celui-ci montre comment ASP.NET peut utiliser la propriété `PageData` pour passer des valeurs d’une page de contenu à la page de disposition. Quand ASP.NET commence à générer la page Web, il crée la collection `PageData`. Dans la page de contenu, vous écrivez du code pour placer des données dans la collection `PageData`. Les valeurs de la collection `PageData` sont également accessibles par d’autres sections de la page de contenu ou par des blocs de contenu supplémentaires.

![Diagramme conceptuel qui montre comment une page de contenu peut remplir un dictionnaire PageData et passer ces informations à la page de disposition.](3-creating-a-consistent-look/_static/image8.jpg)

La procédure suivante indique comment passer des données d’une page de contenu à une page de disposition. Lorsque la page s’exécute, elle affiche un bouton qui permet à l’utilisateur de masquer ou d’afficher une liste définie dans la page de disposition. Quand les utilisateurs cliquent sur le bouton, il définit une valeur true/false (Boolean) dans la propriété `PageData`. La page de disposition lit cette valeur et, si elle est définie sur false, masque la liste. La valeur est également utilisée dans la page de contenu pour déterminer s’il faut afficher le bouton **masquer la liste** ou le bouton **afficher la liste** .

![[image]](3-creating-a-consistent-look/_static/image9.jpg)

1. Dans le dossier racine, créez un fichier nommé *Content3. cshtml* et remplacez le contenu existant par ce qui suit :

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Le code stocke deux éléments de données dans la propriété &#8212; `PageData` le titre de la page Web et true ou false pour spécifier s’il faut afficher une liste.

    Notez que ASP.NET vous permet de placer le balisage HTML dans la page de manière conditionnelle à l’aide d’un bloc de code. Par exemple, le bloc `if/else` dans le corps de la page détermine le formulaire à afficher selon que `PageData["ShowList"]` a la valeur true.
2. Dans le dossier *partagé* , créez un fichier nommé *\_Layout3. cshtml* et remplacez le contenu existant par ce qui suit :

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    La page de disposition comprend une expression dans l’élément `<title>` qui obtient la valeur de titre de la propriété `PageData`. Elle utilise également la valeur `ShowList` de la propriété `PageData` pour déterminer s’il faut afficher le bloc de contenu de la liste.
3. Dans le dossier *partagé* , créez un fichier nommé *\_List. cshtml* et remplacez le contenu existant par ce qui suit :

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Exécutez la page *Content3. cshtml* dans un navigateur. La page s’affiche avec la liste visible sur le côté gauche de la page et un bouton **masquer la liste** en bas.

    ![Capture d’écran montrant la page qui comprend la liste et un bouton indiquant « masquer la liste ».](3-creating-a-consistent-look/_static/image10.png)
5. Cliquez sur **masquer la liste**. La liste disparaît et le bouton devient **afficher la liste**.

    ![Capture d’écran montrant la page qui n’inclut pas la liste et un bouton indiquant « afficher la liste ».](3-creating-a-consistent-look/_static/image11.png)
6. Cliquez sur le bouton **afficher la liste** pour afficher à nouveau la liste.

## <a name="additional-resources"></a>Ressources supplémentaires

[Personnalisation du comportement à l’ensemble du site pour pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
