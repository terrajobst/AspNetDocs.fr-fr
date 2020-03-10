---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Partie 3 : menu disposition et catégorie | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 3 couvre l’ajout de la disposition et d’un menu de catégorie.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639117"
---
# <a name="part-3-layout-and-category-menu"></a>Partie 3 : menu disposition et catégorie

par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.
> 
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 3 couvre l’ajout de la disposition et d’un menu de catégorie.

## <a id="_Toc260221669"></a>Ajout de certaines mises en page et d’un menu catégorie

Dans notre page maître du site, nous allons ajouter une balise div pour la colonne du côté gauche qui contiendra notre menu de catégorie de produits.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Notez que l’alignement souhaité et une autre mise en forme seront fournis par la classe CSS que nous avons ajoutée à notre fichier style. css.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

Le menu catégorie de produit est créé dynamiquement au moment de l’exécution en interrogeant la base de données commerce pour rechercher les catégories de produits existantes et en créant les éléments de menu et les liens correspondants.

Pour ce faire, nous utilisons deux pages ASP. Contrôles de données puissants de NET. Le contrôle « source de données d’entité » et le contrôle « ListView ».

![](tailspin-spyworks-part-3/_static/image1.jpg)

Passons à la « vue conception » et utilisons les applications d’assistance pour configurer nos contrôles.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Nous allons affecter à la propriété EntityDataSource ID la valeur EDS\_Category\_menu et cliquer sur configurer la source de données.

![](tailspin-spyworks-part-3/_static/image3.jpg)

Sélectionnez la connexion CommerceEntities qui a été créée pour nous lors de la création du modèle de source de données d’entité pour notre base de données de commerce, puis cliquez sur « suivant ».

![](tailspin-spyworks-part-3/_static/image4.jpg)

Sélectionnez le nom du jeu d’entités « Categories » et laissez les autres options par défaut. Cliquez sur « Terminer ».

Nous allons maintenant définir la propriété ID de l’instance de contrôle ListView que nous avons placée dans notre page sur ListView\_ProductsMenu et activer son application auxiliaire.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Bien qu’il soit possible d’utiliser des options de contrôle pour mettre en forme l’affichage et la mise en forme des éléments de données, notre création de menu nécessite uniquement un balisage simple. nous allons donc entrer le code dans la vue source.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Veuillez noter l’instruction « eval » : &lt;% # Eval (« CategoryName »)%&gt;

La syntaxe ASP.NET &lt;% #%&gt; est une convention sténographique qui indique au runtime d’exécuter tout ce qui est contenu dans et de générer les résultats « in line ».

L’instruction eval (« CategoryName ») indique que, pour l’entrée actuelle dans la collection liée d’éléments de données, récupérez la valeur des noms d’élément de modèle d’entité « CategoryName ». Il s’agit d’une syntaxe concise pour une fonctionnalité très puissante.

Permet d’exécuter l’application maintenant.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Notez que notre menu de catégorie de produits s’affiche et lorsque nous pointons sur l’un des éléments de menu catégorie, nous pouvons voir le lien de l’élément de menu pointe vers une page que nous avons encore implémentée nommée ProductsList. aspx et que nous avons créé un argument de chaîne de requête dynamique qui contient le  ID de catégorie.

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-2.md)
> [Suivant](tailspin-spyworks-part-4.md)
