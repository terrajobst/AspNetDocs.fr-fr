---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
title: Vue d’ensemble de l’authentificationC#par formulaire () | Microsoft Docs
author: rick-anderson
description: Création d’itinéraires personnalisés
ms.author: riande
ms.date: 01/14/2008
ms.assetid: de2d65b9-aadc-42ba-abe1-4e87e66521a0
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 009c3f84e00d648ede4a15e530ceac2d23e01eec
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74620753"
---
# <a name="an-overview-of-forms-authentication-c"></a>Vue d’ensemble de l’authentificationC#par formulaire ()

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_cs.pdf)

> Dans ce didacticiel, nous allons transformer une simple discussion en implémentation. Nous examinerons en particulier l’implémentation de l’authentification par formulaire. L’application Web que nous commençons à construire dans ce didacticiel continuera à être générée dans les didacticiels suivants, à mesure que nous passerons de l’authentification par formulaire simple à l’appartenance et aux rôles.
> 
> Pour plus d’informations sur cette rubrique, consultez la vidéo suivante : [utilisation de l’authentification par formulaire de base dans ASP.net](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md).

## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](security-basics-and-asp-net-support-cs.md) , nous avons abordé les diverses options d’authentification, d’autorisation et de compte d’utilisateur fournies par ASP.net. Dans ce didacticiel, nous allons transformer une simple discussion en implémentation. Nous examinerons en particulier l’implémentation de l’authentification par formulaire. L’application Web que nous commençons à construire dans ce didacticiel continuera à être générée dans les didacticiels suivants, à mesure que nous passerons de l’authentification par formulaire simple à l’appartenance et aux rôles.

Ce didacticiel commence par une analyse approfondie du flux de travail d’authentification par formulaire, un sujet que nous avons abordé dans le didacticiel précédent. Nous allons ensuite créer un site Web ASP.NET par le biais duquel vous pouvez faire la démonstration des concepts d’authentification par formulaire. Ensuite, nous allons configurer le site pour utiliser l’authentification par formulaire, créer une page de connexion simple et voir comment déterminer, dans le code, si un utilisateur est authentifié et, le cas échéant, le nom d’utilisateur avec lequel il s’est connecté.

La compréhension du flux de travail d’authentification par formulaire, son activation dans une application Web et la création de pages de connexion et de déconnexion sont toutes des étapes essentielles de la création d’une application ASP.NET qui prend en charge les comptes d’utilisateur et authentifie les utilisateurs via une page Web. Pour cette raison, et parce que ces didacticiels s’appuient sur un autre, je vous encourage à suivre ce didacticiel dans son intégralité avant de passer à l’étape suivante, même si vous avez déjà eu l’expérience de la configuration de l’authentification par formulaire dans les projets précédents.

## <a name="understanding-the-forms-authentication-workflow"></a>Fonctionnement du flux de travail d’authentification par formulaire

Lorsque le runtime ASP.NET traite une demande pour une ressource ASP.NET, telle qu’une page ASP.NET ou un service Web ASP.NET, la requête déclenche un certain nombre d’événements pendant son cycle de vie. Des événements sont déclenchés au début et à la fin de la requête, ceux-ci étant déclenchés lorsque la requête est authentifiée et autorisée, un événement déclenché dans le cas d’une exception non gérée, etc. Pour afficher la liste complète des événements, reportez-vous aux [événements de l’objet HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

Les *modules http* sont des classes managées dont le code est exécuté en réponse à un événement particulier dans le cycle de vie de la demande. ASP.NET est fourni avec un certain nombre de modules HTTP qui effectuent des tâches essentielles en arrière-plan. Deux modules HTTP intégrés qui sont particulièrement pertinents pour notre discussion sont les suivants :

- **[`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)** : authentifie l’utilisateur en inspectant le ticket d’authentification par formulaire, qui est généralement inclus dans la collection de cookies de l’utilisateur. Si aucun ticket d’authentification par formulaire n’est présent, l’utilisateur est anonyme.
- **[`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)** : détermine si l’utilisateur actuel est autorisé ou non à accéder à l’URL demandée. Ce module détermine l’autorité en consultant les règles d’autorisation spécifiées dans les fichiers de configuration de l’application. ASP.NET comprend également les [`FileAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) qui déterminent l’autorité en consultant les listes de contrôle d’accès des fichiers demandés.

Le `FormsAuthenticationModule` tente d’authentifier l’utilisateur avant l’exécution de l' `UrlAuthorizationModule` (et `FileAuthorizationModule`). Si l’utilisateur qui effectue la demande n’est pas autorisé à accéder à la ressource demandée, le module d’autorisation met fin à la demande et renvoie un état [non autorisé HTTP 401](http://www.checkupdown.com/status/E401.html) . Dans les scénarios d’authentification Windows, le statut HTTP 401 est renvoyé au navigateur. Ce code d’État amène le navigateur à inviter l’utilisateur à entrer ses informations d’identification via une boîte de dialogue modale. Toutefois, avec l’authentification par formulaire, l’état non autorisé HTTP 401 n’est jamais envoyé au navigateur, car FormsAuthenticationModule détecte cet État et le modifie pour rediriger l’utilisateur vers la page de connexion (via un état de [redirection HTTP 302](http://www.checkupdown.com/status/E302.html) ).

La responsabilité de la page de connexion consiste à déterminer si les informations d’identification de l’utilisateur sont valides et, le cas échéant, à créer un ticket d’authentification par formulaire et à rediriger l’utilisateur vers la page qu’il essayait de visiter. Le ticket d’authentification est inclus dans les demandes suivantes des pages du site Web, que le `FormsAuthenticationModule` utilise pour identifier l’utilisateur.

![Flux de travail d’authentification par formulaire](an-overview-of-forms-authentication-cs/_static/image1.png)

**Figure 1**: flux de travail d’authentification par formulaire

### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Mémorisation du ticket d’authentification sur les visites de page

Une fois connecté, le ticket d’authentification par formulaire doit être renvoyé au serveur Web à chaque demande afin que l’utilisateur reste connecté lorsqu’il parcourt le site. Pour ce faire, vous devez placer le ticket d’authentification dans la collection de cookies de l’utilisateur. Les [cookies](http://en.wikipedia.org/wiki/HTTP_cookie) sont de petits fichiers texte qui résident sur l’ordinateur de l’utilisateur et sont transmis dans les en-têtes HTTP de chaque demande au site Web qui a créé le cookie. Par conséquent, une fois que le ticket d’authentification par formulaire a été créé et stocké dans les cookies du navigateur, chaque visite suivante à ce site envoie le ticket d’authentification avec la demande, identifiant ainsi l’utilisateur.

L’un des aspects des cookies est leur expiration, c’est-à-dire la date et l’heure auxquelles le navigateur rejette le cookie. Lorsque le cookie d’authentification par formulaire expire, l’utilisateur ne peut plus être authentifié et devient donc anonyme. Lorsqu’un utilisateur visite à partir d’un terminal public, il est probable que son ticket d’authentification expire lorsqu’il ferme son navigateur. Toutefois, lorsque vous visitez à partir de chez vous, ce même utilisateur peut souhaiter que le ticket d’authentification soit mémorisé entre les redémarrages du navigateur afin qu’ils n’aient pas à se reconnecter chaque fois qu’ils visitent le site. Cette décision est souvent prise par l’utilisateur sous la forme d’une case à cocher « Mémoriser mon compte » sur la page de connexion. À l’étape 3, nous allons examiner comment implémenter une case à cocher « Mémoriser mes informations » dans la page de connexion. Le didacticiel suivant traite les paramètres du délai d’expiration du ticket d’authentification en détail.

> [!NOTE]
> Il est possible que l’agent utilisateur utilisé pour se connecter au site Web ne prenne pas en charge les cookies. Dans ce cas, ASP.NET peut utiliser des tickets d’authentification par formulaire sans cookie. Dans ce mode, le ticket d’authentification est encodé dans l’URL. Nous allons voir à quel moment les tickets d’authentification sans cookie sont utilisés et comment ils sont créés et gérés dans le didacticiel suivant.

### <a name="the-scope-of-forms-authentication"></a>L’étendue de l’authentification par formulaire

Le `FormsAuthenticationModule` est du code managé qui fait partie du runtime ASP.NET. Avant la version 7 du serveur Web de Microsoft [Internet Information Services (IIS)](https://www.iis.net/) , il existait un obstacle distinct entre le pipeline http d’IIS et le pipeline du runtime ASP.net. En résumé, dans IIS 6 et les versions antérieures, l' `FormsAuthenticationModule` s’exécute uniquement lorsqu’une demande est déléguée d’IIS au runtime ASP.NET. Par défaut, IIS traite le contenu statique lui-même, comme les pages HTML et les fichiers CSS et image, et transmet uniquement les demandes au runtime ASP.NET lorsqu’une page avec une extension. aspx,. asmx ou. ashx est demandée.

Toutefois, IIS 7 permet des pipelines intégrés IIS et ASP.NET. Avec quelques paramètres de configuration, vous pouvez configurer IIS 7 pour appeler le FormsAuthenticationModule pour *toutes les* demandes. En outre, avec IIS 7, vous pouvez définir des règles d’autorisation d’URL pour les fichiers de tout type. Pour plus d’informations, consultez [modifications entre la sécurité IIS6 et IIS7](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [la sécurité de votre plateforme Web](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements)et compréhension de l' [autorisation d’URL IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Pour une longue histoire, dans les versions antérieures à IIS 7, vous pouvez uniquement utiliser l’authentification par formulaire pour protéger les ressources gérées par le runtime ASP.NET. De même, les règles d’autorisation d’URL sont appliquées uniquement aux ressources gérées par le runtime ASP.NET. Toutefois, avec IIS 7, il est possible d’intégrer FormsAuthenticationModule et UrlAuthorizationModule dans le pipeline HTTP d’IIS, ce qui étend cette fonctionnalité à toutes les demandes.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Étape 1 : création d’un site Web ASP.NET pour cette série de didacticiels

Pour atteindre le public le plus vaste possible, le site Web ASP.NET que nous allons créer dans cette série sera créé avec la version gratuite de Microsoft Visual Studio 2008, [Visual Web developer 2008](https://www.microsoft.com/express/vwd/). Nous allons implémenter le magasin de l’utilisateur `SqlMembershipProvider` dans une base de données [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) . Si vous utilisez Visual Studio 2005 ou une autre édition de Visual Studio 2008 ou SQL Server, ne vous inquiétez pas : les étapes seront quasiment identiques et toute différence non négligeable sera indiquée.

> [!NOTE]
> L’application Web de démonstration utilisée dans chaque didacticiel est disponible en téléchargement. Cette application téléchargeable a été créée avec Visual Web Developer 2008 ciblé pour la version 3,5 de .NET Framework. Étant donné que l’application est ciblée pour .NET 3,5, son fichier Web. config comprend des éléments de configuration supplémentaires spécifiques à 3,5. Long Story Short, si vous n’avez pas encore installé .NET 3,5 sur votre ordinateur, l’application Web téléchargeable ne fonctionnera pas sans supprimer au préalable le balisage spécifique à 3,5 du fichier Web. config.

Avant de pouvoir configurer l’authentification par formulaire, nous avons d’abord besoin d’un site Web ASP.NET. Commencez par créer un site Web ASP.NET basé sur un système de fichiers. Pour ce faire, lancez Visual Web Developer, puis accédez au menu fichier et sélectionnez Nouveau site Web, en affichant la boîte de dialogue Nouveau site Web. Choisissez le modèle de site Web ASP.NET, définissez la liste déroulante emplacement sur système de fichiers, choisissez un dossier pour placer le site Web et définissez la langue C#sur. Cette opération crée un nouveau site Web avec une page default. aspx ASP.NET, une application\_le dossier Data et un fichier Web. config.

> [!NOTE]
> Visual Studio prend en charge deux modes de gestion de projet : les projets de site Web et les projets d’application Web. Les projets de site Web n’ont pas de fichier projet, tandis que les projets d’application Web imitent l’architecture du projet dans Visual Studio .NET 2002/2003 : ils incluent un fichier projet et compilent le code source du projet dans un assembly unique, qui est placé dans le dossier/bin. Visual Studio 2005 n’a initialement pris en charge que les projets de site Web, même si le modèle de projet d’application Web a été réintroduit avec Service Pack 1. Visual Studio 2008 offre les deux modèles de projet. Toutefois, les éditions Visual Web Developer 2005 et 2008 ne prennent en charge que les projets de site Web. Je vais utiliser le modèle de projet de site Web. Si vous utilisez une édition non Express et que vous souhaitez utiliser le [modèle de projet d’application Web](https://msdn.microsoft.com/library/aa730880%28vs.80%29.aspx) à la place, n’hésitez pas à le faire, mais sachez qu’il peut y avoir des différences entre ce que vous voyez sur votre écran et les étapes que vous devez prendre par rapport aux captures d’écran indiquées et les instructions fournies dans ces didacticiels.

[![créer un site Web basé sur un système de fichiers](an-overview-of-forms-authentication-cs/_static/image3.png)](an-overview-of-forms-authentication-cs/_static/image2.png)

**Figure 2**: créer un site Web basé sur un système de fichiers ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image4.png))

### <a name="adding-a-master-page"></a>Ajout d’une page maître

Ensuite, ajoutez une nouvelle page maître au site dans le répertoire racine nommé site. Master. Les [pages maîtres](https://msdn.microsoft.com/library/wtxbf3hh.aspx) permettent à un développeur de pages de définir un modèle à l’ensemble du site qui peut être appliqué aux pages ASP.net. Le principal avantage des pages maîtres est que l’apparence globale du site peut être définie dans un emplacement unique, ce qui facilite la mise à jour ou le peaufinage de la disposition du site.

[![ajouter une page maître nommée site. Master au site Web](an-overview-of-forms-authentication-cs/_static/image6.png)](an-overview-of-forms-authentication-cs/_static/image5.png)

**Figure 3**: ajouter une page maître nommée site. Master au site Web ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image7.png))

Définissez la disposition de page à l’ensemble du site dans la page maître. Vous pouvez utiliser la Mode Création et ajouter la disposition ou les contrôles Web dont vous avez besoin, ou vous pouvez ajouter manuellement la balise manuellement en mode Source. J’ai structuré la disposition de ma page maître pour imiter la disposition utilisée dans la série de didacticiels *[utilisation de données dans ASP.NET 2,0](../../data-access/index.md)* (voir figure 4). La page maître utilise des [feuilles de style en cascade](http://www.w3schools.com/css/default.asp) pour le positionnement et les styles avec les paramètres CSS définis dans le fichier style. CSS (inclus dans le téléchargement associé à ce didacticiel). Bien que vous ne soyez pas en mesure de déterminer à partir du balisage illustré ci-dessous, les règles CSS sont définies de telle sorte que le contenu de navigation &lt;div&gt;est positionné de façon absolue afin qu’il apparaisse à gauche et ait une largeur fixe de 200 pixels.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample1.aspx)]

Une page maître définit à la fois la mise en page statique et les régions qui peuvent être modifiées par les pages ASP.NET qui utilisent la page maître. Ces régions modifiables de contenu sont indiquées par le contrôle `ContentPlaceHolder`, qui peut être affiché dans le contenu &lt;div&gt;. Notre page maître a une seule `ContentPlaceHolder` (MainContent), mais la page maître peut avoir plusieurs ContentPlaceHolders.

Avec le balisage entré ci-dessus, le fait de passer au Mode Création affiche la disposition de la page maître. Toutes les pages ASP.NET qui utilisent cette page maître auront cette mise en page uniforme, avec la possibilité de spécifier le balisage pour la région `MainContent`.

[![la page maître, quand elle est consultée en mode conception](an-overview-of-forms-authentication-cs/_static/image9.png)](an-overview-of-forms-authentication-cs/_static/image8.png)

**Figure 4**: page maître, affichée en mode création ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image10.png))

### <a name="creating-content-pages"></a>Création de pages de contenu

À ce stade, nous avons une page default. aspx dans notre site Web, mais elle n’utilise pas la page maître que nous venons de créer. Bien qu’il soit possible de manipuler le balisage déclaratif d’une page Web pour utiliser une page maître, si la page ne contient pas encore de contenu, il est plus facile de simplement supprimer la page et de la rajouter au projet, en spécifiant la page maître à utiliser. Par conséquent, commencez par supprimer default. aspx du projet.

Ensuite, cliquez avec le bouton droit sur le nom du projet dans la Explorateur de solutions et choisissez d’ajouter un nouveau formulaire Web nommé Default. aspx. Cette fois, activez la case à cocher « sélectionner la page maître », puis choisissez la page maître site. Master dans la liste.

[![ajouter une nouvelle page default. aspx en choisissant de sélectionner une page maître](an-overview-of-forms-authentication-cs/_static/image12.png)](an-overview-of-forms-authentication-cs/_static/image11.png)

**Figure 5**: ajouter une nouvelle page default. aspx en choisissant une page maître ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image13.png))

![Utiliser la page maître site. Master](an-overview-of-forms-authentication-cs/_static/image14.png)

**Figure 6**: utiliser la page maître site. Master

> [!NOTE]
> Si vous utilisez le modèle de projet d’application Web, la boîte de dialogue Ajouter un nouvel élément n’inclut pas de case à cocher « sélectionner une page maître ». Au lieu de cela, vous devez ajouter un élément de type « Web Content Form ». Après avoir choisi l’option « formulaire de contenu Web » et cliqué sur Ajouter, Visual Studio affiche la même boîte de dialogue Sélectionner une forme de base, comme illustré à la figure 6.

Le nouveau balisage déclaratif de la page default. aspx comprend simplement une directive @Page spécifiant le chemin d’accès au fichier de la page maître et un contrôle de contenu pour le MainContent ContentPlaceHolder de la page maître.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample2.aspx)]

Pour le moment, laissez default. aspx vide. Nous y reviendrai plus tard dans ce didacticiel pour ajouter du contenu.

> [!NOTE]
> Notre page maître contient une section pour un menu ou une autre interface de navigation. Nous allons créer une telle interface dans un prochain didacticiel.

## <a name="step-2-enabling-forms-authentication"></a>Étape 2 : activation de l’authentification par formulaire

Une fois le site Web ASP.NET créé, la tâche suivante consiste à activer l’authentification par formulaire. La configuration d’authentification de l’application est spécifiée par le biais de l' [élément`<authentication>`](https://msdn.microsoft.com/library/532aee0e.aspx) dans Web. config. L’élément `<authentication>` contient un attribut unique nommé mode qui spécifie le modèle d’authentification utilisé par l’application. Cet attribut peut avoir l’une des quatre valeurs suivantes :

- **Windows** : comme indiqué dans le didacticiel précédent, lorsqu’une application utilise l’authentification Windows, le serveur Web est responsable de l’authentification du visiteur, et cela s’effectue généralement par le biais de l’authentification de base, Digest ou intégrée Windows.
- **Formulaires**: les utilisateurs sont authentifiés via un formulaire sur une page Web.
- **Passport**: les utilisateurs sont authentifiés à l’aide du réseau Passport de Microsoft.
- **Aucun**: aucun modèle d’authentification n’est utilisé ; tous les visiteurs sont anonymes.

Par défaut, les applications ASP.NET utilisent l’authentification Windows. Pour modifier le type d’authentification pour l’authentification par formulaire, vous devez modifier l’attribut mode de l’élément `<authentication>` en Forms.

Si votre projet ne contient pas encore de fichier Web. config, ajoutez-en un maintenant en cliquant avec le bouton droit sur le nom du projet dans le Explorateur de solutions, en sélectionnant Ajouter un nouvel élément, puis en ajoutant un fichier de configuration Web.

[![si votre projet n’inclut pas encore Web. config, ajoutez-le maintenant](an-overview-of-forms-authentication-cs/_static/image16.png)](an-overview-of-forms-authentication-cs/_static/image15.png)

**Figure 7**: Si votre projet n’inclut pas encore Web. config, ajoutez-le maintenant ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image17.png))

Ensuite, localisez l’élément `<authentication>` et mettez-le à jour pour utiliser l’authentification par formulaire. Après cette modification, le balisage de votre fichier Web. config doit ressembler à ce qui suit :

[!code-xml[Main](an-overview-of-forms-authentication-cs/samples/sample3.xml)]

> [!NOTE]
> Comme Web. config est un fichier XML, la casse est importante. Veillez à définir l’attribut mode sur Forms, avec un « F » majuscule. Si vous utilisez une autre casse, telle que « formulaires », vous recevrez une erreur de configuration lors de la visite du site via un navigateur.

L’élément `<authentication>` peut éventuellement inclure un élément enfant `<forms>` qui contient des paramètres spécifiques à l’authentification par formulaire. Pour le moment, utilisons simplement les paramètres d’authentification par défaut des formulaires. Nous allons explorer l’élément enfant `<forms>` plus en détail dans le didacticiel suivant.

## <a name="step-3-building-the-login-page"></a>Étape 3 : création de la page de connexion

Pour prendre en charge l’authentification par formulaire, notre site Web a besoin d’une page de connexion. Comme indiqué dans la section « fonctionnement du flux de travail d’authentification par formulaire », le `FormsAuthenticationModule` redirige automatiquement l’utilisateur vers la page de connexion s’il tente d’accéder à une page qu’il n’est pas autorisé à afficher. Il existe également des contrôles Web ASP.NET qui affichent un lien vers la page de connexion aux utilisateurs anonymes. Cela qui nous amène la question « quelle est l’URL de la page de connexion ? »

Par défaut, le système d’authentification par formulaire s’attend à ce que la page de connexion soit nommée Login. aspx et placée dans le répertoire racine de l’application Web. Si vous souhaitez utiliser une autre URL de page de connexion, vous pouvez le faire en le spécifiant dans Web. config. Nous verrons comment procéder dans le didacticiel suivant.

La page de connexion a trois responsabilités :

1. Fournissez une interface qui permet au visiteur d’entrer ses informations d’identification.
2. Déterminez si les informations d’identification envoyées sont valides.
3. «Connectez-vous à l’utilisateur en créant le ticket d’authentification par formulaire.

### <a name="creating-the-login-pages-user-interface"></a>Création de l’interface utilisateur de la page de connexion

Commençons par la première tâche. Ajoutez une nouvelle page ASP.NET au répertoire racine du site nommé Login. aspx et associez-la à la page maître site. Master.

[![ajouter une nouvelle page ASP.NET nommée Login. aspx](an-overview-of-forms-authentication-cs/_static/image19.png)](an-overview-of-forms-authentication-cs/_static/image18.png)

**Figure 8**: ajouter une nouvelle page ASP.NET nommée Login. aspx ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image20.png))

L’interface de page de connexion type est constituée de deux zones de texte : une pour le nom de l’utilisateur, une pour son mot de passe et un bouton pour envoyer le formulaire. Les sites Web incluent souvent une case à cocher « me souvenir » qui, si elle est activée, conserve le ticket d’authentification résultant dans les redémarrages du navigateur.

Ajoutez deux zones de texte à login. aspx et définissez leurs propriétés `ID` sur nom d’utilisateur et mot de passe, respectivement. Définissez également le mot de passe `TextMode` propriété sur mot de passe. Ensuite, ajoutez un contrôle CheckBox, en affectant à sa propriété `ID` la valeur RememberMe et à sa propriété `Text` la valeur « me souvenir ». Après cela, ajoutez un bouton nommé LoginButton dont la propriété `Text` a la valeur « login ». Enfin, ajoutez un contrôle Web Label et définissez sa propriété `ID` sur InvalidCredentialsMessage, sa propriété `Text` sur «votre nom d’utilisateur ou mot de passe n’est pas valide. Réessayez.», sa propriété `ForeColor` la valeur rouge, et sa propriété `Visible` la valeur false.

À ce stade, votre écran doit ressembler à la capture d’écran de la figure 9, et la syntaxe déclarative de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample4.aspx)]

[![la page de connexion contient deux zones de texte, une case à cocher, un bouton et une étiquette](an-overview-of-forms-authentication-cs/_static/image22.png)](an-overview-of-forms-authentication-cs/_static/image21.png)

**Figure 9**: la page de connexion contient deux zones de texte, une case à cocher, un bouton et une étiquette ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image23.png))

Enfin, créez un gestionnaire d’événements pour l’événement Click du LoginButton. Dans le concepteur, il vous suffit de double-cliquer sur le contrôle Button pour créer ce gestionnaire d’événements.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Détermination de la validité des informations d’identification fournies

Nous devons maintenant implémenter la tâche 2 dans le gestionnaire d’événements Click du bouton, en déterminant si les informations d’identification fournies sont valides. Pour ce faire, vous devez disposer d’un magasin d’utilisateurs qui contient toutes les informations d’identification des utilisateurs afin de pouvoir déterminer si les informations d’identification fournies correspondent à des informations d’identification connues.

Avant ASP.NET 2,0, les développeurs étaient responsables de l’implémentation de leurs propres magasins d’utilisateurs et de l’écriture du code pour valider les informations d’identification fournies par rapport au magasin. La plupart des développeurs implémentent le magasin de l’utilisateur dans une base de données, en créant une table nommée Users avec des colonnes comme UserName, Password, email, LastLoginDate, etc. Cette table, alors, aurait un enregistrement par compte d’utilisateur. La vérification des informations d’identification fournies par un utilisateur implique l’interrogation de la base de données pour un nom d’utilisateur correspondant, puis la vérification que le mot de passe dans la base de données correspond au mot de passe fourni.

Avec ASP.NET 2,0, les développeurs doivent utiliser l’un des fournisseurs d’appartenances pour gérer le magasin de l’utilisateur. Dans cette série de didacticiels, nous allons utiliser SqlMembershipProvider, qui utilise une base de données SQL Server pour le magasin de l’utilisateur. Lors de l’utilisation de SqlMembershipProvider, nous devons implémenter un schéma de base de données spécifique qui comprend les tables, les vues et les procédures stockées attendues par le fournisseur. Nous allons examiner comment implémenter ce schéma dans le didacticiel ***création du schéma d’appartenance dans SQL Server*** . Avec le fournisseur d’appartenances en place, la validation des informations d’identification de l’utilisateur est aussi simple que l’appel de la [méthode ValidateUser (*nom d’utilisateur*, *mot de passe*)](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)de la [classe d’appartenance](https://msdn.microsoft.com/library/system.web.security.membership.aspx), qui retourne une valeur booléenne indiquant si la validité de la combinaison *nom d’utilisateur* / *mot de passe* est validée. Comme nous n’avons pas encore implémenté le magasin de l’utilisateur de SqlMembershipProvider, nous ne pouvons pas utiliser la méthode ValidateUser de la classe d’appartenance pour l’instant.

Au lieu de prendre le temps de créer notre propre table de base de données d’utilisateurs personnalisés (qui serait obsolète une fois que nous avons implémenté le SqlMembershipProvider), nous allons à présent coder en dur les informations d’identification valides dans la page de connexion elle-même. Dans le gestionnaire d’événements Click de LoginButton, ajoutez le code suivant :

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample5.cs)]

Comme vous pouvez le voir, il existe trois comptes d’utilisateur valides : Scott, Jisun et Sam – et les trois ont le même mot de passe (« mot de passe »). Le code parcourt les groupes d’utilisateurs et de mots de passe en recherchant un nom d’utilisateur et un mot de passe valides. Si le nom d’utilisateur et le mot de passe sont valides, nous devons nous connecter à l’utilisateur, puis les rediriger vers la page appropriée. Si les informations d’identification ne sont pas valides, nous affichons l’étiquette InvalidCredentialsMessage.

Quand un utilisateur entre des informations d’identification valides, j’ai indiqué qu’elles sont ensuite redirigées vers la « page appropriée ». Qu’est-ce que la page appropriée ? Rappelez-vous que lorsqu’un utilisateur visite une page qu’il n’est pas autorisé à afficher, le FormsAuthenticationModule les redirige automatiquement vers la page de connexion. Dans ce cas, il comprend l’URL demandée dans la chaîne de requête via le paramètre ReturnUrl. Autrement dit, si un utilisateur a tenté de visiter ProtectedPage. aspx et qu’il n’a pas été autorisé à le faire, le FormsAuthenticationModule les redirige vers :

Login. aspx ? ReturnUrl = ProtectedPage. aspx

Une fois la connexion établie, l’utilisateur doit être redirigé vers ProtectedPage. aspx. Les utilisateurs peuvent également accéder à la page de connexion sur leur propre Volition. Dans ce cas, après avoir connecté l’utilisateur, vous devez l’envoyer à la page default. aspx du dossier racine.

### <a name="logging-in-the-user"></a>Journalisation de l’utilisateur

En supposant que les informations d’identification fournies sont valides, nous devons créer un ticket d’authentification par formulaire, ce qui permet de se connecter à l’utilisateur sur le site. La [classe FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) de l' [espace de noms System. Web. Security](https://msdn.microsoft.com/library/system.web.security.aspx) fournit des méthodes assorties pour la connexion et la déconnexion des utilisateurs via le système d’authentification par formulaire. Bien qu’il existe plusieurs méthodes dans la classe FormsAuthentication, les trois qui nous intéressent sont les suivantes :

- [GetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) : crée un ticket d’authentification par formulaire pour le nom *username*fourni. Ensuite, cette méthode crée et retourne un objet HttpCookie qui contient le contenu du ticket d’authentification. Si *persistCookie* a la valeur true, un cookie persistant est créé.
- [SetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) : appelle la méthode GetAuthCookie (*username*, *persistCookie*) pour générer le cookie d’authentification par formulaire. Cette méthode ajoute ensuite le cookie retourné par GetAuthCookie à la collection de cookies (en supposant que l’authentification par formulaire basée sur les cookies est utilisée ; sinon, cette méthode appelle une classe interne qui gère la logique de ticket sans cookie).
- [RedirectFromLoginPage (*nom d’utilisateur*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) : cette méthode appelle SetAuthCookie (*nom d’utilisateur*, *persistCookie*), puis redirige l’utilisateur vers la page appropriée.

GetAuthCookie est pratique lorsque vous devez modifier le ticket d’authentification avant d’écrire le cookie dans la collection de cookies. SetAuthCookie est utile si vous souhaitez créer le ticket d’authentification par formulaire et l’ajouter à la collection de cookies, mais ne souhaitez pas rediriger l’utilisateur vers la page appropriée. Peut-être souhaitez-vous les conserver sur la page de connexion ou les envoyer à une autre page.

Étant donné que nous voulons nous connecter à l’utilisateur et les rediriger vers la page appropriée, nous allons utiliser RedirectFromLoginPage. Mettez à jour le gestionnaire d’événements Click du LoginButton, en remplaçant les deux lignes TODO commentées par la ligne de code suivante :

FormsAuthentication. RedirectFromLoginPage (UserName. Text, RememberMe. Checked);

Lorsque vous créez le ticket d’authentification par formulaire, nous utilisons la propriété Text de nom d’utilisateur du nom d’utilisateur pour le paramètre de *nom d’utilisateur* du ticket d’authentification par formulaire et l’état activé de la case à cocher RememberMe pour le paramètre *persistCookie* .

Pour tester la page de connexion, accédez-y dans un navigateur. Commencez par entrer des informations d’identification non valides, telles que le nom d’utilisateur « non » et le mot de passe « incorrect ». Lorsque vous cliquez sur le bouton de connexion, une publication (postback) se produit et l’étiquette InvalidCredentialsMessage s’affiche.

[![l’étiquette InvalidCredentialsMessage s’affiche lorsque vous entrez des informations d’identification non valides](an-overview-of-forms-authentication-cs/_static/image25.png)](an-overview-of-forms-authentication-cs/_static/image24.png)

**Figure 10**: l’étiquette InvalidCredentialsMessage s’affiche lorsque vous entrez des informations d’identification non valides ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image26.png))

Entrez ensuite les informations d’identification valides et cliquez sur le bouton de connexion. Cette fois-ci, lorsque la publication se produit, un ticket d’authentification par formulaire est créé et vous êtes automatiquement redirigé vers default. aspx. À ce stade, vous vous êtes connecté au site Web, bien qu’il n’y ait pas de signaux visuels pour indiquer que vous êtes actuellement connecté. À l’étape 4, nous verrons comment déterminer par programmation si un utilisateur est connecté ou non, et comment identifier l’utilisateur visitant la page.

L’étape 5 examine les techniques permettant de déconnecter un utilisateur du site Web.

### <a name="securing-the-login-page"></a>Sécurisation de la page de connexion

Lorsque l’utilisateur entre ses informations d’identification et envoie le formulaire de page de connexion, les informations d’identification (y compris son mot de passe) sont transmises via Internet au serveur Web en *texte brut*. Cela signifie que tout pirate reniflant le trafic réseau peut voir le nom d’utilisateur et le mot de passe. Pour éviter cela, il est essentiel de chiffrer le trafic réseau à l’aide de [SSL (Secure Socket Layer)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Cela permet de s’assurer que les informations d’identification (ainsi que le balisage HTML de la page entière) sont chiffrées à partir du moment où elles sortent le navigateur jusqu’à ce qu’elles soient reçues par le serveur Web.

À moins que votre site Web contienne des informations sensibles, il vous suffit d’utiliser SSL sur la page de connexion et sur d’autres pages où le mot de passe de l’utilisateur serait envoyé sur le réseau en texte brut. Vous n’avez pas à vous soucier de la sécurisation du ticket d’authentification par formulaire dans la mesure où, par défaut, il est à la fois chiffré et signé numériquement (pour empêcher toute falsification). Une discussion plus approfondie sur la sécurité des tickets d’authentification par formulaire est présentée dans le didacticiel suivant.

> [!NOTE]
> De nombreux sites Web financiers et médicaux sont configurés pour utiliser SSL sur *toutes les* pages accessibles aux utilisateurs authentifiés. Si vous créez un tel site Web, vous pouvez configurer le système d’authentification par formulaire afin que le ticket d’authentification par formulaire soit transmis uniquement via une connexion sécurisée. Nous allons examiner les différentes options de configuration de l’authentification par formulaire dans le didacticiel suivant, *[configuration de l’authentification par formulaire et Rubriques avancées](forms-authentication-configuration-and-advanced-topics-cs.md)* .

## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Étape 4 : détection des visiteurs authentifiés et détermination de leur identité

À ce stade, nous avons activé l’authentification par formulaire et créé une page de connexion rudimentaire, mais nous n’avons pas encore examiné comment nous pouvons déterminer si un utilisateur est authentifié ou anonyme. Dans certains scénarios, nous pouvons souhaiter afficher des données ou des informations différentes selon qu’un utilisateur authentifié ou anonyme visite la page. En outre, nous avons souvent besoin de connaître l’identité de l’utilisateur authentifié.

Nous allons augmenter la page default. aspx existante pour illustrer ces techniques. Dans default. aspx, ajoutez deux contrôles Panel, l’un nommé AuthenticatedMessagePanel et l’autre nommé AnonymousMessagePanel. Ajoutez un contrôle Label nommé WelcomeBackMessage dans le premier panneau. Dans le deuxième panneau, ajoutez un contrôle de lien hypertexte, définissez sa propriété Text sur « log in » et sa propriété NavigateUrl sur « ~/Login.aspx ». À ce stade, le balisage déclaratif pour default. aspx doit ressembler à ce qui suit :

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample6.aspx)]

Comme vous l’avez probablement deviné à l’heure actuelle, l’idée est d’afficher uniquement le AuthenticatedMessagePanel pour les visiteurs authentifiés et juste le AnonymousMessagePanel pour les visiteurs anonymes. Pour ce faire, nous devons définir ces propriétés visibles des panneaux, selon que l’utilisateur est connecté ou non.

La [propriété Request. IsAuthenticated](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) retourne une valeur booléenne indiquant si la requête a été authentifiée. Entrez le code suivant dans la page\_chargement du code du gestionnaire d’événements :

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample7.cs)]

Une fois ce code en place, visitez default. aspx via un navigateur. En supposant que vous n’êtes pas encore connecté, vous verrez un lien vers la page de connexion (voir figure 11). Cliquez sur ce lien et connectez-vous au site. Comme nous l’avons vu à l’étape 3, après avoir entré vos informations d’identification, vous êtes redirigé vers default. aspx, mais cette fois-ci, la page affiche le message « Welcome Back ! ». message (voir figure 12).

![Quand vous vous rendez anonyme, un lien de connexion s’affiche.](an-overview-of-forms-authentication-cs/_static/image27.png)

**Figure 11**: lorsque vous vous rendez anonyme, un lien de connexion s’affiche.

![Les utilisateurs authentifiés sont affichés](an-overview-of-forms-authentication-cs/_static/image28.png)

**Figure 12**: les utilisateurs authentifiés sont représentés par le « Bienvenue ! » Message

Nous pouvons déterminer l’identité de l’utilisateur actuellement connecté via la [propriété User](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx)de l' [objet HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx). L’objet HttpContext représente des informations sur la requête en cours et est la page d’hébergement de tels objets ASP.NET courants comme réponse, requête et session, entre autres. La propriété User représente le contexte de sécurité de la requête HTTP actuelle et implémente l' [interface IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

La propriété User est définie par FormsAuthenticationModule. Plus précisément, lorsque FormsAuthenticationModule trouve un ticket d’authentification par formulaire dans la demande entrante, il crée un nouvel objet GenericPrincipal et l’attribue à la propriété de l’utilisateur.

Les objets principal (tels que GenericPrincipal) fournissent des informations sur l’identité de l’utilisateur et les rôles auxquels il appartient. L’interface IPrincipal définit deux membres :

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) : méthode qui retourne une valeur booléenne indiquant si le principal appartient au rôle spécifié.
- [Identity](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) : propriété qui retourne un objet qui implémente l' [interface IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). L’interface IIdentity définit trois propriétés : [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx)et [Name](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Nous pouvons déterminer le nom du visiteur actuel à l’aide du code suivant :

chaîne currentUsersName = User.Identity.Name ;

Lors de l’utilisation de l’authentification par formulaire, un [objet FormsIdentity](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) est créé pour la propriété Identity de GenericPrincipal. La classe FormsIdentity retourne toujours la chaîne « Forms » pour sa propriété AuthenticationType et true pour sa propriété IsAuthenticated. La propriété Name retourne le nom d’utilisateur spécifié lors de la création du ticket d’authentification par formulaire. En plus de ces trois propriétés, FormsIdentity comprend l’accès au ticket d’authentification sous-jacent par le biais de sa [propriété ticket](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). La propriété ticket retourne un objet de type [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), qui a des propriétés telles que expiration, IsPersistent, émis, nom, etc.

Le point important à retirer ici est que le paramètre de *nom d’utilisateur* spécifié dans les méthodes FormsAuthentication. GetAuthCookie (*nom d'* utilisateur, *PersistCookie*), FormsAuthentication. SetAuthCookie (*nom d’utilisateur*, *persistCookie*) et FormsAuthentication. RedirectFromLoginPage (*nom d’utilisateur*, *persistCookie*) est la même valeur retournée par user.Identity.Name. En outre, le ticket d’authentification créé par ces méthodes est disponible en convertissant User. Identity en objet FormsIdentity, puis en accédant à la propriété ticket :

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample8.cs)]

Nous allons fournir un message plus personnalisé dans default. aspx. Mettez à jour la page\_chargement du gestionnaire d’événements afin que la chaîne « Bienvenue, *nom d’utilisateur*! » soit affectée à la propriété Text de l’étiquette WelcomeBackMessage.

WelcomeBackMessage. Text = « Welcome Back », + User.Identity.Name + «  ! »;

La figure 13 montre l’effet de cette modification (lors de la connexion en tant qu’utilisateur Scott).

![Le message d’accueil contient le nom de l’utilisateur actuellement connecté](an-overview-of-forms-authentication-cs/_static/image29.png)

**Figure 13**: le message de bienvenue contient le nom de l’utilisateur actuellement connecté

### <a name="using-the-loginview-and-loginname-controls"></a>Utilisation des contrôles LoginView et LoginName

L’affichage d’un contenu différent pour les utilisateurs authentifiés et anonymes est une exigence courante. ainsi, affiche le nom de l’utilisateur actuellement connecté. Pour cette raison, ASP.NET comprend deux contrôles Web qui fournissent la même fonctionnalité que celle illustrée à la figure 13, mais sans qu’il soit nécessaire d’écrire une seule ligne de code.

Le [contrôle LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) est un contrôle Web basé sur un modèle qui permet d’afficher facilement des données différentes pour les utilisateurs authentifiés et anonymes. Le LoginView comprend deux modèles prédéfinis :

- AnonymousTemplate : tout balisage ajouté à ce modèle est uniquement affiché aux visiteurs anonymes.
- LoggedInTemplate : le balisage de ce modèle s’affiche uniquement pour les utilisateurs authentifiés.

Nous allons ajouter le contrôle LoginView à la page maître de votre site, site. Master. Plutôt que d’ajouter simplement le contrôle LoginView, nous allons ajouter un nouveau contrôle ContentPlaceHolder, puis placer le contrôle LoginView dans ce nouvel ContentPlaceHolder. La logique de cette décision va bientôt apparaître.

> [!NOTE]
> En plus des modèles AnonymousTemplate et LoggedInTemplate, le contrôle LoginView peut inclure des modèles spécifiques aux rôles. Les modèles spécifiques aux rôles affichent le balisage uniquement pour les utilisateurs qui appartiennent à un rôle spécifié. Nous examinerons les fonctionnalités basées sur les rôles du contrôle LoginView dans un prochain didacticiel.

Commencez par ajouter un ContentPlaceHolder nommé LoginContent dans la page maître au sein de l’élément de navigation &lt;div&gt;. Vous pouvez simplement faire glisser un contrôle ContentPlaceHolder de la boîte à outils vers le mode Source, en plaçant le balisage résultant juste au-dessus du menu « TODO :... » financière.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample9.aspx)]

Ensuite, ajoutez un contrôle LoginView dans le LoginContent ContentPlaceHolder. Le contenu placé dans les contrôles ContentPlaceHolder de la page maître est considéré comme du *contenu par défaut* pour ContentPlaceHolder. Autrement dit, les pages ASP.NET qui utilisent cette page maître peuvent spécifier leur propre contenu pour chaque ContentPlaceHolder ou utiliser le contenu par défaut de la page maître.

Les contrôles LoginView et les autres contrôles liés à la connexion se trouvent dans l’onglet Connexion de la boîte à outils.

![Contrôle LoginView dans la boîte à outils](an-overview-of-forms-authentication-cs/_static/image30.png)

**Figure 14**: contrôle LoginView dans la boîte à outils

Ensuite, ajoutez deux éléments &lt;br/&gt; immédiatement après le contrôle LoginView, mais toujours dans ContentPlaceHolder. À ce stade, le balisage de l’élément de navigation &lt;div&gt; doit ressembler à ce qui suit :

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample10.aspx)]

Les modèles de LoginView peuvent être définis à partir du concepteur ou du balisage déclaratif. Dans le concepteur de Visual Studio, développez la balise active du LoginView, qui répertorie les modèles configurés dans une liste déroulante. Tapez le texte « Hello, étranger » dans AnonymousTemplate ; Ensuite, ajoutez un contrôle de lien hypertexte et définissez ses propriétés Text et NavigateUrl sur « log in » et « ~/login.aspx », respectivement.

Après avoir configuré le AnonymousTemplate, passez à la page LoggedInTemplate et entrez le texte « Welcome Back ». Faites ensuite glisser un contrôle LoginName de la boîte à outils vers le LoggedInTemplate, en le plaçant juste après le texte « welcome ». Le [contrôle LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), comme son nom l’indique, affiche le nom de l’utilisateur actuellement connecté. En interne, le contrôle LoginName renvoie simplement la propriété User.Identity.Name

Après avoir apporté ces ajouts aux modèles de LoginView, le balisage doit ressembler à ce qui suit :

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample11.aspx)]

Avec cet ajout à la page maître site. Master, chaque page de notre site Web affiche un message différent selon que l’utilisateur est authentifié ou non. La figure 15 illustre la page default. aspx quand elle est visitée via un navigateur par l’utilisateur Jisun. Le message « Welcome Back, Jisun » est répété deux fois : une fois dans la section de navigation de la page maître à gauche (via le contrôle LoginView que nous venons d’ajouter) et une fois dans la zone de contenu default. aspx (via les contrôles de panneau et la logique de programmation).

![Le contrôle LoginView affiche](an-overview-of-forms-authentication-cs/_static/image31.png)

**Figure 15**: le contrôle LoginView affiche « Welcome Back, Jisun. »

Étant donné que nous avons ajouté le LoginView à la page maître, il peut apparaître dans chaque page de notre site. Toutefois, il peut y avoir des pages Web pour lesquelles vous ne souhaitez pas afficher ce message. Une page de ce type correspond à la page de connexion, car un lien vers la page de connexion semble être introuvable ici. Étant donné que nous avons placé le contrôle LoginView dans un ContentPlaceHolder dans la page maître, nous pouvons remplacer ce balisage par défaut dans notre page de contenu. Ouvrez login. aspx et accédez au concepteur. Étant donné que nous n’avons pas défini explicitement un contrôle de contenu dans login. aspx pour LoginContent ContentPlaceHolder dans la page maître, la page de connexion affiche le balisage par défaut de la page maître pour ce ContentPlaceHolder. Vous pouvez le voir par le biais du concepteur : le LoginContent ContentPlaceHolder affiche le balisage par défaut (le contrôle LoginView).

[![la page de connexion affiche le contenu par défaut pour le LoginContent ContentPlaceHolder de la page maître](an-overview-of-forms-authentication-cs/_static/image33.png)](an-overview-of-forms-authentication-cs/_static/image32.png)

**Figure 16**: la page de connexion affiche le contenu par défaut pour le LoginContent ContentPlaceHolder de la page maître ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image34.png))

Pour remplacer le balisage par défaut de l’ContentPlaceHolder LoginContent, cliquez simplement avec le bouton droit sur la zone dans le concepteur, puis choisissez l’option créer un contenu personnalisé dans le menu contextuel. (Lors de l’utilisation de Visual Studio 2008, ContentPlaceHolder inclut une balise active qui, lorsqu’elle est sélectionnée, offre la même option.) Cela ajoute un nouveau contrôle de contenu au balisage de la page, ce qui nous permet de définir un contenu personnalisé pour cette page. Vous pouvez ajouter un message personnalisé ici, par exemple « Connectez-vous... », mais laissez ce champ vide.

> [!NOTE]
> Dans Visual Studio 2005, la création d’un contenu personnalisé crée un contrôle de contenu vide dans la page ASP.NET. Toutefois, dans Visual Studio 2008, la création de contenu personnalisé copie le contenu par défaut de la page maître dans le contrôle de contenu nouvellement créé. Si vous utilisez Visual Studio 2008, après avoir créé le contrôle de contenu, veillez à effacer le contenu copié à partir de la page maître.

La figure 17 illustre la page Login. aspx quand elle est visitée à partir d’un navigateur après avoir apporté cette modification. Notez qu’il n’y a pas de message « Hello, étranger » ou « Welcome, *username*» dans le volet de navigation gauche &lt;div&gt; comme c’est le cas lorsque vous visitez default. aspx.

[![la page de connexion masque le balisage par défaut de l’ContentPlaceHolder LoginContent](an-overview-of-forms-authentication-cs/_static/image36.png)](an-overview-of-forms-authentication-cs/_static/image35.png)

**Figure 17**: la page de connexion masque le balisage par défaut de LoginContent ContentPlaceHolder ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image37.png))

## <a name="step-5-logging-out"></a>Étape 5 : déconnexion

À l’étape 3, nous avons examiné la création d’une page de connexion pour connecter un utilisateur au site, mais nous avons encore vu comment déconnecter un utilisateur. En plus des méthodes de journalisation d’un utilisateur dans, la classe FormsAuthentication fournit également une [méthode SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). La méthode SignOut détruit simplement le ticket d’authentification par formulaire, ce qui permet de déconnecter l’utilisateur du site.

L’offre d’un lien de déconnexion est une fonctionnalité commune qui ASP.NET comprend un contrôle spécifiquement conçu pour déconnecter un utilisateur. Le [contrôle LoginStatus](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) affiche un LinkButton « connexion » ou un LinkButton « déconnexion », en fonction de l’état d’authentification de l’utilisateur. Un LinkButton « connexion » est rendu pour les utilisateurs anonymes, alors qu’un LinkButton « déconnexion » s’affiche pour les utilisateurs authentifiés. Le texte des LinkButtons « connexion » et « déconnexion » peut être configuré via les propriétés LoginText et LogoutText de LoginStatus.

Le fait de cliquer sur le lien « connexion » entraîne une publication, à partir de laquelle une redirection est envoyée à la page de connexion. Si vous cliquez sur le bouton de déconnexion, le contrôle LoginStatus appelle la méthode FormsAuthentication. aval, puis redirige l’utilisateur vers une page. La page vers laquelle l’utilisateur déconnecté est redirigé dépend de la propriété LogoutAction, qui peut être affectée à l’une des trois valeurs suivantes :

- Actualiser : valeur par défaut ; redirige l’utilisateur vers la page visitée. Si la page sur laquelle ils se trouvaient n’autorise pas les utilisateurs anonymes, FormsAuthenticationModule redirige automatiquement l’utilisateur vers la page de connexion.

Vous pouvez être curieux de savoir pourquoi une redirection est effectuée ici. Si l’utilisateur souhaite rester sur la même page, pourquoi la nécessité d’une redirection explicite est-elle nécessaire ? En effet, lorsque l’utilisateur clique sur le LinkButton « Logoff », l’utilisateur a toujours le ticket d’authentification par formulaire dans sa collection de cookies. Par conséquent, la demande de publication (postback) est une demande authentifiée. Le contrôle LoginStatus appelle la méthode SignOut, mais cela se produit une fois que FormsAuthenticationModule a authentifié l’utilisateur. Par conséquent, une redirection explicite entraîne la nouvelle demande de la page par le navigateur. Au moment où le navigateur demande à nouveau la page, le ticket d’authentification par formulaire a été supprimé et, par conséquent, la demande entrante est anonyme.

- Redirection : l’utilisateur est redirigé vers l’URL spécifiée par la propriété LogoutPageUrl de LoginStatus.
- RedirectToLoginPage – l’utilisateur est redirigé vers la page de connexion.

Nous allons ajouter un contrôle LoginStatus à la page maître et le configurer pour utiliser l’option de redirection pour envoyer l’utilisateur à une page qui affiche un message confirmant qu’il a été déconnecté. Commencez par créer une page dans le répertoire racine nommé logout. aspx. N’oubliez pas d’associer cette page à la page maître site. Master. Ensuite, entrez un message dans le balisage de la page expliquant à l’utilisateur qu’il a été déconnecté.

Ensuite, revenez à la page maître site. Master et ajoutez un contrôle LoginStatus sous le LoginView dans LoginContent ContentPlaceHolder. Affectez à la propriété LogoutAction du contrôle LoginStatus la valeur Redirect et à sa propriété LogoutPageUrl la valeur « ~/logout.aspx ».

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample12.aspx)]

Dans la mesure où LoginStatus est en dehors du contrôle LoginView, il apparaîtra pour les utilisateurs anonymes et authentifiés, mais ce n’est pas le cas, car LoginStatus affichera correctement un LinkButton « connexion » ou « déconnexion ». Avec l’ajout du contrôle LoginStatus, le lien hypertexte « log in » dans AnonymousTemplate est superflu, donc supprimez-le.

La figure 18 montre default. aspx lors des visites Jisun. Notez que la colonne de gauche affiche le message « Welcome Back, Jisun », ainsi qu’un lien pour se déconnecter. Le fait de cliquer sur Déconnecter le lien LinkButton provoque une publication (postback), signe Jisun dans le système, puis le redirige vers logout. aspx. Comme illustré dans la figure 19, le Jisun atteint logout. aspx qu’il a déjà été déconnecté et est par conséquent anonyme. Par conséquent, la colonne de gauche affiche le texte « Welcome, étranger » et un lien vers la page de connexion.

[![default. aspx affiche](an-overview-of-forms-authentication-cs/_static/image39.png)](an-overview-of-forms-authentication-cs/_static/image38.png)

**Figure 18**: default. aspx affiche « Welcome Back, Jisun » avec un LinkButton « Logout » ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image40.png))

[![logout. aspx affiche](an-overview-of-forms-authentication-cs/_static/image42.png)](an-overview-of-forms-authentication-cs/_static/image41.png)

**Figure 19**: Logout. aspx affiche « Welcome, étranger » avec un LinkButton « login » ([cliquez pour afficher l’image en taille réelle](an-overview-of-forms-authentication-cs/_static/image43.png))

> [!NOTE]
> Je vous encourage à personnaliser la page logout. aspx pour masquer le LoginContent ContentPlaceHolder de la page maître (comme nous l’avons fait pour login. aspx à l’étape 4). La raison est que le LinkButton « login » rendu par le contrôle LoginStatus (celui qui se trouve sous « Hello, étranger ») envoie l’utilisateur à la page de connexion en passant l’URL actuelle dans le paramètre ReturnUrl querystring. En résumé, si un utilisateur qui se connecte clique sur le LinkButton « connexion » de LoginStatus, puis se connecte, il est redirigé vers logout. aspx, ce qui peut facilement dérouter l’utilisateur.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons commencé avec un examen du flux de travail d’authentification par formulaire, puis j’ai activé l’implémentation de l’authentification par formulaire dans une application ASP.NET. L’authentification par formulaire est alimentée par le FormsAuthenticationModule, qui a deux responsabilités : l’identification des utilisateurs en fonction de leur ticket d’authentification par formulaire et la redirection des utilisateurs non autorisés vers la page de connexion.

La classe FormsAuthentication de .NET Framework comprend des méthodes permettant de créer, d’inspecter et de supprimer des tickets d’authentification par formulaire. La propriété Request. IsAuthenticated et l’objet User fournissent une prise en charge de programmation supplémentaire pour déterminer si une demande est authentifiée et fournit des informations sur l’identité de l’utilisateur. Il existe également des contrôles Web LoginView, LoginStatus et LoginName, qui offrent aux développeurs un moyen rapide et gratuit de code pour effectuer de nombreuses tâches courantes liées aux connexions. Nous examinerons ces autres contrôles Web de connexion plus en détail dans les prochains didacticiels.

Ce didacticiel vous a présenté une vue d’ensemble de l’authentification par formulaire. Nous n’avons pas examiné les options de configuration assorties, voyons comment fonctionnent les tickets d’authentification par formulaire sans cookies, ou Découvrez comment ASP.NET protège le contenu du ticket d’authentification par formulaire. Nous aborderons ces sujets et plus encore dans le [prochain didacticiel](forms-authentication-configuration-and-advanced-topics-cs.md).

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Modifications entre la sécurité IIS6 et IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Login ASP.NET contrôles](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Gestion de la sécurité, de l’appartenance et des rôles de ASP.net Professional 2,0](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN : 978-0-7645-9698-8)
- [Élément `<authentication>`](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Élément `<forms>` pour `<authentication>`](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formation vidéo sur les rubriques contenues dans ce didacticiel

- [Utilisation de l’authentification par formulaire de base dans ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à...

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel a été révisé par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel incluent Alicja maziarz, John SURU et Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](security-basics-and-asp-net-support-cs.md)
> [Suivant](forms-authentication-configuration-and-advanced-topics-cs.md)
