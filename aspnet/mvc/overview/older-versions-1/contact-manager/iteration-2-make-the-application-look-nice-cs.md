---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: '#2 d’itération : rendre l’application attrayanteC#() | Microsoft Docs'
author: microsoft
description: Dans cette itération, nous améliorons l’apparence de l’application en modifiant la page maître par défaut de la vue MVC ASP.NET et la feuille de style en cascade.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: 246cb4b4668339cc4b7e4e03ea005102c6a2a5c3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602017"
---
# <a name="iteration-2--make-the-application-look-nice-c"></a>#2 d’itération : rendre l’application attrayanteC#()

par [Microsoft](https://github.com/microsoft)

[Télécharger le code](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> Dans cette itération, nous améliorons l’apparence de l’application en modifiant la page maître par défaut de la vue MVC ASP.NET et la feuille de style en cascade.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Création d’une application MVC ASP.NET de gestionC#des contacts ()

Dans cette série de didacticiels, nous créons une application de gestion de contacts entière du début à la fin. L’application Gestionnaire de contacts vous permet de stocker les informations de contact, les noms de téléphone et les adresses de messagerie, pour obtenir la liste des personnes.

Nous générons l’application sur plusieurs itérations. À chaque itération, nous améliorons progressivement l’application. L’objectif de cette approche à plusieurs itérations est de vous permettre de comprendre la raison de chaque modification.

- #1 d’itération : créez l’application. Dans la première itération, nous créons le gestionnaire de contacts de la manière la plus simple possible. Nous ajoutons la prise en charge des opérations de base de données de base : créer, lire, mettre à jour et supprimer (CRUD).

- Itération #2 : rendez l’application agréable. Dans cette itération, nous améliorons l’apparence de l’application en modifiant la page maître par défaut de la vue MVC ASP.NET et la feuille de style en cascade.

- Itération #3 : ajouter une validation de formulaire. Dans la troisième itération, nous ajoutons la validation de base du formulaire. Nous empêchons les utilisateurs de soumettre un formulaire sans remplir les champs de formulaire requis. Nous validons également les adresses de messagerie et les numéros de téléphone.

- Itération #4 : rendez l’application faiblement couplée. Dans cette quatrième itération, nous tirant parti de plusieurs modèles de conception de logiciels pour faciliter la gestion et la modification de l’application de gestionnaire de contacts. Par exemple, nous refactorisons notre application pour utiliser le modèle de référentiel et le modèle d’injection de dépendances.

- #5 d’itération-créer des tests unitaires. Dans la cinquième itération, nous rendons notre application plus facile à gérer et à modifier en ajoutant des tests unitaires. Nous imitons nos classes de modèle de données et créons des tests unitaires pour nos contrôleurs et la logique de validation.

- Itération #6-Utilisez le développement piloté par les tests. Dans cette sixième itération, nous ajoutons de nouvelles fonctionnalités à notre application en écrivant d’abord des tests unitaires et en écrivant du code sur les tests unitaires. Dans cette itération, nous ajoutons des groupes de contacts.

- #7 d’itération-ajoutez des fonctionnalités AJAX. Dans la septième itération, nous améliorons la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.

## <a name="this-iteration"></a>Cette itération

L’objectif de cette itération est d’améliorer l’apparence de l’application du gestionnaire de contacts. Actuellement, le gestionnaire de contacts utilise la page maître par défaut de la vue MVC ASP.NET et la feuille de style en cascade (voir figure 1). Ce n’est pas une mauvaise apparence, mais je ne souhaite pas que le gestionnaire de contacts ressemble à tous les autres sites Web ASP.NET MVC. Je souhaite remplacer ces fichiers par des fichiers personnalisés.

[![la boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Figure 01**: apparence par défaut d’une application ASP.NET MVC ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image2.png))

Dans cette itération, j’aborderai deux approches pour améliorer la conception visuelle de notre application. Tout d’abord, je vous montrerai comment tirer parti de la Galerie de conception ASP.NET MVC pour télécharger un modèle de conception ASP.NET MVC gratuit. La Galerie de conception ASP.NET MVC vous permet de créer une application Web professionnelle sans effectuer de travail.

J’ai décidé de ne pas utiliser de modèle à partir de la Galerie de conception ASP.NET MVC pour l’application du gestionnaire de contacts. Au lieu de cela, j’avais une conception personnalisée créée par une société de conception professionnelle. Dans la deuxième partie de ce didacticiel, j’explique comment j’ai travaillé avec une société de conception professionnelle pour créer la conception ASP.NET MVC finale.

## <a name="the-aspnet-mvc-design-gallery"></a>La Galerie de conception ASP.NET MVC

La Galerie de conception ASP.NET MVC est une ressource gratuite fournie par Microsoft. La Galerie ASP.NET MVC se trouve à l’adresse suivante :

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

La Galerie de conception ASP.NET MVC héberge une collection de conceptions de sites Web gratuites qui ont été créées spécifiquement pour une utilisation dans un projet MVC ASP.NET. Les conceptions sont chargées par les membres de la communauté. Les visiteurs de la Galerie peuvent voter pour leurs conceptions favorites (voir figure 2).

[![la boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Figure 02**: bibliothèque de conception ASP.NET MVC ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image4.png))

À mesure que j’écris ce didacticiel, la conception la plus populaire de la Galerie est une conception nommée octobre de David Hauser. Vous pouvez utiliser cette conception pour un projet MVC ASP.NET en procédant comme suit :

1. Cliquez sur le bouton **Télécharger** pour télécharger le fichier. zip d’octobre sur votre ordinateur.
2. Cliquez avec le bouton droit sur le fichier. zip d’octobre téléchargé, puis cliquez sur le bouton **débloquer** (voir figure 3).
3. Décompressez le fichier dans un dossier nommé octobre.
4. Sélectionnez tous les fichiers du dossier DesignTemplate contenu dans le dossier d’octobre, cliquez avec le bouton droit sur les fichiers, puis sélectionnez l’option de menu **copier**.
5. Cliquez avec le bouton droit sur le nœud de projet ContactManager dans la fenêtre de Explorateur de solutions Visual Studio, puis sélectionnez l’option de menu **coller** (voir figure 4).
6. Sélectionnez l’option de menu Visual Studio **Edition, Rechercher et remplacer, remplacement rapide** et remplacer *[Nomdemonprojet]* par *ContactManager* (voir figure 5).

[![la boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Figure 03**: déblocage d’un fichier téléchargé à partir du Web ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image6.png))

[![la boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Figure 04**: remplacement des fichiers dans le Explorateur de solutions ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image8.png))

[![la boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Figure 05**: remplacement de [ProjectName] par ContactManager ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image10.png))

Une fois ces étapes effectuées, votre application Web utilisera la nouvelle conception. La page de la figure 6 illustre l’apparence de l’application de gestionnaire de contacts avec la conception d’octobre.

[![la boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Figure 06**: ContactManager avec le modèle d’octobre ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image12.png))

## <a name="creating-a-custom-aspnet-mvc-design"></a>Création d’une conception ASP.NET MVC personnalisée

La Galerie de conception ASP.NET MVC offre une bonne sélection de styles de conception différents. La Galerie vous offre un moyen simple de personnaliser l’apparence de vos applications ASP.NET MVC. Et, bien sûr, la Galerie présente le grand avantage d’être totalement gratuite.

Toutefois, vous devrez peut-être créer une conception entièrement unique pour votre site Web. Dans ce cas, il est logique de travailler avec une société de conception de site Web. J’ai décidé de prendre cette approche pour la conception de l’application de gestionnaire de contacts.

J’ai compressé le gestionnaire de contacts de l’itération #1 et j’ai envoyé le projet à la société de conception. Il n’était pas propriétaire de Visual Studio (dommage !), mais cela n’a pas rencontré de problème. Ils ont pu télécharger gratuitement Microsoft Visual Web Developer à partir du site Web de [https://www.asp.net](https://www.asp.net) et ouvrir l’application Gestionnaire de contacts dans Visual Web Developer. Dans quelques jours, ils avaient produit la conception de la figure 7.

[![la boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Figure 07**: conception du gestionnaire de contacts ASP.NET MVC ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image14.png))

La nouvelle conception est composée de deux fichiers principaux : un nouveau fichier de feuille de style en cascade et un nouveau fichier de page maître d’affichage. Une page maître de vue contient la disposition et le contenu partagé pour les vues dans une application MVC ASP.NET. Par exemple, la page maître de la vue comprend l’en-tête, les onglets de navigation et le pied de page qui s’affichent à la figure 7. J’ai remplacé la page maître site. Master View dans le dossier Views\Shared par le nouveau fichier site. Master de l’entreprise de conception,

La société de conception a également créé une nouvelle feuille de style en cascade et un ensemble d’images. J’ai placé ces nouveaux fichiers dans le dossier Content et j’ai remplacé le fichier site. CSS existant. Vous devez placer tout le contenu statique dans le dossier Content.

Notez que la nouvelle conception du gestionnaire de contacts comprend des images pour la modification et la suppression de contacts. Une image de modification et de suppression apparaît en regard de chaque contact dans la table de contacts HTML.

À l’origine, ces liens étaient rendus avec le code HTML. Application auxiliaire ActionLink () comme suit :

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

La méthode html. ActionLink () ne prend pas en charge les images (la méthode HTML encode le texte du lien pour des raisons de sécurité). J’ai donc remplacé les appels à html. ActionLink () par des appels à URL. action () comme suit :

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

La méthode html. ActionLink () restitue l’intégralité d’un lien hypertexte HTML. En revanche, la méthode URL. action () affiche simplement l’URL sans la &lt;une balise de&gt;.

Notez, en outre, que la nouvelle conception comprend à la fois les onglets sélectionnés et non sélectionnés. Par exemple, dans la figure 8, l’onglet **créer un contact** est sélectionné et l’onglet **mes contacts** n’est pas sélectionné.

[![la boîte de dialogue Nouveau projet](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Figure 08**: onglets sélectionnés et désélectionnés ([cliquez pour afficher l’image en taille réelle](iteration-2-make-the-application-look-nice-cs/_static/image16.png))

Pour prendre en charge le rendu des onglets sélectionnés et désélectionnés, j’ai créé un programme d’assistance HTML personnalisé nommé MenuItemHelper. Cette méthode d’assistance restitue une balise &lt;Li&gt; ou une balise &lt;li class = "Selected"&gt; selon que le contrôleur et l’action actuels correspondent au contrôleur et au nom d’action passés à l’application auxiliaire. Le code pour le MenuItemHelper est contenu dans la liste 1.

**Liste 1-Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

MenuItemHelper utilise la classe TagBuilder en interne pour générer la balise HTML &lt;Li&gt;. La classe TagBuilder est une classe utilitaire très utile que vous pouvez utiliser chaque fois que vous devez générer une nouvelle balise HTML. Il comprend des méthodes pour l’ajout d’attributs, l’ajout de classes CSS, la génération d’ID et la modification du code HTML interne de la balise.

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons amélioré la conception visuelle de notre application ASP.NET MVC. Tout d’abord, vous avez été introduit dans la Galerie de conception ASP.NET MVC. Vous avez appris à télécharger des modèles de conception gratuits à partir de la Galerie de conception ASP.NET MVC, que vous pouvez utiliser dans vos applications ASP.NET MVC.

Ensuite, nous avons abordé la manière dont vous pouvez créer une conception personnalisée en modifiant le fichier de feuille de style en cascade et le fichier de page de vue maître par défaut. Pour prendre en charge la nouvelle conception, nous avons dû apporter quelques modifications mineures à notre application de gestion des contacts. Par exemple, nous avons ajouté un nouveau programme d’assistance HTML nommé MenuItemHelper qui affiche les onglets sélectionnés et désélectionnés.

Dans l’itération suivante, nous allons aborder l’objet très important de la validation. Nous ajoutons le code de validation à notre application afin qu’un utilisateur ne puisse pas créer un nouveau contact sans fournir les valeurs requises, telles que le prénom et le nom de la personne.

> [!div class="step-by-step"]
> [Précédent](iteration-1-create-the-application-cs.md)
> [Suivant](iteration-3-add-form-validation-cs.md)
