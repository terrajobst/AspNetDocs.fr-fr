---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Comment : ajouter des pages mobiles à votre application ASP.NET Web Forms/MVC | Microsoft Docs'
author: rick-anderson
description: Cette rubrique décrit les différentes façons de fournir des pages optimisées pour les appareils mobiles à partir de votre application ASP.NET Web Forms/MVC, et suggère l’architecture et...
ms.author: riande
ms.date: 01/20/2011
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 63c555358d06a9506bb5c8c993800c3307108192
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78572736"
---
# <a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Comment : ajouter des pages mobiles à votre application ASP.NET Web Forms/MVC

> **S’applique à**
> 
> - ASP.NET Web Forms version 4,0
> - ASP.NET MVC version 3,0
> 
> **Résumé**
> 
> Ce guide décrit les différentes façons de fournir des pages optimisées pour les appareils mobiles à partir de votre application ASP.NET Web Forms/MVC, et suggère des problèmes d’architecture et de conception à prendre en compte lors du ciblage d’un large éventail d’appareils. Ce document explique également pourquoi les contrôles mobiles ASP.NET de ASP.NET 2,0 à 3,5 sont désormais obsolètes et aborde certaines alternatives modernes.

## <a name="contents"></a>Contenu

- Présentation
- Options d’architecture
- Détection des navigateurs et des appareils
- Comment les applications Web Forms ASP.NET peuvent présenter des pages spécifiques aux appareils mobiles
- Comment les applications ASP.NET MVC peuvent présenter des pages spécifiques à mobile
- Ressources supplémentaires

Pour obtenir des exemples de code téléchargeables illustrant les techniques de ce livre blanc pour ASP.NET Web Forms et MVC, consultez [Mobile Apps & sites avec ASP.net](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Présentation

Appareils mobiles : smartphones, téléphones et tablettes : continuez à augmenter leur popularité en tant que moyen d’accéder au Web. Pour de nombreux développeurs Web et entreprises orientées Web, cela signifie qu’il est de plus en plus important de fournir une expérience de navigation exceptionnelle aux visiteurs qui utilisent ces appareils.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Comment les versions antérieures de ASP.NET prenait en charge les navigateurs mobiles

Les versions 2,0 à 3,5 comprenaient les *contrôles mobiles ASP.net*: un ensemble de contrôles serveur pour les périphériques mobiles dans l’assembly *System. Web. mobile. dll* et l’espace de noms *System. Web. UI. MobileControls* . L’assembly est toujours inclus dans ASP.NET 4, mais il est déconseillé. Les développeurs sont invités à migrer vers des approches plus modernes, telles que celles décrites dans ce document.

La raison pour laquelle les contrôles mobiles ASP.NET ont été marqués comme obsolètes est que leur conception est axée sur les téléphones mobiles qui étaient courantes autour de 2005 et des versions antérieures. Les contrôles sont principalement conçus pour restituer le balisage WML ou cHTML (au lieu du code HTML normal) pour les navigateurs WAP de cette ère. Mais WAP, WML et cHTML ne sont plus pertinents pour la plupart des projets, car HTML est devenu le langage de balisage omniprésent pour les navigateurs mobiles et de bureau.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Défis actuels de la prise en charge des appareils mobiles

Bien que les navigateurs mobiles prennent désormais presque en charge le format HTML, vous serez toujours confronté à de nombreux défis quand vous cherchiez à créer des expériences de navigation mobile exceptionnelles :

- ***Taille*** de l’écran : les périphériques mobiles varient considérablement en termes de format et leurs écrans sont souvent beaucoup plus petits que les moniteurs de bureau. Par conséquent, vous devrez peut-être concevoir des mises en page entièrement différentes pour celles-ci.
- ***Méthodes d’entrée*** : certains appareils ont des pavés tactiles, d’autres ont des styluses, d’autres utilisent Touch. Vous devrez peut-être prendre en compte plusieurs mécanismes de navigation et méthodes d’entrée de données.
- ***Conformité aux normes*** : de nombreux navigateurs mobiles ne prennent pas en charge les dernières normes html, CSS et JavaScript.
- ***Bande passante*** : les performances du réseau de données cellulaires varient d’un caractère à l’autres, et certains utilisateurs finaux sont sur les tarifs facturés au mégaoctet.

Il n’existe pas de solution unique et adaptée à tous. votre application doit s’afficher et se comporter différemment en fonction de l’appareil qui y accède. En fonction du niveau de prise en charge mobile que vous souhaitez, il peut s’agir d’un défi plus important pour les développeurs Web que les « conflits de navigateur ».

Les développeurs qui abordent la prise en charge des navigateurs mobiles pour la première fois sont souvent considérés comme étant importants pour prendre en charge les smartphones les plus récents et les plus sophistiqués (par exemple, Windows Phone 7, iPhone ou Android), peut-être parce que les développeurs sont souvent personnellement propriétaires de tels appareil. Toutefois, les téléphones les plus chers sont toujours extrêmement populaires et leurs propriétaires les utilisent pour naviguer sur le Web, en particulier dans les pays où les téléphones mobiles sont plus faciles à obtenir qu’une connexion large bande. Votre entreprise doit décider de la plage d’appareils à prendre en compte en considérant ses clients probables. Si vous créez une brochure en ligne pour un spa d’intégrité de luxe, vous pouvez prendre une décision commerciale uniquement pour cibler des smartphones avancés, tandis que si vous créez un système de réservation de tickets pour un Cinema, vous devrez probablement tenir compte des visiteurs avec des fonctionnalités moins puissantes numéros.

## <a name="architectural-options"></a>Options d’architecture

Avant d’accéder aux détails techniques spécifiques de ASP.NET Web Forms ou MVC, sachez que les développeurs Web en général disposent de trois options principales pour la prise en charge des navigateurs mobiles :

1. ***Ne rien faire :*** Il vous suffit de créer une application Web standard et orientée bureau, et de vous appuyer sur des navigateurs mobiles pour le rendre acceptable. 

    - **Avantage**: il s’agit de l’option la plus économique pour mettre en œuvre et gérer, pas de travail supplémentaire
    - **Inconvénient**: donne le pire expérience de l’utilisateur final : 

        - Les derniers smartphones peuvent afficher votre code HTML tout aussi bien qu’un navigateur de bureau, mais les utilisateurs sont toujours obligés de zoomer et de faire défiler horizontalement et verticalement pour consommer votre contenu sur un petit écran. Cette solution est loin d’être optimale.
        - Les appareils plus anciens et les téléphones de fonctionnalités risquent de ne pas pouvoir afficher votre balisage de manière satisfaisante.
        - Même sur les derniers appareils tablettes (dont les écrans peuvent être aussi volumineux que les écrans des ordinateurs portables), différentes règles d’interaction s’appliquent. Les entrées tactiles fonctionnent mieux avec les plus grands boutons et liens, et il n’existe aucun moyen de pointer le curseur de la souris sur un menu volant.
2. ***Résoudre le problème sur le client* :** avec une utilisation soigneuse de CSS et une [amélioration progressive](http://en.wikipedia.org/wiki/Progressive_enhancement) , vous pouvez créer des balises, des styles et des scripts qui s’adaptent à n’importe quel navigateur qui les exécute. Par exemple, avec les [requêtes de média CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), vous pouvez créer une disposition à plusieurs colonnes qui convertit une disposition de colonne unique sur les appareils dont les écrans sont plus étroits qu’un seuil choisi. 

    - **Avantage**: optimise le rendu pour l’appareil spécifique en cours d’utilisation, même pour les futurs appareils inconnus en fonction de l’écran et des caractéristiques d’entrée dont il dispose.
    - **Avantage**: vous permet de partager facilement la logique côté serveur sur tous les types d’appareils : duplication minimale du code ou de l’effort
    - **Inconvénient**: les appareils mobiles sont tellement différents des appareils de bureau que vous pouvez vraiment souhaiter que vos pages mobiles soient complètement différentes de celles de votre bureau, en présentant différentes informations. Ces variations peuvent être inefficaces ou impossibles à réaliser de manière fiable par le biais de CSS uniquement, en tenant compte notamment de la façon dont les appareils plus anciens interprètent les règles CSS. Cela est particulièrement vrai pour les attributs CSS 3.
    - **Inconvénient**: ne fournit pas de prise en charge pour divers flux de travail et logique côté serveur pour différents appareils. Vous ne pouvez pas, par exemple, implémenter un flux de travail de validation de panier d’achat simplifié pour les utilisateurs mobiles au moyen de CSS uniquement.
    - **Inconvénient**: utilisation inefficace de la bande passante. Vous devrez peut-être transmettre le balisage et les styles qui s’appliquent à tous les appareils possibles, même si l’appareil cible n’utilise qu’un sous-ensemble de ces informations.
3. ***Résoudre le problème sur le serveur* :** Si votre serveur connaît l’appareil qui y accède, ou au moins les caractéristiques de ce périphérique, telles que sa taille d’écran et sa méthode d’entrée, et s’il s’agit d’un périphérique mobile, il peut exécuter une logique différente et générer une autre balise HTML. 

    - **Avantage :** Flexibilité maximale. Il n’y a aucune limite à la variation de la logique côté serveur pour les appareils mobiles ou à l’optimisation de votre balisage pour la mise en page souhaitée spécifique à l’appareil.
    - **Avantage :** Utilisation efficace de la bande passante. Vous devez uniquement transmettre les informations de balisage et de style que l’appareil cible va utiliser.
    - **Inconvénient :** Impose parfois des répétitions d’effort ou de code (par exemple, en créant des copies similaires, mais légèrement différentes, de vos pages Web Forms ou vues MVC). Dans la mesure du possible, vous allez factoriser la logique commune dans une couche ou un service sous-jacent, mais il se peut que certaines parties du code ou du balisage de l’interface utilisateur doivent être dupliquées, puis maintenues en parallèle.
    - **Inconvénient :** La détection des appareils n’est pas simple. Elle nécessite une liste ou une base de données de types d’appareils connus et leurs caractéristiques (qui ne sont pas toujours parfaitement à jour) et n’est pas garantie de correspondre précisément à chaque demande entrante. Ce document décrit quelques options et leurs pièges plus tard.

Pour obtenir les meilleurs résultats, la plupart des développeurs doivent combiner les options (2) et (3). Les différences stylistiques mineures sont mieux adaptées sur le client à l’aide de CSS ou même de JavaScript, tandis que les principales différences entre les données, le flux de travail ou le balisage sont implémentées le plus efficacement dans le code côté serveur.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Ce document se concentre sur les techniques côté serveur

Étant donné que ASP.NET Web Forms et MVC sont tous deux essentiellement des technologies côté serveur, ce livre blanc se concentre sur les techniques côté serveur qui vous permettent de produire différentes balises et logiques pour les navigateurs mobiles. Bien entendu, vous pouvez également combiner cela avec toute technique côté client (par exemple, les requêtes de média CSS 3, le JavaScript d’amélioration progressive), mais c’est plus une question de conception Web que le développement ASP.NET.

## <a name="browser-and-device-detection"></a>Détection des navigateurs et des appareils

La condition essentielle pour toutes les techniques côté serveur pour la prise en charge des appareils mobiles consiste à savoir quel appareil votre visiteur utilise. En fait, il est même préférable de connaître le fabricant et le numéro de modèle de cet appareil en connaissant les *caractéristiques* de l’appareil. Les caractéristiques peuvent inclure :

- S’agit-il d’un appareil mobile ?
- Méthode d’entrée (souris/clavier, toucher, clavier, manette de jeu,...)
- Taille de l’écran (physiquement et en pixels)
- Formats de données et de média pris en charge
- Etc.

Il est préférable de prendre des décisions basées sur des caractéristiques que le numéro de modèle, car vous serez alors mieux armé pour gérer les futurs appareils.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Utilisation d’ASP. Prise en charge intégrée de la détection du navigateur par le réseau

ASP.NET Web Forms et les développeurs MVC peuvent découvrir immédiatement les caractéristiques importantes d’un navigateur visité en inspectant les propriétés de l’objet *Request. Browser* . Par exemple, consultez

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...et beaucoup d’autres

En arrière-plan, la plateforme ASP.NET correspond à l’en-tête HTTP UA ( *user-agent* ) entrant par rapport aux expressions régulières dans un ensemble de fichiers XML de définition de navigateur. Par défaut, la plateforme comprend des définitions pour de nombreux périphériques mobiles courants et vous pouvez ajouter des fichiers de définition de navigateur personnalisés pour d’autres que vous souhaitez reconnaître. Pour plus d’informations, consultez la page MSDN [ASP.net les contrôles serveur Web et les fonctionnalités du navigateur](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Utilisation de la base de données d’appareils WURFL via 51Degrees.mobi Foundation

Pendant l’ASP. La prise en charge intégrée de la détection du navigateur de .net suffira à de nombreuses applications, mais il existe deux cas principaux dans lesquels il peut ne pas suffire :

- ***Vous souhaitez reconnaître les appareils les plus récents***(sans créer manuellement de fichiers de définition de navigateur pour eux). Notez que les fichiers de définition de navigateur de .NET 4 ne sont pas suffisamment récents pour reconnaître Windows Phone 7, les téléphones Android, les navigateurs mobiles Opera ou Apple iPad.
- ***Vous avez besoin d’informations plus détaillées sur les fonctionnalités de l’appareil***. Vous devrez peut-être en savoir plus sur la méthode d’entrée d’un appareil (par exemple, tactile ou le clavier) ou sur les formats audio pris en charge par le navigateur. Ces informations ne sont pas disponibles dans les fichiers de définition de navigateur standard.

Le [projet WURFL ( *Wireless Universal Resource File* )](http://wurfl.sourceforge.net/) gère des informations plus récentes et détaillées sur les appareils mobiles utilisés aujourd’hui.

La bonne nouvelle pour les développeurs .NET est qu’ASP. La fonctionnalité de détection du navigateur .net est extensible. il est donc possible de l’améliorer pour surmonter ces problèmes. Par exemple, vous pouvez ajouter la bibliothèque open source [*51Degrees.mobi Foundation*](https://github.com/51Degrees/dotNET-Device-Detection) à votre projet. Il s’agit d’un fournisseur de fonctionnalités ASP.NET IHttpModule ou Browser (utilisable sur les applications Web Forms et MVC), qui lit directement les données WURFL et les raccorde à ASP. Mécanisme de détection de navigateur intégré au réseau. Une fois que vous avez installé le module, *Request. Browser* contient soudainement des informations plus précises et détaillées : il reconnaît correctement un grand nombre des appareils mentionnés précédemment et répertorie leurs fonctionnalités (y compris des fonctionnalités supplémentaires telles que la méthode d’entrée). Pour plus d’informations, consultez la documentation du projet.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Comment Web Forms applications peuvent présenter des pages spécifiques à mobile

Par défaut, voici comment une nouvelle Web Forms application s’affiche sur les appareils mobiles courants :

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

En clair, aucune disposition n’est très conviviale : cette page a été conçue pour une grande analyse orientée paysage, et non pour un petit écran orienté portrait. Que pouvez-vous faire ?

Comme nous l’avons vu plus haut dans ce document, il existe de nombreuses façons de personnaliser vos pages pour les appareils mobiles. Certaines techniques sont basées sur un serveur, d’autres s’exécutent sur le client.

### <a name="creating-a-mobile-specific-master-page"></a>Création d’une page maître mobile

En fonction de vos besoins, vous pourrez peut-être utiliser le même Web Forms pour tous les visiteurs, mais avoir deux pages maîtres distinctes : une pour les visiteurs du bureau, une autre pour les visiteurs mobiles. Cela vous donne la possibilité de modifier la feuille de style CSS ou votre balise HTML de niveau supérieur pour l’adapter aux appareils mobiles, sans avoir à vous obliger à dupliquer toute logique de page.

C’est facile à faire. Par exemple, vous pouvez ajouter un gestionnaire de préinit comme celui-ci à un formulaire Web :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Créez maintenant une page maître appelée mobile. Master dans le dossier de niveau supérieur de votre application, et elle sera utilisée lorsqu’un appareil mobile est détecté. Votre page maître mobile peut faire référence à une feuille de style CSS spécifique à mobile si nécessaire. Les visiteurs du Bureau verront toujours votre page maître par défaut, pas la page mobile.

### <a name="creating-independent-mobile-specific-web-forms"></a>Création de Web Forms indépendantes des appareils mobiles

Pour une flexibilité maximale, vous pouvez aller plus loin que le simple fait d’avoir des pages maîtres distinctes pour différents types d’appareils. Vous pouvez implémenter deux *jeux de pages de Web Forms entièrement distincts* , un ensemble pour les navigateurs de bureau, un autre pour les appareils mobiles. Cela fonctionne mieux si vous souhaitez présenter des informations ou des flux de travail très différents aux visiteurs mobiles. Le reste de cette section décrit cette approche en détail.

En supposant que vous disposiez déjà d’une application Web Forms conçue pour les navigateurs de bureau, le moyen le plus simple consiste à créer un sous-dossier appelé « mobile » dans votre projet et à y créer vos pages mobiles. Vous pouvez créer un sous-site entier, avec ses propres pages maîtres, feuilles de style et pages, à l’aide des mêmes techniques que celles que vous utiliseriez pour n’importe quelle autre application Web Forms. Vous n’avez pas nécessairement besoin de produire un équivalent mobile pour *chaque* page de votre site de bureau. vous pouvez choisir le sous-ensemble de fonctionnalités le plus approprié pour les visiteurs mobiles.

Vos pages mobiles peuvent partager des ressources statiques courantes (telles que des images, des fichiers JavaScript ou CSS) avec vos pages normales si vous le souhaitez. Étant donné que votre dossier « mobile » n’est *pas* marqué comme une application distincte lorsqu’il est hébergé dans IIS (il s’agit simplement d’un sous-dossier simple de pages Web Forms), il partage également les mêmes données de configuration, de session et d’autres infrastructures que les pages de votre bureau.

> [!NOTE]
> Étant donné que cette approche implique généralement une duplication de code (les pages mobiles sont susceptibles de partager des similarités avec les pages du bureau), il est important de prendre en compte toute logique métier ou code d’accès aux données commun dans une couche ou un service sous-jacent partagé. Dans le cas contraire, vous doublez l’effort de création et de maintenance de votre application.

#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Redirection des visiteurs mobiles vers vos pages mobiles

Il est souvent pratique de rediriger les visiteurs des appareils mobiles vers les pages mobiles uniquement lors de la *première* requête de leur session de navigation (et non à chaque demande dans leur session), car :

- Vous pouvez ensuite facilement permettre aux visiteurs mobiles d’accéder aux pages de votre bureau, le cas échéant, de simplement placer un lien sur votre page maître qui passe à « version du bureau ». Le visiteur n’est pas redirigé vers une page mobile, car il ne s’agit plus de la première demande dans sa session.
- Cela évite le risque d’interférence avec les demandes pour toutes les ressources dynamiques partagées entre les composants mobiles et de bureau de votre site (par exemple, si vous avez un formulaire Web commun qui s’affiche à la fois dans un IFRAME ou certains gestionnaires Ajax)

Pour ce faire, vous pouvez placer votre logique de redirection dans une **Session\_méthode Start** . Par exemple, ajoutez la méthode suivante à votre fichier Global.asax.cs :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configuration de l’authentification par formulaire pour respecter vos pages mobiles

Notez que l’authentification par formulaire fait certaines suppositions quant à l’emplacement où elle peut rediriger les visiteurs pendant et après le processus d’authentification :

- Lorsqu’un utilisateur doit être authentifié, l’authentification par formulaire les redirige vers la page de connexion du bureau, qu’il s’agisse d’un ordinateur de bureau ou d’un utilisateur mobile (parce qu’il n’a qu’un *concept d’URL* de connexion). En supposant que vous souhaitiez appliquer un style différent à votre page de connexion mobile, vous devez améliorer la page de connexion de votre bureau afin qu’il redirige les utilisateurs mobiles vers une page de connexion mobile distincte. Par exemple, ajoutez le code suivant à la page de connexion du **Bureau** code-behind : 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Une fois que l’utilisateur a réussi à se connecter, l’authentification par formulaire les redirige par défaut vers la page d’hébergement de votre bureau (car elle n’a qu’un concept d' *une* page par défaut). Vous devez améliorer votre page de connexion mobile afin qu’elle redirige vers la page d’hébergement mobile après une connexion réussie. Par exemple, ajoutez le code suivant à votre page de connexion **mobile** code-behind : 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Ce code suppose que votre page a un contrôle serveur de connexion appelé LoginUser, comme dans le modèle de projet par défaut.

### <a name="working-with-output-caching"></a>Utilisation de la mise en cache de sortie

Si vous utilisez la mise en cache de sortie, Méfiez-vous que, par défaut, un utilisateur de bureau peut consulter une certaine URL (provoquant la mise en cache de sa sortie), puis un utilisateur mobile qui reçoit ensuite la sortie du bureau mis en cache. Cet avertissement s’applique si vous faites simplement varier votre page maître par type d’appareil ou si vous implémentez des Web Forms entièrement séparés par type d’appareil.

Pour éviter ce problème, vous pouvez demander à ASP.NET de faire varier l’entrée du cache selon que le visiteur utilise ou non un appareil mobile. Ajoutez un paramètre VaryByCustom à la déclaration OutputCache de votre page comme suit :

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Ensuite, définissez *IsMobileDevice* en tant que paramètre de cache personnalisé en ajoutant la substitution de méthode suivante à votre fichier global.asax.cs :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Cela permet de s’assurer que les visiteurs mobiles de la page ne reçoivent pas la sortie placée précédemment dans le cache par un visiteur du bureau.

### <a name="a-working-example"></a>Exemple fonctionnel

Pour voir ces techniques en action, téléchargez les [exemples de code de ce livre blanc](https://docs.microsoft.com/aspnet/mobile/overview). L’exemple d’application Web Forms redirige automatiquement les utilisateurs mobiles vers un ensemble de pages spécifiques à mobile dans un sous-dossier appelé mobile. Le balisage et le style de ces pages sont mieux optimisés pour les navigateurs mobiles, comme vous pouvez le constater dans les captures d’écran suivantes :

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Pour plus d’informations sur l’optimisation de vos balises et CSS pour les navigateurs mobiles, consultez la section « stylisation des pages mobiles pour les navigateurs mobiles » plus loin dans ce document.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Comment les applications ASP.NET MVC peuvent présenter des pages spécifiques à mobile

Étant donné que le modèle Model-View-Controller dissocie la logique d’application (dans les contrôleurs) de la logique de présentation (dans les affichages), vous pouvez choisir l’une des approches suivantes pour gérer la prise en charge des appareils mobiles dans le code côté serveur :

1. ***utiliser les mêmes contrôleurs et vues pour les navigateurs de bureau et mobiles, mais afficher les vues avec différentes dispositions Razor en fonction du type d’appareil *.** Cette option fonctionne mieux si vous affichez des données identiques sur tous les appareils, mais que vous souhaitez simplement fournir des feuilles de style CSS différentes ou modifier quelques éléments HTML de niveau supérieur pour les appareils mobiles.
2. ***Utilisez les mêmes contrôleurs pour les navigateurs de bureau et mobiles, mais Affichez des vues différentes selon le type d’appareil***. Cette option fonctionne mieux si vous affichez approximativement les mêmes données et fournissez les mêmes flux de travail pour les utilisateurs finaux, mais que vous souhaitez afficher des balises HTML très différentes pour les adapter à l’appareil utilisé.
3. ***créer des zones distinctes pour les navigateurs de bureau et mobiles, en implémentant des contrôleurs et des vues indépendants pour chaque *.** Cette option fonctionne mieux si vous affichez des écrans très différents, contenant des informations différentes et conduisant l’utilisateur à l’aide de différents flux de travail optimisés pour leur type d’appareil. Cela peut signifier une répétition de code, mais vous pouvez réduire cela en factorisant une logique commune dans une couche ou un service sous-jacent.

Si vous souhaitez prendre la **première** option et modifier uniquement la disposition Razor par type d’appareil, c’est très facile. Il vous suffit de modifier votre \_fichier ViewStart. cshtml comme suit :

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Vous pouvez maintenant créer une disposition spécifique à mobile appelée \_LayoutMobile. cshtml avec une structure de page et des règles CSS optimisées pour les appareils mobiles.

Si vous souhaitez que la **deuxième** option affiche des affichages entièrement différents en fonction du type d’appareil du visiteur, consultez le billet [de blog de Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Le reste de ce document se concentre sur la **troisième** option : la création de contrôleurs *et* de vues distincts pour les appareils mobiles. vous pouvez ainsi contrôler exactement le sous-ensemble de fonctionnalités qui sont proposées aux visiteurs mobiles.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Configuration d’une zone mobile au sein de votre application ASP.NET MVC

Vous pouvez ajouter une zone appelée « mobile » à une application ASP.NET MVC existante de manière normale : cliquez avec le bouton droit sur le nom de votre projet dans Explorateur de solutions, puis choisissez Ajouter à la zone. Vous pouvez ensuite ajouter des contrôleurs et des vues comme vous le feriez pour n’importe quelle autre zone au sein d’une application MVC ASP.NET. Par exemple, ajoutez à votre zone mobile un nouveau contrôleur appelé HomeController pour agir en tant que page d’accueil pour les visiteurs mobiles.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Vérification que l’URL/mobile atteint la page d’accueil mobile

Si vous souhaitez que l’URL/mobile atteigne l’action d’index sur le HomeController à l’intérieur de votre zone mobile, vous devez apporter deux petites modifications à votre configuration de routage. Tout d’abord, mettez à jour votre classe MobileAreaRegistration pour que le HomeController soit le contrôleur par défaut dans votre zone mobile, comme illustré dans le code suivant :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Cela signifie que la page d’accueil mobile se trouve à présent dans/mobile, plutôt que/Mobile/Home, car « Accueil » est maintenant le nom de contrôleur par défaut implicite pour les pages mobiles.

Ensuite, Notez qu’en ajoutant un deuxième HomeController à votre application (par exemple, le point de vue mobile, en plus de l’ordinateur de bureau existant), vous aurez endommagé votre page d’accueil de bureau standard. Elle échoue avec l’erreur «*plusieurs types trouvés qui correspondent au contrôleur nommé «orig »* ». Pour résoudre ce cas, mettez à jour votre configuration de routage de niveau supérieur (dans Global.asax.cs) pour spécifier que votre HomeController de bureau doit prendre la priorité lorsqu’il existe une ambiguïté :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

L’erreur disparaît alors et l’URL http :\/\/*yoursite*/atteindra la page d’accueil du bureau et http :\/\/*yoursite*/mobile/atteindra la page d’accueil mobile.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Redirection des visiteurs mobiles vers votre zone mobile

Il existe de nombreux points d’extensibilité différents dans ASP.NET MVC, de sorte qu’il existe de nombreuses façons d’injecter la logique de redirection. Une option intéressante consiste à créer un attribut de filtre, [RedirectMobileDevicesToMobileArea], qui effectue une redirection si les conditions suivantes sont remplies :

1. Il s’agit de la première requête dans la session de l’utilisateur (par exemple, session. IsNewSession est égal à true)
2. La requête provient d’un navigateur mobile (par exemple, Request. Browser. IsMobileDevice est égal à true)
3. L’utilisateur n’a pas encore demandé de ressource dans la zone mobile (autrement dit, la partie *chemin d’accès* de l’URL ne commence pas par/mobile)

L’exemple téléchargeable inclus dans ce livre blanc inclut une implémentation de cette logique. Elle est implémentée en tant que filtre d’autorisation, dérivé de AuthorizeAttribute, ce qui signifie qu’elle peut fonctionner correctement même si vous utilisez la mise en cache de sortie (sinon, si un visiteur du Bureau accède pour la première fois à une certaine URL, la sortie du Bureau peut être mise en cache, puis traitée à visiteurs itinérants suivants).

Comme il s’agit d’un filtre, vous pouvez choisir de l’appliquer à des contrôleurs et des actions spécifiques, par exemple,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… vous pouvez également l’appliquer à tous les contrôleurs et actions sous la forme d’un *filtre global* MVC 3 dans votre fichier global.asax.cs :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

L’exemple téléchargeable montre également comment vous pouvez créer des sous-classes de cet attribut qui redirigent vers des emplacements spécifiques au sein de votre zone mobile. Cela signifie, par exemple, que vous pouvez :

- Inscrivez un filtre global comme indiqué ci-dessus, qui envoie par défaut les visiteurs mobiles à la page d’accueil mobile.
- Appliquez également un filtre [RedirectMobileDevicesToMobileProductPage] spécial à une action « afficher le produit » qui amène les visiteurs mobiles à la version mobile de n’importe quelle page produit demandée.
- Appliquer également d’autres sous-classes spéciales du filtre à des actions spécifiques, en redirigeant les visiteurs mobiles vers la page mobile équivalente

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configuration de l’authentification par formulaire pour respecter vos pages mobiles

Si vous utilisez l’authentification par formulaire, vous devez noter que lorsqu’un utilisateur doit se connecter, il redirige automatiquement l’utilisateur vers une seule URL de « connexion » spécifique, qui est par défaut **/Account/Logon**. Cela signifie que les utilisateurs mobiles peuvent être redirigés vers l’action « connexion » de votre bureau.

Pour éviter ce problème, ajoutez une logique à l’action « connexion » de votre bureau afin qu’il redirige les utilisateurs mobiles vers une action « connexion » mobile. Si vous utilisez le modèle d’application ASP.NET MVC par défaut, mettez à jour l’action d’ouverture de session de AccountController comme suit :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… puis implémentez une action « ouvrir une session » propre au mobile sur un contrôleur appelé AccountController dans votre zone mobile.

### <a name="working-with-output-caching"></a>Utilisation de la mise en cache de sortie

Si vous utilisez le filtre [OutputCache], vous devez forcer l’entrée du cache à varier selon le type d’appareil. Par exemple, écrivez :

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Ensuite, ajoutez la méthode suivante à votre fichier Global.asax.cs :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Cela permet de s’assurer que les visiteurs mobiles de la page ne reçoivent pas la sortie placée précédemment dans le cache par un visiteur du bureau.

### <a name="a-working-example"></a>Exemple fonctionnel

Pour voir ces techniques en action, téléchargez les [exemples associés au code de ce livre blanc](https://docs.microsoft.com/aspnet/mobile/overview). L’exemple comprend une application ASP.NET MVC 3 (version Release Candidate) améliorée pour la prise en charge des appareils mobiles à l’aide des méthodes décrites ci-dessus.

## <a name="further-guidance-and-suggestions"></a>Conseils et suggestions supplémentaires

La discussion suivante s’applique à la fois aux Web Forms et aux développeurs MVC qui utilisent les techniques décrites dans ce document.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Amélioration de votre logique de redirection à l’aide de 51Degrees.mobi Foundation

La logique de redirection indiquée dans ce document peut être parfaitement suffisante pour votre application, mais elle ne fonctionne pas si vous devez désactiver les sessions, ou avec des navigateurs mobiles qui rejettent les cookies (ces sessions ne peuvent pas avoir de sessions), car elles ne savent pas si une demande donnée est le premier de ce visiteur.

Vous avez déjà appris comment la base open source 51Degrees.mobi Foundation peut améliorer la précision d’ASP. Détection du navigateur du réseau. Il offre également une capacité intégrée de rediriger les visiteurs mobiles vers des emplacements spécifiques configurés dans Web. config. Il peut fonctionner sans dépendre des sessions ASP.NET (et donc des cookies) en stockant un journal temporaire des hachages des en-têtes HTTP et des adresses IP des visiteurs. il sait donc si chaque demande est la première d’un centre de données donné.

L’élément suivant ajouté à la section fiftyOne du fichier Web. config redirige la première requête à partir d’un appareil mobile détecté vers la page ~/Mobile/Default.aspx. Les requêtes sur les pages du dossier mobile ne sont *pas* redirigées, quel que soit le type d’appareil. Si le périphérique mobile a été inactif pendant une période de 20 minutes ou plus, l’appareil sera oublié et les demandes ultérieures seront traitées comme nouvelles dans le cadre de la redirection.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Pour plus d’informations, consultez [la documentation 51Degrees.mobi Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Vous *pouvez* utiliser la fonctionnalité de redirection de 51Degrees.mobi Foundation sur les applications ASP.NET MVC, mais vous devrez définir la configuration de la redirection en termes d’URL brutes, non en termes de paramètres de routage ou en plaçant des filtres MVC sur les actions. Cela est dû au fait que (au moment de l’écriture) 51Degrees.mobi Foundation ne reconnaît pas les filtres ou le routage.

### <a name="disabling-transcoders-and-proxy-servers"></a>Désactivation des transcodeurs et des serveurs proxy

Les opérateurs de réseau mobile ont deux objectifs généraux dans leur approche de l’Internet mobile :

1. Fournir le contenu le plus pertinent possible
2. Maximiser le nombre de clients pouvant partager la bande passante réseau limitée

Étant donné que la plupart des pages Web ont été conçues pour des écrans de grande taille et des connexions haut débit fixe, de nombreux opérateurs utilisent des *transcodeurs* ou des *serveurs proxy* qui modifient dynamiquement le contenu Web. Ils peuvent modifier votre balisage HTML ou CSS pour les adapter à des écrans plus petits (en particulier pour les « téléphones fonctionnalité » qui manquent de puissance de traitement pour gérer des dispositions complexes) et peuvent recompresser vos images (ce qui réduit considérablement leur qualité) pour améliorer les vitesses de remise des pages.

Toutefois, si vous avez pris le temps de créer une version optimisée pour le mobile de votre site, vous ne souhaiterez sans doute pas que l’opérateur de réseau interfère avec lui. Vous pouvez ajouter la ligne suivante à la page\_événement de chargement dans n’importe quel formulaire Web ASP.NET :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Ou, pour un contrôleur ASP.NET MVC, vous pouvez ajouter le remplacement de méthode suivant afin qu’il s’applique à toutes les actions sur ce contrôleur :

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Le message HTTP qui en résulte informe les transcodeurs conformes au W3C et les proxys de ne pas modifier le contenu. Bien entendu, il n’y a aucune garantie que les opérateurs de réseau mobile respecteront ce message.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Définition de styles pour les pages mobiles pour les navigateurs mobiles

Ce document décrit en détail les genres de balises HTML qui fonctionnent correctement ou les techniques de conception Web qui optimisent la convivialité sur des appareils particuliers. C’est à vous de trouver une disposition suffisamment simple, optimisée pour un écran de taille mobile, sans utiliser des astuces de positionnement HTML ou CSS non fiables. Une technique importante qui mérite d’être mentionnée est cependant la *balise méta*de la fenêtre d’affichage.

Certains navigateurs mobiles modernes, dans un effort d’affichage des pages Web destinées aux moniteurs de bureau, effectuent le rendu de la page sur un canevas virtuel, également appelé « Viewport » (par exemple, la fenêtre d’affichage virtuelle est de 980 pixels de grande largeur sur iPhone, et 850 pixels de largeur sur Opera Mobile par défaut), puis mettre à l’échelle le résultat en fonction des pixels physiques de l’écran. L’utilisateur peut ensuite effectuer un zoom avant et un panoramique sur cette fenêtre d’affichage. Cela présente l’avantage de permettre au navigateur d’afficher la page dans sa disposition prévue, mais il présente également l’inconvénient de forcer le zoom et la panoramisation, ce qui est peu pratique pour l’utilisateur. Si vous concevez pour un appareil mobile, il est préférable de concevoir un écran étroit afin qu’aucun zoom ou défilement horizontal ne soit nécessaire.

Pour indiquer au navigateur mobile la largeur de la fenêtre d’affichage, vous pouvez utiliser la balise méta non standard *Viewport* . Par exemple, si vous ajoutez le code suivant à la section d’en-tête de votre page,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… la prise en charge des navigateurs smartphone aura pour disposition la page sur un canevas virtuel de 480 pixels de largeur. Cela signifie que si vos éléments HTML définissent leurs largeurs en pourcentage, les pourcentages sont interprétés par rapport à cette largeur de 480 pixels, et non à la largeur de la fenêtre d’affichage par défaut. Par conséquent, l’utilisateur est moins susceptible d’avoir à effectuer un zoom et un panoramique horizontalement, ce qui améliore considérablement l’expérience de navigation mobile.

Si vous souhaitez que la largeur de la fenêtre d’affichage corresponde aux pixels physiques de l’appareil, vous pouvez spécifier les éléments suivants :

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Pour que cela fonctionne correctement, vous ne devez pas forcer explicitement les éléments à dépasser cette largeur (par exemple, à l’aide d’un attribut *Width* ou d’une propriété CSS), sinon le navigateur sera contraint d’utiliser une fenêtre d’affichage plus grande, quelle que soit la valeur. Voir aussi : [plus de détails sur la balise Viewport non standard](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

La plupart des smartphones modernes prennent en charge l' *orientation double*: ils peuvent être en mode portrait ou paysage. Il est donc important de ne pas faire d’hypothèses sur la largeur de l’écran en pixels. Ne supposez même pas que la largeur de l’écran est fixe, car l’utilisateur peut réorienter son appareil pendant qu’il se trouve sur votre page.

Les appareils Windows Mobile et BlackBerry plus anciens peuvent également accepter les balises meta suivantes dans l’en-tête de page pour leur indiquer que le contenu a été optimisé pour les appareils mobiles et ne doit donc pas être transformé.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Ressources supplémentaires

Pour obtenir une liste des émulateurs et simulateurs d’appareils mobiles que vous pouvez utiliser pour tester votre application Web mobile ASP.NET, consultez la page [simuler des appareils mobiles populaires à des fins de test](../mobile/device-simulators.md).

## <a name="credits"></a>Crédits

- Auteur principal : Steven Sanderson
- Réviseurs/rédacteurs de contenu supplémentaires : James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
