---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: Configuration de l’authentification par formulaire etC#rubriques avancées () | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner les différents paramètres d’authentification par formulaire et voir comment les modifier par le biais de l’élément Forms. Cela entraînera une description détaillée...
ms.author: riande
ms.date: 01/14/2008
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: b296f31da1c73df97175d94402b4d618df425d8d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74579282"
---
# <a name="forms-authentication-configuration-and-advanced-topics-c"></a>Configuration et questions avancées de l’authentification par formulaire (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> Dans ce didacticiel, nous allons examiner les différents paramètres d’authentification par formulaire et voir comment les modifier par le biais de l’élément Forms. Cela entraînera un examen détaillé de la personnalisation de la valeur du délai d’attente du ticket d’authentification par formulaire à l’aide d’une page de connexion avec une URL personnalisée (comme Sign. aspx au lieu de login. aspx) et des tickets d’authentification par formulaire sans cookie.

## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](an-overview-of-forms-authentication-cs.md) , nous avons examiné les étapes nécessaires à l’implémentation de l’authentification par formulaire dans une application ASP.net, de la spécification des paramètres de configuration dans Web. config à la création d’une page de connexion pour afficher un contenu différent pour les utilisateurs authentifiés et anonymes. Rappelez-vous que nous avons configuré le site Web pour utiliser l’authentification par formulaire en affectant à l’attribut mode de l’élément&gt; de l’authentification &lt;la valeur Forms. L’élément &lt;&gt; d’authentification peut éventuellement inclure un élément enfant &lt;Forms&gt;, par l’intermédiaire duquel un assortiment de paramètres d’authentification par formulaire peut être spécifié.

Dans ce didacticiel, nous allons examiner les différents paramètres d’authentification par formulaire et voir comment les modifier par le biais de l’élément &lt;Forms&gt;. Cela entraînera un examen détaillé de la personnalisation de la valeur du délai d’attente du ticket d’authentification par formulaire à l’aide d’une page de connexion avec une URL personnalisée (comme Sign. aspx au lieu de login. aspx) et des tickets d’authentification par formulaire sans cookie. Nous examinerons également plus en détail la composition du ticket d’authentification par formulaire et voyons les précautions que ASP.NET prend pour s’assurer que les données du ticket sont protégées contre l’inspection et la falsification. Enfin, nous verrons comment stocker des données utilisateur supplémentaires dans le ticket d’authentification par formulaire et comment modéliser ces données par le biais d’un objet principal personnalisé.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Étape 1 : examen des paramètres de configuration de &lt;Forms&gt;

Le système d’authentification par formulaire dans ASP.NET offre un certain nombre de paramètres de configuration qui peuvent être personnalisés au niveau de l’application. Cela comprend des paramètres tels que : la durée de vie du ticket d’authentification par formulaire ; le type de protection appliqué au ticket ; dans quelles conditions les tickets d’authentification sans cookies sont utilisés ; chemin d’accès à la page de connexion ; et d’autres informations. Pour modifier les valeurs par défaut, ajoutez un [élément&lt;forms&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx) en tant qu’enfant de l' [élément&lt;&gt;](https://msdn.microsoft.com/library/532aee0e.aspx)de l’authentification, en spécifiant les valeurs de propriété que vous souhaitez personnaliser comme attributs XML comme suit :

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

Le tableau 1 résume les propriétés qui peuvent être personnalisées par le biais de l’élément &lt;Forms&gt;. Comme Web. config est un fichier XML, les noms d’attributs dans la colonne de gauche sont sensibles à la casse.

| <strong>Attribut</strong> |                                                                                                                                                                                                                                     <strong>Description</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         sans cookies         |                                                                                                                Cet attribut spécifie dans quelles conditions le ticket d’authentification est stocké dans un cookie et est incorporé dans l’URL. Les valeurs autorisées sont : UseCookies ; UseUri; Détection automatique et UseDeviceProfile (valeur par défaut). L’étape 2 examine ce paramètre plus en détail.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Indique l’URL vers laquelle les utilisateurs sont redirigés après s’être connecté à partir de la page de connexion si aucune valeur RedirectUrl n’est spécifiée dans la chaîne de chaîne. La valeur par défaut est default. aspx.                                                                                                                                                         |
|           de domaine           | Lorsque vous utilisez des tickets d’authentification par cookie, ce paramètre spécifie la valeur de domaine du cookie. La valeur par défaut est une chaîne vide, ce qui amène le navigateur à utiliser le domaine à partir duquel il a été émis (par exemple, www.yourdomain.com). Dans ce cas, le cookie n’est <strong>pas</strong> envoyé lors de la demande de sous-domaines, tels que admin.yourdomain.com. Si vous souhaitez que le cookie soit passé à tous les sous-domaines, vous devez personnaliser l’attribut de domaine en lui affectant la valeur yourdomain.com. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Valeur booléenne indiquant si les utilisateurs authentifiés sont mémorisés lorsqu’ils sont redirigés vers des URL dans d’autres applications Web sur le même serveur. La valeur par défaut est False.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      URL de la page de connexion. La valeur par défaut est Login. aspx.                                                                                                                                                                                                                      |
|            nom            |                                                                                                                                                                                                   Lorsque vous utilisez des tickets d’authentification par cookie, le nom du cookie. La valeur par défaut est. ASPXAUTH.                                                                                                                                                                                                   |
|            path            |                                                                             Lorsque vous utilisez des tickets d’authentification par cookie, ce paramètre spécifie l’attribut de chemin d’accès du cookie. L’attribut path permet à un développeur de limiter l’étendue d’un cookie à une hiérarchie de répertoires particulière. La valeur par défaut est/, qui indique au navigateur d’envoyer le cookie de ticket d’authentification à toute demande adressée au domaine.                                                                              |
|         protection         |                                                                                                                                            Indique les techniques utilisées pour protéger le ticket d’authentification par formulaire. Les valeurs autorisées sont : All (valeur par défaut); Chiffre None et la validation. Ces paramètres sont décrits en détail à l’étape 3.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Valeur booléenne qui indique si une connexion SSL est requise pour transmettre le cookie d’authentification. La valeur par défaut est false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Valeur booléenne qui indique si le délai d’expiration du cookie d’authentification est réinitialisé chaque fois que l’utilisateur visite le site au cours d’une seule session. La valeur par défaut est true. La stratégie d’expiration du ticket d’authentification est décrite plus en détail dans la section Spécification de la valeur du délai d’attente du ticket.                                                                                                 |
|          délai d'expiration           |                                                                                                                               Spécifie la durée, en minutes, au terme de laquelle le cookie de ticket d’authentification expire. La valeur par défaut est 30. La stratégie d’expiration du ticket d’authentification est décrite plus en détail dans la section Spécification de la valeur du délai d’attente du ticket.                                                                                                                               |

**Tableau 1**: Résumé des attributs de l’élément de &lt;forms&gt;

Dans ASP.NET 2,0 et au-delà, les valeurs d’authentification par défaut des formulaires sont codées en dur dans la classe FormsAuthenticationConfiguration du .NET Framework. Toutes les modifications doivent être appliquées au niveau application par application dans le fichier Web. config. Cela diffère de ASP.NET 1. x, où les valeurs d’authentification par défaut des formulaires étaient stockées dans le fichier machine. config (et peuvent donc être modifiées via la modification de machine. config). Dans le sujet de ASP.NET 1. x, il est utile de mentionner qu’un certain nombre de paramètres système d’authentification par formulaire ont des valeurs par défaut différentes dans le ASP.NET 2,0 et au-delà de ASP.NET 1. x. Si vous migrez votre application à partir d’un environnement ASP.NET 1. x, il est important de connaître ces différences. Pour obtenir la liste des différences, consultez [la documentation technique de l’élément &lt;forms&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx) .

> [!NOTE]
> Plusieurs paramètres d’authentification par formulaire, tels que le délai d’attente, le domaine et le chemin d’accès, spécifient des détails pour le cookie de ticket d’authentification par formulaire résultant. Pour plus d’informations sur les cookies, leur fonctionnement et leurs différentes propriétés, lisez [ce didacticiel sur les cookies](http://www.quirksmode.org/js/cookies.html).

### <a name="specifying-the-tickets-timeout-value"></a>Spécification de la valeur du délai d’attente du ticket

Le ticket d’authentification par formulaire est un jeton qui représente une identité. Avec les tickets d’authentification basés sur les cookies, ce jeton est conservé sous la forme d’un cookie et est envoyé au serveur Web à chaque demande. La possession du jeton, en résumé, déclare, je suis le *nom d’utilisateur*, j’ai déjà ouvert une session et est utilisé pour que l’identité d’un utilisateur puisse être mémorisée au fil des visites de page.

Le ticket d’authentification par formulaire inclut non seulement l’identité de l’utilisateur, mais contient également des informations pour garantir l’intégrité et la sécurité du jeton. Après tout, nous ne souhaitons pas qu’un utilisateur être mal intentionné soit en mesure de créer un jeton contrefait ou de modifier un jeton de manière légitime.

L’une des informations incluses dans le ticket est l' *expiration*, c’est-à-dire la date et l’heure auxquelles le ticket n’est plus valide. Chaque fois que le FormsAuthenticationModule inspecte un ticket d’authentification, il s’assure que l’expiration du ticket n’est pas encore passée. Si c’est le cas, il ignore le ticket et identifie l’utilisateur comme étant anonyme. Cette protection permet de se protéger contre les attaques par relecture. Sans expiration, si un pirate était en mesure d’obtenir ses mains sur le ticket d’authentification valide d’un utilisateur (peut-être en obtenant un accès physique à son ordinateur et en utilisant ses cookies), il pourrait envoyer une requête au serveur avec ce ticket d’authentification volé et entrée gain. Bien que l’expiration n’empêche pas ce scénario, elle limite la fenêtre pendant laquelle une telle attaque peut être menée à bien.

> [!NOTE]
> L’étape 3 détaille les techniques supplémentaires utilisées par le système d’authentification par formulaire pour protéger le ticket d’authentification.

Lors de la création du ticket d’authentification, le système d’authentification par formulaire détermine son expiration en consultant le paramètre de délai d’attente. Comme indiqué dans le tableau 1, la valeur par défaut du délai d’expiration est de 30 minutes, ce qui signifie que lorsque le ticket d’authentification par formulaire est créé, son expiration est définie sur une date et une heure 30 minutes à l’avenir.

L’expiration définit une heure absolue à l’avenir lorsque le ticket d’authentification par formulaire expire. Mais généralement, les développeurs veulent implémenter une expiration glissante, qui est réinitialisée chaque fois que l’utilisateur revisite le site. Ce comportement est déterminé par les paramètres slidingExpiration. Si la valeur est true (valeur par défaut), chaque fois que FormsAuthenticationModule authentifie un utilisateur, il met à jour l’expiration du ticket. Si la valeur est false, l’expiration n’est pas mise à jour à chaque demande, provoquant ainsi l’expiration exacte du ticket en fonction du délai d’expiration du ticket.

> [!NOTE]
> L’expiration stockée dans le ticket d’authentification est une valeur de date et d’heure absolue, comme le 2 août 2008 11:34 AM. En outre, la date et l’heure sont relatives à l’heure locale du serveur Web. Cette décision de conception peut avoir des effets secondaires intéressants sur l’heure d’été (DST), c’est-à-dire lorsque les horloges du États-Unis sont déplacées avant une heure (en supposant que le serveur Web est hébergé dans des paramètres régionaux dans lesquels l’heure d’été est observée). Envisagez ce qui se passerait pour un site Web ASP.NET avec une expiration de 30 minutes près de la date de début de l’heure d’été (à 2:00 AM). Imaginez qu’un visiteur se connecte au site le 11 mars à 2008 à 1:55 AM. Cela génère un ticket d’authentification par formulaire qui expire à partir du 11 mars à 2008 à 2:25 AM (30 minutes dans le futur). Toutefois, une fois que 2:00 passe, l’horloge passe à 3:00 AM en raison de l’heure d’été. Lorsque l’utilisateur charge une nouvelle page six minutes après la connexion (à 3:01 AM), FormsAuthenticationModule note que le ticket a expiré et redirige l’utilisateur vers la page de connexion. Pour une discussion plus approfondie sur ce singularités et les autres délais d’expiration du ticket d’authentification, ainsi que des solutions de contournement, prenez une copie de la gestion de la *sécurité, de l’appartenance et de la gestion des rôles ASP.NET 2,0* de Stefan Schackow (ISBN : 978-0-7645-9698-8).

La figure 1 illustre le flux de travail lorsque slidingExpiration a la valeur false et que timeout a la valeur 30. Notez que le ticket d’authentification généré à la connexion contient la date d’expiration et que cette valeur n’est pas mise à jour lors des requêtes suivantes. Si FormsAuthenticationModule constate que le ticket a expiré, il le supprime et traite la demande comme anonyme.

[![une représentation graphique de l’expiration du ticket d’authentification par formulaire lorsque slidingExpiration a la valeur false](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Figure 01**: représentation graphique de l’expiration du ticket d’authentification par formulaire lorsque SlidingExpiration a la valeur false ([cliquez pour afficher l’image en taille réelle](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))

La figure 2 illustre le flux de travail lorsque slidingExpiration a la valeur true et que timeout a la valeur 30. Lorsqu’une demande authentifiée est reçue (avec un ticket non expiré), son expiration est mise à jour en fonction du délai d’attente en minutes à l’avenir.

[![une représentation graphique du ticket d’authentification par formulaire lorsque slidingExpiration a la valeur true](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Figure 02**: représentation graphique du ticket d’authentification par formulaire lorsque SlidingExpiration a la valeur true ([cliquez pour afficher l’image en taille réelle](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))

Lors de l’utilisation de tickets d’authentification basés sur les cookies (valeur par défaut), cette discussion devient un peu plus confuse, car les cookies peuvent également avoir leur propre expiration spécifiée. L’expiration (ou l’absence d’un cookie) indique au navigateur si le cookie doit être détruit. Si le cookie n’a pas expiré, il est détruit lorsque le navigateur s’arrête. Toutefois, si un délai d’expiration est présent, le cookie reste stocké sur l’ordinateur de l’utilisateur jusqu’à ce que la date et l’heure spécifiées dans l’expiration se soient écoulées. Lorsqu’un cookie est détruit par le navigateur, il n’est plus envoyé au serveur Web. Par conséquent, la destruction d’un cookie est analogue à celle de l’utilisateur qui se déconnecte du site.

> [!NOTE]
> Bien entendu, un utilisateur peut supprimer de manière proactive les cookies stockés sur son ordinateur. Dans Internet Explorer 7, accédez à outils, options, puis cliquez sur le bouton supprimer dans la section historique de navigation. À partir de là, cliquez sur le bouton supprimer les cookies.

Le système d’authentification par formulaire crée des cookies basés sur une session ou une expiration en fonction de la valeur transmise au paramètre *persistCookie* . N’oubliez pas que les méthodes GetAuthCookie, SetAuthCookie et RedirectFromLoginPage de la classe FormsAuthentication acceptent deux paramètres d’entrée : *username* et *persistCookie*. La page de connexion que nous avons créée dans le didacticiel précédent comprenait une case à cocher mémoriser, qui déterminait si un cookie persistant a été créé. Les cookies persistants sont basés sur l’expiration ; les cookies non persistants sont basés sur une session.

Les concepts du délai d’expiration et de l’slidingExpiration déjà abordés s’appliquent de la même façon aux cookies de session et d’expiration. Il n’y a qu’une seule différence mineure d’exécution : lorsque vous utilisez des cookies d’expiration avec slidingTimeout défini sur true, l’expiration du cookie n’est mise à jour que lorsque plus de la moitié du temps spécifié s’est écoulée.

Nous allons mettre à jour les stratégies de délai d’expiration du ticket d’authentification du site Web afin que les tickets expirent après une heure (60 minutes), à l’aide d’une expiration décalée. Pour appliquer cette modification, mettez à jour le fichier Web. config, en ajoutant un élément &lt;Forms&gt; à la &lt;élément&gt; de l’authentification avec le balisage suivant :

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Utilisation d’une URL de page de connexion différente de login. aspx

Étant donné que FormsAuthenticationModule redirige automatiquement les utilisateurs non autorisés vers la page de connexion, il doit connaître l’URL de la page de connexion. Cette URL est spécifiée par l’attribut loginUrl dans l’élément &lt;Forms&gt; et par défaut, login. aspx. Si vous effectuez un portage sur un site Web existant, vous disposez peut-être déjà d’une page de connexion avec une autre URL, qui a déjà été marquée d’un signet et indexé par les moteurs de recherche. Au lieu de renommer votre page de connexion existante en login. aspx et de rompre les liens et les signets des utilisateurs, vous pouvez modifier l’attribut loginUrl pour qu’il pointe vers votre page de connexion.

Par exemple, si votre page de connexion s’appelait Sign. aspx et se trouvait dans le répertoire Users, vous pouviez faire pointer le paramètre de configuration loginUrl sur ~/Users/SignIn.aspx comme suit :

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Étant donné que notre application actuelle a déjà une page de connexion nommée Login. aspx, il n’est pas nécessaire de spécifier une valeur personnalisée dans l’élément &lt;Forms&gt;.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Étape 2 : utilisation de tickets d’authentification par formulaire sans cookie

Par défaut, le système d’authentification par formulaire détermine s’il faut stocker ses tickets d’authentification dans la collection de cookies ou les incorporer dans l’URL en fonction de l’agent utilisateur visitant le site. Tous les navigateurs de bureau standard comme Internet Explorer, Firefox, Opera et Safari prennent en charge les cookies, mais pas tous les périphériques mobiles.

La stratégie de cookie utilisée par le système d’authentification par formulaire dépend du paramètre sans cookies dans l’élément &lt;Forms&gt;, qui peut être affecté à l’une des quatre valeurs suivantes :

- UseCookies : spécifie que les tickets d’authentification basés sur les cookies sont toujours utilisés.
- UseUri : indique que les tickets d’authentification basés sur les cookies ne seront jamais utilisés.
- Détection automatique : si le profil de l’appareil ne prend pas en charge les cookies, les tickets d’authentification basés sur les cookies ne sont pas utilisés. Si le profil de périphérique prend en charge les cookies, un mécanisme de détection est utilisé pour déterminer si les cookies sont activés.
- UseDeviceProfile-la valeur par défaut ; utilise les tickets d’authentification par cookie uniquement si le profil de périphérique prend en charge les cookies. Aucun mécanisme de détection n’est utilisé.

Les paramètres AutoDetect et UseDeviceProfile s’appuient sur un *profil d’appareil* pour déterminer s’il faut utiliser des tickets d’authentification par cookie ou sans cookie. ASP.NET gère une base de données de divers appareils et leurs fonctionnalités, par exemple s’ils prennent en charge les cookies, la version de JavaScript qu’ils prennent en charge, etc. Chaque fois qu’un appareil demande une page Web à partir d’un serveur Web, il l’envoie le long d’un en-tête http de *l’agent utilisateur* qui identifie le type d’appareil. ASP.NET correspond automatiquement à la chaîne User-agent fournie avec le profil correspondant spécifié dans sa base de données.

> [!NOTE]
> Cette base de données de fonctionnalités d’appareil est stockée dans un certain nombre de fichiers XML qui adhèrent au [schéma du fichier de définition de navigateur](https://msdn.microsoft.com/library/ms228122.aspx). Les fichiers de profil d’appareil par défaut se trouvent dans%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. Vous pouvez également ajouter des fichiers personnalisés au dossier des navigateurs de\_d’applications de votre application. Pour plus d’informations, consultez [Comment : détecter des types de navigateurs dans pages Web ASP.net](https://msdn.microsoft.com/library/3yekbd5b.aspx).

Étant donné que le paramètre par défaut est UseDeviceProfile, les tickets d’authentification par formulaire sans cookie sont utilisés lorsque le site est visité par un périphérique dont le profil signale qu’il ne prend pas en charge les cookies.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Encodage du ticket d’authentification dans l’URL

Les cookies sont un moyen naturel d’inclure des informations du navigateur dans chaque requête adressée à un site Web particulier, ce qui explique pourquoi les paramètres d’authentification par défaut des formulaires utilisent des cookies si l’appareil visité les prend en charge. Si les cookies ne sont pas pris en charge, un autre moyen de transmettre le ticket d’authentification du client au serveur doit être utilisé. Une solution de contournement courante utilisée dans les environnements sans cookies consiste à encoder les données de cookie dans l’URL.

La meilleure façon de voir comment ces informations peuvent être incorporées dans l’URL est de forcer le site à utiliser des tickets d’authentification sans cookies. Pour ce faire, définissez le paramètre de configuration cookieless sur UseUri :

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Une fois que vous avez apporté cette modification, visitez le site via un navigateur. Lorsque vous vous rendez en tant qu’utilisateur anonyme, les URL se présentent exactement comme avant. Par exemple, lors de la visite de la page default. aspx, la barre d’adresse de mon navigateur affiche l’URL suivante :

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Toutefois, lors de la connexion, le ticket d’authentification par formulaire est incorporé dans l’URL. Par exemple, après avoir visité la page de connexion et vous être connecté en tant que Sam, je suis renvoyé à la page default. aspx, mais l’URL est la suivante :

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Le ticket d’authentification par formulaire a été incorporé dans l’URL. La chaîne (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2) représente les informations de ticket d’authentification avec codage hexadécimal, et les mêmes données sont généralement stockées dans un cookie.

Pour que les tickets d’authentification sans cookie fonctionnent, le système doit encoder toutes les URL sur la page pour inclure les données du ticket d’authentification. dans le cas contraire, le ticket d’authentification sera perdu lorsque l’utilisateur cliquera sur un lien. Heureusement, cette logique d’incorporation est exécutée automatiquement. Pour illustrer cette fonctionnalité, ouvrez la page default. aspx et ajoutez un contrôle de lien hypertexte, en affectant respectivement à ses propriétés Text et NavigateUrl la valeur Test Link et SomePage. aspx. Peu importe qu’il ne s’agit pas d’une page de notre projet nommée SomePage. aspx.

Enregistrez les modifications apportées à default. aspx, puis accédez-y dans un navigateur. Connectez-vous au site afin que le ticket d’authentification par formulaire soit incorporé dans l’URL. Ensuite, à partir de default. aspx, cliquez sur le lien du lien de test. que se passe-t-il ? Si aucune page nommée SomePage. aspx n’existe, une erreur 404 s’est produite, mais ce n’est pas ce qui est important ici. Au lieu de cela, vous devez vous concentrer sur la barre d’adresses de votre navigateur. Notez qu’il comprend le ticket d’authentification par formulaire dans l’URL !

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

L’URL SomePage. aspx dans le lien a été automatiquement convertie en URL incluant le ticket d’authentification, nous n’avons pas eu à écrire de code. Le ticket d’authentification par formulaire sera automatiquement incorporé dans l’URL pour tous les liens hypertexte qui ne commencent pas par `http://` ou `/`. Peu importe si le lien hypertexte s’affiche dans un appel à Response. Redirect, dans un contrôle de lien hypertexte ou dans un élément HTML d’ancrage (c’est-à-dire `<a href="...">...</a>`). Tant que l’URL n’est pas semblable à `http://www.someserver.com/SomePage.aspx` ou `/SomePage.aspx`, le ticket d’authentification par formulaire sera incorporé pour nous.

> [!NOTE]
> Les tickets d’authentification par formulaire sans cookie adhèrent aux mêmes stratégies de délai d’attente que les tickets d’authentification basés sur les cookies. Toutefois, les tickets d’authentification sans cookies sont plus sujets aux attaques par relecture puisque le ticket d’authentification est incorporé directement dans l’URL. Imaginez un utilisateur qui visite un site Web, se connecte, puis colle l’URL d’un courrier électronique à un collègue. Si le collègue clique sur ce lien avant que la date d’expiration soit atteinte, il est connecté en tant qu’utilisateur qui a envoyé l’e-mail.

## <a name="step-3-securing-the-authentication-ticket"></a>Étape 3 : sécurisation du ticket d’authentification

Le ticket d’authentification par formulaire est transmis sur le réseau dans un cookie ou incorporé directement dans l’URL. En plus des informations d’identité, le ticket d’authentification peut également inclure des données utilisateur (comme nous le verrons à l’étape 4). Par conséquent, il est important que les données du ticket soient chiffrées à partir d’yeux indiscrets et (encore plus important encore) que le système d’authentification par formulaire puisse garantir que le ticket n’a pas été falsifié.

Pour garantir la confidentialité des données du ticket, le système d’authentification par formulaire peut chiffrer les données du ticket. L’impossibilité de chiffrer les données de tickets envoie des informations potentiellement sensibles sur le réseau en texte brut.

Pour garantir l’authenticité d’un ticket, le système d’authentification par formulaire doit *valider* le ticket. La validation consiste à s’assurer qu’un élément de données particulier n’a pas été modifié et est accompli par le biais d’un *[code d’authentification de message (Mac)](http://en.wikipedia.org/wiki/Message_authentication_code)* . En résumé, le MAC est un petit élément d’information qui identifie les données qui doivent être validées (dans ce cas, le ticket). Si les données représentées par le MAC sont modifiées, le MAC et les données ne correspondent pas. En outre, il est difficile pour un pirate informatique de modifier les données et de générer son propre MAC pour correspondre aux données modifiées.

Lors de la création (ou de la modification) d’un ticket, le système d’authentification par formulaire crée un MAC et l’attache aux données du ticket. Lorsqu’une requête suivante arrive, le système d’authentification par formulaire compare les données de ticket et MAC pour valider l’authenticité des données de ticket. La figure 3 illustre ce flux de travail graphiquement.

[![l’authenticité du ticket est garantie via un MAC](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Figure 03**: l’authenticité du ticket est garantie via un Mac ([cliquez pour afficher l’image en taille réelle](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))

Les mesures de sécurité appliquées au ticket d’authentification dépendent du paramètre de protection de l’élément &lt;Forms&gt;. Le paramètre de protection peut être affecté à l’une des trois valeurs suivantes :

- Tout : le ticket est à la fois chiffré et signé numériquement (valeur par défaut).
- Le chiffrement avec chiffrement uniquement est appliqué-aucun MAC n’est généré.
- Aucun : le ticket n’est ni chiffré ni signé numériquement.
- Validation : un MAC est généré, mais les données de ticket sont envoyées sur le réseau en texte brut.

Microsoft recommande vivement d’utiliser le paramètre All.

### <a name="setting-the-validation-and-decryption-keys"></a>Définition des clés de validation et de déchiffrement

Les algorithmes de chiffrement et de hachage utilisés par le système d’authentification par formulaire pour chiffrer et valider le ticket d’authentification sont personnalisables via l' [&lt;machineKey&gt; élément](https://msdn.microsoft.com/library/w8h3skw9.aspx) dans Web. config. Le tableau 2 présente les attributs de l’élément &lt;machineKey&gt; et leurs valeurs possibles.

| **Attribut** | **Description** |
| --- | --- |
| déchiffrement | Indique l’algorithme utilisé pour le chiffrement. Cet attribut peut avoir l’une des quatre valeurs suivantes :-auto-la valeur par défaut ; détermine l’algorithme en fonction de la longueur de l’attribut decryptionKey. -AES-utilise l’algorithme [Advanced Encryption Standard (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) . -DES-utilise [Data Encryption Standard (des)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) . cet algorithme est considéré comme faible en termes de calcul et ne doit pas être utilisé. -3DES-utilise l’algorithme [triple des](http://en.wikipedia.org/wiki/Triple_DES) , qui fonctionne en appliquant l’algorithme des trois fois. |
| decryptionKey | Clé secrète utilisée par l’algorithme de chiffrement. Cette valeur doit être une chaîne hexadécimale de la longueur appropriée (basée sur la valeur dans le déchiffrement), une valeur AutoGenerate ou une valeur ajoutée à, IsolateApps. L’ajout de IsolateApps demande à ASP.NET d’utiliser une valeur unique pour chaque application. La valeur par défaut est AutoGenerate, IsolateApps. |
| validation | Indique l’algorithme utilisé pour la validation. Cet attribut peut avoir l’une des quatre valeurs suivantes :-AES-utilise l’algorithme Advanced Encryption Standard (AES). -MD5-utilise l’algorithme [MD5 (Message-Digest 5)](http://en.wikipedia.org/wiki/MD5) . -SHA1-utilise l’algorithme [SHA1](http://en.wikipedia.org/wiki/Sha1) (valeur par défaut). -3DES-utilise l’algorithme Triple DES. |
| validationKey | Clé secrète utilisée par l’algorithme de validation. Cette valeur doit être une chaîne hexadécimale de la longueur appropriée (en fonction de la valeur de la validation), Generate ou une valeur ajoutée avec, IsolateApps. L’ajout de IsolateApps demande à ASP.NET d’utiliser une valeur unique pour chaque application. La valeur par défaut est AutoGenerate, IsolateApps. |

**Tableau 2**: attributs de l’élément &lt;machineKey&gt;

Une présentation détaillée de ces options de chiffrement et de validation, ainsi que des avantages et des inconvénients des différents algorithmes, dépasse le cadre de ce didacticiel. Pour une analyse approfondie de ces problèmes, notamment des conseils sur les algorithmes de chiffrement et de validation à utiliser, les longueurs de clé à utiliser et la meilleure façon de générer ces clés, reportez-vous à la rubrique *Professional ASP.NET 2,0 Security, Membership and Role Management*.

Par défaut, les clés utilisées pour le chiffrement et la validation sont générées automatiquement pour chaque application, et ces clés sont stockées dans l’autorité de sécurité locale (LSA). En résumé, les paramètres par défaut garantissent des clés uniques sur un serveur Web par serveur Web et application par application. Par conséquent, ce comportement par défaut ne fonctionne pas pour les deux scénarios suivants :

- **Batteries** de serveurs Web : dans un scénario de batterie de serveurs [Web](http://en.wikipedia.org/wiki/Web_farm) , une application Web unique est hébergée sur plusieurs serveurs Web à des fins d’extensibilité et de redondance. Chaque demande entrante est distribuée à un serveur de la batterie, ce qui signifie que, pendant la durée de vie de la session d’un utilisateur, des serveurs différents peuvent être utilisés pour gérer ses diverses demandes. Par conséquent, chaque serveur doit utiliser les mêmes clés de chiffrement et de validation afin que le ticket d’authentification par formulaire créé, chiffré et validé sur un serveur puisse être déchiffré et validé sur un autre serveur de la batterie.
- **Cross application ticket Sharing** : un serveur Web unique peut héberger plusieurs applications ASP.net. Si vous avez besoin que ces différentes applications partagent un ticket d’authentification par formulaire unique, il est impératif que leurs clés de chiffrement et de validation correspondent.

Lorsque vous travaillez dans un paramètre de batterie de serveurs Web ou que vous partagez des tickets d’authentification entre des applications sur le même serveur, vous devez configurer l’élément &lt;machineKey&gt; dans les applications affectées afin que leurs valeurs decryptionKey et validationKey correspondent.

Si aucun des scénarios ci-dessus ne s’applique à notre exemple d’application, nous pouvons toujours spécifier des valeurs decryptionKey et validationKey explicites et définir les algorithmes à utiliser. Ajoutez un paramètre &lt;machineKey&gt; au fichier Web. config :

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Pour plus d’informations [, consultez Comment : configurer machineKey dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Les valeurs decryptionKey et validationKey proviennent de la [page Web des mots de passe parfait](https://www.grc.com/passwords.htm)de [Steve Gibson](http://www.grc.com/stevegibson.htm), qui génère 64 caractères hexadécimaux aléatoires à chaque visite de page. Pour réduire la probabilité que ces clés se trouvent dans vos applications de production, il est recommandé de remplacer les clés ci-dessus par des clés générées de manière aléatoire à partir de la page des mots de passe idéaux.

## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Étape 4 : stockage des données utilisateur supplémentaires dans le ticket

De nombreuses applications Web affichent des informations sur l’utilisateur actuellement connecté ou sur la base de l’affichage de la page. Par exemple, une page Web peut afficher le nom de l’utilisateur et la date de sa dernière connexion dans le coin supérieur de chaque page. Le ticket d’authentification par formulaire stocke le nom d’utilisateur de l’utilisateur actuellement connecté, mais lorsqu’une autre information est nécessaire, la page doit accéder au magasin de l’utilisateur (généralement une base de données) pour rechercher les informations non stockées dans le ticket d’authentification.

Avec un peu de code, nous pouvons stocker des informations utilisateur supplémentaires dans le ticket d’authentification par formulaire. Ces données peuvent être exprimées par le biais de la [propriété UserData](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx)de la [classe FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx). Il s’agit d’un emplacement utile pour mettre de petites quantités d’informations sur l’utilisateur qui sont généralement nécessaires. La valeur spécifiée dans la propriété UserData est incluse dans le cookie de ticket d’authentification et, comme les autres champs de ticket, est chiffrée et validée en fonction de la configuration du système d’authentification par formulaire. Par défaut, UserData est une chaîne vide.

Pour stocker les données utilisateur dans le ticket d’authentification, nous devons écrire un peu de code dans la page de connexion qui récupère les informations spécifiques à l’utilisateur et les stocke dans le ticket. Dans la mesure où UserData est une propriété de type chaîne, les données stockées dans celui-ci doivent être correctement sérialisées comme une chaîne. Par exemple, imaginez que notre magasin d’utilisateurs incluait la date de naissance de chaque utilisateur et le nom de son employeur. nous voulons stocker ces deux valeurs de propriété dans le ticket d’authentification. Nous pourrions sérialiser ces valeurs dans une chaîne en concaténant la date de la chaîne de naissance de l’utilisateur avec un canal (|), suivi du nom de l’employeur. Pour un utilisateur né le 15 août 1974 qui travaille pour Northwind Traders, nous attribuons à la propriété UserData la valeur String : 1974-08-15 | Northwind Traders.

Chaque fois que nous avons besoin d’accéder aux données stockées dans le ticket, nous pouvons le faire en saisissant le FormsAuthenticationTicket de la requête actuelle et en désérialisant la propriété UserData. Dans le cas de la date de naissance et de l’exemple de nom de l’employeur, nous fractionnerons la chaîne UserData en deux sous-chaînes en fonction du délimiteur (|).

[![informations supplémentaires sur l’utilisateur peuvent être stockées dans le ticket d’authentification](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Figure 04**: les informations utilisateur supplémentaires peuvent être stockées dans le ticket d’authentification ([cliquez pour afficher l’image en taille réelle](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))

### <a name="writing-information-to-userdata"></a>Écriture d’informations dans UserData

Malheureusement, l’ajout d’informations spécifiques à l’utilisateur dans un ticket d’authentification par formulaire n’est pas aussi simple que possible. La propriété UserData de la classe FormsAuthenticationTicket est en lecture seule et ne peut être spécifiée que par le biais du constructeur de classe FormsAuthenticationTicket. Lorsque vous spécifiez la propriété UserData dans le constructeur, nous devons également fournir les autres valeurs du ticket : le nom d’utilisateur, la date d’émission, l’expiration, etc. Lors de la création de la page de connexion dans le didacticiel précédent, cette dernière était gérée pour nous par la classe FormsAuthentication. Lorsque vous ajoutez des UserData à FormsAuthenticationTicket, nous devons écrire du code pour répliquer la plupart des fonctionnalités déjà fournies par la classe FormsAuthentication.

Nous allons explorer le code nécessaire à l’utilisation de UserData en mettant à jour la page Login. aspx pour enregistrer des informations supplémentaires sur l’utilisateur dans le ticket d’authentification. Supposons que notre magasin d’utilisateurs contient des informations sur la société pour laquelle l’utilisateur travaille et sur son titre, et que nous souhaitons capturer ces informations dans le ticket d’authentification. Mettez à jour le gestionnaire d’événements LoginButton Click de la page Login. aspx afin que le code ressemble à ce qui suit :

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Commençons par parcourir ce code une ligne à la fois. La méthode commence par définir quatre tableaux de chaînes : utilisateurs, mots de passe, companyName et titleAtCompany. Ces tableaux contiennent les noms d’utilisateur, mots de passe, noms de société et titres pour les comptes d’utilisateur du système, dont trois : Scott, Jisun et Sam. Dans une application réelle, ces valeurs sont interrogées à partir du magasin de l’utilisateur, et ne sont pas codées en dur dans le code source de la page.

Dans le didacticiel précédent, si les informations d’identification fournies étaient valides, nous avons simplement appelé FormsAuthentication. RedirectFromLoginPage (UserName. Text, RememberMe. Checked), qui effectuait les étapes suivantes :

1. Création du ticket d’authentification par formulaire
2. Écriture du ticket dans le magasin approprié. Pour les tickets d’authentification basés sur les cookies, le regroupement cookies du navigateur est utilisé ; pour les tickets d’authentification sans cookies, les données de ticket sont sérialisées dans l’URL
3. Redirection de l’utilisateur vers la page appropriée

Ces étapes sont répliquées dans le code ci-dessus. Tout d’abord, la chaîne que nous allons finalement stocker dans la propriété UserData est formée en combinant le nom de la société et le titre, en séparant les deux valeurs par un caractère de barre verticale (|).

chaîne userDataString = chaîne. Concat (companyName [i], "|", titleAtCompany [i]);

Ensuite, la méthode FormsAuthentication. GetAuthCookie est appelée, ce qui crée le ticket d’authentification, le chiffre et le valide en fonction des paramètres de configuration, puis le place dans un objet HttpCookie.

HttpCookie authCookie = FormsAuthentication. GetAuthCookie (UserName. Text, RememberMe. Checked);

Pour pouvoir utiliser le FormAuthenticationTicket incorporé dans le cookie, nous devons appeler la [méthode Decrypt](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx)de la classe FormAuthentication, en passant la valeur de cookie.

FormsAuthenticationTicket ticket = FormsAuthentication. Decrypt (authCookie. value);

Nous créons ensuite une *nouvelle* instance FormsAuthenticationTicket basée sur les valeurs de FormsAuthenticationTicket existantes. Toutefois, ce nouveau ticket contient les informations spécifiques à l’utilisateur (userDataString).

FormsAuthenticationTicket newTicket = New FormsAuthenticationTicket (ticket. Version, ticket. Nom, ticket. Délivré, ticket. Expiration, ticket. IsPersistent, userDataString);

Nous chiffreons ensuite (et validons) la nouvelle instance FormsAuthenticationTicket en appelant la [méthode Encrypt](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)et replaçons ces données chiffrées (et validées) dans authCookie.

authCookie. Value = FormsAuthentication. Encrypt (newTicket);

Enfin, authCookie est ajouté à la collection Response. cookies et la méthode GetRedirectUrl est appelée pour déterminer la page appropriée à envoyer à l’utilisateur.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

Tout ce code est nécessaire, car la propriété UserData est en lecture seule et la classe FormsAuthentication ne fournit aucune méthode pour spécifier les informations UserData dans ses méthodes GetAuthCookie, SetAuthCookie ou RedirectFromLoginPage.

> [!NOTE]
> Le code que nous venons d’examiner stocke des informations spécifiques à l’utilisateur dans un ticket d’authentification basé sur des cookies. Les classes chargées de sérialiser le ticket d’authentification par formulaire vers l’URL sont internes au .NET Framework. Long Story Short, vous ne pouvez pas stocker de données utilisateur dans un ticket d’authentification par formulaire sans cookie.

### <a name="accessing-the-userdata-information"></a>Accès aux informations UserData

À ce stade, le nom et le titre de la société de chaque utilisateur sont stockés dans la propriété UserData du ticket d’authentification par formulaire lorsqu’ils se connectent. Ces informations sont accessibles à partir du ticket d’authentification sur n’importe quelle page sans nécessiter de trajet vers le magasin de l’utilisateur. Pour illustrer la façon dont ces informations peuvent être récupérées à partir de la propriété UserData, nous allons mettre à jour default. aspx afin que son message d’accueil inclue non seulement le nom de l’utilisateur, mais également la société pour laquelle il travaille et son titre.

Actuellement, default. aspx contient un panneau AuthenticatedMessagePanel avec un contrôle Label nommé WelcomeBackMessage. Ce panneau s’affiche uniquement pour les utilisateurs authentifiés. Mettez à jour le code dans la page default. aspx\_le gestionnaire d’événements Load afin qu’il se présente comme suit :

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Si Request. IsAuthenticated a la valeur true, la propriété Text de WelcomeBackMessage est d’abord définie sur Welcome, *nom d’utilisateur*. Ensuite, la propriété User. Identity est convertie en objet FormsIdentity afin que nous puissions accéder au FormsAuthenticationTicket sous-jacent. Une fois le FormsAuthenticationTicket, nous désérialiserons la propriété UserData dans le nom et le titre de la société. Pour ce faire, fractionnez la chaîne sur le caractère de barre verticale. Le nom et le titre de la société sont ensuite affichés dans l’étiquette WelcomeBackMessage.

La figure 5 illustre une capture d’écran de cet affichage en action. La connexion en tant que Scott affiche un message de bienvenue qui comprend la société et le titre de Scott.

[![la société et le titre de l’utilisateur actuellement connecté sont affichés](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Figure 05**: la société et le titre de l’utilisateur actuellement connecté sont affichés ([cliquez pour afficher l’image en taille réelle](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))

> [!NOTE]
> La propriété UserData du ticket d’authentification sert de cache pour le magasin de l’utilisateur. Comme n’importe quel cache, il doit être mis à jour lors de la modification des données sous-jacentes. Par exemple, s’il existe une page Web à partir de laquelle les utilisateurs peuvent mettre à jour leur profil, les champs mis en cache dans la propriété UserData doivent être actualisés pour refléter les modifications apportées par l’utilisateur.

## <a name="step-5-using-a-custom-principal"></a>Étape 5 : utilisation d’un principal personnalisé

À chaque demande entrante, FormsAuthenticationModule tente d’authentifier l’utilisateur. Si un ticket d’authentification non expiré est présent, FormsAuthenticationModule affecte la propriété HttpContext. User à un nouvel objet GenericPrincipal. Cet objet GenericPrincipal a une identité de type FormsIdentity, qui inclut une référence au ticket d’authentification par formulaire. La classe GenericPrincipal contient les fonctionnalités minimales requises par une classe qui implémente IPrincipal. elle a simplement une propriété Identity et une méthode IsInRole.

L’objet principal a deux responsabilités : pour indiquer les rôles auxquels l’utilisateur appartient et pour fournir des informations d’identité. Cela s’effectue à l’aide de la méthode IsInRole (*roleName*) et de la propriété Identity de l’interface IPrincipal, respectivement. La classe GenericPrincipal permet de spécifier un tableau de chaînes de noms de rôles à l’aide de son constructeur. sa méthode IsInRole (*roleName*) vérifie simplement si le *roleName* passé existe dans le tableau de chaînes. Lorsque FormsAuthenticationModule crée le GenericPrincipal, il passe un tableau de chaînes vide au constructeur de GenericPrincipal. Par conséquent, tout appel à IsInRole retourne toujours la valeur false.

La classe GenericPrincipal répond aux besoins de la plupart des scénarios d’authentification basée sur les formulaires, où les rôles ne sont pas utilisés. Dans les cas où la gestion du rôle par défaut est insuffisante ou si vous devez associer un objet IIdentity personnalisé à l’utilisateur, vous pouvez créer un objet IPrincipal personnalisé pendant le flux de travail d’authentification et l’assigner à la propriété HttpContext. User.

> [!NOTE]
> Comme nous le verrons dans les prochains didacticiels, quand ASP. L’infrastructure des rôles du réseau est activée. elle crée un objet principal personnalisé de type [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) et remplace l’objet GenericPrincipal créé par l’authentification par formulaire. Cela permet de personnaliser la méthode IsInRole du principal pour l’interface avec l’API Roles Framework.

Étant donné que nous ne nous sommes pas encore associés à des rôles, la seule raison pour laquelle nous aurions dû créer un principal personnalisé à ce moment serait d’associer un objet IIdentity personnalisé au principal. À l’étape 4, nous avons examiné le stockage d’informations utilisateur supplémentaires dans la propriété UserData du ticket d’authentification, en particulier le nom de la société de l’utilisateur et son titre. Toutefois, les informations UserData sont accessibles uniquement via le ticket d’authentification, puis comme chaîne sérialisée, ce qui signifie que chaque fois que nous souhaitons afficher les informations utilisateur stockées dans le ticket, nous devons analyser la propriété UserData.

Nous pouvons améliorer l’expérience du développeur en créant une classe qui implémente IIdentity et qui comprend les propriétés CompanyName et title. De cette façon, un développeur peut accéder au nom de société et au titre de l’utilisateur actuellement connecté directement par le biais des propriétés CompanyName et title, sans qu’il soit nécessaire de savoir comment analyser la propriété UserData.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Création des classes d’identité et de principal personnalisées

Pour ce didacticiel, nous allons créer les objets principal et Identity personnalisés dans le dossier de code App\_. Commencez par ajouter une application\_dossier de code à votre projet : cliquez avec le bouton droit sur le nom du projet dans Explorateur de solutions, sélectionnez l’option Ajouter le dossier ASP.NET, puis choisissez application\_code. Le dossier App\_code est un dossier ASP.NET spécial qui contient des fichiers de classe spécifiques au site Web.

> [!NOTE]
> L’application\_dossier de code doit être utilisé uniquement lors de la gestion de votre projet via le modèle de projet de site Web. Si vous utilisez le [modèle de projet d’application Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), créez un dossier standard et ajoutez-y les classes. Par exemple, vous pouvez ajouter un nouveau dossier nommé classes et y placer votre code.

Ensuite, ajoutez deux nouveaux fichiers de classe au dossier App\_code, l’un nommé CustomIdentity.cs et l’autre nommé CustomPrincipal.cs.

[![ajouter les classes CustomIdentity et CustomPrincipal à votre projet](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Figure 06**: ajouter les classes CustomIdentity et CustomPrincipal à votre projet ([cliquez pour afficher l’image en taille réelle](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))

La classe CustomIdentity est chargée d’implémenter l’interface IIdentity, qui définit les propriétés AuthenticationType, IsAuthenticated et Name. En plus de ces propriétés requises, nous nous intéressons à exposer le ticket d’authentification des formulaires sous-jacent ainsi que les propriétés du nom et du titre de la société de l’utilisateur. Entrez le code suivant dans la classe CustomIdentity.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Notez que la classe comprend une variable membre FormsAuthenticationTicket (ticket\_) et que ces informations de ticket doivent être fournies par le biais du constructeur. Ces données de ticket sont utilisées pour retourner le nom de l’identité ; sa propriété UserData est analysée pour retourner les valeurs des propriétés CompanyName et title.

Ensuite, créez la classe CustomPrincipal. Étant donné que nous n’avons pas besoin des rôles à ce moment, le constructeur de la classe CustomPrincipal accepte uniquement un objet CustomIdentity ; sa méthode IsInRole retourne toujours false.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Assignation d’un objet CustomPrincipal au contexte de sécurité de la requête entrante

Nous avons maintenant une classe qui étend la spécification IIdentity par défaut pour inclure les propriétés CompanyName et title, ainsi qu’une classe principal personnalisée qui utilise l’identité personnalisée. Nous sommes prêts à effectuer un pas à pas détaillé dans le pipeline ASP.NET et à affecter notre objet principal personnalisé au contexte de sécurité de la requête entrante.

Le pipeline ASP.NET prend une demande entrante et le traite à travers un certain nombre d’étapes. À chaque étape, un événement particulier est déclenché, ce qui permet aux développeurs d’exploiter le pipeline ASP.NET et de modifier la demande à certains points de son cycle de vie. Le FormsAuthenticationModule, par exemple, attend que ASP.NET déclenche l' [événement AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx). à ce moment-là, il inspecte la demande entrante pour obtenir un ticket d’authentification. Si un ticket d’authentification est trouvé, un objet GenericPrincipal est créé et affecté à la propriété HttpContext. User.

Après l’événement AuthenticateRequest, le pipeline ASP.NET déclenche l' [événement PostAuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), ce qui nous permet de remplacer l’objet GenericPrincipal créé par FormsAuthenticationModule par une instance de notre objet CustomPrincipal. La figure 7 illustre ce flux de travail.

[![le GenericPrincipal est remplacé par un CustomPrincipal dans l’événement PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Figure 07**: le GenericPrincipal est remplacé par un CustomPrincipal dans l’événement PostAuthenticationRequest ([cliquez pour afficher l’image en taille réelle](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))

Pour exécuter du code en réponse à un événement de pipeline ASP.NET, nous pouvons soit créer le gestionnaire d’événements approprié dans global. asax, soit créer notre propre module HTTP. Pour ce didacticiel, nous allons créer le gestionnaire d’événements dans global. asax. Commencez par ajouter global. asax à votre site Web. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet, puis ajoutez un élément de type classe d’application globale nommé global. asax.

[![ajouter un fichier global. asax à votre site Web](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Figure 08**: ajouter un fichier global. asax à votre site Web ([cliquez pour afficher l’image en taille réelle](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))

Le modèle global. asax par défaut inclut des gestionnaires d’événements pour un certain nombre d’événements de pipeline ASP.NET, y compris l’événement de début, de fin et d' [erreur](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), entre autres. N’hésitez pas à supprimer ces gestionnaires d’événements, car nous n’en avons pas besoin pour cette application. L’événement qui nous intéresse est PostAuthenticateRequest. Mettez à jour votre fichier global. asax afin que son balisage ressemble à ce qui suit :

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

L’application\_méthode OnPostAuthenticateRequest s’exécute chaque fois que le runtime ASP.NET déclenche l’événement PostAuthenticateRequest, qui se produit une fois sur chaque demande de page entrante. Le gestionnaire d’événements commence par vérifier si l’utilisateur est authentifié et a été authentifié par le biais de l’authentification par formulaire. Dans ce cas, un nouvel objet CustomIdentity est créé et reçoit le ticket d’authentification de la requête actuelle dans son constructeur. À la suite de cela, un objet CustomPrincipal est créé et passé l’objet CustomIdentity juste créé dans son constructeur. Enfin, le contexte de sécurité de la requête actuelle est assigné à l’objet CustomPrincipal nouvellement créé.

Notez que la dernière étape associant l’objet CustomPrincipal au contexte de sécurité de la requête affecte le principal à deux propriétés : HttpContext. User et thread. CurrentPrincipal. Ces deux attributions sont nécessaires en raison de la façon dont les contextes de sécurité sont gérés dans ASP.NET. Le .NET Framework associe un contexte de sécurité à chaque thread en cours d’exécution. ces informations sont disponibles sous la forme d’un objet IPrincipal via la [propriété CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx)de l' [objet thread](https://msdn.microsoft.com/library/system.threading.thread.aspx). Ce qui est confus, c’est que ASP.NET possède ses propres informations de contexte de sécurité (HttpContext. User).

Dans certains scénarios, la propriété Thread. CurrentPrincipal est examinée lors de la détermination du contexte de sécurité. dans d’autres scénarios, HttpContext. User est utilisé. Par exemple, il existe des fonctionnalités de sécurité dans .NET qui permettent aux développeurs de déterminer de façon déclarative les utilisateurs ou les rôles qui peuvent instancier une classe ou appeler des méthodes spécifiques (consultez [Ajout de règles d’autorisation à des couches de données et métier à l’aide de PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). En coulisses, ces techniques déclaratives déterminent le contexte de sécurité via la propriété Thread. CurrentPrincipal.

Dans d’autres scénarios, la propriété HttpContext. User est utilisée. Par exemple, dans le didacticiel précédent, nous avons utilisé cette propriété pour afficher le nom d’utilisateur de l’utilisateur actuellement connecté. En clair, il est impératif que les informations de contexte de sécurité dans les propriétés thread. CurrentPrincipal et HttpContext. User correspondent.

Le runtime ASP.NET synchronise automatiquement ces valeurs de propriétés pour nous. Toutefois, cette synchronisation se produit après l’événement AuthenticateRequest, mais *avant* l’événement PostAuthenticateRequest. Par conséquent, lors de l’ajout d’un principal personnalisé dans l’événement PostAuthenticateRequest, nous devons être certain d’assigner manuellement le thread. CurrentPrincipal ou Else. CurrentPrincipal et HttpContext. User ne seront pas synchronisés. Pour une discussion plus détaillée sur ce problème, consultez [Context. User et thread. CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) .

### <a name="accessing-the-companyname-and-title-properties"></a>Accès aux propriétés CompanyName et title

Chaque fois qu’une demande arrive et est distribuée au moteur ASP.NET, l’application\_gestionnaire d’événements OnPostAuthenticateRequest dans global. asax se déclenche. Si la demande a été correctement authentifiée par FormsAuthenticationModule, le gestionnaire d’événements crée un nouvel objet CustomPrincipal avec un objet CustomIdentity basé sur le ticket d’authentification par formulaire. Avec cette logique en place, l’accès aux informations sur le nom et le titre de la société de l’utilisateur actuellement connecté est incroyablement simple.

Revenez à la page\_chargement du gestionnaire d’événements dans default. aspx, où, à l’étape 4, nous avons écrit du code pour récupérer le ticket d’authentification par formulaire et analysé la propriété UserData afin d’afficher le nom et le titre de la société de l’utilisateur. Avec les objets CustomPrincipal et CustomIdentity en cours d’utilisation, il n’est pas nécessaire d’analyser les valeurs en dehors de la propriété UserData du ticket. Au lieu de cela, il vous suffit d’obtenir une référence à l’objet CustomIdentity et d’utiliser ses propriétés CompanyName et title :

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons examiné comment personnaliser les paramètres du système d’authentification par formulaire par le biais de Web. config. Nous avons examiné comment l’expiration du ticket d’authentification est gérée et comment les protections de chiffrement et de validation sont utilisées pour protéger le ticket de l’inspection et de la modification. Enfin, nous avons abordé l’utilisation de la propriété UserData du ticket d’authentification pour stocker des informations utilisateur supplémentaires dans le ticket proprement dit, et comment utiliser des objets principal et Identity personnalisés pour exposer ces informations de manière plus conviviale pour les développeurs.

Ce didacticiel conclut notre examen de l’authentification par formulaire dans ASP.NET. Le didacticiel suivant commence notre parcours de l’infrastructure d’appartenance.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Discroisement de l’authentification par formulaire](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Explication : authentification par formulaire dans ASP.NET 2,0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Comment : protéger l’authentification par formulaire dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Gestion de la sécurité, de l’appartenance et des rôles de ASP.net Professional 2,0](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN : 978-0-7645-9698-8)
- [Sécurisation des contrôles de connexion](https://msdn.microsoft.com/library/ms178346.aspx)
- [Élément&gt; de l’authentification &lt;](https://msdn.microsoft.com/library/532aee0e.aspx)
- [L’élément &lt;Forms&gt; pour l’authentification &lt;&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [Élément &lt;machineKey&gt;](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Comprendre le ticket d’authentification par formulaire et le cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formation vidéo sur les rubriques contenues dans ce didacticiel

- [Comment modifier les propriétés d’authentification par formulaire](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Comment configurer et utiliser l’authentification sans cookie dans une application ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Réadressage de connexion par formulaires ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Configuration de clé personnalisée de la connexion par formulaires](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Ajouter des données personnalisées à la méthode d’authentification](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Utiliser des objets de principal personnalisés](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est *[Sams vous apprend vous-même ASP.NET 2,0 en 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott peut être contacté au [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Alicja maziarz. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Précédent](an-overview-of-forms-authentication-cs.md)
> [Suivant](security-basics-and-asp-net-support-vb.md)
