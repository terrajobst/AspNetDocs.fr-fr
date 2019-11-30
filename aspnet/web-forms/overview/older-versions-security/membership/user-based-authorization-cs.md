---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: Autorisation basée sur l’utilisateurC#() | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons nous intéresser à la limitation de l’accès aux pages et à la restriction des fonctionnalités au niveau des pages par le biais de diverses techniques.
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 059dbf42956268884dcfdade696491ac39e32da9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614665"
---
# <a name="user-based-authorization-c"></a>Autorisation basée sur l’utilisateur (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> Dans ce didacticiel, nous allons nous intéresser à la limitation de l’accès aux pages et à la restriction des fonctionnalités au niveau des pages par le biais de diverses techniques.

## <a name="introduction"></a>Introduction

La plupart des applications Web qui offrent des comptes d’utilisateur le font en partie pour empêcher certains visiteurs d’accéder à certaines pages du site. Dans la plupart des sites en ligne, par exemple, tous les utilisateurs (anonymes et authentifiés) sont en mesure d’afficher les publications de ce dernier, mais seuls les utilisateurs authentifiés peuvent accéder à la page Web pour créer une nouvelle publication. De même, il peut y avoir des pages d’administration qui ne sont accessibles qu’à un utilisateur particulier (ou à un ensemble particulier d’utilisateurs). En outre, les fonctionnalités au niveau de la page peuvent différer d’un utilisateur à l’autre. Lorsque vous affichez une liste de publications, les utilisateurs authentifiés voient une interface pour évaluer chaque publication, alors que cette interface n’est pas disponible pour les visiteurs anonymes.

ASP.NET permet de définir facilement des règles d’autorisation basées sur l’utilisateur. Avec un peu de balises dans `Web.config`, des pages Web spécifiques ou des répertoires entiers peuvent être verrouillés de façon à ce qu’ils soient accessibles uniquement à un sous-ensemble d’utilisateurs spécifié. Les fonctionnalités au niveau de la page peuvent être activées ou désactivées en fonction de l’utilisateur actuellement connecté par le biais de la programmation et des moyens déclaratifs.

Dans ce didacticiel, nous allons nous intéresser à la limitation de l’accès aux pages et à la restriction des fonctionnalités au niveau des pages par le biais de diverses techniques. Commençons !

## <a name="a-look-at-the-url-authorization-workflow"></a>Examen du flux de travail d’autorisation d’URL

Comme indiqué dans le didacticiel [*vue d’ensemble du processus d’authentification par formulaire*](../introduction/an-overview-of-forms-authentication-cs.md) , lorsque le runtime ASP.net traite une demande pour une ressource ASP.net, la requête déclenche un certain nombre d’événements pendant son cycle de vie. Les *modules http* sont des classes managées dont le code est exécuté en réponse à un événement particulier dans le cycle de vie de la demande. ASP.NET est fourni avec un certain nombre de modules HTTP qui effectuent des tâches essentielles en arrière-plan.

L’un de ces modules HTTP est [`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Comme nous l’avons vu dans les didacticiels précédents, la fonction principale du `FormsAuthenticationModule` consiste à déterminer l’identité de la requête actuelle. Pour ce faire, vous devez inspecter le ticket d’authentification par formulaire, qui se trouve dans un cookie ou incorporé dans l’URL. Cette identification a lieu lors de l' [événement`AuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Un autre module HTTP important est le [`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), qui est généré en réponse à l' [événement`AuthorizeRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (qui se produit après l’événement `AuthenticateRequest`). Le `UrlAuthorizationModule` examine le balisage de configuration dans `Web.config` pour déterminer si l’identité actuelle est habilitée à visiter la page spécifiée. Ce processus est appelé « *autorisation d’URL*».

Nous allons examiner la syntaxe des règles d’autorisation d’URL à l’étape 1, mais nous allons tout d’abord examiner ce que fait la `UrlAuthorizationModule` selon que la demande est autorisée ou non. Si le `UrlAuthorizationModule` détermine que la demande est autorisée, il ne fait rien, et la demande continue dans son cycle de vie. Toutefois, si la demande n’est *pas* autorisée, le `UrlAuthorizationModule` abandonne le cycle de vie et demande à l’objet `Response` de retourner un état [non autorisé http 401](http://www.checkupdown.com/status/E401.html) . Lors de l’utilisation de l’authentification par formulaire, cet état HTTP 401 n’est jamais retourné au client, car si le `FormsAuthenticationModule` détecte un état HTTP 401, il le modifie en une [redirection http 302](http://www.checkupdown.com/status/E302.html) vers la page de connexion.

La figure 1 illustre le flux de travail du pipeline ASP.NET, le `FormsAuthenticationModule`et le `UrlAuthorizationModule` lors de l’arrivée d’une requête non autorisée. En particulier, la figure 1 illustre une demande par un visiteur anonyme pour `ProtectedPage.aspx`, qui est une page qui refuse l’accès aux utilisateurs anonymes. Étant donné que le visiteur est anonyme, le `UrlAuthorizationModule` abandonne la demande et renvoie un état HTTP 401 non autorisé. Le `FormsAuthenticationModule` convertit ensuite l’état 401 en une page de redirection 302 vers la page de connexion. Une fois que l’utilisateur est authentifié via la page de connexion, il est redirigé vers `ProtectedPage.aspx`. Cette fois, le `FormsAuthenticationModule` identifie l’utilisateur en fonction de son ticket d’authentification. Maintenant que le visiteur est authentifié, le `UrlAuthorizationModule` autorise l’accès à la page.

[![le flux de travail d’authentification par formulaire et d’autorisation d’URL](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Figure 1**: flux de travail de l’authentification par formulaire et de l’autorisation d’URL ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image3.png))

La figure 1 illustre l’interaction qui se produit lorsqu’un visiteur anonyme tente d’accéder à une ressource qui n’est pas disponible pour les utilisateurs anonymes. Dans ce cas, le visiteur anonyme est redirigé vers la page de connexion avec la page qu’il a tenté de visiter spécifiée dans la chaîne de connexion. Une fois que l’utilisateur a ouvert une session, il est automatiquement redirigé vers la ressource à laquelle il tente d’accéder initialement.

Lorsque la requête non autorisée est effectuée par un utilisateur anonyme, ce flux de travail est simple et il est facile pour le visiteur de comprendre ce qui s’est produit et pourquoi. Toutefois, gardez à l’esprit que le `FormsAuthenticationModule` redirige *tout* utilisateur non autorisé vers la page de connexion, même si la demande est effectuée par un utilisateur authentifié. Cela peut entraîner une certaine confusion dans le cas d’un utilisateur authentifié qui tente d’accéder à une page pour laquelle il n’a pas d’autorité.

Imaginez que notre site Web avait ses règles d’autorisation d’URL configurées de telle sorte que la page ASP.NET `OnlyTito.aspx` était accessibly uniquement à Tito. Imaginez maintenant que Sam visite le site, se connecte, puis tente de visiter `OnlyTito.aspx`. Le `UrlAuthorizationModule` arrêtera le cycle de vie de la demande et renverra un État non autorisé HTTP 401, que le `FormsAuthenticationModule` détectera, puis redirigera Sam vers la page de connexion. Dans la mesure où Sam s’est déjà connecté, elle peut se demander pourquoi elle a été renvoyée à la page de connexion. Elle peut être à l’origine de la perte de ses informations d’identification de connexion ou de l’entrée d’informations d’identification non valides. Si Sam saisit à nouveau ses informations d’identification à partir de la page de connexion, il se reconnecte (à nouveau) et est redirigé vers `OnlyTito.aspx`. Le `UrlAuthorizationModule` détectera que Sam ne peut pas accéder à cette page et qu’elle sera renvoyée à la page de connexion.

La figure 2 illustre ce flux de travail confus.

[![le flux de travail par défaut peut entraîner un cycle confus](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Figure 2**: le flux de travail par défaut peut entraîner un cycle confus ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image6.png))

Le flux de travail illustré dans la figure 2 peut rapidement befuddle même le visiteur le plus expérimenté. Nous allons voir comment éviter ce cycle confus à l’étape 2.

> [!NOTE]
> ASP.NET utilise deux mécanismes pour déterminer si l’utilisateur actuel peut accéder à une page Web particulière : autorisation d’URL et autorisation de fichier. L’autorisation de fichier est implémentée par le [`FileAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), qui détermine l’autorité en consultant les listes de contrôle d’accès des fichiers demandés. L’autorisation de fichier est couramment utilisée avec l’authentification Windows, car les listes de contrôle d’accès sont des autorisations qui s’appliquent aux comptes Windows. Lors de l’utilisation de l’authentification par formulaire, toutes les demandes au niveau du système d’exploitation et du système de fichiers sont exécutées par le même compte Windows, quel que soit l’utilisateur visitant le site. Dans la mesure où cette série de didacticiels se concentre sur l’authentification par formulaire, nous ne parlerons pas de l’autorisation de fichier.

### <a name="the-scope-of-url-authorization"></a>Étendue de l’autorisation d’URL

Le `UrlAuthorizationModule` est du code managé qui fait partie du runtime ASP.NET. Avant la version 7 du serveur Web de Microsoft [Internet Information Services (IIS)](https://www.iis.net/) , il existait un obstacle distinct entre le pipeline http d’IIS et le pipeline du runtime ASP.net. En résumé, dans IIS 6 et les versions antérieures, ASP. Le `UrlAuthorizationModule` du réseau s’exécute uniquement lorsqu’une requête est déléguée d’IIS au runtime ASP.NET. Par défaut, IIS traite le contenu statique lui-même, comme les pages HTML et les fichiers CSS, JavaScript et image, et transmet uniquement les demandes au runtime ASP.NET lorsqu’une page avec une extension de `.aspx`, `.asmx`ou `.ashx` est demandée.

Toutefois, IIS 7 permet des pipelines intégrés IIS et ASP.NET. Avec quelques paramètres de configuration, vous pouvez configurer IIS 7 pour appeler le `UrlAuthorizationModule` pour *toutes les* requêtes, ce qui signifie que les règles d’autorisation d’URL peuvent être définies pour des fichiers de n’importe quel type. En outre, IIS 7 comprend son propre moteur d’autorisation d’URL. Pour plus d’informations sur l’intégration de ASP.NET et la fonctionnalité d’autorisation d’URL native d’IIS 7, consultez [Understanding IIS7 URL Authorization](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)(en anglais). Pour une présentation plus approfondie de ASP.NET et de l’intégration d’IIS 7, consultez une copie du livre de Shahram Khosravi, *Professional IIS 7 et ASP.net Integrated Programming* (ISBN : 978-0470152539).

En résumé, dans les versions antérieures à IIS 7, les règles d’autorisation d’URL sont appliquées uniquement aux ressources gérées par le runtime ASP.NET. Toutefois, avec IIS 7, il est possible d’utiliser la fonctionnalité d’autorisation d’URL native d’IIS ou d’intégrer ASP. Le `UrlAuthorizationModule` NET dans le pipeline HTTP d’IIS, ce qui étend cette fonctionnalité à toutes les demandes.

> [!NOTE]
> Il existe des différences subtiles mais importantes dans ASP. La fonctionnalité d’autorisation d’URL du `UrlAuthorizationModule` et de IIS 7 traite les règles d’autorisation. Ce didacticiel n’examine pas les fonctionnalités d’autorisation d’URL d’IIS 7, ni les différences en matière d’analyse des règles d’autorisation par rapport au `UrlAuthorizationModule`. Pour plus d’informations sur ces rubriques, reportez-vous à la documentation IIS 7 sur MSDN ou sur [www.IIS.net](https://www.iis.net/).

## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Étape 1 : définition des règles d’autorisation d’URL dans`Web.config`

Le `UrlAuthorizationModule` détermine s’il convient d’accorder ou de refuser l’accès à une ressource demandée pour une identité particulière en fonction des règles d’autorisation d’URL définies dans la configuration de l’application. Les règles d’autorisation sont épelées dans l' [élément`<authorization>`](https://msdn.microsoft.com/library/8d82143t.aspx) sous la forme d’éléments enfants `<allow>` et `<deny>`. Chaque `<allow>` et `<deny>` élément enfant peut spécifier les éléments suivants :

- Un utilisateur particulier
- Liste d’utilisateurs délimités par des virgules
- Tous les utilisateurs anonymes, signalés par un point d’interrogation ( ?)
- Tous les utilisateurs, dénotés par un astérisque (\*)

Le balisage suivant montre comment utiliser les règles d’autorisation d’URL pour autoriser les utilisateurs Tito et Scott et à refuser tous les autres :

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

L’élément `<allow>` définit les utilisateurs autorisés-Tito et Scott, tandis que l’élément `<deny>` indique que *tous* les utilisateurs sont refusés.

> [!NOTE]
> Les éléments `<allow>` et `<deny>` peuvent également spécifier des règles d’autorisation pour les rôles. Nous examinerons l’autorisation basée sur les rôles dans un prochain didacticiel.

Le paramètre suivant accorde l’accès à toute autre personne que Sam (y compris les visiteurs anonymes) :

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Pour autoriser uniquement les utilisateurs authentifiés, utilisez la configuration suivante, qui refuse l’accès à tous les utilisateurs anonymes :

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Les règles d’autorisation sont définies dans l’élément `<system.web>` dans `Web.config` et s’appliquent à toutes les ressources ASP.NET dans l’application Web. Souvent, une application a des règles d’autorisation différentes pour différentes sections. Par exemple, sur un site de commerce électronique, tous les visiteurs peuvent parcourir les produits, consulter les revues de produits, Rechercher dans le catalogue, et ainsi de suite. Toutefois, seuls les utilisateurs authentifiés peuvent atteindre l’extraction ou les pages pour gérer l’historique d’expédition d’un seul. En outre, certaines parties du site peuvent être accessibles uniquement par certains utilisateurs, tels que les administrateurs de site.

ASP.NET permet de définir facilement différentes règles d’autorisation pour différents fichiers et dossiers dans le site. Les règles d’autorisation spécifiées dans le fichier `Web.config` du dossier racine s’appliquent à toutes les ressources ASP.NET du site. Toutefois, ces paramètres d’autorisation par défaut peuvent être remplacés pour un dossier particulier en ajoutant un `Web.config` avec une section `<authorization>`.

Nous allons mettre à jour notre site Web afin que seuls les utilisateurs authentifiés puissent consulter les pages ASP.NET dans le dossier `Membership`. Pour ce faire, nous devons ajouter un fichier `Web.config` dans le dossier `Membership` et définir ses paramètres d’autorisation pour refuser les utilisateurs anonymes. Cliquez avec le bouton droit sur le dossier `Membership` dans le Explorateur de solutions, choisissez le menu Ajouter un nouvel élément dans le menu contextuel, puis ajoutez un nouveau fichier de configuration Web nommé `Web.config`.

[![ajouter un fichier Web. config au dossier d’appartenance](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Figure 3**: ajouter un fichier de `Web.config` au dossier `Membership` ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image9.png))

À ce stade, votre projet doit contenir deux fichiers `Web.config` : l’un dans le répertoire racine et l’autre dans le dossier `Membership`.

[![votre application doit maintenant contenir deux fichiers Web. config](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Figure 4**: votre application doit maintenant contenir deux fichiers `Web.config` ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image12.png))

Mettez à jour le fichier de configuration dans le dossier `Membership` pour empêcher l’accès aux utilisateurs anonymes.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

C’est aussi simple que cela !

Pour tester cette modification, accédez à la page d’accueil dans un navigateur et assurez-vous que vous êtes déconnecté. Étant donné que le comportement par défaut d’une application ASP.NET consiste à autoriser tous les visiteurs et, puisque nous n’avons pas apporté de modifications d’autorisation au fichier `Web.config` du répertoire racine, nous sommes en mesure de consulter les fichiers dans le répertoire racine en tant que visiteur anonyme.

Cliquez sur le lien création de comptes d’utilisateurs figurant dans la colonne de gauche. Cela vous permet d’atteindre le `~/Membership/CreatingUserAccounts.aspx`. Étant donné que le fichier `Web.config` dans le dossier `Membership` définit des règles d’autorisation pour interdire l’accès anonyme, le `UrlAuthorizationModule` abandonne la demande et renvoie un État non autorisé HTTP 401. Le `FormsAuthenticationModule` le modifie en état de redirection 302, en nous envoyant à la page de connexion. Notez que la page à laquelle nous tentions d’accéder (`CreatingUserAccounts.aspx`) est transmise à la page de connexion via le paramètre `ReturnUrl` QueryString.

[![étant donné que les règles d’autorisation d’URL interdisent l’accès anonyme, nous sommes redirigés vers la page de connexion.](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Figure 5**: étant donné que les règles d’autorisation d’URL interdisent l’accès anonyme, nous sommes redirigés vers la page de connexion ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image15.png))

Une fois la connexion établie, nous sommes redirigés vers la page `CreatingUserAccounts.aspx`. Cette fois, le `UrlAuthorizationModule` autorise l’accès à la page, car nous ne sommes plus anonymes.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Application de règles d’autorisation d’URL à un emplacement spécifique

Les paramètres d’autorisation définis dans la section `<system.web>` de `Web.config` s’appliquent à toutes les ressources ASP.NET de ce répertoire et de ses sous-répertoires (jusqu’à ce qu’ils soient remplacés par un autre fichier de `Web.config`). Dans certains cas, cependant, il est possible que toutes les ressources ASP.NET d’un répertoire donné aient une configuration d’autorisation particulière, à l’exception d’une ou deux pages spécifiques. Pour ce faire, ajoutez un élément `<location>` dans `Web.config`, en le pointant vers le fichier dont les règles d’autorisation diffèrent, et en définissant ses règles d’autorisation uniques dans celui-ci.

Pour illustrer l’utilisation de l’élément `<location>` pour remplacer les paramètres de configuration d’une ressource spécifique, nous allons personnaliser les paramètres d’autorisation afin que seul Tito puisse visiter `CreatingUserAccounts.aspx`. Pour ce faire, ajoutez un élément `<location>` au fichier `Web.config` du dossier `Membership` et mettez à jour son balisage afin qu’il ressemble à ce qui suit :

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

L’élément `<authorization>` dans `<system.web>` définit les règles d’autorisation d’URL par défaut pour les ressources ASP.NET dans le dossier `Membership` et ses sous-dossiers. L’élément `<location>` nous permet de remplacer ces règles pour une ressource particulière. Dans le balisage ci-dessus, l’élément `<location>` fait référence à la page `CreatingUserAccounts.aspx` et spécifie ses règles d’autorisation, telles que pour autoriser Tito, mais refuser à tous les autres utilisateurs.

Pour tester ce changement d’autorisation, commencez par visiter le site Web en tant qu’utilisateur anonyme. Si vous tentez d’accéder à une page du dossier `Membership`, par exemple `UserBasedAuthorization.aspx`, le `UrlAuthorizationModule` refuse la demande et vous êtes redirigé vers la page de connexion. Après vous être connecté en tant que, par exemple Scott, vous pouvez accéder à n’importe quelle page du dossier `Membership` *, à l’exception* de `CreatingUserAccounts.aspx`. Si vous tentez de consulter des `CreatingUserAccounts.aspx` connectés en tant que n’importe qui, mais Tito, une tentative d’accès non autorisée se produit, vous redirigez vers la page de connexion.

> [!NOTE]
> L’élément `<location>` doit figurer en dehors de l’élément `<system.web>` de la configuration. Vous devez utiliser un élément `<location>` distinct pour chaque ressource dont vous souhaitez substituer les paramètres d’autorisation.

### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Découvrez comment l'`UrlAuthorizationModule`utilise les règles d’autorisation pour accorder ou refuser l’accès

L' `UrlAuthorizationModule` détermine s’il convient d’autoriser une identité particulière pour une URL particulière en analysant les règles d’autorisation d’URL une par une, en commençant par la première et en le retardant. Dès qu’une correspondance est trouvée, l’accès est accordé ou refusé à l’utilisateur, selon si la correspondance a été trouvée dans un élément `<allow>` ou `<deny>`. <strong>Si aucune correspondance n’est trouvée, l’utilisateur est autorisé à y accéder.</strong> Par conséquent, si vous souhaitez restreindre l’accès, il est impératif d’utiliser un élément `<deny>` comme dernier élément de la configuration d’autorisation d’URL. <strong>Si vous omettez un</strong> élément<strong>`<deny>`</strong> <strong>, l’accès est accordé à tous les utilisateurs.</strong>

Pour mieux comprendre le processus utilisé par le `UrlAuthorizationModule` pour déterminer l’autorité, prenez en compte les exemples de règles d’autorisation d’URL que nous avons examinés plus haut dans cette étape. La première règle est un élément `<allow>` qui autorise l’accès à Tito et Scott. La deuxième règle est un élément `<deny>` qui refuse l’accès à tout le monde. Si un utilisateur anonyme visite, le `UrlAuthorizationModule` commence par demander, est-il anonyme Scott ou Tito ? La réponse est évidemment non, donc elle passe à la deuxième règle. Est-il anonyme dans le jeu de tout le monde ? Dans la mesure où la réponse est oui, la règle de `<deny>` est mise en vigueur et le visiteur est redirigé vers la page de connexion. De même, si Jisun est en visite, le `UrlAuthorizationModule` commence par demander, Jisun soit Scott, soit Tito ? Dans la mesure où elle ne l’est pas, le `UrlAuthorizationModule` passe à la deuxième question, est Jisun dans l’ensemble de tout le monde ? Elle est, par conséquent, elle se voit également refuser l’accès. Enfin, si Tito visite, la première question posée par le `UrlAuthorizationModule` est une réponse affirmative, Tito est donc accordé à l’accès.

Étant donné que le `UrlAuthorizationModule` traite les règles d’autorisation de haut en haut, en s’arrêtant à toute correspondance, il est important que les règles plus spécifiques soient antérieures à celles qui sont moins spécifiques. Autrement dit, pour définir des règles d’autorisation qui interdisent les Jisun et les utilisateurs anonymes, mais qui autorisent tous les autres utilisateurs authentifiés, vous devez commencer par la règle la plus spécifique, qui a un impact sur Jisun, puis passer aux règles moins spécifiques : celles qui autorisent toutes les autres utilisateurs authentifiés, mais refus de tous les utilisateurs anonymes. Les règles d’autorisation d’URL suivantes implémentent cette stratégie en refusant d’abord Jisun, puis en refusant tout utilisateur anonyme. L’accès n’est accordé à aucun utilisateur authentifié autre que Jisun, car aucune de ces instructions de `<deny>` ne correspond.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Étape 2 : correction du flux de travail pour les utilisateurs authentifiés non autorisés

Comme nous l’avons vu précédemment dans ce didacticiel, dans la section examen du flux de travail d’autorisation d’URL, chaque fois qu’une demande non autorisée s’est dépassée, le `UrlAuthorizationModule` abandonne la demande et renvoie un état HTTP 401 non autorisé. Cet état 401 est modifié par le `FormsAuthenticationModule` dans un état de redirection 302 qui envoie l’utilisateur à la page de connexion. Ce flux de travail se produit sur toute requête non autorisée, même si l’utilisateur est authentifié.

Le retour d’un utilisateur authentifié à la page de connexion est susceptible de les confondre, car ils se sont déjà connectés au système. Avec un peu de travail, nous pouvons améliorer ce flux de travail en redirigeant les utilisateurs authentifiés qui effectuent des requêtes non autorisées sur une page qui explique qu’ils ont tenté d’accéder à une page restreinte.

Commencez par créer une nouvelle page ASP.NET dans le dossier racine de l’application Web nommée `UnauthorizedAccess.aspx`; n’oubliez pas d’associer cette page à la page maître `Site.master`. Après avoir créé cette page, supprimez le contrôle de contenu qui référence le `LoginContent` ContentPlaceHolder afin que le contenu par défaut de la page maître s’affiche. Ensuite, ajoutez un message expliquant la situation, à savoir que l’utilisateur a tenté d’accéder à une ressource protégée. Après l’ajout d’un tel message, le balisage déclaratif de la page `UnauthorizedAccess.aspx` doit se présenter comme suit :

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

Nous devons maintenant modifier le flux de travail de sorte que si une demande non autorisée est exécutée par un utilisateur authentifié, elle est envoyée à la page de `UnauthorizedAccess.aspx` au lieu de la page de connexion. La logique qui redirige les demandes non autorisées vers la page de connexion est enfouie dans une méthode privée de la classe `FormsAuthenticationModule`. nous ne pouvons donc pas personnaliser ce comportement. Toutefois, nous pouvons ajouter notre propre logique à la page de connexion qui redirige l’utilisateur vers `UnauthorizedAccess.aspx`, si nécessaire.

Lorsque le `FormsAuthenticationModule` redirige un visiteur non autorisé vers la page de connexion, il ajoute l’URL non autorisée demandée à la chaîne de requête portant le nom `ReturnUrl`. Par exemple, si un utilisateur non autorisé a tenté de visiter `OnlyTito.aspx`, le `FormsAuthenticationModule` le redirige vers `Login.aspx?ReturnUrl=OnlyTito.aspx`. Par conséquent, si la page de connexion est atteinte par un utilisateur authentifié avec une chaîne de connexion qui comprend le paramètre `ReturnUrl`, nous savons que cet utilisateur non authentifié tente simplement de visiter une page qu’il n’est pas autorisé à afficher. Dans ce cas, nous voulons le rediriger vers `UnauthorizedAccess.aspx`.

Pour ce faire, ajoutez le code suivant au gestionnaire d’événements `Page_Load` de la page de connexion :

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

Le code ci-dessus redirige les utilisateurs authentifiés et non autorisés vers la page `UnauthorizedAccess.aspx`. Pour voir cette logique en action, visitez le site en tant que visiteur anonyme, puis cliquez sur le lien création de comptes d’utilisateur dans la colonne de gauche. Vous accédez alors à la page `~/Membership/CreatingUserAccounts.aspx`, qui, à l’étape 1, nous avons configuré pour autoriser uniquement l’accès à Tito. Étant donné que les utilisateurs anonymes sont interdits, le `FormsAuthenticationModule` les redirige vers la page de connexion.

À ce stade, nous sommes anonymes, donc `Request.IsAuthenticated` retourne `false` et nous ne sommes pas redirigés vers `UnauthorizedAccess.aspx`. Au lieu de cela, la page de connexion s’affiche. Connectez-vous en tant qu’utilisateur autre que Tito, par exemple Bruce. Après avoir entré les informations d’identification appropriées, la page de connexion les redirige vers `~/Membership/CreatingUserAccounts.aspx`. Toutefois, étant donné que cette page n’est accessible qu’à Tito, nous n’avons pas l’autorisation de l’afficher et vous revenez rapidement à la page de connexion. Cette fois, toutefois, `Request.IsAuthenticated` retourne `true` (et le paramètre `ReturnUrl` QueryString EXISTS), de sorte que nous sommes redirigés vers la page `UnauthorizedAccess.aspx`.

[![les utilisateurs non autorisés authentifiés sont redirigés vers UnauthorizedAccess. aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Figure 6**: les utilisateurs authentifiés et non autorisés sont redirigés vers `UnauthorizedAccess.aspx` ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image18.png))

Ce flux de travail personnalisé offre une expérience utilisateur plus rationnelle et plus simple en court-circuitant le cycle illustré à la figure 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Étape 3 : limitation des fonctionnalités en fonction de l’utilisateur actuellement connecté

L’autorisation d’URL vous permet de spécifier facilement des règles d’autorisation grossistes. Comme nous l’avons vu à l’étape 1, avec l’autorisation d’URL, nous pouvons déterminer succinctement les identités autorisées et celles qui ne sont pas autorisées à afficher une page particulière ou toutes les pages d’un dossier. Toutefois, dans certains scénarios, nous pouvons autoriser tous les utilisateurs à visiter une page, mais limiter les fonctionnalités de la page en fonction de l’utilisateur qui les visite.

Prenons le cas d’un site Web de commerce électronique qui permet aux visiteurs authentifiés d’examiner leurs produits. Lorsqu’un utilisateur anonyme visite la page d’un produit, il ne voit que les informations sur le produit et n’a pas la possibilité de sortir d’une revue. Toutefois, un utilisateur authentifié visitant la même page voit l’interface de révision. Si l’utilisateur authentifié n’avait pas encore examiné ce produit, l’interface lui permettrait de soumettre une revue. dans le cas contraire, elle indiquera la révision précédemment soumise. Pour aller plus loin dans ce scénario, la page produit peut afficher des informations supplémentaires et offrir des fonctionnalités étendues pour les utilisateurs qui travaillent pour la société de commerce électronique. Par exemple, la page produit peut répertorier l’inventaire en stock et inclure des options permettant de modifier le prix et la description du produit lorsqu’il est visité par un employé.

Ces règles d’autorisation fine grain peuvent être implémentées de façon déclarative ou par programme (ou par le biais d’une combinaison des deux). Dans la section suivante, nous verrons comment implémenter l’autorisation fine graine via le contrôle LoginView. Après cela, nous explorerons les techniques de programmation. Avant de pouvoir examiner l’application des règles d’autorisation affinées, toutefois, nous devons d’abord créer une page dont la fonctionnalité dépend de l’utilisateur qui la visite.

Nous allons créer une page qui répertorie les fichiers d’un répertoire particulier au sein d’un contrôle GridView. En plus de répertorier le nom, la taille et d’autres informations de chaque fichier, le GridView inclut deux colonnes de LinkButtons : une vue intitulée et l’autre intitulée Delete. Si l’utilisateur clique sur l’affichage LinkButton, le contenu du fichier sélectionné s’affiche. Si l’utilisateur clique sur supprimer LinkButton, le fichier est supprimé. Nous allons commencer par créer cette page de sorte que ses fonctionnalités d’affichage et de suppression soient disponibles pour tous les utilisateurs. Dans les sections utilisation du contrôle LoginView et limitation des fonctionnalités par programmation, nous verrons comment activer ou désactiver ces fonctionnalités en fonction de l’utilisateur visitant la page.

> [!NOTE]
> La page ASP.NET que nous allons créer utilise un contrôle GridView pour afficher une liste de fichiers. Dans la mesure où cette série de didacticiels se concentre sur l’authentification par formulaire, l’autorisation, les comptes d’utilisateurs et les rôles, je ne souhaite pas consacrer trop de temps à discuter du fonctionnement interne du contrôle GridView. Bien que ce didacticiel fournisse des instructions pas à pas pour configurer cette page, il n’aborde pas en détail les raisons pour lesquelles certains choix ont été effectués, ou les effets spécifiques sur la sortie rendue. Pour un examen approfondi du contrôle GridView, consultez la série de didacticiels mon *[utilisation de données dans ASP.NET 2,0](../../data-access/index.md)* .

Commencez par ouvrir le fichier `UserBasedAuthorization.aspx` dans le dossier `Membership` et ajoutez un contrôle GridView à la page nommée `FilesGrid`. À partir de la balise active de GridView, cliquez sur le lien modifier les colonnes pour ouvrir la boîte de dialogue champs. À partir de là, désactivez la case à cocher générer automatiquement les champs dans le coin inférieur gauche. Ensuite, ajoutez un bouton Sélectionner, un bouton supprimer et deux BoundFields dans le coin supérieur gauche (les boutons Sélectionner et supprimer se trouvent sous le type CommandField). Affectez à la propriété `SelectText` du bouton Select la valeur View et aux propriétés `HeaderText` et `DataField` du premier BoundField la valeur Name. Affectez à la propriété de `HeaderText` du deuxième BoundField la taille en octets, à sa propriété `DataField` la valeur Length, à sa propriété `DataFormatString` la valeur {0:N0} et à sa propriété `HtmlEncode` la valeur false.

Après avoir configuré les colonnes du contrôle GridView, cliquez sur OK pour fermer la boîte de dialogue champs. À partir de la Fenêtre Propriétés, définissez la propriété `DataKeyNames` de GridView sur `FullName`. À ce stade, le balisage déclaratif de GridView doit se présenter comme suit :

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Une fois le balisage de GridView créé, nous sommes prêts à écrire le code permettant de récupérer les fichiers dans un répertoire particulier et de les lier au GridView. Ajoutez le code suivant au gestionnaire d’événements `Page_Load` de la page :

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

Le code ci-dessus utilise la [classe`DirectoryInfo`](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) pour obtenir une liste des fichiers dans le dossier racine de l’application. La [méthode`GetFiles()`](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) retourne tous les fichiers du répertoire sous la forme d’un tableau d' [objets`FileInfo`](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), qui est ensuite lié au GridView. L’objet `FileInfo` a un assortiment de propriétés, comme `Name`, `Length`et `IsReadOnly`, entre autres. Comme vous pouvez le voir à partir de son balisage déclaratif, le contrôle GridView affiche uniquement les propriétés `Name` et `Length`.

> [!NOTE]
> Les classes `DirectoryInfo` et `FileInfo` se trouvent dans l' [espace de noms`System.IO`](https://msdn.microsoft.com/library/system.io.aspx). Par conséquent, vous devrez faire précéder ces noms de classe de leurs noms d’espaces de noms ou l’importation de l’espace de noms dans le fichier de classe (via `using System.IO`).

Prenez un moment pour visiter cette page via un navigateur. La liste des fichiers résidant dans le répertoire racine de l’application s’affiche. Le fait de cliquer sur l’un des LinkButtons d’affichage ou de suppression entraîne une publication, mais aucune action ne se produit, car nous n’avons pas encore créé les gestionnaires d’événements nécessaires.

[![le contrôle GridView répertorie les fichiers dans le répertoire racine de l’application Web](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Figure 7**: le contrôle GridView répertorie les fichiers dans le répertoire racine de l’application Web ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image21.png))

Nous avons besoin d’un moyen d’afficher le contenu du fichier sélectionné. Revenez à Visual Studio et ajoutez une zone de texte nommée `FileContents` au-dessus du contrôle GridView. Définissez sa propriété `TextMode` sur `MultiLine` et ses propriétés `Columns` et `Rows` sur 95% et 10 respectivement.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Ensuite, créez un gestionnaire d’événements pour l' [événement`SelectedIndexChanged`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) de GridView et ajoutez le code suivant :

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Ce code utilise la propriété `SelectedValue` de GridView pour déterminer le nom de fichier complet du fichier sélectionné. En interne, la collection `DataKeys` est référencée pour obtenir le `SelectedValue`. il est donc impératif que vous dédéfinissez la propriété `DataKeyNames` de GridView sur Name, comme décrit précédemment dans cette étape. La [classe`File`](https://msdn.microsoft.com/library/system.io.file.aspx) est utilisée pour lire le contenu du fichier sélectionné dans une chaîne, qui est ensuite assignée à la propriété `Text` de la zone de texte `FileContents`, affichant ainsi le contenu du fichier sélectionné sur la page.

[![le contenu du fichier sélectionné s’affiche dans la zone de texte.](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Figure 8**: le contenu du fichier sélectionné s’affiche dans la zone de texte ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image24.png))

> [!NOTE]
> Si vous affichez le contenu d’un fichier qui contient un balisage HTML, puis tentez d’afficher ou de supprimer un fichier, vous recevrez une erreur de `HttpRequestValidationException`. Cela se produit parce que, lors de la publication (postback), le contenu de la zone de texte est renvoyé au serveur Web. Par défaut, ASP.NET déclenche une erreur `HttpRequestValidationException` chaque fois que du contenu de publication potentiellement dangereux, tel que le balisage HTML, est détecté. Pour empêcher l’apparition de cette erreur, désactivez la validation de demande pour la page en ajoutant `ValidateRequest="false"` à la directive `@Page`. Pour plus d’informations sur les avantages de la validation des demandes, ainsi que sur les précautions à prendre lors de la désactivation, consultez [validation des demandes de lecture-prévention des attaques de script](https://asp.net/learn/whitepapers/request-validation/).

Enfin, ajoutez un gestionnaire d’événements avec le code suivant pour l' [événement`RowDeleting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx)du GridView :

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

Le code affiche simplement le nom complet du fichier à supprimer dans le `FileContents` zone de texte *sans* supprimer réellement le fichier.

[![le fait de cliquer sur le bouton supprimer ne supprime pas réellement le fichier.](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Figure 9**: le fait de cliquer sur le bouton supprimer ne supprime pas réellement le fichier ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image27.png))

À l’étape 1, nous avons configuré les règles d’autorisation d’URL pour empêcher les utilisateurs anonymes d’afficher les pages dans le dossier `Membership`. Pour mieux exposer l’authentification fine grain, autorisez les utilisateurs anonymes à visiter la page `UserBasedAuthorization.aspx`, mais avec des fonctionnalités limitées. Pour ouvrir cette page pour que tous les utilisateurs puissent y accéder, ajoutez l’élément `<location>` suivant au fichier `Web.config` dans le dossier `Membership` :

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Après avoir ajouté cet élément `<location>`, testez les nouvelles règles d’autorisation d’URL en vous déconnectant du site. En tant qu’utilisateur anonyme, vous devez être autorisé à accéder à la page `UserBasedAuthorization.aspx`.

À l’heure actuelle, tout utilisateur authentifié ou anonyme peut accéder à la page de `UserBasedAuthorization.aspx` et afficher ou supprimer des fichiers. Nous allons le faire afin que seuls les utilisateurs authentifiés puissent afficher le contenu d’un fichier et que seul Tito puisse supprimer un fichier. Ces règles d’autorisation précises peuvent être appliquées de façon déclarative, par programmation ou par le biais d’une combinaison des deux méthodes. Utilisons l’approche déclarative pour limiter les utilisateurs autorisés à afficher le contenu d’un fichier. Nous allons utiliser l’approche par programmation pour limiter les personnes autorisées à supprimer un fichier.

### <a name="using-the-loginview-control"></a>Utilisation du contrôle LoginView

Comme nous l’avons vu dans les didacticiels précédents, le contrôle LoginView est utile pour afficher des interfaces différentes pour les utilisateurs authentifiés et anonymes, et offre un moyen simple de masquer des fonctionnalités qui ne sont pas accessibles aux utilisateurs anonymes. Étant donné que les utilisateurs anonymes ne peuvent pas afficher ou supprimer des fichiers, nous devons uniquement afficher la zone de texte `FileContents` lorsque la page est visitée par un utilisateur authentifié. Pour ce faire, ajoutez un contrôle LoginView à la page, nommez-le `LoginViewForFileContentsTextBox`et déplacez le balisage déclaratif du `FileContents` TextBox dans le `LoggedInTemplate`du contrôle LoginView.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

Les contrôles Web dans les modèles de LoginView ne sont plus directement accessibles à partir de la classe code-behind. Par exemple, les gestionnaires d’événements `SelectedIndexChanged` et `RowDeleting` du GridView `FilesGrid` référencent actuellement le contrôle de zone de texte `FileContents` à l’aide d’un code tel que :

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Toutefois, ce code n’est plus valide. En déplaçant la zone de texte `FileContents` dans le `LoggedInTemplate` la zone de texte ne peut pas être accédée directement. Au lieu de cela, nous devons utiliser la méthode `FindControl("controlId")` pour référencer le contrôle par programmation. Mettez à jour les gestionnaires d’événements `FilesGrid` pour référencer la zone de texte comme suit :

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Après avoir déplacé la zone de texte vers le `LoggedInTemplate` du LoginView et mis à jour le code de la page pour faire référence à la zone de texte à l’aide du modèle de `FindControl("controlId")`, accédez à la page en tant qu’utilisateur anonyme. Comme le montre la figure 10, la zone de texte `FileContents` n’est pas affichée. Toutefois, la vue LinkButton est toujours affichée.

[![le contrôle LoginView affiche uniquement la zone de texte FileContents pour les utilisateurs authentifiés](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Figure 10**: le contrôle LoginView affiche uniquement la zone de texte `FileContents` pour les utilisateurs authentifiés ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image30.png))

L’une des façons de masquer le bouton d’affichage pour les utilisateurs anonymes consiste à convertir le champ GridView en TemplateField. Cela génère un modèle qui contient le balisage déclaratif pour la vue LinkButton. Nous pouvons ensuite ajouter un contrôle LoginView au TemplateField et placer le LinkButton dans le `LoggedInTemplate`du LoginView, masquant ainsi le bouton afficher des visiteurs anonymes. Pour ce faire, cliquez sur le lien modifier les colonnes à partir de la balise active de GridView pour ouvrir la boîte de dialogue champs. Ensuite, sélectionnez le bouton Sélectionner dans la liste dans l’angle inférieur gauche, puis cliquez sur le lien convertir ce champ en TemplateField. Si vous procédez ainsi, le balisage déclaratif du champ sera modifié à partir de :

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 À : 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

À ce stade, nous pouvons ajouter un LoginView au TemplateField. La balise suivante affiche la vue LinkButton uniquement pour les utilisateurs authentifiés.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Comme le montre la figure 11, le résultat final n’est pas le même que si la colonne de vue est toujours affichée, même si la vue LinkButtons de la colonne est masquée. Nous allons voir comment masquer la totalité de la colonne GridView (et pas seulement le LinkButton) dans la section suivante.

[![le contrôle LoginView masque l’affichage LinkButtons pour les visiteurs anonymes](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Figure 11**: le contrôle LoginView masque l’affichage LinkButtons pour les visiteurs anonymes ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image33.png))

### <a name="programmatically-limiting-functionality"></a>Limitation des fonctionnalités par programmation

Dans certains cas, les techniques déclaratives sont insuffisantes pour limiter les fonctionnalités à une page. Par exemple, la disponibilité de certaines fonctionnalités de page peut dépendre de critères au-delà du fait que l’utilisateur visitant la page soit anonyme ou authentifié. Dans ce cas, les différents éléments de l’interface utilisateur peuvent être affichés ou masqués par le biais d’une méthode de programmation.

Pour limiter la fonctionnalité par programme, vous devez effectuer deux tâches :

1. Déterminer si l’utilisateur visitant la page peut accéder aux fonctionnalités, et
2. Modifiez par programme l’interface utilisateur selon que l’utilisateur a accès à la fonctionnalité en question.

Pour illustrer l’application de ces deux tâches, autorisez uniquement Tito à supprimer des fichiers du contrôle GridView. Notre première tâche, ensuite, est de déterminer s’il s’agit de Tito visitant la page. Une fois qu’elle a été déterminée, nous devons masquer (ou afficher) la colonne de suppression de GridView. Les colonnes du contrôle GridView sont accessibles par le biais de sa propriété `Columns` ; une colonne est rendue uniquement si sa propriété `Visible` est définie sur `true` (valeur par défaut).

Ajoutez le code suivant au gestionnaire d’événements `Page_Load` avant de lier les données au GridView :

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Comme nous l’avons vu dans le didacticiel sur l' [*authentification par formulaire*](../introduction/an-overview-of-forms-authentication-cs.md) , `User.Identity.Name` retourne le nom de l’identité. Cela correspond au nom d’utilisateur entré dans le contrôle de connexion. S’il s’agit d’Tito visitant la page, la propriété `Visible` de la deuxième colonne de GridView est définie sur `true`; dans le cas contraire, il est défini sur `false`. Le résultat net est que lorsqu’une personne autre que Tito visite la page, qu’il s’agisse d’un autre utilisateur authentifié ou d’un utilisateur anonyme, la colonne de suppression n’est pas rendue (voir figure 12); Toutefois, lorsque Tito visite la page, la colonne Delete est présente (voir la figure 13).

[![la colonne de suppression n’est pas rendue lorsqu’elle est visitée par une personne autre que Tito (par exemple, Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Figure 12**: la colonne de suppression n’est pas rendue lorsqu’elle est visitée par une personne autre que Tito (par exemple, Bruce) ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image36.png))

[![la colonne de suppression est rendue pour Tito](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Figure 13**: rendu de la colonne de suppression pour Tito ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image39.png))

## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Étape 4 : application de règles d’autorisation à des classes et des méthodes

À l’étape 3, nous n’avons pas autorisé les utilisateurs anonymes à afficher le contenu d’un fichier et à interdire à tous les utilisateurs, mais Tito de supprimer des fichiers. Pour ce faire, vous devez masquer les éléments d’interface utilisateur associés pour les visiteurs non autorisés par le biais de techniques déclaratives et par programmation. Pour notre exemple simple, le masquage correct des éléments de l’interface utilisateur était simple, mais qu’en est-il des sites plus complexes dans lesquels il peut y avoir de nombreuses façons d’effectuer la même fonctionnalité ? En limitant cette fonctionnalité à des utilisateurs non autorisés, que se passe-t-il si nous oublions de masquer ou de désactiver tous les éléments d’interface utilisateur applicables ?

Un moyen simple de garantir qu’une fonctionnalité particulière n’est pas accessible par un utilisateur non autorisé consiste à décorer cette classe ou méthode avec l' [attribut`PrincipalPermission`](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Lorsque le Runtime .NET utilise une classe ou exécute l’une de ses méthodes, il vérifie que le contexte de sécurité actuel est autorisé à utiliser la classe ou à exécuter la méthode. L’attribut `PrincipalPermission` fournit un mécanisme permettant de définir ces règles.

Nous allons démontrer l’utilisation de l’attribut `PrincipalPermission` sur les gestionnaires d’événements `SelectedIndexChanged` et `RowDeleting` de GridView pour interdire l’exécution par des utilisateurs et utilisateurs anonymes autres que Tito, respectivement. Il vous suffit d’ajouter l’attribut approprié à chaque définition de fonction :

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

L’attribut du gestionnaire d’événements `SelectedIndexChanged` stipule que seuls les utilisateurs authentifiés peuvent exécuter le gestionnaire d’événements, où l’attribut sur le gestionnaire d’événements `RowDeleting` limite l’exécution à Tito.

Si, d’un autre côté, un utilisateur autre que Tito tente d’exécuter le gestionnaire d’événements `RowDeleting` ou qu’un utilisateur non authentifié tente d’exécuter le gestionnaire d’événements `SelectedIndexChanged`, le Runtime .NET déclenchera un `SecurityException`.

[![si le contexte de sécurité n’est pas autorisé à exécuter la méthode, une exception SecurityException est levée](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Figure 14**: si le contexte de sécurité n’est pas autorisé à exécuter la méthode, une `SecurityException` est levée ([cliquez pour afficher l’image en taille réelle](user-based-authorization-cs/_static/image42.png))

> [!NOTE]
> Pour permettre à plusieurs contextes de sécurité d’accéder à une classe ou une méthode, Décorez la classe ou la méthode avec un attribut `PrincipalPermission` pour chaque contexte de sécurité. Autrement dit, pour autoriser à la fois Tito et Bruce à exécuter le gestionnaire d’événements `RowDeleting`, ajoutez *deux* attributs `PrincipalPermission` :

[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

En plus des pages ASP.NET, de nombreuses applications ont également une architecture qui comprend différentes couches, telles que la logique métier et les couches d’accès aux données. Ces couches sont généralement implémentées en tant que bibliothèques de classes et offrent des classes et des méthodes pour l’exécution de la logique métier et des fonctionnalités liées aux données. L’attribut `PrincipalPermission` est utile pour appliquer des règles d’autorisation à ces couches.

Pour plus d’informations sur l’utilisation de l’attribut `PrincipalPermission` pour définir des règles d’autorisation sur des classes et des méthodes, reportez-vous au billet de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/) [Ajout de règles d’autorisation à des couches de données et métier à l’aide de `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment appliquer les règles d’autorisation basées sur l’utilisateur. Nous avons commencé par regarder ASP. Infrastructure d’autorisation d’URL du réseau. À chaque demande, le `UrlAuthorizationModule` du moteur ASP.NET inspecte les règles d’autorisation d’URL définies dans la configuration de l’application pour déterminer si l’identité est autorisée à accéder à la ressource demandée. En bref, l’autorisation d’URL permet de spécifier facilement des règles d’autorisation pour une page spécifique ou pour toutes les pages d’un répertoire particulier.

L’infrastructure d’autorisation d’URL applique les règles d’autorisation page par page. Avec l’autorisation d’URL, soit l’identité à l’origine de la demande est autorisée à accéder à une ressource particulière. De nombreux scénarios, toutefois, appellent des règles d’autorisation plus précises. Plutôt que de définir les personnes autorisées à accéder à une page, nous pouvons être amenés à autoriser tout le monde à accéder à une page, mais à afficher des données différentes ou à proposer des fonctionnalités différentes en fonction de l’utilisateur visitant la page. L’autorisation au niveau de la page implique généralement le masquage d’éléments d’interface utilisateur spécifiques afin d’empêcher les utilisateurs non autorisés d’accéder à des fonctionnalités interdites. En outre, il est possible d’utiliser des attributs pour limiter l’accès aux classes et l’exécution de ses méthodes pour certains utilisateurs.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Ajout de règles d’autorisation à des couches de données et métier à l’aide de `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autorisation ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Modifications entre la sécurité IIS6 et IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Configuration de fichiers et de sous-répertoires spécifiques](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Limitation des fonctionnalités de modification des données en fonction de l’utilisateur](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [Démarrages rapides du contrôle LoginView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Compréhension de l’autorisation d’URL IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [Documentation technique `UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Utilisation des données dans ASP.NET 2,0](../../data-access/index.md)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est *[Sams vous apprend vous-même ASP.NET 2,0 en 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott peut être contacté au [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](validating-user-credentials-against-the-membership-user-store-cs.md)
> [Suivant](storing-additional-user-information-cs.md)
