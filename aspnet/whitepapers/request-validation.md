---
uid: whitepapers/request-validation
title: Validation des demandes-prévention des attaques de script | Microsoft Docs
author: rick-anderson
description: Ce document décrit la fonctionnalité de validation de la demande de ASP.NET où, par défaut, l’application n’est pas en mesure de traiter le contenu HTML non encodé submitt...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 807cccd6fe1acdd6359b014387abd3878840d4cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640846"
---
# <a name="request-validation---preventing-script-attacks"></a>Validation des demandes - Prévention des attaques par script

> Ce document décrit la fonctionnalité de validation de la demande de ASP.NET où, par défaut, l’application n’est pas en mesure de traiter le contenu HTML non encodé envoyé au serveur. Cette fonctionnalité de validation de requête peut être désactivée lorsque l’application a été conçue pour traiter en toute sécurité des données HTML.
> 
> S’applique à ASP.NET 1,1 et ASP.NET 2,0.

La fonctionnalité de validation des demandes, disponible dans ASP.NET depuis la version 1.1, empêche le serveur d’accepter les contenus intégrant du code HTML non encodé. Cette fonctionnalité est destinée à éviter certaines attaques par injection de script dans lesquelles un code de script client ou HTML peut être, à l’insu de tous, envoyé à un serveur, stocké, puis présenté à d’autres utilisateurs. Nous vous recommandons vivement de valider toutes les données d’entrée et de les encoder en HTML s’il y a lieu.

Par exemple, vous créez une page Web qui demande l’adresse de messagerie d’un utilisateur, puis stocke cette adresse de messagerie dans une base de données. Si l’utilisateur entre &lt;SCRIPT&gt;alerte (« Bonjour à partir du script »)&lt;/SCRIPT&gt; au lieu d’une adresse e-mail valide, lorsque ces données sont présentées, ce script peut être exécuté si le contenu n’a pas été correctement encodé. La fonctionnalité de validation de la demande de ASP.NET empêche ce problème.

## <a name="why-this-feature-is-useful"></a>Pourquoi cette fonctionnalité est utile

De nombreux sites ne savent pas qu’ils sont ouverts aux attaques par injection de script simples. Que le but de ces attaques soit de défaire le site en affichant du code HTML ou d’exécuter potentiellement le script client pour rediriger l’utilisateur vers le site d’un pirate, les attaques par injection de script sont un problème que les développeurs Web doivent faire face.

Les attaques par injection de script sont un problème de tous les développeurs Web, qu’ils utilisent ASP.NET, ASP ou d’autres technologies de développement Web.

La fonctionnalité de validation des demandes ASP.NET empêche ces attaques en n’autorisant pas le traitement du contenu HTML non encodé par le serveur, sauf si le développeur décide d’autoriser ce contenu.

## <a name="what-to-expect-error-page"></a>À quoi s’attendre : page d’erreur

La capture d’écran ci-dessous montre un exemple de code ASP.NET :

![](request-validation/_static/image1.png)

L’exécution de ce code génère une page simple qui vous permet d’entrer du texte dans la zone de texte, de cliquer sur le bouton et d’afficher le texte dans le contrôle Label :

![](request-validation/_static/image2.png)

Toutefois, il s’agissait de JavaScript, par exemple `<script>alert("hello!")</script>` à entrer et soumis, nous obtenons une exception :

![](request-validation/_static/image3.png)

Le message d’erreur indique qu’une valeur de demande de formulaire potentiellement dangereuse a été détectée et fournit des détails supplémentaires dans la description pour savoir exactement ce qui s’est produit et comment modifier le comportement. Exemple :

La validation de la demande a détecté une valeur d’entrée client potentiellement dangereuse et le traitement de la requête a été abandonné. Cette valeur peut indiquer une tentative de compromettre la sécurité de votre application, telle qu’une attaque de script entre sites. Vous pouvez désactiver la validation de la demande en définissant `validateRequest=false` dans la directive de page ou dans la section de configuration. Toutefois, il est fortement recommandé que votre application vérifie explicitement toutes les entrées dans ce cas.

## <a name="disabling-request-validation-on-a-page"></a>Désactivation de la validation de requête sur une page

Pour désactiver la validation de la demande sur une page, vous devez définir l’attribut `validateRequest` de la directive page sur `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Lorsque la validation de la demande est désactivée, le contenu peut être soumis à une page ; Il incombe au développeur de page de s’assurer que le contenu est correctement encodé ou traité.

## <a name="disabling-request-validation-for-your-application"></a>Désactivation de la validation de demande pour votre application

Pour désactiver la validation de la demande pour votre application, vous devez modifier ou créer un fichier Web. config pour votre application et définir l’attribut validateRequest de la section `<pages />` sur `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Si vous souhaitez désactiver la validation de la demande pour toutes les applications sur votre serveur, vous pouvez apporter cette modification à votre fichier machine. config.

> [!CAUTION]
> Lorsque la validation de la demande est désactivée, le contenu peut être soumis à votre application. Il incombe au développeur de l’application de s’assurer que le contenu est correctement encodé ou traité.

Le code ci-dessous est modifié pour désactiver la validation de la demande :

![](request-validation/_static/image4.png)

Maintenant, si le code JavaScript suivant a été entré dans la zone de texte `<script>alert("hello!")</script>` le résultat serait :

![](request-validation/_static/image5.png)

Pour éviter cela, si la validation de la demande est désactivée, nous devons coder le contenu en HTML.

## <a name="how-to-html-encode-content"></a>Codage HTML du contenu

Si vous avez désactivé la validation des demandes, il est recommandé d’encoder le contenu HTML qui sera stocké pour une utilisation ultérieure. L’encodage HTML remplace automatiquement tout'&lt;'ou'&gt;' (avec plusieurs autres symboles) par leur représentation HTML encodée correspondante. Par exemple, «&lt;» est remplacé par «&amp;lt ; » et «&gt;» est remplacé par «&amp;gt ; ». Les navigateurs utilisent ces codes spéciaux pour afficher le «&lt;» ou le «&gt;» dans le navigateur.

Le contenu peut être facilement encodé en HTML sur le serveur à l’aide de l’API `Server.HtmlEncode(string)`. Le contenu peut également être facilement décodé en HTML, c’est-à-dire revenir au code HTML standard à l’aide de la méthode `Server.HtmlDecode(string)`.

![](request-validation/_static/image6.png)

Ce qui donne :

![](request-validation/_static/image7.png)
