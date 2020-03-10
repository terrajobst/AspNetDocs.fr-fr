---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Pages maîtres | Microsoft Docs
author: microsoft
description: L’un des principaux composants d’un site Web réussi est un aspect cohérent. Dans ASP.NET 1. x, les développeurs utilisaient des contrôles utilisateur pour répliquer la page commune elem...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: 36f2caf7c2c9bcafd22c8f6681c1d6b19fe5078a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78567332"
---
# <a name="master-pages"></a>Pages maîtres

par [Microsoft](https://github.com/microsoft)

> L’un des principaux composants d’un site Web réussi est un aspect cohérent. Dans ASP.NET 1. x, les développeurs utilisaient des contrôles utilisateur pour répliquer des éléments de page communs dans une application Web. Bien qu’il s’agit certainement d’une solution réalisable, l’utilisation de contrôles utilisateur présente certains inconvénients. Par exemple, une modification de la position d’un contrôle utilisateur requiert une modification de plusieurs pages sur un site. Les contrôles utilisateur ne sont pas non plus rendus dans Mode Création une fois insérés dans une page.

L’un des principaux composants d’un site Web réussi est un aspect cohérent. Dans ASP.NET 1. x, les développeurs utilisaient des contrôles utilisateur pour répliquer des éléments de page communs dans une application Web. Bien qu’il s’agit certainement d’une solution réalisable, l’utilisation de contrôles utilisateur présente certains inconvénients. Par exemple, une modification de la position d’un contrôle utilisateur requiert une modification de plusieurs pages sur un site. Les contrôles utilisateur ne sont pas non plus rendus dans Mode Création une fois insérés dans une page.

ASP.NET 2,0 introduit les pages maîtres comme un moyen de conserver une apparence cohérente, et comme vous le verrez bientôt, les pages maîtres représentent une amélioration significative par rapport à la méthode de contrôle utilisateur.

## <a name="why-master-pages"></a>Pourquoi les pages maîtres ?

Vous vous demandez peut-être pourquoi les pages maîtres étaient nécessaires dans ASP.NET 2,0. Après tout, les développeurs de sites Web utilisent déjà des contrôles utilisateur dans ASP.NET 1. x pour partager des zones de contenu entre les pages. Il existe en fait plusieurs raisons pour lesquelles les contrôles utilisateur sont une solution peu optimale pour la création d’une disposition commune.

Les contrôles utilisateur ne définissent pas réellement la mise en page. Au lieu de cela, ils définissent la disposition et les fonctionnalités d’une partie d’une page. La distinction entre ces deux aspects est importante, car elle simplifie grandement la gestion d’une solution de contrôle utilisateur. Par exemple, lorsque vous souhaitez modifier la position d’un contrôle utilisateur sur votre page, vous devez modifier la page réelle sur laquelle le contrôle utilisateur apparaît. Cela fonctionne bien si vous n’avez que quelques pages, mais dans les sites de grande taille, il devient rapidement un véritable cauchemar pour la gestion de site.

L’un des inconvénients de l’utilisation de contrôles utilisateur pour la définition d’une disposition commune est enraciné dans l’architecture de ASP.NET. Si un membre public d’un contrôle utilisateur est modifié, vous devez recompiler toutes les pages qui utilisent le contrôle utilisateur. À son tour, ASP.NET réexécutera ensuite un JIT sur vos pages lors de leur premier accès. Cela génère une nouvelle fois une architecture non évolutive et un problème de gestion de site pour les sites plus importants.

Ces deux problèmes (et bien plus encore) sont correctement traités par les pages maîtres dans ASP.NET 2,0.

## <a name="how-master-pages-work"></a>Fonctionnement des pages maîtres

Une page maître est analogue à un modèle pour d’autres pages. Les éléments de page qui doivent être partagés entre d’autres pages (c’est-à-dire des menus, des bordures, etc.) sont ajoutés à la page maître. Lorsque de nouvelles pages sont ajoutées au site, vous pouvez les associer à une page maître. Une page associée à une page maître s’appelle une page de **contenu**. Par défaut, une page de contenu prend l’apparence de la page maître. Toutefois, lorsque vous créez une page maître, vous pouvez définir des parties de la page que la page de contenu peut remplacer par son propre contenu. Ces parties sont définies à l’aide d’un nouveau contrôle introduit dans ASP.NET 2,0 ; contrôle **ContentPlaceHolder** .

Une page maître peut contenir un nombre quelconque de contrôles ContentPlaceHolder (ou aucun). Sur la page contenu, le contenu des contrôles ContentPlaceHolder s’affiche à l’intérieur des contrôles de **contenu** , un autre nouveau contrôle dans ASP.NET 2,0. Par défaut, les contrôles de contenu des pages de contenu sont vides, ce qui vous permet de fournir votre propre contenu. Si vous souhaitez utiliser le contenu de la page maître dans les contrôles de contenu, vous pouvez le faire comme vous le verrez plus loin dans ce module. Le contrôle de contenu est mappé au contrôle ContentPlaceHolder via l’attribut ContentPlaceHolderID du contrôle de contenu. Le code ci-dessous mappe un contrôle de contenu à un contrôle ContentPlaceHolder appelé mainBody sur une page maître.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Vous entendrez souvent que les gens décrivent les pages maîtres comme une classe de base pour d’autres pages. Cela n’est en fait pas vrai. La relation entre les pages maîtres et les pages de contenu ne fait pas partie de l’héritage.

La **figure 1** illustre une page maître et une page de contenu associée qui apparaissent dans Visual Studio 2005. Vous pouvez voir le contrôle ContentPlaceHolder dans la page maître et le contrôle de contenu correspondant dans la page de contenu. Notez que le contenu des pages maîtres qui est en dehors du ContentPlaceHolder est visible, mais grisé dans la page de contenu. Seul le contenu à l’intérieur de ContentPlaceHolder peut être supplanté par la page de contenu. Tout autre contenu provenant de la page maître est immuable.

![Une page maître et sa page de contenu associée](master-pages/_static/image1.jpg)

**Figure 1**: page maître et page de contenu associée

## <a name="creating-a-master-page"></a>Création d’une page maître

Pour créer une nouvelle page maître :

1. Ouvrez Visual Studio 2005 et créez un nouveau site Web.
2. Cliquez sur fichier, nouveau, fichier.
3. Choisissez fichier maître dans la boîte de dialogue Ajouter un nouvel élément, comme illustré à la **figure 2**.
4. Cliquez sur Ajouter.

![Création d’une nouvelle page maître](master-pages/_static/image2.jpg)

**Figure 2**: création d’une nouvelle page maître

Notez que l’extension de fichier d’une page maître est *. Master*. Il s’agit de l’une des façons dont une page maître diffère d’une page ordinaire. L’autre différence principale est que, à la place d’une directive @Page, la page maître contient une directive @Master. Basculez en mode source pour la page maître que vous venez de créer et passez en revue le code.

Une nouvelle page maître aura un contrôle ContentPlaceHolder par défaut. Dans la plupart des cas, il est plus judicieux de créer d’abord les éléments de page courants, puis d’insérer les contrôles ContentPlaceHolder où le contenu personnalisé est souhaité. Dans ces cas, les développeurs souhaitent supprimer le contrôle ContentPlaceHolder par défaut et en insérer de nouveaux lors du développement de la page. Les contrôles ContentPlaceHolder ne sont pas redimensionnables malgré le fait qu’ils affichent des poignées de dimensionnement. La taille du contrôle ContentPlaceHolder est automatiquement basée sur le contenu qu’il contient, à une exception près. Si vous placez un contrôle ContentPlaceHolder à l’intérieur d’un élément de bloc tel qu’une cellule de tableau, il est redimensionné en fonction de la taille de l’élément.

## <a name="lab-1-working-with-master-pages"></a>Atelier 1 utilisation des pages maîtres

Dans ce laboratoire, vous allez créer une nouvelle page maître et définir trois contrôles ContentPlaceHolder. Vous allez ensuite créer une nouvelle page de contenu et remplacer le contenu d’au moins un des contrôles ContentPlaceHolder.

1. Créer une page maître et insérer des contrôles ContentPlaceHolder. 

    1. Créez une nouvelle page maître comme décrit ci-dessus.
    2. Supprimez le contrôle ContentPlaceHolder par défaut.
    3. Sélectionnez le contrôle ContentPlaceHolder en cliquant sur la bordure ombrée supérieure du contrôle, puis supprimez-le en appuyant sur la touche Suppr de votre clavier.
    4. Insérez une nouvelle table à l’aide de l' *en-tête et* du modèle côté, comme illustré à la figure 3. Remplacez la largeur et la hauteur par 90% chacune afin que la totalité de la table soit visible dans le concepteur.

![](master-pages/_static/image3.jpg)

**Figure 3**

1. Placez le curseur dans chaque cellule de la table et définissez la propriété *VAlign* sur *Top*.
2. À partir de la boîte à outils, insérez un contrôle ContentPlaceHolder dans la cellule supérieure du tableau (la cellule d’en-tête).
3. Lorsque vous insérez ce contrôle ContentPlaceHolder, vous remarquerez que la hauteur de ligne occupera presque toute la page, comme illustré à la figure 4. Ne vous inquiétez pas à ce stade.

![L’espace vide se trouve dans la même cellule que ContentPlaceHolder](master-pages/_static/image1.gif)

**Figure 4**: l’espace vide se trouve dans la même cellule que ContentPlaceHolder

1. Placez un contrôle ContentPlaceHolder dans les deux autres cellules. Une fois les autres contrôles ContentPlaceHolder insérés, la taille des cellules de la table doit être identique à celle que vous attendez. La page doit maintenant ressembler à la page illustrée à la **figure 5**.

![Le maître avec tous les contrôles ContentPlaceHolder. Notez que la hauteur de cellule de la cellule d’en-tête est maintenant ce qu’elle doit être](master-pages/_static/image2.gif)

**Figure 5**: le maître avec tous les contrôles ContentPlaceHolder. Notez que la hauteur de cellule de la cellule d’en-tête est maintenant ce qu’elle doit être

1. Entrez le texte de votre choix dans chacun des trois contrôles ContentPlaceHolder.
2. Enregistrez la page maître en tant que exercise1. Master.
3. Créez un nouveau formulaire Web et associez-le à la page maître exercise1. Master.
4. Sélectionnez fichier, nouveau, fichier dans Visual Studio 2005.
5. Sélectionnez **Web Form** dans la boîte de dialogue Ajouter un nouvel élément.
6. Assurez-vous que la case à cocher sélectionner une page maître est activée, comme illustré à la figure 6.

![Ajout d’une nouvelle page de contenu](master-pages/_static/image3.gif)

**Figure 6**: ajout d’une nouvelle page de contenu

1. Cliquez sur Ajouter.
2. Sélectionnez exercise1. Master dans la boîte de dialogue Sélectionner une page maître, comme illustré à la figure 7.
3. Cliquez sur OK pour ajouter la page nouveau contenu.

La page nouveau contenu s’affiche dans Visual Studio avec un contrôle de contenu pour chaque contrôle ContentPlaceHolder sur la page maître. Par défaut, les contrôles de contenu sont vides, ce qui vous permet d’ajouter votre propre contenu. Si vous souhaitez qu’ils utilisent le contenu du contrôle ContentPlaceHolder sur la page maître, il vous suffit de cliquer sur le symbole de balise active (la petite flèche noire dans l’angle supérieur droit du contrôle) et de choisir la *valeur par défaut pour le contenu maître* de la balise active, comme illustré à la **figure 8**. Dans ce cas, l’élément de menu devient *créer un contenu personnalisé*. En cliquant dessus, vous supprimez le contenu de la page maître, ce qui vous permet de définir un contenu personnalisé pour ce contrôle de contenu particulier.

![Affectation de la valeur par défaut d’un contrôle de contenu au contenu des pages maîtres](master-pages/_static/image4.gif)

**Figure 7**: définition de la valeur par défaut d’un contrôle de contenu sur le contenu des pages maîtres

## <a name="connecting-master-page-and-content-pages"></a>Connexion à la page maître et aux pages de contenu

L’association entre une page maître et une page de contenu peut être configurée de quatre façons différentes :

- Attribut <strong>MasterPageFile</strong> de la directive @Page
- Définition de la propriété **page. MasterPageFile** dans le code.
- L’élément **&lt;pages&gt;** dans le fichier de configuration des applications (Web. config dans le dossier racine de l’application)
- L’élément **&lt;pages&gt;** dans un fichier de configuration de sous-dossiers (Web. config dans un sous-dossier)

## <a name="masterpagefile-attribute"></a>MasterPageFile (attribut)

L’attribut MasterPageFile permet d’appliquer facilement une page maître à une page ASP.NET particulière. C’est également la méthode utilisée pour appliquer la page maître lorsque vous activez la case à cocher **Sélectionner la page maître** comme vous l’avez fait dans l’exercice 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Définition de page. MasterPageFile dans le code

En définissant la propriété MasterPageFile dans le code, vous pouvez appliquer une page maître particulière à votre contenu au moment de l’exécution. Cela est utile dans les cas où vous devrez peut-être appliquer une page maître spécifique en fonction d’un rôle d’utilisateur ou d’autres critères. La propriété MasterPageFile doit être définie dans la méthode PreInit. S’il est défini après la méthode PreInit, une exception InvalidOperationException est levée. La page sur laquelle cette propriété est définie doit également avoir un contrôle de contenu comme contrôle de niveau supérieur pour la page. Dans le cas contraire, une exception HttpException est levée lorsque la propriété MasterPageFile est définie.

## <a name="using-the-ltpagesgt-element"></a>Utilisation de l’élément&gt; &lt;pages

Vous pouvez configurer une page maître pour vos pages en définissant l’attribut masterPageFile dans l’élément &lt;pages&gt; du fichier Web. config. Lorsque vous utilisez cette méthode, gardez à l’esprit que les fichiers Web. config inférieurs dans la structure de l’application peuvent remplacer ce paramètre. Tout attribut MasterPageFile défini dans une directive @Page remplacera également ce paramètre. L’utilisation de l’élément &lt;pages&gt; facilite la création d’une page *maître maître qui* peut être remplacée si nécessaire dans des dossiers ou des fichiers particuliers.

## <a name="properties-in-master-pages"></a>Propriétés dans les pages maîtres

Une page maître peut exposer des propriétés en rendant simplement ces propriétés publiques dans la page maître. Par exemple, le code suivant définit une propriété appelée SomeProperty :

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Pour accéder à la propriété SomeProperty à partir de la page de contenu, vous devez utiliser la propriété Master comme suit :

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Imbrication de pages maîtres

Les pages maîtres constituent la solution idéale pour garantir une apparence commune dans une application Web de grande taille. Toutefois, il n’est pas rare de faire en sorte que certaines parties d’un site de grande taille partagent une interface commune, tandis que les autres parties partagent une interface différente. Pour répondre à ce besoin, plusieurs pages maîtres constituent la solution idéale. Toutefois, cela ne traite toujours pas du fait qu’une application de grande taille peut avoir certains composants (par exemple, un menu) qui sont partagés entre toutes les pages et d’autres composants qui sont partagés uniquement dans certaines sections du site. Pour ce type de situation, les pages maîtres imbriquées remplissent le besoin. Comme vous l’avez vu, une page maître normale se compose d’une page maître et d’une page de contenu. Dans une situation de page maître imbriquée, il existe deux pages maîtres : un maître parent et un maître enfant. La page maître enfant est également une page de contenu et son maître est la page maître parente.

Voici le code d’une page maître classique :

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

Dans un scénario maître imbriqué, il s’agit du maître parent. Une autre page maître utilise cette page comme page maître et ce code ressemble à ceci :

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Notez que dans ce scénario, le maître enfant est également une page de contenu pour le maître parent. Tout le contenu du maître enfant apparaît à l’intérieur d’un contrôle de contenu qui obtient son contenu à partir du contrôle ContentPlaceHolder du parent.

> [!NOTE]
> La prise en charge du concepteur n’est pas disponible pour les pages maîtres imbriquées. Lorsque vous développez à l’aide de maîtres imbriqués, vous devez utiliser le mode Source.

Cette vidéo présente une procédure pas à pas d’utilisation de pages maîtres imbriquées.

![](master-pages/_static/image1.png)

[Ouvrir la vidéo en plein écran](master-pages/_static/nested1.wmv)

![Sélection d’une page maître](master-pages/_static/image4.jpg)

**Figure 8**: sélection d’une page maître
