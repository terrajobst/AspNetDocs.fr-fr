---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Prévention des attaques par injection de code JavaScript (VB) | Microsoft Docs
author: StephenWalther
description: Empêchez les attaques par injection de code JavaScript et les attaques de script entre sites. Dans ce didacticiel, Stephen Walther explique comment vous pouvez facilement...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: dfe09085f26c62c566649bc6f570aa25367a0f07
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594721"
---
# <a name="preventing-javascript-injection-attacks-vb"></a>Prévention des attaques par injection de code JavaScript (VB)

par [Stephen Walther](https://github.com/StephenWalther)

[Télécharger PDF](https://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Empêchez les attaques par injection de code JavaScript et les attaques de script entre sites. Dans ce didacticiel, Stephen Walther explique comment vous pouvez facilement contrecarrer ces types d’attaques en encodant votre contenu.

L’objectif de ce didacticiel est de vous expliquer comment vous pouvez empêcher les attaques par injection de code JavaScript dans vos applications MVC ASP.NET. Ce didacticiel présente deux approches de la protection de votre site Web contre une attaque par injection de code JavaScript. Vous allez apprendre à empêcher les attaques par injection de code JavaScript en encodant les données que vous affichez. Vous apprendrez également comment empêcher les attaques par injection de code JavaScript en encodant les données que vous acceptez.

## <a name="what-is-a-javascript-injection-attack"></a>Qu’est-ce qu’une attaque par injection de code JavaScript ?

Chaque fois que vous acceptez l’entrée de l’utilisateur et réaffichez l’entrée utilisateur, vous ouvrez votre site Web pour les attaques par injection de code JavaScript. Examinons une application concrète ouverte aux attaques par injection de code JavaScript.

Imaginez que vous avez créé un site Web de commentaires client (voir la figure 1). Les clients peuvent visiter le site Web et entrer des commentaires sur leur expérience à l’aide de vos produits. Lorsqu’un client envoie ses commentaires, les commentaires sont réaffichés sur la page de commentaires.

[![le site Web Commentaires client](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Figure 01**: site Web de commentaires des clients ([cliquez pour afficher l’image en taille réelle](preventing-javascript-injection-attacks-vb/_static/image3.png))

Le site Web Commentaires client utilise le `controller` de la liste 1. Cette `controller` contient deux actions nommées `Index()` et `Create()`.

**Liste 1 – `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

La méthode `Index()` affiche la vue de `Index`. Cette méthode transmet toutes les commentaires des clients précédents à la vue `Index` en extrayant les commentaires de la base de données (à l’aide d’une requête LINQ to SQL).

La méthode `Create()` crée un nouvel élément de commentaires et l’ajoute à la base de données. Le message que le client entre dans le formulaire est passé à la méthode `Create()` dans le paramètre message. Un élément de commentaires est créé et le message est affecté à la propriété de `Message` de l’élément de commentaires. L’élément de commentaires est soumis à la base de données avec l’appel de la méthode `DataContext.SubmitChanges()`. Enfin, le visiteur est redirigé vers la vue `Index` où tous les commentaires s’affichent.

La vue de `Index` est contenue dans la liste 2.

**Liste 2 – `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

La vue `Index` comporte deux sections. La section supérieure contient le formulaire de commentaires client réel. La section inférieure contient une pour.. Chaque boucle qui parcourt tous les éléments de commentaires client précédents et affiche les propriétés de EntryDate et de message pour chaque élément de commentaires.

Le site Web Commentaires client est un site Web simple. Malheureusement, le site Web est ouvert aux attaques par injection de code JavaScript.

Imaginez que vous entrez le texte suivant dans le formulaire de commentaires des clients :

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Ce texte représente un script JavaScript qui affiche une boîte de message d’alerte. Une fois que quelqu’un a envoyé ce script dans le formulaire de commentaires, le message <em>Boo !</em> s’affiche chaque fois que quelqu’un visite le site Web de commentaires des clients à l’avenir (voir figure 2).

[Injection de code JavaScript ![](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Figure 02**: injection JavaScript ([cliquez pour afficher l’image en taille réelle](preventing-javascript-injection-attacks-vb/_static/image6.png))

À présent, votre réponse initiale aux attaques par injection de code JavaScript peut être Apathy. Vous pensez peut-être que les attaques par injection de code JavaScript sont simplement un type d’attaque par *dégradation* . Vous pensez peut-être que personne ne peut faire quoi que ce soit véritablement mal, en validant une attaque par injection de code JavaScript.

Malheureusement, un pirate peut faire des choses vraiment malveillantes en injectant du code JavaScript dans un site Web. Vous pouvez utiliser une attaque par injection de code JavaScript pour effectuer une attaque XSS (cross-site scripting). Dans une attaque par script intersite, vous dérobez les informations confidentielles de l’utilisateur et envoyez les informations à un autre site Web.

Par exemple, un pirate peut utiliser une attaque par injection de code JavaScript pour voler les valeurs des cookies des navigateurs d’autres utilisateurs. Si des informations sensibles (telles que des mots de passe, des numéros de carte de crédit ou des numéros de sécurité sociale) sont stockées dans les cookies du navigateur, un pirate peut utiliser une attaque par injection de code JavaScript pour voler ces informations. Ou bien, si un utilisateur entre des informations sensibles dans un champ de formulaire contenu dans une page qui a été compromise par une attaque JavaScript, le pirate peut utiliser le code JavaScript injecté pour récupérer les données de formulaire et les envoyer à un autre site Web.

*Veuillez effrayé*. Effectuez des attaques par injection de code JavaScript sérieusement et protégez les informations confidentielles de votre utilisateur. Dans les deux sections suivantes, nous aborderons deux techniques que vous pouvez utiliser pour défendre vos applications ASP.NET MVC contre les attaques par injection de code JavaScript.

## <a name="approach-1-html-encode-in-the-view"></a>Approche #1 : codage HTML dans la vue

Une méthode simple pour empêcher les attaques par injection de code JavaScript consiste à encoder en HTML toutes les données entrées par les utilisateurs du site Web lorsque vous réaffichez les données dans une vue. La vue mise à jour `Index` de la liste 3 suit cette approche.

**Liste 3 – `Index.aspx` (encodée en HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Notez que la valeur de `feedback.Message` est encodée en HTML avant que la valeur ne soit affichée avec le code suivant :

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

Qu’est-ce que cela signifie pour encoder une chaîne au format HTML ? Quand vous encodez une chaîne en HTML, les caractères dangereux tels que `<` et `>` sont remplacés par des références d’entité HTML telles que `&lt;` et `&gt;`. Ainsi, lorsque la chaîne `<script>alert("Boo!")</script>` est encodée en HTML, elle est convertie en `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. La chaîne encodée ne s’exécute plus en tant que script JavaScript lorsqu’elle est interprétée par un navigateur. Au lieu de cela, vous accédez à la page sans danger de la figure 3.

[Attaque JavaScript de ![invalidée](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Figure 03**: attaque JavaScript invalidée ([cliquez pour afficher l’image en taille réelle](preventing-javascript-injection-attacks-vb/_static/image9.png))

Notez que dans la vue `Index` de la liste 3, seule la valeur de `feedback.Message` est encodée. La valeur de `feedback.EntryDate` n’est pas encodée. Il vous suffit d’encoder les données entrées par un utilisateur. Étant donné que la valeur de EntryDate a été générée dans le contrôleur, vous n’avez pas besoin d’encoder cette valeur en HTML.

## <a name="approach-2-html-encode-in-the-controller"></a>Approche #2 : codage HTML dans le contrôleur

Au lieu d’encodage HTML des données lorsque vous affichez les données dans une vue, vous pouvez encoder les données au format HTML juste avant d’envoyer les données à la base de données. Cette deuxième approche est prise dans le cas de la `controller` dans la liste 4.

**Liste 4 – `HomeController.cs` (encodée en HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Notez que la valeur du message est encodée en HTML avant que la valeur ne soit envoyée à la base de données au sein de l’action `Create()`. Lorsque le message est réaffiché dans la vue, le message est encodé au format HTML et tout JavaScript injecté dans le message n’est pas exécuté.

En règle générale, vous devez privilégier la première approche décrite dans ce didacticiel sur cette seconde approche. Le problème de cette deuxième approche est que vous vous retrouvez avec des données encodées en HTML dans votre base de données. En d’autres termes, les données de votre base de données sont modifiées avec des caractères drôles.

Pourquoi est-ce mauvais ? Si vous avez besoin d’afficher les données de la base de données dans une autre chose qu’une page Web, vous rencontrerez des problèmes. Par exemple, vous ne pouvez plus afficher facilement les données dans une application Windows Forms.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel était de vous effrayer le potentiel d’une attaque par injection de code JavaScript. Ce didacticiel a présenté deux approches pour la protection de vos applications ASP.NET MVC contre les attaques par injection de code JavaScript : vous pouvez encoder les données envoyées par l’utilisateur dans la vue ou vous pouvez coder en HTML les données soumises par l’utilisateur dans le contrôleur.

> [!div class="step-by-step"]
> [Précédent](authenticating-users-with-windows-authentication-vb.md)
