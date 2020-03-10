---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Empêcher les attaques par redirection ouverteC#() | Microsoft Docs
author: jongalloway
description: Ce didacticiel explique comment empêcher les attaques par redirection ouverte dans vos applications ASP.NET MVC. Ce didacticiel présente les modifications apportées...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: cfa635d4fd14d031993c5b452325cbe334f82dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538170"
---
# <a name="preventing-open-redirection-attacks-c"></a>Blocage des attaques par redirection ouverte (C#)

par [Jon Galloway](https://github.com/jongalloway)

> Ce didacticiel explique comment empêcher les attaques par redirection ouverte dans vos applications ASP.NET MVC. Ce didacticiel décrit les modifications apportées à AccountController dans ASP.NET MVC 3 et montre comment vous pouvez appliquer ces modifications dans vos applications ASP.NET MVC 1,0 et 2 existantes.

## <a name="what-is-an-open-redirection-attack"></a>Qu’est-ce qu’une attaque de redirection ouverte ?

Toute application Web qui effectue une redirection vers une URL spécifiée via la requête telle que QueryString ou des données de formulaire peut potentiellement être falsifiée pour rediriger les utilisateurs vers une URL externe et malveillante. Cette manipulation est appelée une attaque de redirection ouverte.

Chaque fois que votre logique d’application redirige vers une URL spécifiée, vous devez vérifier que l’URL de redirection n’a pas été falsifiée. La connexion utilisée dans le AccountController par défaut pour ASP.NET MVC 1,0 et ASP.NET MVC 2 est vulnérable aux attaques de redirection ouvertes. Heureusement, il est facile de mettre à jour vos applications existantes pour utiliser les corrections de la version préliminaire de ASP.NET MVC 3.

Pour comprendre la vulnérabilité, voyons comment la redirection de connexion fonctionne dans un projet d’application Web ASP.NET MVC 2 par défaut. Dans cette application, toute tentative de visite d’une action de contrôleur qui a l’attribut [Authorize] redirige les utilisateurs non autorisés vers la vue/Account/LogOn. Cette redirection vers/Account/LogOn inclut un paramètre returnUrl QueryString afin que l’utilisateur puisse être renvoyé à l’URL demandée à l’origine une fois qu’il a réussi à se connecter.

Dans la capture d’écran ci-dessous, nous voyons qu’une tentative d’accès à la vue/Account/ChangePassword lorsqu’elle n’est pas connectée entraîne une redirection vers/Account/LogOn. ReturnUrl =% 2fAccount% 2fChangePassword% 2F.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Figure 01**: page de connexion avec une redirection ouverte

Étant donné que le paramètre ReturnUrl querystring n’est pas validé, une personne malveillante peut la modifier pour injecter une adresse URL dans le paramètre pour effectuer une attaque de redirection ouverte. Pour illustrer cela, nous pouvons modifier le paramètre ReturnUrl en [http://bing.com](http://bing.com), de sorte que l’URL de connexion résultante sera/Account/Logon ? ReturnUrl =<http://www.bing.com/>. Une fois connecté au site, nous sommes redirigés vers [http://bing.com](http://bing.com). Étant donné que cette redirection n’est pas validée, elle peut pointer vers un site malveillant qui tente de tromper l’utilisateur.

### <a name="a-more-complex-open-redirection-attack"></a>Une attaque de redirection ouverte plus complexe

Les attaques de redirection ouverte sont particulièrement dangereuses, car une personne malveillante sait que nous essayons de se connecter à un site Web spécifique, ce qui nous rend vulnérable à une [attaque par hameçonnage](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Par exemple, une personne malveillante peut envoyer des e-mails malveillants à des utilisateurs de sites Web en tentant de capturer leur mot de passe. Voyons comment cela fonctionnerait sur le site NerdDinner. (Notez que le site Live NerdDinner a été mis à jour pour vous protéger contre les attaques de redirection ouvertes.)

Tout d’abord, un attaquant nous envoie un lien vers la page de connexion sur NerdDinner qui comprend une redirection vers sa page falsifiée :

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Notez que l’URL de retour pointe vers nerddiner.com, ce qui manque un « n » dans le dîner de mots. Dans cet exemple, il s’agit d’un domaine que l’attaquant contrôle. Lorsque nous allons accéder au lien ci-dessus, nous sommes dirigés vers la page de connexion NerdDinner.com légitime.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Figure 02**: page de connexion NerdDinner avec une redirection ouverte

Lorsque nous nous connectons correctement, l’action d’ouverture de session du AccountController MVC ASP.NET redirige vers l’URL spécifiée dans le paramètre returnUrl QueryString. Dans ce cas, il s’agit de l’URL entrée par l’attaquant, [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn). À moins d’être extrêmement Watchful, il est très probable que nous ne le remarquons pas, en particulier parce que l’attaquant a veillé à s’assurer que sa page falsifiée ressemble exactement à la page de connexion légitime. Cette page de connexion contient un message d’erreur vous demandant de vous reconnecter. Nous devons nous faire part de votre mot de passe.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Figure 03**: écran de connexion NerdDinner falsifié

Lorsque nous revenons de nouveau le nom d’utilisateur et le mot de passe, la page de connexion falsifiée enregistre les informations et nous renvoie au site NerdDinner.com légitime. À ce stade, le site NerdDinner.com a déjà été authentifié et la page de connexion falsifiée peut donc se rediriger directement vers cette page. Le résultat final est que l’attaquant a le nom d’utilisateur et le mot de passe, et que nous n’avons pas conscience que nous l’avons fourni.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Examen du code vulnérable dans l’action d’ouverture de session AccountController

Le code de l’action d’ouverture de session dans une application ASP.NET MVC 2 est présenté ci-dessous. Notez que lors d’une connexion réussie, le contrôleur retourne une redirection vers returnUrl. Vous pouvez voir qu’aucune validation n’est effectuée par rapport au paramètre returnUrl.

**Liste 1 – action d’ouverture de session ASP.NET MVC 2 dans `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Examinons maintenant les modifications apportées à l’action d’ouverture de session ASP.NET MVC 3. Ce code a été modifié pour valider le paramètre returnUrl en appelant une nouvelle méthode dans la classe d’assistance System. Web. Mvc. URL nommée `IsLocalUrl()`.

**Liste 2 : action d’ouverture de session ASP.NET MVC 3 dans `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Il a été modifié pour valider le paramètre URL de renvoi en appelant une nouvelle méthode dans la classe d’assistance System. Web. Mvc. URL, `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Protection de vos applications ASP.NET MVC 1,0 et MVC 2

Nous pouvons tirer parti des modifications apportées à ASP.NET MVC 3 dans nos applications ASP.NET MVC 1,0 et 2 existantes en ajoutant la méthode d’assistance IsLocalUrl () et en mettant à jour l’action d’ouverture de session pour valider le paramètre returnUrl.

La méthode UrlHelper IsLocalUrl () appelle en fait simplement une méthode dans System. Web. webpages, car cette validation est également utilisée par les applications pages Web ASP.NET.

**Liste 3 : méthode IsLocalUrl () à partir de la `class` ASP.NET MVC 3 UrlHelper**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

La méthode IsUrlLocalToHost contient la logique de validation réelle, comme indiqué dans la liste 4.

**Liste 4 – méthode IsUrlLocalToHost () de la classe System. Web. webpages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

Dans notre application ASP.NET MVC 1,0 ou 2, nous allons ajouter une méthode IsLocalUrl () au AccountController, mais il est recommandé de l’ajouter à une classe d’assistance distincte si possible. Nous allons apporter deux petites modifications à la version ASP.NET MVC 3 de IsLocalUrl () afin qu’elle fonctionne dans le AccountController. Tout d’abord, nous allons passer d’une méthode publique à une méthode privée, car les méthodes publiques dans les contrôleurs sont accessibles en tant qu’actions de contrôleur. Deuxièmement, nous allons modifier l’appel qui vérifie l’hôte d’URL par rapport à l’hôte d’application. Cet appel utilise un champ RequestContext local dans la classe UrlHelper. Au lieu d’utiliser ce. RequestContext. HttpContext. Request. URL. Host, nous allons l’utiliser. Request. URL. Host. Le code suivant illustre la méthode IsLocalUrl () modifiée pour une utilisation avec une classe de contrôleur dans les applications ASP.NET MVC 1,0 et 2.

**Liste 5 – méthode IsLocalUrl (), qui est modifiée pour être utilisée avec une classe de contrôleur MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Maintenant que la méthode IsLocalUrl () est en place, nous pouvons l’appeler à partir de notre action d’ouverture de session pour valider le paramètre returnUrl, comme indiqué dans le code suivant.

**Liste 6-méthode de connexion mise à jour qui valide le paramètre returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Nous pouvons maintenant tester une attaque de redirection ouverte en tentant de se connecter à l’aide d’une URL de retour externe. Utilisons/Account/LogOn ? ReturnUrl =<http://www.bing.com/>.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Figure 04**: test de l’action de connexion mise à jour

Une fois la connexion établie, nous sommes redirigés vers l’action du contrôleur d’index et de la page d’index au lieu de l’URL externe.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Figure 05**: attaque de redirection ouverte invalidée

## <a name="summary"></a>Récapitulatif

Les attaques de redirection ouverte peuvent se produire lorsque les URL de redirection sont transmises en tant que paramètres dans l’URL d’une application. Le modèle ASP.NET MVC 3 comprend du code pour vous protéger contre les attaques de redirection ouvertes. Vous pouvez ajouter ce code en modifiant les applications ASP.NET MVC 1,0 et 2. Pour vous protéger contre les attaques par redirection ouverte lors de la connexion aux applications ASP.NET 1,0 et 2, ajoutez une méthode IsLocalUrl () et validez le paramètre returnUrl dans l’action d’ouverture de session.
