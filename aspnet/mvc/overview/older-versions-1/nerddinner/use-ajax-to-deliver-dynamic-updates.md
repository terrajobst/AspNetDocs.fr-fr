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
# <a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="42bd3-103">Utiliser AJAX pour fournir des mises à jour dynamiques</span><span class="sxs-lookup"><span data-stu-id="42bd3-103">Use AJAX to Deliver Dynamic Updates</span></span>

<span data-ttu-id="42bd3-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="42bd3-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="42bd3-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="42bd3-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="42bd3-106">Il s’agit de l’étape 10 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="42bd3-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="42bd3-107">L’étape 10 implémente la prise en charge des utilisateurs connectés pour répondre à un dîner, en utilisant une approche basée sur AJAX intégrée dans la page de détails du dîner.</span><span class="sxs-lookup"><span data-stu-id="42bd3-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="42bd3-108">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.</span><span class="sxs-lookup"><span data-stu-id="42bd3-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="42bd3-109">NerdDinner étape 10 : l’activation d’AJAX par les RSVP accepte</span><span class="sxs-lookup"><span data-stu-id="42bd3-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="42bd3-110">Nous allons maintenant implémenter la prise en charge pour les utilisateurs connectés afin de leur permettre de participer à un dîner.</span><span class="sxs-lookup"><span data-stu-id="42bd3-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="42bd3-111">Nous allons l’activer à l’aide d’une approche basée sur AJAX intégrée dans la page de détails du dîner.</span><span class="sxs-lookup"><span data-stu-id="42bd3-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="42bd3-112">Indiquant si l’utilisateur est RSVP</span><span class="sxs-lookup"><span data-stu-id="42bd3-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="42bd3-113">Les utilisateurs peuvent accéder à l’URL */dinners/Details/[ID*] pour afficher les détails d’un dîner particulier :</span><span class="sxs-lookup"><span data-stu-id="42bd3-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="42bd3-114">La méthode d’action Details () est implémentée comme suit :</span><span class="sxs-lookup"><span data-stu-id="42bd3-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="42bd3-115">La première étape de l’implémentation de la prise en charge de RSVP consiste à ajouter une méthode d’assistance « IsUserRegistered (username) » à notre objet dîner (dans la classe partielle Dinner.cs que nous avons créée précédemment).</span><span class="sxs-lookup"><span data-stu-id="42bd3-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="42bd3-116">Cette méthode d’assistance retourne la valeur true ou false selon que l’utilisateur est actuellement RSVP pour le dîner :</span><span class="sxs-lookup"><span data-stu-id="42bd3-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="42bd3-117">Nous pouvons ensuite ajouter le code suivant à notre modèle de vue Details. aspx pour afficher un message approprié indiquant si l’utilisateur est inscrit ou non pour l’événement :</span><span class="sxs-lookup"><span data-stu-id="42bd3-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="42bd3-118">Désormais, lorsqu’un utilisateur visite un dîner, il voit ce message :</span><span class="sxs-lookup"><span data-stu-id="42bd3-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="42bd3-119">Et lorsqu’ils visitent un dîner, ils n’y sont pas inscrits, le message ci-dessous s’affiche :</span><span class="sxs-lookup"><span data-stu-id="42bd3-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="42bd3-120">Implémentation de la méthode d’action Register</span><span class="sxs-lookup"><span data-stu-id="42bd3-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="42bd3-121">Nous allons maintenant ajouter les fonctionnalités nécessaires pour permettre aux utilisateurs de répondre à un dîner à partir de la page de détails.</span><span class="sxs-lookup"><span data-stu-id="42bd3-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="42bd3-122">Pour l’implémenter, nous allons créer une nouvelle classe « RSVPController » en cliquant avec le bouton droit sur le répertoire \Controllers et en choisissant la commande de menu Add-&gt;Controller.</span><span class="sxs-lookup"><span data-stu-id="42bd3-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="42bd3-123">Nous allons implémenter une méthode d’action « Register » dans la nouvelle classe RSVPController qui prend un ID pour un dîner comme argument, récupère l’objet dîner approprié, vérifie si l’utilisateur connecté figure actuellement dans la liste des utilisateurs qui se sont inscrits à ce dernier, et si n’ajoute pas d’objet RSVP pour eux :</span><span class="sxs-lookup"><span data-stu-id="42bd3-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="42bd3-124">Notez que nous revenons une chaîne simple en tant que sortie de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="42bd3-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="42bd3-125">Nous aurions pu incorporer ce message dans un modèle de vue, mais puisqu’il est tellement petit, nous allons simplement utiliser la méthode d’assistance content () sur la classe de base du contrôleur et retourner un message de type chaîne comme ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="42bd3-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="42bd3-126">Appel de la méthode d’action RSVPForEvent à l’aide d’AJAX</span><span class="sxs-lookup"><span data-stu-id="42bd3-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="42bd3-127">Nous allons utiliser AJAX pour appeler la méthode d’action Register à partir de notre vue Details.</span><span class="sxs-lookup"><span data-stu-id="42bd3-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="42bd3-128">Il est assez facile d’implémenter cela.</span><span class="sxs-lookup"><span data-stu-id="42bd3-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="42bd3-129">Tout d’abord, nous allons ajouter deux références de bibliothèque de scripts :</span><span class="sxs-lookup"><span data-stu-id="42bd3-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="42bd3-130">La première bibliothèque fait référence à la bibliothèque de scripts côté client ASP.NET AJAX principale.</span><span class="sxs-lookup"><span data-stu-id="42bd3-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="42bd3-131">Ce fichier a une taille d’environ 24k (compressée) et contient des fonctionnalités AJAX principales côté client.</span><span class="sxs-lookup"><span data-stu-id="42bd3-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="42bd3-132">La deuxième bibliothèque contient des fonctions utilitaires qui s’intègrent aux méthodes d’assistance AJAX intégrées à ASP.NET MVC (que nous allons bientôt utiliser).</span><span class="sxs-lookup"><span data-stu-id="42bd3-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="42bd3-133">Nous pouvons ensuite mettre à jour le code du modèle de vue que nous avons ajouté précédemment, de sorte qu’au lieu de sortir un message « vous n’êtes pas inscrit pour cet événement », nous rendons à la place un lien qui, lorsque Push effectue un appel AJAX qui appelle notre méthode d’action RSVPForEvent sur notre contrôleur RSVP. et RSVP l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="42bd3-133">We can then update the view template code we added earlier so that instead of outputting a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="42bd3-134">La méthode d’assistance Ajax. ActionLink () utilisée ci-dessus est intégrée à ASP.NET MVC et est semblable à la méthode d’assistance HTML. ActionLink (), mais au lieu d’effectuer une navigation standard, elle effectue un appel AJAX à la méthode d’action lorsque l’utilisateur clique sur le lien.</span><span class="sxs-lookup"><span data-stu-id="42bd3-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="42bd3-135">Au-dessus, nous appelons la méthode d’action « Register » sur le contrôleur « RSVP » et transmettons le DinnerID en tant que paramètre « ID ».</span><span class="sxs-lookup"><span data-stu-id="42bd3-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="42bd3-136">Le paramètre AjaxOptions final que nous passons indique que nous souhaitons prendre le contenu renvoyé par la méthode d’action et mettre à jour l’élément HTML &lt;div&gt; sur la page dont l’ID est « rsvpmsg ».</span><span class="sxs-lookup"><span data-stu-id="42bd3-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="42bd3-137">Et maintenant, lorsqu’un utilisateur accède à un dîner, il n’est pas encore inscrit à un lien vers le service RSVP :</span><span class="sxs-lookup"><span data-stu-id="42bd3-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="42bd3-138">S’ils cliquent sur le lien « RSVP pour cet événement », ils effectuent un appel AJAX à la méthode d’action Register sur le contrôleur RSVP, et lorsqu’ils se terminent, ils voient un message mis à jour comme ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="42bd3-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="42bd3-139">La bande passante réseau et le trafic impliqués lors de l’exécution de cet appel AJAX sont vraiment légers.</span><span class="sxs-lookup"><span data-stu-id="42bd3-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="42bd3-140">Quand l’utilisateur clique sur le lien « RSVP pour cet événement », une petite requête de réseau HTTP POSTALe est transmise à l’URL */dinners/Register/1* qui ressemble à l’exemple ci-dessous sur le réseau :</span><span class="sxs-lookup"><span data-stu-id="42bd3-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="42bd3-141">Et la réponse de notre méthode d’action Register est tout simplement :</span><span class="sxs-lookup"><span data-stu-id="42bd3-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="42bd3-142">Cet appel léger est rapide et fonctionnera même sur un réseau lent.</span><span class="sxs-lookup"><span data-stu-id="42bd3-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="42bd3-143">Ajout d’une animation jQuery</span><span class="sxs-lookup"><span data-stu-id="42bd3-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="42bd3-144">Les fonctionnalités AJAX que nous avons implémentées fonctionnent correctement et rapidement.</span><span class="sxs-lookup"><span data-stu-id="42bd3-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="42bd3-145">Parfois, cela peut se produire si rapide, mais qu’un utilisateur ne peut pas remarquer que le lien RSVP a été remplacé par un nouveau texte.</span><span class="sxs-lookup"><span data-stu-id="42bd3-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="42bd3-146">Pour que le résultat soit un peu plus évident, nous pouvons ajouter une animation simple pour attirer l’attention sur le message de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="42bd3-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="42bd3-147">Le modèle de projet ASP.NET MVC par défaut comprend jQuery – une excellente bibliothèque JavaScript open source qui est également prise en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="42bd3-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="42bd3-148">jQuery fournit un certain nombre de fonctionnalités, notamment une bibliothèque de sélection et d’effets de modèle DOM HTML.</span><span class="sxs-lookup"><span data-stu-id="42bd3-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="42bd3-149">Pour utiliser jQuery, nous allons d’abord y ajouter une référence de script.</span><span class="sxs-lookup"><span data-stu-id="42bd3-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="42bd3-150">Étant donné que nous allons utiliser jQuery dans divers emplacements de notre site, nous allons ajouter la référence de script dans notre fichier de page maître site. Master afin que toutes les pages puissent l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="42bd3-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="42bd3-151">*Conseil : Assurez-vous que vous avez installé le correctif JavaScript IntelliSense pour VS 2008 SP1 qui permet une prise en charge IntelliSense plus riche pour les fichiers JavaScript (y compris jQuery). Vous pouvez le télécharger à partir de : http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="42bd3-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="42bd3-152">Le code écrit à l’aide de JQuery utilise souvent une méthode JavaScript « $ () » globale qui extrait un ou plusieurs éléments HTML à l’aide d’un sélecteur CSS.</span><span class="sxs-lookup"><span data-stu-id="42bd3-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="42bd3-153">Par exemple, *$ (« #rsvpmsg »)* sélectionne tout élément HTML avec l’ID rsvpmsg, tandis que *$ (« . quoi »)* sélectionne tous les éléments avec le nom de la classe CSS « quelque élément ».</span><span class="sxs-lookup"><span data-stu-id="42bd3-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="42bd3-154">Vous pouvez également écrire des requêtes plus avancées telles que « retourner toutes les cases d’option cochées » à l’aide d’une requête de sélecteur comme : *$ (« entrée [@type= radio] [@checked] »)* .</span><span class="sxs-lookup"><span data-stu-id="42bd3-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="42bd3-155">Une fois que vous avez sélectionné des éléments, vous pouvez appeler des méthodes pour effectuer une action, par exemple les masquer : *$ ("#rsvpmsg"). Hide ();*</span><span class="sxs-lookup"><span data-stu-id="42bd3-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="42bd3-156">Pour notre scénario RSVP, nous allons définir une fonction JavaScript simple nommée « AnimateRSVPMessage » qui sélectionne la&gt; « rsvpmsg » &lt;div et anime la taille de son contenu de texte.</span><span class="sxs-lookup"><span data-stu-id="42bd3-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="42bd3-157">Le code ci-dessous démarre la petite taille du texte, puis entraîne une augmentation du délai de 400 millisecondes :</span><span class="sxs-lookup"><span data-stu-id="42bd3-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="42bd3-158">Nous pouvons ensuite associer cette fonction JavaScript pour qu’elle soit appelée une fois que notre appel AJAX se termine avec succès en transmettant son nom à notre méthode d’assistance Ajax. ActionLink () (via la propriété d’événement AjaxOptions "OnSuccess") :</span><span class="sxs-lookup"><span data-stu-id="42bd3-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="42bd3-159">Et maintenant, lorsque l’utilisateur clique sur le lien « RSVP pour cet événement » et que notre appel AJAX se termine correctement, le message de contenu renvoyé s’anime et augmente en taille :</span><span class="sxs-lookup"><span data-stu-id="42bd3-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="42bd3-160">En plus de fournir un événement « OnSuccess », l’objet AjaxOptions expose les événements OnBegin, OnFailure et OnComplete que vous pouvez gérer (ainsi que diverses autres propriétés et options utiles).</span><span class="sxs-lookup"><span data-stu-id="42bd3-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="42bd3-161">Nettoyage-refactorisation d’une vue partielle RSVP</span><span class="sxs-lookup"><span data-stu-id="42bd3-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="42bd3-162">Notre modèle de vue Détails commence à être un peu long, ce qui rendra un peu plus difficile à comprendre.</span><span class="sxs-lookup"><span data-stu-id="42bd3-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="42bd3-163">Pour aider à améliorer la lisibilité du code, commençons par créer une vue partielle – RSVPStatus. ascx, qui encapsule tout le code d’affichage RSVP pour notre page de détails.</span><span class="sxs-lookup"><span data-stu-id="42bd3-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="42bd3-164">Pour ce faire, cliquez avec le bouton droit sur le dossier \Views\Dinners, puis choisissez la commande de menu Ajouter-&gt;afficher.</span><span class="sxs-lookup"><span data-stu-id="42bd3-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="42bd3-165">Nous aurons besoin d’un objet dîner en tant que ViewModel fortement typé.</span><span class="sxs-lookup"><span data-stu-id="42bd3-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="42bd3-166">Nous pouvons ensuite copier/coller le contenu RSVP de la vue Details. aspx dans celui-ci.</span><span class="sxs-lookup"><span data-stu-id="42bd3-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="42bd3-167">Une fois que nous avons terminé, créons également une autre vue partielle, EditAndDeleteLinks. ascx, qui encapsule notre code d’affichage de lien de modification et de suppression.</span><span class="sxs-lookup"><span data-stu-id="42bd3-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="42bd3-168">Nous aurons également besoin d’un objet dîner en tant que ViewModel fortement typé, et copiez/collez la logique de modification et de suppression de la vue Details. aspx dans celui-ci.</span><span class="sxs-lookup"><span data-stu-id="42bd3-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="42bd3-169">Notre modèle de vue détails peut ensuite inclure deux appels de méthode html. RenderPartial () en bas :</span><span class="sxs-lookup"><span data-stu-id="42bd3-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="42bd3-170">Cela rend le nettoyeur de code à lire et à gérer.</span><span class="sxs-lookup"><span data-stu-id="42bd3-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="42bd3-171">Étape suivante</span><span class="sxs-lookup"><span data-stu-id="42bd3-171">Next Step</span></span>

<span data-ttu-id="42bd3-172">Voyons maintenant comment nous pouvons utiliser AJAX encore plus loin et ajouter une prise en charge interactive du mappage à notre application.</span><span class="sxs-lookup"><span data-stu-id="42bd3-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="42bd3-173">[Précédent](secure-applications-using-authentication-and-authorization.md)
> [Suivant](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="42bd3-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
