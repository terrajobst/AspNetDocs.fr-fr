---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Utiliser AJAX pour fournir des mises à jour dynamiques | Microsoft Docs
author: microsoft
description: L’étape 10 met en œuvre la prise en charge des utilisateurs connectés pour répondre à un dîner, en utilisant une approche basée sur AJAX intégrée dans les détails du dîner...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 3edc02fec546609505b5e085440fa684abe7acd0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600848"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a>Utiliser AJAX pour fournir des mises à jour dynamiques

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 10 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.
> 
> L’étape 10 implémente la prise en charge des utilisateurs connectés pour répondre à un dîner, en utilisant une approche basée sur AJAX intégrée dans la page de détails du dîner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner étape 10 : l’activation d’AJAX par les RSVP accepte

Nous allons maintenant implémenter la prise en charge pour les utilisateurs connectés afin de leur permettre de participer à un dîner. Nous allons l’activer à l’aide d’une approche basée sur AJAX intégrée dans la page de détails du dîner.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Indiquant si l’utilisateur est RSVP

Les utilisateurs peuvent accéder à l’URL */dinners/Details/[ID*] pour afficher les détails d’un dîner particulier :

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

La méthode d’action Details () est implémentée comme suit :

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

La première étape de l’implémentation de la prise en charge de RSVP consiste à ajouter une méthode d’assistance « IsUserRegistered (username) » à notre objet dîner (dans la classe partielle Dinner.cs que nous avons créée précédemment). Cette méthode d’assistance retourne la valeur true ou false selon que l’utilisateur est actuellement RSVP pour le dîner :

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Nous pouvons ensuite ajouter le code suivant à notre modèle de vue Details. aspx pour afficher un message approprié indiquant si l’utilisateur est inscrit ou non pour l’événement :

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Désormais, lorsqu’un utilisateur visite un dîner, il voit ce message :

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Et lorsqu’ils visitent un dîner, ils n’y sont pas inscrits, le message ci-dessous s’affiche :

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implémentation de la méthode d’action Register

Nous allons maintenant ajouter les fonctionnalités nécessaires pour permettre aux utilisateurs de répondre à un dîner à partir de la page de détails.

Pour l’implémenter, nous allons créer une nouvelle classe « RSVPController » en cliquant avec le bouton droit sur le répertoire \Controllers et en choisissant la commande de menu Add-&gt;Controller.

Nous allons implémenter une méthode d’action « Register » dans la nouvelle classe RSVPController qui prend un ID pour un dîner comme argument, récupère l’objet dîner approprié, vérifie si l’utilisateur connecté figure actuellement dans la liste des utilisateurs qui se sont inscrits à ce dernier, et si n’ajoute pas d’objet RSVP pour eux :

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Notez que nous revenons une chaîne simple en tant que sortie de la méthode d’action. Nous aurions pu incorporer ce message dans un modèle de vue, mais puisqu’il est tellement petit, nous allons simplement utiliser la méthode d’assistance content () sur la classe de base du contrôleur et retourner un message de type chaîne comme ci-dessus.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Appel de la méthode d’action RSVPForEvent à l’aide d’AJAX

Nous allons utiliser AJAX pour appeler la méthode d’action Register à partir de notre vue Details. Il est assez facile d’implémenter cela. Tout d’abord, nous allons ajouter deux références de bibliothèque de scripts :

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

La première bibliothèque fait référence à la bibliothèque de scripts côté client ASP.NET AJAX principale. Ce fichier a une taille d’environ 24k (compressée) et contient des fonctionnalités AJAX principales côté client. La deuxième bibliothèque contient des fonctions utilitaires qui s’intègrent aux méthodes d’assistance AJAX intégrées à ASP.NET MVC (que nous allons bientôt utiliser).

Nous pouvons ensuite mettre à jour le code du modèle de vue que nous avons ajouté précédemment, de sorte qu’au lieu de sortir un message « vous n’êtes pas inscrit pour cet événement », nous rendons à la place un lien qui, lorsque Push effectue un appel AJAX qui appelle notre méthode d’action RSVPForEvent sur notre contrôleur RSVP. et RSVP l’utilisateur :

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

La méthode d’assistance Ajax. ActionLink () utilisée ci-dessus est intégrée à ASP.NET MVC et est semblable à la méthode d’assistance HTML. ActionLink (), mais au lieu d’effectuer une navigation standard, elle effectue un appel AJAX à la méthode d’action lorsque l’utilisateur clique sur le lien. Au-dessus, nous appelons la méthode d’action « Register » sur le contrôleur « RSVP » et transmettons le DinnerID en tant que paramètre « ID ». Le paramètre AjaxOptions final que nous passons indique que nous souhaitons prendre le contenu renvoyé par la méthode d’action et mettre à jour l’élément HTML &lt;div&gt; sur la page dont l’ID est « rsvpmsg ».

Et maintenant, lorsqu’un utilisateur accède à un dîner, il n’est pas encore inscrit à un lien vers le service RSVP :

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

S’ils cliquent sur le lien « RSVP pour cet événement », ils effectuent un appel AJAX à la méthode d’action Register sur le contrôleur RSVP, et lorsqu’ils se terminent, ils voient un message mis à jour comme ci-dessous :

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

La bande passante réseau et le trafic impliqués lors de l’exécution de cet appel AJAX sont vraiment légers. Quand l’utilisateur clique sur le lien « RSVP pour cet événement », une petite requête de réseau HTTP POSTALe est transmise à l’URL */dinners/Register/1* qui ressemble à l’exemple ci-dessous sur le réseau :

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

Et la réponse de notre méthode d’action Register est tout simplement :

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Cet appel léger est rapide et fonctionnera même sur un réseau lent.

### <a name="adding-a-jquery-animation"></a>Ajout d’une animation jQuery

Les fonctionnalités AJAX que nous avons implémentées fonctionnent correctement et rapidement. Parfois, cela peut se produire si rapide, mais qu’un utilisateur ne peut pas remarquer que le lien RSVP a été remplacé par un nouveau texte. Pour que le résultat soit un peu plus évident, nous pouvons ajouter une animation simple pour attirer l’attention sur le message de mise à jour.

Le modèle de projet ASP.NET MVC par défaut comprend jQuery – une excellente bibliothèque JavaScript open source qui est également prise en charge par Microsoft. jQuery fournit un certain nombre de fonctionnalités, notamment une bibliothèque de sélection et d’effets de modèle DOM HTML.

Pour utiliser jQuery, nous allons d’abord y ajouter une référence de script. Étant donné que nous allons utiliser jQuery dans divers emplacements de notre site, nous allons ajouter la référence de script dans notre fichier de page maître site. Master afin que toutes les pages puissent l’utiliser.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Conseil : Assurez-vous que vous avez installé le correctif JavaScript IntelliSense pour VS 2008 SP1 qui permet une prise en charge IntelliSense plus riche pour les fichiers JavaScript (y compris jQuery). Vous pouvez le télécharger à partir de : http://tinyurl.com/vs2008javascripthotfix*

Le code écrit à l’aide de JQuery utilise souvent une méthode JavaScript « $ () » globale qui extrait un ou plusieurs éléments HTML à l’aide d’un sélecteur CSS. Par exemple, *$ (« #rsvpmsg »)* sélectionne tout élément HTML avec l’ID rsvpmsg, tandis que *$ (« . quoi »)* sélectionne tous les éléments avec le nom de la classe CSS « quelque élément ». Vous pouvez également écrire des requêtes plus avancées telles que « retourner toutes les cases d’option cochées » à l’aide d’une requête de sélecteur comme : *$ (« entrée [@type= radio] [@checked] »)* .

Une fois que vous avez sélectionné des éléments, vous pouvez appeler des méthodes pour effectuer une action, par exemple les masquer : *$ ("#rsvpmsg"). Hide ();*

Pour notre scénario RSVP, nous allons définir une fonction JavaScript simple nommée « AnimateRSVPMessage » qui sélectionne la&gt; « rsvpmsg » &lt;div et anime la taille de son contenu de texte. Le code ci-dessous démarre la petite taille du texte, puis entraîne une augmentation du délai de 400 millisecondes :

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Nous pouvons ensuite associer cette fonction JavaScript pour qu’elle soit appelée une fois que notre appel AJAX se termine avec succès en transmettant son nom à notre méthode d’assistance Ajax. ActionLink () (via la propriété d’événement AjaxOptions "OnSuccess") :

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

Et maintenant, lorsque l’utilisateur clique sur le lien « RSVP pour cet événement » et que notre appel AJAX se termine correctement, le message de contenu renvoyé s’anime et augmente en taille :

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

En plus de fournir un événement « OnSuccess », l’objet AjaxOptions expose les événements OnBegin, OnFailure et OnComplete que vous pouvez gérer (ainsi que diverses autres propriétés et options utiles).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Nettoyage-refactorisation d’une vue partielle RSVP

Notre modèle de vue Détails commence à être un peu long, ce qui rendra un peu plus difficile à comprendre. Pour aider à améliorer la lisibilité du code, commençons par créer une vue partielle – RSVPStatus. ascx, qui encapsule tout le code d’affichage RSVP pour notre page de détails.

Pour ce faire, cliquez avec le bouton droit sur le dossier \Views\Dinners, puis choisissez la commande de menu Ajouter-&gt;afficher. Nous aurons besoin d’un objet dîner en tant que ViewModel fortement typé. Nous pouvons ensuite copier/coller le contenu RSVP de la vue Details. aspx dans celui-ci.

Une fois que nous avons terminé, créons également une autre vue partielle, EditAndDeleteLinks. ascx, qui encapsule notre code d’affichage de lien de modification et de suppression. Nous aurons également besoin d’un objet dîner en tant que ViewModel fortement typé, et copiez/collez la logique de modification et de suppression de la vue Details. aspx dans celui-ci.

Notre modèle de vue détails peut ensuite inclure deux appels de méthode html. RenderPartial () en bas :

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Cela rend le nettoyeur de code à lire et à gérer.

### <a name="next-step"></a>Étape suivante

Voyons maintenant comment nous pouvons utiliser AJAX encore plus loin et ajouter une prise en charge interactive du mappage à notre application.

> [!div class="step-by-step"]
> [Précédent](secure-applications-using-authentication-and-authorization.md)
> [Suivant](use-ajax-to-implement-mapping-scenarios.md)
