---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: Vue d’ensemble des vues MVC ASP.NET (VB) | Microsoft Docs
author: StephenWalther
description: Qu’est-ce qu’une vue MVC ASP.NET et comment elle diffère-t-elle d’une page HTML ? Dans ce didacticiel, Stephen Walther vous présente les vues et montre comment vous pouvez...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: f02728ed248f29b09d654e509977ed43889cbb83
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541306"
---
# <a name="aspnet-mvc-views-overview-vb"></a>Vue d’ensemble des vues ASP.NET MVC (VB)

par [Stephen Walther](https://github.com/StephenWalther)

> Qu’est-ce qu’une vue MVC ASP.NET et comment elle diffère-t-elle d’une page HTML ? Dans ce didacticiel, Stephen Walther vous présente les vues et montre comment tirer parti des données d’affichage et des applications auxiliaires HTML dans une vue.

L’objectif de ce didacticiel est de vous fournir une brève introduction aux vues ASP.NET MVC, aux données d’affichage et aux applications d’assistance HTML. À la fin de ce didacticiel, vous devez comprendre comment créer des vues, passer des données d’un contrôleur à une vue et utiliser des applications auxiliaires HTML pour générer du contenu dans une vue.

## <a name="understanding-views"></a>Présentation des vues

Contrairement à ASP.NET ou Active Server pages, ASP.NET MVC n’inclut rien qui correspond directement à une page. Dans une application MVC ASP.NET, il n’existe pas de page sur le disque qui correspond au chemin d’accès dans l’URL que vous tapez dans la barre d’adresses de votre navigateur. L’élément le plus proche d’une page dans une application ASP.NET MVC est appelé *vue*.

Dans une application MVC ASP.NET, les demandes de navigateur entrantes sont mappées à des actions de contrôleur. Une action de contrôleur peut retourner une vue. Toutefois, une action de contrôleur peut exécuter un autre type d’action, comme la redirection vers une autre action de contrôleur.

La liste 1 contient un contrôleur simple nommé HomeController. Le HomeController expose deux actions de contrôleur nommées index () et Details ().

**Liste 1-HomeController. vb**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

Vous pouvez appeler la première action, l’action index (), en tapant l’URL suivante dans la barre d’adresse de votre navigateur :

/Home/Index

Vous pouvez appeler la deuxième action, l’action Details (), en tapant cette adresse dans votre navigateur :

/Home/Details

L’action index () renvoie une vue. La plupart des actions que vous créez retournent des vues. Toutefois, une action peut retourner d’autres types de résultats d’action. Par exemple, l’action Details () renvoie un RedirectToActionResult qui redirige la requête entrante vers l’action index ().

L’action index () contient la seule ligne de code suivante :

Vue ()

Cette ligne de code retourne une vue qui doit se trouver sur le chemin d’accès suivant sur votre serveur Web :

\Views\Home\Index.aspx

Le chemin d’accès à la vue est déduit à partir du nom du contrôleur et du nom de l’action du contrôleur.

Si vous préférez, vous pouvez être explicite sur la vue. La ligne de code suivante retourne une vue nommée Fred :

Vue (Fred)

Lorsque cette ligne de code est exécutée, une vue est retournée à partir du chemin d’accès suivant :

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Si vous envisagez de créer des tests unitaires pour votre application ASP.NET MVC, il est judicieux d’être explicite en ce qui concerne les noms de vues. De cette façon, vous pouvez créer un test unitaire pour vérifier que la vue attendue a été retournée par une action de contrôleur.

## <a name="adding-content-to-a-view"></a>Ajout de contenu à une vue

Une vue est un document HTML standard (X) qui peut contenir des scripts. Vous utilisez des scripts pour ajouter du contenu dynamique à une vue.

Par exemple, la vue dans la liste 2 affiche la date et l’heure actuelles.

**Liste 2-\Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

Notez que le corps de la page HTML dans le Listing 2 contient le script suivant :

&lt;% Response. Write (DateTime. Now)%&gt;

Vous utilisez les délimiteurs de script &lt;% et%&gt; pour marquer le début et la fin d’un script. Ce script est écrit en Visual Basic. Elle affiche la date et l’heure actuelles en appelant la méthode Response. Write () pour afficher le contenu dans le navigateur. Les délimiteurs de script &lt;% et%&gt; peuvent être utilisés pour exécuter une ou plusieurs instructions.

Étant donné que vous appelez Response. Write (), Microsoft vous fournit souvent un raccourci pour appeler la méthode Response. Write (). La vue de la liste 3 utilise les délimiteurs &lt;% = et%&gt; comme raccourci pour appeler Response. Write ().

**Liste 3-Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

Vous pouvez utiliser n’importe quel langage .NET pour générer du contenu dynamique dans une vue. Normalement, vous utilisez l’un ou l’autre C# Visual Basic .net ou pour écrire vos contrôleurs et vues.

## <a name="using-html-helpers-to-generate-view-content"></a>Utilisation des applications auxiliaires HTML pour générer le contenu de la vue

Pour faciliter l’ajout de contenu à une vue, vous pouvez tirer parti d’un *programme d’assistance HTML*. Une application auxiliaire HTML, en général, est une méthode qui génère une chaîne. Vous pouvez utiliser des applications auxiliaires HTML pour générer des éléments HTML standard, tels que des zones de texte, des liens, des listes déroulantes et des zones de liste.

Par exemple, la vue de la liste 4 tire parti de trois applications auxiliaires HTML : BeginForm (), zone de texte () et mot de passe () pour générer un formulaire de connexion (voir la figure 1).

**Liste 4--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]

[![la boîte de dialogue Nouveau projet](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)

**Figure 01**: formulaire de connexion standard ([cliquez pour afficher l’image en taille réelle](asp-net-mvc-views-overview-vb/_static/image2.png))

Toutes les méthodes d’assistance HTML sont appelées sur la propriété HTML de la vue. Par exemple, vous affichez une zone de texte en appelant la méthode html. TextBox ().

Notez que vous utilisez les délimiteurs de script &lt;% = et%&gt; lors de l’appel des applications d’assistance HTML. TextBox () et html. Password (). Ces applications auxiliaires retournent simplement une chaîne. Vous devez appeler Response. Write () pour afficher la chaîne dans le navigateur.

L’utilisation des méthodes d’assistance HTML est facultative. Ils facilitent votre vie en réduisant la quantité de code HTML et de script que vous devez écrire. La vue dans la liste 5 affiche exactement la même forme que la vue dans la liste 4 sans utiliser les applications auxiliaires HTML.

**Liste 5--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

Vous avez également la possibilité de créer vos propres applications auxiliaires HTML. Par exemple, vous pouvez créer une méthode d’assistance GridView () qui affiche automatiquement un ensemble d’enregistrements de base de données dans un tableau HTML. Nous explorons cette rubrique dans le didacticiel **création d’applications auxiliaires html personnalisées**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Utilisation de l’affichage des données pour passer des données à une vue

Vous utilisez les données d’affichage pour passer des données d’un contrôleur à une vue. Considérez les données d’affichage comme un package que vous envoyez par courrier électronique. Toutes les données transmises d’un contrôleur à une vue doivent être envoyées à l’aide de ce package. Par exemple, le contrôleur de la liste 6 ajoute un message pour afficher les données.

**Liste 6-ProductController. vb**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

La propriété du contrôleur ViewData représente une collection de paires nom/valeur. Dans la liste 6, la méthode index () ajoute un élément à la collection de données d’affichage nommée message avec la valeur Hello World !. Lorsque la vue est retournée par la méthode index (), les données d’affichage sont automatiquement transmises à la vue.

La vue dans la liste 7 récupère le message à partir des données d’affichage et restitue le message dans le navigateur.

**Liste 7--\Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

Notez que la vue tire parti de la méthode d’assistance HTML HTML. Encode () lors du rendu du message. Le programme d’assistance HTML HTML. Encode () code les caractères spéciaux tels que les &lt; et les &gt; en caractères qui peuvent être affichés en toute sécurité dans une page Web. Chaque fois que vous affichez le contenu qu’un utilisateur envoie à un site Web, vous devez encoder le contenu pour empêcher les attaques par injection de code JavaScript.

(Étant donné que nous avons créé le message nous-mêmes dans le ProductController, nous n’avons pas vraiment besoin d’encoder le message. Toutefois, il est judicieux de toujours appeler la méthode html. Encode () lors de l’affichage de contenu récupéré à partir de données d’affichage dans une vue.)

Dans la liste 7, nous avons tiré parti des données d’affichage pour passer un message de chaîne simple d’un contrôleur à une vue. Vous pouvez également utiliser afficher les données pour transmettre d’autres types de données, tels qu’une collection d’enregistrements de base de données, d’un contrôleur à une vue. Par exemple, si vous souhaitez afficher le contenu de la table de base de données Products dans une vue, vous devez passer la collection d’enregistrements de base de données dans les données d’affichage.

Vous avez également la possibilité de passer des données d’affichage fortement typées d’un contrôleur à une vue. Nous explorons cette rubrique dans le didacticiel **Présentation des vues et des données d’affichage fortement typées**.

## <a name="summary"></a>Récapitulatif

Ce didacticiel vous a présenté une brève introduction aux vues ASP.NET MVC, à l’affichage des données et aux applications auxiliaires HTML. Dans la première section, vous avez appris à ajouter de nouvelles vues à votre projet. Vous avez appris que vous devez ajouter une vue au dossier approprié afin de l’appeler à partir d’un contrôleur particulier. Nous avons ensuite abordé le sujet des applications auxiliaires HTML. Vous avez appris comment les applications auxiliaires HTML vous permettent de générer facilement du contenu HTML standard. Enfin, vous avez appris à tirer parti des données d’affichage pour passer des données d’un contrôleur à une vue.

> [!div class="step-by-step"]
> [Précédent](passing-data-to-view-master-pages-cs.md)
> [Suivant](creating-custom-html-helpers-vb.md)
