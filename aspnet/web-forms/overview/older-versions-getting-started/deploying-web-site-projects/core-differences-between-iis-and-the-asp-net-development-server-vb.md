---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: Principales différences entre IIS et le Serveur de développement ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Lors du test d’une application ASP.NET localement, il est probable que vous utilisez le serveur Web de développement ASP.NET. Toutefois, le site Web de production est probablement pow...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 880bb403e671446a77d7eebccf578a1dc714d1f9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586515"
---
# <a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>Différences principales entre IIS et le serveur de développement ASP.NET (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> Lors du test d’une application ASP.NET localement, il est probable que vous utilisez le serveur Web de développement ASP.NET. Toutefois, le site Web de production est probablement doté d’IIS. Il existe quelques différences entre la façon dont ces serveurs Web gèrent les demandes et ces différences peuvent avoir des conséquences importantes. Ce didacticiel explore quelques-unes des différences les plus allemandes.

## <a name="introduction"></a>Introduction

Chaque fois qu’un utilisateur visite une application ASP.NET, son navigateur envoie une demande au site Web. Cette requête est récupérée par le logiciel de serveur Web, qui coordonne le runtime ASP.NET pour générer et retourner le contenu de la ressource demandée. Le[ POI i nformations **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) est une suite de services qui fournit des fonctionnalités Internet courantes pour les serveurs Windows. IIS est le serveur Web le plus couramment utilisé pour les applications ASP.NET dans des environnements de production. Il est probable que le logiciel de serveur Web utilisé par votre fournisseur d’hébergement Web serve à votre application ASP.NET. IIS peut également être utilisé comme logiciel de serveur Web dans l’environnement de développement, même si cela implique l’installation d’IIS et sa configuration correcte.

Le Serveur de développement ASP.NET est une autre option de serveur Web pour l’environnement de développement. elle est fournie avec et est intégrée à Visual Studio. À moins que l’application Web ne soit configurée pour utiliser IIS, le Serveur de développement ASP.NET est automatiquement démarré et utilisé comme serveur Web la première fois que vous visitez une page Web dans Visual Studio. Les applications Web de démonstration que nous avons créées dans le didacticiel [*détermination des fichiers qui doivent être déployés*](determining-what-files-need-to-be-deployed-vb.md) étaient à la fois des applications Web basées sur le système de fichiers qui n’ont pas été configurées pour utiliser IIS. Par conséquent, lorsque vous visitez l’un de ces sites Web dans Visual Studio, le Serveur de développement ASP.NET est utilisé.

Dans un monde parfait, les environnements de développement et de production seraient identiques. Toutefois, comme nous l’avons vu dans le didacticiel précédent, il n’est pas rare que les environnements aient des paramètres de configuration différents. L’utilisation de différents logiciels de serveur Web dans les environnements ajoute une autre variable qui doit être prise en considération lors du déploiement d’une application. Ce didacticiel couvre les principales différences entre IIS et le Serveur de développement ASP.NET. En raison de ces différences, il existe des scénarios dans lesquels le code qui s’exécute parfaitement dans l’environnement de développement lève une exception ou se comporte différemment lorsqu’il est exécuté en production.

## <a name="security-context-differences"></a>Différences du contexte de sécurité

Chaque fois que le logiciel de serveur Web gère une demande entrante, il l’associe à un contexte de sécurité particulier. Ces informations de contexte de sécurité sont utilisées par le système d’exploitation pour déterminer les actions autorisées par la demande. Par exemple, une page ASP.NET peut inclure du code qui journalise un message dans un fichier sur le disque. Pour que cette page ASP.NET s’exécute sans erreur, le contexte de sécurité doit avoir les autorisations de niveau système de fichiers appropriées, à savoir les autorisations d’écriture sur ce fichier.

Le Serveur de développement ASP.NET associe les demandes entrantes au contexte de sécurité de l’utilisateur actuellement connecté. Si vous êtes connecté à votre bureau en tant qu’administrateur, les pages ASP.NET desservies par le Serveur de développement ASP.NET auront les mêmes droits d’accès qu’un administrateur. Toutefois, les requêtes ASP.NET gérées par IIS sont associées à un compte d’ordinateur spécifique. Par défaut, le compte d’ordinateur de service réseau est utilisé par les versions 6 et 7 d’IIS, même si votre fournisseur d’hébergement Web peut avoir configuré un compte unique pour chaque client. De plus, votre fournisseur d’hébergement Web a probablement accordé des autorisations limitées à ce compte d’ordinateur. Le résultat net est que vous pouvez avoir des pages Web qui s’exécutent sans erreur dans l’environnement de développement, mais génèrent des exceptions liées aux autorisations lorsqu’elles sont hébergées dans l’environnement de production.

Pour afficher ce type d’erreur dans l’action, j’ai créé une page sur le site Web de révisions de livres qui crée un fichier sur le disque qui stocke la date et l’heure les plus récentes auxquelles quelqu’un a consulté le *ASP.NET 3,5 en 24 heures* . Pour suivre la procédure, ouvrez la page `~/Tech/TYASP35.aspx` et ajoutez le code suivant au gestionnaire d’événements `Page_Load` :

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> La [méthode`File.WriteAllText`](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) crée un fichier s’il n’existe pas, puis y écrit le contenu spécifié. Si le fichier existe déjà, son contenu est remplacé.

Ensuite, visitez la page *enseigner vous-même ASP.NET 3,5 en 24 heures* dans l’environnement de développement à l’aide du serveur de développement ASP.net. En supposant que vous êtes connecté à votre ordinateur à l’aide d’un compte disposant des autorisations adéquates pour créer et modifier un fichier texte dans le répertoire racine de l’application Web, la revue de livre est la même que précédemment, mais chaque fois que la page est visitée, la date et l’heure et l’adresse IP de l’utilisateur sont stockées dans le fichier `LastTYASP35Access.txt`. Pointez votre navigateur sur ce fichier ; vous devez voir un message similaire à celui illustré à la figure 1.

[![le fichier texte contient la date et l’heure de la dernière consultation du livre&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**Figure 1**: le fichier texte contient la date et l’heure de la dernière consultation du livre ([cliquez pour afficher l’image en taille réelle](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))

Déployez l’application Web en production, puis accédez à la page de vérification *de l’apprentissage hébergé par vous-même ASP.NET 3,5 dans 24 heures* . À ce stade, vous devriez voir la page de révision de livre normale ou le message d’erreur illustré à la figure 2. Certains fournisseurs d’hôtes Web accordent des autorisations en écriture au compte d’ordinateur ASP.NET anonyme, auquel cas la page fonctionne sans erreur. Toutefois, si votre fournisseur d’hébergement Web interdit l’accès en écriture au compte anonyme, une [exception`UnauthorizedAccessException`](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) est déclenchée lorsque la page `TYASP35.aspx` tente d’écrire la date et l’heure actuelles dans le fichier `LastTYASP35Access.txt`.

[![le compte d’ordinateur par défaut utilisé par IIS ne dispose pas des autorisations nécessaires pour écrire dans le système de fichiers](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**Figure 2**: le compte d’ordinateur par défaut utilisé par IIS ne dispose pas des autorisations nécessaires pour écrire dans le système de fichiers ([Cliquer pour afficher l’image en taille réelle](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))

La bonne nouvelle, c’est que la plupart des fournisseurs d’hôtes Web disposent d’un outil d’autorisation qui vous permet de spécifier des autorisations de système de fichiers dans votre site Web. Accordez au compte ASP.NET anonyme l’accès en écriture au répertoire racine, puis revisitez la page de révision du livre. (Si nécessaire, contactez votre fournisseur d’hébergement Web pour obtenir de l’aide sur la façon d’accorder des autorisations en écriture au compte ASP.NET par défaut.) Cette fois, la page doit se charger sans erreur et le fichier `LastTYASP35Access.txt` doit être créé avec succès.

Le fait est que, étant donné que le Serveur de développement ASP.NET fonctionne dans un contexte de sécurité différent de celui d’IIS, il est possible que vos pages ASP.NET lisent ou écrivent dans le système de fichiers, lisent ou écrivent dans le journal des événements Windows, ou lisent ou écrivent dans Windows  le registre fonctionne comme prévu au moment du développement, mais génère des exceptions en cas de production. Lors de la création d’une application Web qui sera déployée dans un environnement d’hébergement Web partagé, n’effectuez aucune opération de lecture ou d’écriture dans le journal des événements ou le Registre Windows. Notez également les pages ASP.NET qui lisent ou écrivent dans le système de fichiers, car vous devrez peut-être accorder des privilèges de lecture et d’écriture sur les dossiers appropriés dans l’environnement de production.

## <a name="differences-on-serving-static-content"></a>Différences relatives au service du contenu statique

Une autre différence fondamentale entre IIS et le Serveur de développement ASP.NET est la manière dont ils gèrent les demandes de contenu statique. Chaque demande qui arrive dans le Serveur de développement ASP.NET, qu’il s’agisse d’une page ASP.NET, d’une image ou d’un fichier JavaScript, est traitée par le runtime ASP.NET. Par défaut, IIS appelle uniquement le runtime ASP.NET lorsqu’une demande est envoyée pour une ressource ASP.NET, telle qu’une page Web ASP.NET, un service Web, etc. Les demandes de contenu statique-images, fichiers CSS, fichiers JavaScript, fichiers PDF, fichiers ZIP et similaires sont récupérées par IIS sans implication du runtime ASP.NET. (Il est possible d’indiquer à IIS de travailler avec le runtime ASP.NET en cas de contenu statique. pour plus d’informations, consultez la section « exécution de l’authentification basée sur les formulaires et authentification des URL sur les fichiers statiques avec IIS 7 » dans ce didacticiel.)

Le runtime ASP.NET effectue un certain nombre d’étapes pour générer le contenu demandé, y compris l’authentification (identifiant le demandeur) et l’autorisation (en déterminant si le demandeur est autorisé à afficher le contenu demandé). Une forme d’authentification courante est *l’authentification basée sur les formulaires*, dans laquelle un utilisateur est identifié en entrant ses informations d’identification (généralement un nom d’utilisateur et un mot de passe dans les zones de texte dans une page Web). Lors de la validation de leurs informations d’identification, le site Web stocke un cookie de *ticket d’authentification* sur le navigateur de l’utilisateur, qui est envoyé avec chaque requête suivante au site Web et est utilisé pour authentifier l’utilisateur. En outre, il est possible de spécifier des règles *d’autorisation d’URL* qui déterminent ce que les utilisateurs peuvent ou ne peuvent pas accéder à un dossier particulier. De nombreux sites Web ASP.NET utilisent l’authentification basée sur les formulaires et l’autorisation d’URL pour prendre en charge les comptes d’utilisateur et pour définir des parties du site qui sont uniquement accessibles aux utilisateurs authentifiés ou aux utilisateurs qui appartiennent à un certain rôle.

> [!NOTE]
> Pour un examen approfondi d’ASP. L’authentification basée sur les formulaires, l’autorisation d’URL et d’autres fonctionnalités associées au compte d’utilisateur, consultez les didacticiels sur la sécurité de mon [site Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Imaginez un site Web qui prend en charge les comptes d’utilisateur à l’aide de l’autorisation basée sur les formulaires et qui possède un dossier qui, à l’aide de l’autorisation d’URL, est configuré pour autoriser uniquement les utilisateurs authentifiés. Imaginez que ce dossier contient des pages ASP.NET et des fichiers PDF et que seuls les utilisateurs authentifiés peuvent afficher ces fichiers PDF.

Que se passe-t-il si un visiteur tente d’afficher l’un de ces fichiers PDF en entrant l’URL directement dans la barre d’adresse de son navigateur ? Pour le savoir, nous allons créer un nouveau dossier dans le site de révisions de livres, ajouter des fichiers PDF et configurer le site pour qu’il utilise l’autorisation d’URL pour empêcher les utilisateurs anonymes de visiter ce dossier. Si vous téléchargez l’application de démonstration, vous verrez que j’ai créé un dossier nommé `PrivateDocs` et ajouté un fichier PDF à partir des didacticiels sur la sécurité de mon [site Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (comment ajuster !). Le dossier `PrivateDocs` contient également un fichier `Web.config` qui spécifie les règles d’autorisation d’URL pour refuser les utilisateurs anonymes :

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

Enfin, j’ai configuré l’application Web pour utiliser l’authentification basée sur les formulaires en mettant à jour le fichier Web. config dans le répertoire racine, en remplaçant :

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

Par :

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

À l’aide de la Serveur de développement ASP.NET, accédez au site et entrez l’URL directe vers l’un des fichiers PDF dans la barre d’adresse de votre navigateur. Si vous avez téléchargé le site Web associé à ce didacticiel, l’URL doit se présenter comme suit : `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Si vous entrez cette URL dans la barre d’adresses, le navigateur envoie une demande au Serveur de développement ASP.NET pour le fichier. Le Serveur de développement ASP.NET transmet la requête au runtime ASP.NET pour traitement. Étant donné que nous n’avons pas encore ouvert de session et que la `Web.config` dans le dossier `PrivateDocs` est configurée pour refuser l’accès anonyme, le runtime ASP.NET redirige automatiquement vers la page de connexion, `Login.aspx` (voir figure 3). Lorsque vous redirigez l’utilisateur vers la page de connexion, ASP.NET comprend un paramètre `ReturnUrl` QueryString qui indique la page que l’utilisateur tentait d’afficher. Une fois la connexion établie, l’utilisateur peut retourner à cette page.

[![les utilisateurs non autorisés sont automatiquement redirigés vers la page de connexion.](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**Figure 3**: les utilisateurs non autorisés sont automatiquement redirigés vers la page de connexion ([cliquez pour afficher l’image en taille réelle](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))

Voyons maintenant comment cela se comporte en production. Déployez votre application et entrez l’URL directe vers l’un des fichiers PDF dans le dossier `PrivateDocs` en production. Cela invite votre navigateur à envoyer une requête IIS pour le fichier. Comme un fichier statique est demandé, IIS récupère et retourne le fichier sans appeler le runtime ASP.NET. Par conséquent, aucun contrôle d’autorisation d’URL n’a été effectué. le contenu du fichier PDF privé est accessible à toute personne connaissant l’URL directe du fichier.

[![utilisateurs anonymes peuvent télécharger les fichiers PDF privés en entrant l’URL directe vers le fichier](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**Figure 4**: les utilisateurs anonymes peuvent télécharger les fichiers PDF privés en entrant l’URL directe vers le fichier ([cliquez pour afficher l’image en taille réelle](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))

### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Exécution de l’authentification basée sur les formulaires et authentification des URL sur des fichiers statiques avec IIS 7

Il existe quelques techniques que vous pouvez utiliser pour protéger le contenu statique contre les utilisateurs non autorisés. IIS 7 a introduit le *pipeline intégré*qui alliant le flux de travail d’IIS avec le flux de travail du runtime ASP.net. En résumé, vous pouvez demander à IIS d’appeler les modules d’authentification et d’autorisation du runtime ASP.NET toutes les demandes entrantes (y compris le contenu statique comme les fichiers PDF). Contactez votre fournisseur d’hébergement Web pour savoir comment configurer votre site Web pour utiliser le pipeline intégré.

Une fois qu’IIS a été configuré pour utiliser le pipeline intégré, ajoutez le balisage suivant au fichier `Web.config` dans le répertoire racine :

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

Ce balisage indique à IIS 7 d’utiliser les modules d’authentification et d’autorisation basés sur ASP.NET. Redéployez votre application, puis réexaminez le fichier PDF. Cette fois-ci, lorsque IIS traite la demande, il donne à la logique d’authentification et d’autorisation de l’exécution de ASP.NET la possibilité d’inspecter la requête. Étant donné que seuls les utilisateurs authentifiés sont autorisés à afficher le contenu du dossier `PrivateDocs`, le visiteur anonyme est automatiquement redirigé vers la page de connexion (reportez-vous à la figure 3).

> [!NOTE]
> Si votre fournisseur d’hébergement Web utilise toujours IIS 6, vous ne pouvez pas utiliser la fonctionnalité de pipeline intégré. Une solution de contournement consiste à placer vos documents privés dans un dossier qui interdit l’accès HTTP (par exemple, `App_Data`), puis à créer une page pour traiter ces documents. Cette page peut être appelée `GetPDF.aspx`et reçoit le nom du fichier PDF via un paramètre QueryString. La page `GetPDF.aspx` vérifie d’abord que l’utilisateur est autorisé à afficher le fichier et, le cas échéant, utilise la méthode [`Response.WriteFile(filePath)`](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) pour renvoyer le contenu du fichier PDF demandé au client demandeur. Cette technique fonctionne également pour IIS 7 Si vous ne souhaitez pas activer le pipeline intégré.

## <a name="summary"></a>Récapitulatif

Les applications Web dans un environnement de production sont hébergées à l’aide du logiciel de serveur Web IIS de Microsoft. Dans l’environnement de développement, toutefois, l’application peut être hébergée à l’aide d’IIS ou de l’Serveur de développement ASP.NET. Dans l’idéal, le même logiciel de serveur Web doit être utilisé dans les deux environnements, car l’utilisation de différents logiciels ajoute une autre variable dans la combinaison. Toutefois, la facilité d’utilisation du Serveur de développement ASP.NET en fait un choix attrayant dans l’environnement de développement. La bonne nouvelle, c’est qu’il n’y a que quelques différences fondamentales entre IIS et le Serveur de développement ASP.NET, et si vous êtes conscient de ces différences, vous pouvez prendre des mesures pour vous assurer que l’application fonctionne et fonctionne de la même manière, quelle que soit la environnement.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Intégration de ASP.NET avec IIS 7,0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Utilisation de l’authentification des Forums ASP.net avec tous les types de contenu sur IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (vidéo)
- [Serveurs Web dans Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Précédent](common-configuration-differences-between-development-and-production-vb.md)
> [Suivant](deploying-a-database-vb.md)
