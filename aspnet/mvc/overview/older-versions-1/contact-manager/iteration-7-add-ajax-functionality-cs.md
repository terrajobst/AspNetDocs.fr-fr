---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: '#7 d’itération – ajouter des fonctionnalitésC#Ajax () | Microsoft Docs'
author: microsoft
description: Dans la septième itération, nous améliorons la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c8eb3d3688674dd2c220b4bd1b5982f2610d0eb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544176"
---
# <a name="iteration-7--add-ajax-functionality-c"></a>#7 d’itération – ajouter des fonctionnalitésC#Ajax ()

par [Microsoft](https://github.com/microsoft)

[Télécharger le code](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> Dans la septième itération, nous améliorons la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.

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

Dans cette itération de l’application du gestionnaire de contacts, nous refactorisons notre application pour utiliser AJAX. En tirant parti d’Ajax, nous rendons notre application plus réactive. Nous pouvons éviter de rendre une page entière quand nous devons mettre à jour uniquement une certaine région dans une page.

Nous allons Refactoriser notre vue d’index afin de ne pas avoir à réafficher la page entière chaque fois qu’un utilisateur sélectionne un nouveau groupe de contacts. Au lieu de cela, quand quelqu’un clique sur un groupe de contacts, nous allons simplement mettre à jour la liste des contacts et laisser le reste de la page seul.

Nous allons également modifier le mode de fonctionnement de notre lien de suppression. Au lieu d’afficher une page de confirmation distincte, nous affichons une boîte de dialogue de confirmation JavaScript. Si vous confirmez que vous souhaitez supprimer un contact, une opération de suppression HTTP est effectuée sur le serveur pour supprimer l’enregistrement de contact de la base de données.

En outre, nous allons tirer parti de jQuery pour ajouter des effets d’animation à notre vue d’index. Nous allons afficher une animation lorsque la nouvelle liste de contacts est récupérée à partir du serveur.

Enfin, nous allons tirer parti de la prise en charge de l’infrastructure ASP.NET AJAX pour gérer l’historique du navigateur. Nous allons créer des points d’historique chaque fois que nous effectuons un appel Ajax pour mettre à jour la liste des contacts. De cette façon, les boutons précédent et suivant du navigateur fonctionnent.

## <a name="why-use-ajax"></a>Pourquoi utiliser AJAX ?

L’utilisation d’Ajax présente de nombreux avantages. Tout d’abord, l’ajout de fonctionnalités AJAX à une application se traduit par une meilleure expérience utilisateur. Dans une application Web normale, la page entière doit être publiée sur le serveur chaque fois qu’un utilisateur effectue une action. Chaque fois que vous effectuez une action, le navigateur se verrouille et l’utilisateur doit attendre que la page entière soit extraite et réaffichée.

Il s’agit d’une expérience inacceptable dans le cas d’une application de bureau. Mais, traditionnellement, nous avons vécu avec cette expérience utilisateur incorrecte dans le cas d’une application Web, car nous ne savons pas que nous pouvions améliorer. Nous avons pensé qu’il s’agissait d’une limitation des applications Web quand, en réalité, il s’agissait simplement d’une limitation de nos imagination.

Dans une application AJAX, vous n’avez pas besoin d’apporter l’expérience utilisateur à un arrêt juste pour mettre à jour une page. Au lieu de cela, vous pouvez exécuter une demande asynchrone en arrière-plan pour mettre à jour la page. Vous ne forcez pas l’utilisateur à attendre la mise à jour d’une partie de la page.

En tirant parti d’Ajax, vous pouvez également améliorer les performances de votre application. Réfléchissez à la façon dont l’application du gestionnaire de contacts fonctionne maintenant sans fonctionnalité Ajax. Lorsque vous cliquez sur un groupe de contacts, la vue entière de l’index doit être réaffichée. La liste des contacts et la liste des groupes de contacts doivent être récupérées à partir du serveur de base de données. Toutes ces données doivent être transmises sur le réseau à partir du serveur Web vers le navigateur Web.

Toutefois, une fois que nous avons ajouté les fonctionnalités AJAX à notre application, nous pouvons éviter de réafficher la page entière quand un utilisateur clique sur un groupe de contacts. Nous n’avons plus besoin de récupérer les groupes de contacts de la base de données. Nous avons également besoin de pousser l’intégralité de la vue index sur le câble. En tirant parti d’Ajax, nous réduisons la quantité de travail que notre serveur de base de données doit effectuer et nous réduisons le volume de trafic réseau requis par notre application.

## <a name="don-t-be-afraid-of-ajax"></a>Je n’ai pas peur d’Ajax

Certains développeurs évitent d’utiliser AJAX, car ils se soucient des navigateurs de niveau inférieur. Ils souhaitent s’assurer que leurs applications Web continueront de fonctionner lorsqu’un navigateur ne prend pas en charge JavaScript. Comme Ajax dépend de JavaScript, certains développeurs évitent d’utiliser AJAX.

Toutefois, si vous faites attention à la manière dont vous implémentez Ajax, vous pouvez créer des applications qui fonctionnent avec les navigateurs de niveau supérieur et inférieur. Notre application de gestion des contacts fonctionne avec les navigateurs qui prennent en charge JavaScript et les navigateurs qui ne le sont pas.

Si vous utilisez l’application de gestionnaire de contacts avec un navigateur qui prend en charge JavaScript, vous bénéficierez d’une meilleure expérience utilisateur. Par exemple, lorsque vous cliquez sur un groupe de contacts, seule la zone de la page qui affiche les contacts est mise à jour.

En revanche, si vous utilisez l’application du gestionnaire de contacts avec un navigateur qui ne prend pas en charge JavaScript (ou dont JavaScript est désactivé), vous bénéficiez d’une expérience utilisateur légèrement moins souhaitable. Par exemple, lorsque vous cliquez sur un groupe de contacts, la totalité de la vue d’index doit être republiée dans le navigateur pour afficher la liste de contacts correspondante.

## <a name="adding-the-required-javascript-files"></a>Ajout des fichiers JavaScript requis

Nous devrons utiliser trois fichiers JavaScript pour ajouter des fonctionnalités AJAX à notre application. Ces trois fichiers sont inclus dans le dossier scripts d’une nouvelle application ASP.NET MVC.

Si vous envisagez d’utiliser AJAX dans plusieurs pages de votre application, il est judicieux d’inclure les fichiers JavaScript requis dans la page maître de l’affichage de votre application. De cette façon, les fichiers JavaScript sont inclus automatiquement dans toutes les pages de votre application.

Ajoutez les inclusions JavaScript suivantes à l’intérieur de la balise &lt;Head&gt; de votre page maître d’affichage :

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refactorisation de la vue index pour utiliser AJAX

Commençons par modifier notre vue d’index afin que le fait de cliquer sur un groupe de contacts met uniquement à jour la région de la vue qui affiche les contacts. La zone rouge de la figure 1 contient la région que vous souhaitez mettre à jour.

[![de la mise à jour des contacts uniquement](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**Figure 01**: mise à jour des contacts uniquement ([cliquez pour afficher l’image en taille réelle](iteration-7-add-ajax-functionality-cs/_static/image2.png))

La première étape consiste à séparer la partie de la vue que vous souhaitez mettre à jour de façon asynchrone en un point de contrôle partiel distinct (View User Control). La section de la vue index qui affiche la table de contacts a été déplacée vers le partiel de la liste 1.

**Liste 1-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

Notez que la partie partielle de la liste 1 a un modèle différent de la vue index. L’attribut *Inherits* dans la &lt;directive% @ Page%&gt; spécifie que le partiel hérite de la classe de&gt; du groupe ViewUserControl&lt;.

La vue d’index mise à jour est contenue dans la liste 2.

**Liste 2-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

Il y a deux choses que vous devez noter sur la vue mise à jour de la liste 2. Tout d’abord, Notez que tout le contenu déplacé dans le partiel est remplacé par un appel à html. RenderPartial (). La méthode html. RenderPartial () est appelée lorsque la vue d’index est demandée pour la première fois afin d’afficher l’ensemble initial de contacts.

Deuxièmement, Notez que le fichier html. ActionLink () utilisé pour afficher les groupes de contacts a été remplacé par un fichier Ajax. ActionLink (). Ajax. ActionLink () est appelé avec les paramètres suivants :

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

Le premier paramètre représente le texte à afficher pour le lien, le deuxième paramètre représente les valeurs de route et le troisième paramètre représente les options Ajax. Dans ce cas, nous utilisons l’option UpdateTargetId AJAX pour pointer vers la balise HTML &lt;div&gt; que nous voulons mettre à jour une fois la requête Ajax terminée. Nous voulons mettre à jour la balise de&gt; &lt;div avec la nouvelle liste de contacts.

La méthode indexée () mise à jour du contrôleur de contact est contenue dans la liste 3.

**Liste 3-Controllers\ContactController.cs (méthode d’index)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

L’action mise à jour de l’index () retourne de manière conditionnelle l’un des deux éléments suivants : Si l’action d’index () est appelée par une demande Ajax, le contrôleur retourne une valeur partielle. Dans le cas contraire, l’action index () renvoie une vue entière.

Notez que l’action index () n’a pas besoin de retourner autant de données lorsqu’elles sont appelées par une demande Ajax. Dans le contexte d’une demande normale, l’action d’index renvoie la liste de tous les groupes de contacts et le groupe de contacts sélectionné. Dans le contexte d’une requête Ajax, l’action index () retourne uniquement le groupe sélectionné. Ajax signifie moins de travail sur votre serveur de base de données.

Notre vue d’index modifiée fonctionne dans le cas des navigateurs de niveau supérieur et inférieur. Si vous cliquez sur un groupe de contacts et que votre navigateur prend en charge JavaScript, seule la région de la vue qui contient la liste des contacts est mise à jour. Si, en revanche, votre navigateur ne prend pas en charge JavaScript, la vue entière est mise à jour.

Notre vue d’index mise à jour présente un problème. Lorsque vous cliquez sur un groupe de contacts, le groupe sélectionné n’est pas mis en surbrillance. Étant donné que la liste des groupes est affichée en dehors de la région qui est mise à jour pendant une demande Ajax, le groupe droit n’est pas mis en surbrillance. Nous allons résoudre ce problème dans la section suivante.

## <a name="adding-jquery-animation-effects"></a>Ajout d’effets d’animation jQuery

En règle générale, lorsque vous cliquez sur un lien dans une page Web, vous pouvez utiliser la barre de progression du navigateur pour détecter si le navigateur récupère activement le contenu mis à jour. En revanche, lors de l’exécution d’une requête Ajax, la barre de progression du navigateur n’affiche aucune progression. Cela peut rendre les utilisateurs nerveux. Comment savoir si le navigateur est figé ?

Il existe plusieurs façons d’indiquer à un utilisateur que le travail est effectué lors de l’exécution d’une requête Ajax. Une approche consiste à afficher une animation simple. Par exemple, vous pouvez faire disparaître en fondu une région quand une demande Ajax commence et se met en fondu dans la région lorsque la demande se termine.

Nous allons utiliser la bibliothèque jQuery qui est incluse dans le Microsoft ASP.NET Framework MVC pour créer les effets d’animation. La vue d’index mise à jour est contenue dans la liste 4.

**Liste 4-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

Notez que la vue d’index mise à jour contient trois nouvelles fonctions JavaScript. Les deux premières fonctions utilisent jQuery pour faire disparaître et disparaître en fondu dans la liste des contacts lorsque vous cliquez sur un nouveau groupe de contacts. La troisième fonction affiche un message d’erreur quand une demande Ajax génère une erreur (par exemple, le délai d’attente réseau).

La première fonction prend également en charge la mise en surbrillance du groupe sélectionné. Un attribut Class = Selected est ajouté à l’élément parent (l’élément LI) de l’élément sur lequel l’utilisateur a cliqué. De nouveau, jQuery facilite la sélection de l’élément approprié et l’ajout de la classe CSS.

Ces scripts sont liés aux liens de groupe avec l’aide du paramètre AjaxOptions Ajax. ActionLink (). L’appel de la méthode Ajax. ActionLink () mis à jour ressemble à ceci :

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Ajout de la prise en charge de l’historique du navigateur

En règle générale, lorsque vous cliquez sur un lien pour mettre à jour une page, l’historique de votre navigateur est mis à jour. De cette façon, vous pouvez cliquer sur le bouton précédent du navigateur pour revenir à l’état précédent de la page. Par exemple, si vous cliquez sur le groupe amis contact, puis sur le groupe contact professionnel, vous pouvez cliquer sur le bouton précédent du navigateur pour revenir à l’état de la page lorsque le groupe amis contact a été sélectionné.

Malheureusement, l’exécution d’une demande Ajax ne met pas à jour l’historique du navigateur automatiquement. Si vous cliquez sur un groupe de contacts et que la liste des contacts correspondants est récupérée avec une demande Ajax, l’historique du navigateur n’est pas mis à jour. Vous ne pouvez pas utiliser le bouton précédent du navigateur pour revenir à un groupe de contacts après avoir sélectionné un nouveau groupe de contacts.

Si vous souhaitez que les utilisateurs puissent utiliser le bouton précédent du navigateur après avoir exécuté des demandes Ajax, vous devez effectuer un peu plus de travail. Vous devez tirer parti de la fonctionnalité de gestion de l’historique du navigateur créée dans l’infrastructure ASP.NET AJAX.

L’historique du navigateur ASP.NET AJAX, vous devez effectuer trois opérations :

1. Activez l’historique du navigateur en affectant à la propriété enableBrowserHistory la valeur true.
2. Enregistrez des points d’historique lorsque l’état d’une vue change en appelant la méthode addHistoryPoint ().
3. Reconstruisez l’état de la vue lorsque l’événement de navigation est déclenché.

La vue d’index mise à jour est contenue dans la liste 5.

**Liste 5-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

Dans la liste 5, l’historique du navigateur est activé dans la fonction pageInit (). La fonction pageInit () est également utilisée pour configurer le gestionnaire d’événements pour l’événement Navigate. L’événement Navigate est déclenché chaque fois que le bouton suivant ou précédent du navigateur provoque la modification de l’état de la page.

La méthode beginContactList () est appelée lorsque vous cliquez sur un groupe de contacts. Cette méthode crée un point d’historique en appelant la méthode addHistoryPoint (). L’ID du groupe de contacts sur lequel vous avez cliqué est ajouté à l’historique.

L’ID de groupe est récupéré à partir d’un attribut expando sur le lien du groupe contact. Le lien est rendu avec l’appel suivant à Ajax. ActionLink ().

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

Le dernier paramètre passé à Ajax. ActionLink () ajoute un attribut expando nommé GroupID au lien (en minuscules pour la compatibilité XHTML).

Quand un utilisateur appuie sur le bouton précédent ou suivant du navigateur, l’événement Navigate est déclenché et la méthode Navigate () est appelée. Cette méthode met à jour les contacts affichés dans la page pour qu’ils correspondent à l’état de la page qui correspond au point d’historique du navigateur passé à la méthode Navigate.

## <a name="performing-ajax-deletes"></a>Exécution de suppressions Ajax

Actuellement, pour supprimer un contact, vous devez cliquer sur le lien supprimer, puis cliquer sur le bouton supprimer affiché dans la page confirmation de la suppression (voir figure 2). Cela ressemble à un grand nombre de demandes de pages pour effectuer une opération simple, par exemple la suppression d’un enregistrement de base de données.

[![la page de confirmation de la suppression](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**Figure 02**: page de confirmation de la suppression ([cliquez pour afficher l’image en taille réelle](iteration-7-add-ajax-functionality-cs/_static/image4.png))

Il est tentant d’ignorer la page de confirmation de la suppression et de supprimer un contact directement à partir de la vue index. Vous devez éviter cette tentation, car cette approche permet d’ouvrir votre application dans les brèches de sécurité. En général, vous ne souhaitez pas effectuer une opération HTTP de récupération lors de l’appel d’une action qui modifie l’état de votre application Web. Lorsque vous effectuez une suppression, vous souhaitez effectuer une opération HTTP, ou mieux encore, une opération de suppression HTTP.

Le lien de suppression est contenu dans le ContactList partiel. Une version mise à jour de la ContactList partielle est contenue dans la liste 6.

**Liste 6-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

Le lien supprimer est rendu avec l’appel suivant à la méthode Ajax. ImageActionLink () :

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax. ImageActionLink () n’est pas une partie standard de l’infrastructure MVC ASP.NET. Ajax. ImageActionLink () est une méthode d’assistance personnalisée incluse dans le projet de gestionnaire de contacts.

Le paramètre AjaxOptions a deux propriétés. Tout d’abord, la propriété Confirm est utilisée pour afficher une boîte de dialogue de confirmation JavaScript contextuelle. Deuxièmement, la propriété HttpMethod est utilisée pour effectuer une opération de suppression HTTP.

La liste 7 contient une nouvelle action AjaxDelete () qui a été ajoutée au contrôleur de contact.

**Liste 7-Controllers\ContactController.cs (AjaxDelete)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

L’action AjaxDelete () est décorée avec un attribut AcceptVerbs. Cet attribut empêche l’appel de l’action, sauf en cas d’opération HTTP autre qu’une opération HTTP DELETE. En particulier, vous ne pouvez pas appeler cette action avec un HTTP.

Une fois que vous avez supprimé un enregistrement de base de données, vous devez afficher la liste mise à jour des contacts qui ne contient pas l’enregistrement supprimé. La méthode AjaxDelete () retourne le ContactList partiel et la liste mise à jour des contacts.

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons ajouté des fonctionnalités AJAX à notre application de gestion des contacts. Nous avons utilisé AJAX pour améliorer la réactivité et les performances de notre application.

Tout d’abord, nous avons refactorisé la vue index afin que le fait de cliquer sur un groupe de contacts ne met pas à jour l’intégralité de la vue. Au lieu de cela, le fait de cliquer sur un groupe de contacts met à jour uniquement la liste des contacts.

Nous avons ensuite utilisé les effets d’animation jQuery pour faire disparaître et disparaître en fondu dans la liste des contacts. L’ajout d’une animation à une application AJAX peut être utilisé pour fournir aux utilisateurs de l’application l’équivalent d’une barre de progression du navigateur.

Nous avons également ajouté la prise en charge de l’historique du navigateur à notre application AJAX. Nous avons permis aux utilisateurs de cliquer sur les boutons précédent et suivant du navigateur pour modifier l’état de la vue index.

Enfin, nous avons créé un lien de suppression qui prend en charge les opérations HTTP DELETE. En effectuant des suppressions Ajax, nous permettons aux utilisateurs de supprimer des enregistrements de base de données sans demander à l’utilisateur de demander une page de confirmation de suppression supplémentaire.

> [!div class="step-by-step"]
> [Précédent](iteration-6-use-test-driven-development-cs.md)
> [Suivant](iteration-1-create-the-application-vb.md)
