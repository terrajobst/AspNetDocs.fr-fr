---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
title: Journalisation des détails des erreursC#avec ELMAH () | Microsoft Docs
author: rick-anderson
description: Les modules de journalisation des erreurs et les gestionnaires (ELMAH) offrent une autre approche pour enregistrer les erreurs d’exécution dans un environnement de production. ELMAH est une erreur Open source gratuite...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 11f6fe44-64ef-4a38-a3b4-35c7bb992352
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
msc.type: authoredcontent
ms.openlocfilehash: 5018023eced23e7a70eab90e649f85862c548940
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74570481"
---
# <a name="logging-error-details-with-elmah-c"></a>Journalisation des détails des erreurs avec ELMAH (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_cs.pdf)

> Les modules de journalisation des erreurs et les gestionnaires (ELMAH) offrent une autre approche pour enregistrer les erreurs d’exécution dans un environnement de production. ELMAH est une bibliothèque de journalisation des erreurs Open source gratuite qui comprend des fonctionnalités telles que le filtrage des erreurs et la possibilité d’afficher le journal des erreurs à partir d’une page Web, sous forme de flux RSS ou de le télécharger en tant que fichier délimité par des virgules. Ce didacticiel vous guide dans le téléchargement et la configuration de ELMAH.

## <a name="introduction"></a>Introduction

Le [didacticiel précédent](logging-error-details-with-asp-net-health-monitoring-cs.md) a examiné ASP. Système de contrôle d’intégrité de NET, qui offre une bibliothèque prête à l’emploi pour l’enregistrement d’un vaste éventail d’événements Web. De nombreux développeurs utilisent la surveillance de l’intégrité pour consigner et envoyer par courrier électronique les détails des exceptions non gérées. Toutefois, il existe quelques points difficiles avec ce système. Tout d’abord, le manque d’une interface utilisateur pour afficher des informations sur les événements journalisés. Si vous souhaitez afficher un résumé des 10 dernières erreurs ou afficher les détails d’une erreur qui s’est produite la semaine dernière, vous devez soit lire la base de données, parcourir la boîte de réception de votre messagerie électronique, soit créer une page Web qui affiche des informations à partir de la table `aspnet_WebEvent_Events`.

Une autre difficulté est axée sur la complexité de la surveillance de l’intégrité. Étant donné que la surveillance de l’intégrité peut être utilisée pour enregistrer une multitude d’événements différents et comme il existe de nombreuses options pour indiquer comment et quand les événements sont journalisés, la configuration correcte du système de contrôle d’intégrité peut être une tâche onéreuse. Enfin, il existe des problèmes de compatibilité. Étant donné que la surveillance de l’intégrité a été ajoutée pour la première fois au .NET Framework dans la version 2,0, elle n’est pas disponible pour les applications Web plus anciennes générées à l’aide de ASP.NET version 1. x. En outre, la classe `SqlWebEventProvider`, que nous avons utilisée dans le didacticiel précédent pour enregistrer les détails de l’erreur dans une base de données, fonctionne uniquement avec les bases de données Microsoft SQL Server. Vous devrez créer une classe de module fournisseur d’informations personnalisée si vous devez consigner les erreurs dans un autre magasin de données, tel qu’un fichier XML ou une base de données Oracle.

Une alternative au système de contrôle d’intégrité est la journalisation des erreurs modules et gestionnaires (ELMAH), un système de journalisation des erreurs Open source gratuit créé par [Atif Aziz](http://www.raboof.com/). La différence la plus notable entre les deux systèmes est que ELAMH permet d’afficher une liste d’erreurs et les détails d’une erreur spécifique à partir d’une page Web et en tant que flux RSS. ELMAH est plus facile à configurer que le monitoring d’intégrité, car il ne consigne que les erreurs. En outre, ELMAH inclut la prise en charge des applications ASP.NET 1. x, ASP.NET 2,0 et ASP.NET 3,5, et est livré avec un large éventail de fournisseurs de sources de journaux.

Ce didacticiel vous guide tout au long des étapes d’ajout de ELMAH à une application ASP.NET. Commençons !

> [!NOTE]
> Le système de contrôle d’intégrité et les ELMAH ont leurs propres jeux de professionnels et inconvénients. Je vous encourage à essayer les deux systèmes et à choisir celui qui convient le mieux à vos besoins.

## <a name="adding-elmah-to-an-aspnet-web-application"></a>Ajout de ELMAH à une application Web ASP.NET

L’intégration de ELMAH dans une application ASP.NET nouvelle ou existante est un processus simple et simple qui prend moins de cinq minutes. En résumé, il implique quatre étapes simples :

1. Téléchargez ELMAH et ajoutez l’assembly `Elmah.dll` à votre application Web,
2. Inscrire les modules et le gestionnaire HTTP de ELMAH dans `Web.config`,
3. Spécifiez les options de configuration de ELMAH et
4. Créez l’infrastructure source du journal des erreurs, si nécessaire.

Passons en revue chacune de ces quatre étapes, une à la fois.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Étape 1 : Téléchargement des fichiers de projet ELMAH et ajout de`Elmah.dll`à votre application Web

ELMAH 1,0 bêta 3 (Build 10617), la version la plus récente au moment de la rédaction, est inclus dans le téléchargement disponible dans ce didacticiel. Vous pouvez également visiter le [site Web ELMAH](https://code.google.com/p/elmah/) pour accéder à la version la plus récente ou pour télécharger le code source. Extrayez le téléchargement ELMAH dans un dossier sur votre bureau et recherchez le fichier d’assembly ELMAH (`Elmah.dll`).

> [!NOTE]
> Le fichier `Elmah.dll` se trouve dans le dossier de `Bin` du téléchargement, qui contient des sous-dossiers pour différentes versions de .NET Framework et pour les versions release et Debug. Utilisez la version Release pour la version de Framework appropriée. Par exemple, si vous générez une application Web ASP.NET 3,5, copiez le fichier `Elmah.dll` à partir du dossier `Bin\net-3.5\Release`.

Ensuite, ouvrez Visual Studio et ajoutez l’assembly à votre projet en cliquant avec le bouton droit sur le nom du site Web dans le Explorateur de solutions et en choisissant Ajouter une référence dans le menu contextuel. La boîte de dialogue Ajouter une référence s’affiche. Accédez à l’onglet Parcourir et choisissez le fichier `Elmah.dll`. Cette action ajoute le fichier `Elmah.dll` au dossier `Bin` de l’application Web.

> [!NOTE]
> Le type de projet d’application Web (WAP) n’affiche pas le dossier `Bin` dans le Explorateur de solutions. Au lieu de cela, il répertorie ces éléments dans le dossier références.

L’assembly `Elmah.dll` comprend les classes utilisées par le système ELMAH. Ces classes sont classées dans l’une des trois catégories suivantes :

- **Modules http** : un module http est une classe qui définit des gestionnaires d’événements pour les événements de `HttpApplication`, tels que l’événement `Error`. ELMAH comprend plusieurs modules HTTP, les trois plus allemands étant : 

    - `ErrorLogModule`-journalise les exceptions non gérées dans une source de journal.
    - `ErrorMailModule`-envoie les détails d’une exception non gérée dans un message électronique.
    - `ErrorFilterModule` : applique les filtres spécifiés par le développeur pour déterminer les exceptions qui sont journalisées et celles qui sont ignorées.
- **Gestionnaires http** : un gestionnaire HTTP est une classe qui est chargée de générer le balisage pour un type particulier de demande. ELMAH comprend des gestionnaires HTTP qui affichent les détails de l’erreur sous la forme d’une page Web, sous forme de flux RSS ou sous la forme d’un fichier délimité par des virgules (CSV).
- Les **sources du journal des erreurs** ELMAH peuvent consigner les erreurs dans la mémoire, dans une base de données Microsoft SQL Server, dans une base de données Microsoft Access, dans une base de données Oracle, dans un fichier XML, dans une base de données SQLite ou dans une base de données de base de données Vista. À l’instar du système de contrôle d’intégrité, l’architecture de ELMAH a été créée à l’aide du modèle de fournisseur, ce qui signifie que vous pouvez créer et intégrer en toute transparence vos propres fournisseurs de sources de journaux personnalisés, si nécessaire.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Étape 2 : inscription du module et du gestionnaire HTTP de ELMAH

Tandis que le fichier `Elmah.dll` contient les modules et le gestionnaire HTTP nécessaires pour enregistrer automatiquement les exceptions non gérées et afficher les détails de l’erreur à partir d’une page Web, ceux-ci doivent être inscrits explicitement dans la configuration de l’application Web. Une fois inscrit, le module HTTP `ErrorLogModule` s’abonne à l’événement `Error` de l' `HttpApplication`. Chaque fois que cet événement est déclenché, le `ErrorLogModule` enregistre les détails de l’exception dans une source de journal spécifiée. Nous allons voir comment définir le fournisseur de source de journal dans la section suivante, « configuration de ELMAH ». La fabrique de gestionnaires HTTP `ErrorLogPageFactory` est chargée de générer le balisage lors de l’affichage du journal des erreurs à partir d’une page Web.

La syntaxe spécifique pour l’enregistrement des modules et gestionnaires HTTP dépend du serveur Web qui alimente le site. Pour les Serveur de développement ASP.NET et IIS version 6,0 et antérieures, les modules et gestionnaires HTTP sont enregistrés dans les sections `<httpModules>` et `<httpHandlers>`, qui apparaissent dans l’élément `<system.web>`. Si vous utilisez IIS 7,0, ceux-ci doivent être inscrits dans les sections `<modules>` et `<handlers>` de l’élément `<system.webServer>`. Heureusement, vous pouvez définir les modules et gestionnaires HTTP aux *deux* emplacements, quel que soit le serveur Web utilisé. Cette option est la plus portable, car elle permet d’utiliser la même configuration dans les environnements de développement et de production, quel que soit le serveur Web utilisé.

Commencez par inscrire le module HTTP `ErrorLogModule` et le gestionnaire HTTP `ErrorLogPageFactory` dans la section `<httpModules>` et `<httpHandlers>` de `<system.web>`. Si votre configuration définit déjà ces deux éléments, il suffit d’inclure l’élément `<add>` pour le module et le gestionnaire HTTP de ELMAH.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample1.xml)]

Ensuite, inscrivez le module et le gestionnaire HTTP de ELMAH dans l’élément `<system.webServer>`. Comme précédemment, si cet élément n’est pas déjà présent dans votre configuration, ajoutez-le.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample2.xml)]

Par défaut, IIS 7 se plaint si les modules et gestionnaires HTTP sont inscrits dans la section `<system.web>`. L’attribut `validateIntegratedModeConfiguration` dans l’élément `<validation>` indique à IIS 7 de supprimer ces messages d’erreur.

Notez que la syntaxe d’inscription du gestionnaire HTTP `ErrorLogPageFactory` comprend un attribut `path`, qui est défini sur `elmah.axd`. Cet attribut indique à l’application Web que si une requête arrive pour une page nommée `elmah.axd`, la requête doit être traitée par le gestionnaire HTTP `ErrorLogPageFactory`. Nous verrons le gestionnaire HTTP `ErrorLogPageFactory` en action plus loin dans ce didacticiel.

### <a name="step-3-configuring-elmah"></a>Étape 3 : configuration de ELMAH

ELMAH recherche ses options de configuration dans le fichier `Web.config` du site Web dans une section de configuration personnalisée nommée `<elmah>`. Pour pouvoir utiliser une section personnalisée dans `Web.config` elle doit d’abord être définie dans l’élément `<configSections>`. Ouvrez le fichier `Web.config` et ajoutez le balisage suivant au `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample3.xml)]

> [!NOTE]
> Si vous configurez ELMAH pour une application ASP.NET 1. x, supprimez l’attribut `requirePermission="false"` des éléments `<section>` ci-dessus.

La syntaxe ci-dessus inscrit la section de `<elmah>` personnalisée et ses sous-sections : `<security>`, `<errorLog>`, `<errorMail>`et `<errorFilter>`.

Ajoutez ensuite la section `<elmah>` à `Web.config`. Cette section doit apparaître au même niveau que l’élément `<system.web>`. Dans la section `<elmah>`, ajoutez les sections `<security>` et `<errorLog>` comme suit :

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample4.xml)]

L’attribut `allowRemoteAccess` de la section `<security>` indique si l’accès à distance est autorisé. Si cette valeur est définie sur 0, la page Web du journal des erreurs ne peut être affichée que localement. Si cet attribut est défini sur 1, la page Web du journal des erreurs est activée pour les visiteurs locaux et distants. Pour le moment, nous allons désactiver la page Web du journal des erreurs pour les visiteurs distants. Nous allons autoriser l’accès à distance ultérieurement une fois que nous aurons la possibilité de discuter des problèmes de sécurité.

La section `<errorLog>` définit la source du journal des erreurs, qui détermine où sont enregistrés les détails de l’erreur ; elle est similaire à la section `<providers>` du système de contrôle d’intégrité. La syntaxe ci-dessus spécifie la classe `SqlErrorLog` comme source du journal des erreurs, qui consigne les erreurs dans une Microsoft SQL Server base de données spécifiée par la valeur de l’attribut `connectionStringName`.

> [!NOTE]
> ELMAH est fourni avec des fournisseurs de journaux d’erreurs supplémentaires qui peuvent être utilisés pour consigner les erreurs dans un fichier XML, une base de données Microsoft Access, une base de données Oracle et d’autres magasins de données. Reportez-vous à l’exemple de fichier de `Web.config` inclus avec le téléchargement ELMAH pour plus d’informations sur l’utilisation de ces fournisseurs de journaux d’erreurs.

### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Étape 4 : création de l’infrastructure source du journal des erreurs

Le fournisseur de `SqlErrorLog` de ELMAH enregistre les détails de l’erreur dans une base de données Microsoft SQL Server spécifiée. Le fournisseur `SqlErrorLog` s’attend à ce que cette base de données ait une table nommée `ELMAH_Error` et trois procédures stockées : `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`et `ELMAH_LogError`. Le téléchargement ELMAH inclut un fichier nommé `SQLServer.sql` dans le dossier `db` qui contient T-SQL pour la création de cette table et de ces procédures stockées. Vous devrez exécuter ces instructions sur votre base de données pour utiliser le fournisseur de `SqlErrorLog`.

Les **figures 1** et **2** illustrent les Explorateur de base de données dans Visual Studio une fois que les objets de base de données requis par le fournisseur de `SqlErrorLog` ont été ajoutés.

[![](logging-error-details-with-elmah-cs/_static/image2.png)](logging-error-details-with-elmah-cs/_static/image1.png)

**Figure 1**: le fournisseur de `SqlErrorLog` journalise les erreurs dans la table `ELMAH_Error`

[![](logging-error-details-with-elmah-cs/_static/image4.png)](logging-error-details-with-elmah-cs/_static/image3.png)

**Figure 2**: le fournisseur de `SqlErrorLog` utilise trois procédures stockées

## <a name="elmah-in-action"></a>ELMAH en action

À ce stade, nous avons ajouté ELMAH à l’application Web, inscrit le module HTTP `ErrorLogModule` et le gestionnaire HTTP `ErrorLogPageFactory`, spécifié les options de configuration de ELMAH dans `Web.config`, et ajouté les objets de base de données nécessaires pour le fournisseur de journaux d’erreurs `SqlErrorLog`. Nous sommes maintenant prêts à voir ELMAH en action ! Visitez le site Web de révisions de livres et visitez une page qui génère une erreur d’exécution, telle que `Genre.aspx?ID=foo`ou une page inexistante, telle que `NoSuchPage.aspx`. Ce que vous voyez lors de la consultation de ces pages dépend de la configuration de `<customErrors>` et de la consultation locale ou à distance. (Reportez-vous au [didacticiel *affichage d’une page d’erreurs personnalisée* ](displaying-a-custom-error-page-cs.md) pour obtenir un actualisateur sur cette rubrique.)

ELMAH n’affecte pas le contenu présenté à l’utilisateur lorsqu’une exception non gérée se produit ; Il enregistre simplement les détails. Ce journal des erreurs est accessible à partir de la page Web `elmah.axd` à partir de la racine de votre site Web, par exemple `http://localhost/BookReviews/elmah.axd`. (Ce fichier n’existe pas physiquement dans votre projet, mais lorsqu’une demande est envoyée pour `elmah.axd` le runtime le distribue au gestionnaire HTTP `ErrorLogPageFactory`, ce qui génère le balisage renvoyé au navigateur.)

> [!NOTE]
> Vous pouvez également utiliser la page `elmah.axd` pour indiquer à ELMAH de générer une erreur de test. Si vous vous rendez `elmah.axd/test` (comme dans, `http://localhost/BookReviews/elmah.axd/test`), ELMAH lève une exception de type `Elmah.TestException`, qui contient le message d’erreur : « il s’agit d’une exception de test qui peut être ignorée en toute sécurité. »

La **figure 3** montre le journal des erreurs lorsque vous vous rendez `elmah.axd` à partir de l’environnement de développement.

[![](logging-error-details-with-elmah-cs/_static/image6.png)](logging-error-details-with-elmah-cs/_static/image5.png)

**Figure 3**: `Elmah.axd` affiche le journal des erreurs d’une page Web  
([Cliquez pour afficher l’image en taille réelle](logging-error-details-with-elmah-cs/_static/image7.png))

Le journal des erreurs de la **figure 3** contient six entrées d’erreur. Chaque entrée comprend le code d’état HTTP (404 ou 500, pour ces erreurs), le type, la description, le nom de l’utilisateur connecté lorsque l’erreur s’est produite, ainsi que la date et l’heure. Le fait de cliquer sur le lien Détails affiche une page qui comprend le même message d’erreur affiché dans les détails de l’erreur écran jaune de la mort (voir **figure 4**), ainsi que les valeurs des variables de serveur au moment de l’erreur (voir **figure 5**). Vous pouvez également afficher le code XML brut dans lequel les détails de l’erreur sont enregistrés, ce qui comprend des informations supplémentaires telles que les valeurs dans l’en-tête HTTP.

[![](logging-error-details-with-elmah-cs/_static/image9.png)](logging-error-details-with-elmah-cs/_static/image8.png)

**Figure 4**: afficher les détails de l’erreur YSOD  
([Cliquez pour afficher l’image en taille réelle](logging-error-details-with-elmah-cs/_static/image10.png))

[![](logging-error-details-with-elmah-cs/_static/image12.png)](logging-error-details-with-elmah-cs/_static/image11.png)

**Figure 5**: explorer les valeurs de la collection de variables serveur au moment de l’erreur  
([Cliquez pour afficher l’image en taille réelle](logging-error-details-with-elmah-cs/_static/image13.png))

Le déploiement de ELMAH sur le site Web de production implique :

- Copie du fichier `Elmah.dll` dans le dossier `Bin` en production,
- Copie des paramètres de configuration spécifiques à ELMAH dans le fichier `Web.config` utilisé en production, et
- Ajout de l’infrastructure source du journal des erreurs à la base de données de production.

Nous avons exploré les techniques permettant de copier les fichiers du développement vers la production dans les didacticiels précédents. La méthode la plus simple pour récupérer l’infrastructure source du journal des erreurs sur la base de données de production consiste peut-être à utiliser SQL Server Management Studio pour vous connecter à la base de données de production, puis à exécuter le fichier de script `SqlServer.sql`, qui créera la table et les procédures stockées nécessaires.

### <a name="viewing-the-error-details-page-on-production"></a>Affichage de la page Détails de l’erreur en production

Après avoir déployé votre site en production, visitez le site Web de production et générez une exception non gérée. Comme dans l’environnement de développement, ELMAH n’a aucun effet sur la page d’erreur affichée lorsqu’une exception non gérée se produit ; au lieu de cela, elle consigne simplement l’erreur. Si vous tentez d’accéder à la page du journal des erreurs (`elmah.axd`) à partir de l’environnement de production, vous serez accueilli avec la page interdite présentée à la **figure 6**.

[![](logging-error-details-with-elmah-cs/_static/image15.png)](logging-error-details-with-elmah-cs/_static/image14.png)

**Figure 6**: par défaut, les visiteurs distants ne peuvent pas afficher la page Web du journal des erreurs  
([Cliquez pour afficher l’image en taille réelle](logging-error-details-with-elmah-cs/_static/image16.png))

N’oubliez pas que dans la section `<security>` de la configuration de ELMAH, nous définissons l’attribut `allowRemoteAccess` sur 0, ce qui empêche les utilisateurs distants de consulter le journal des erreurs. Il est important d’empêcher les visiteurs anonymes de consulter le journal des erreurs, car les détails de l’erreur peuvent révéler des failles de sécurité ou d’autres informations sensibles. Si vous décidez d’affecter la valeur 1 à cet attribut et d’activer l’accès à distance au journal des erreurs, il est important de verrouiller le chemin d’accès `elmah.axd` afin que seuls les visiteurs autorisés puissent y accéder. Pour ce faire, vous pouvez ajouter un élément `<location>` au fichier `Web.config`.

La configuration suivante autorise uniquement les utilisateurs du rôle d’administrateur à accéder à la page Web du journal des erreurs :

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample5.xml)]

> [!NOTE]
> Le rôle d’administrateur et les trois utilisateurs dans le système, Scott, Jisun et Alice, ont été ajoutés dans le [didacticiel *configuration d’un site Web qui utilise services d’application* ](configuring-a-website-that-uses-application-services-cs.md). Les utilisateurs Scott et Jisun sont membres du rôle d’administrateur. Pour plus d’informations sur l’authentification et l’autorisation, consultez les didacticiels sur la sécurité de mon [site Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Le journal des erreurs de l’environnement de production peut désormais être consulté par des utilisateurs distants. reportez-vous aux **figures 3**, **4**et **5** pour les captures d’écran de la page Web du journal des erreurs. Toutefois, si un utilisateur anonyme ou non-administrateur tente d’afficher la page du journal des erreurs, il est automatiquement redirigé vers la page de connexion (`Login.aspx`), comme le montre la **figure 7** .

[![](logging-error-details-with-elmah-cs/_static/image18.png)](logging-error-details-with-elmah-cs/_static/image17.png)

**Figure 7**: les utilisateurs non autorisés sont automatiquement redirigés vers la page de connexion  
([Cliquez pour afficher l’image en taille réelle](logging-error-details-with-elmah-cs/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Journalisation des erreurs par programmation

Le module HTTP `ErrorLogModule` de ELMAH journalise automatiquement les exceptions non gérées dans la source de journal spécifiée. Vous pouvez également consigner une erreur sans avoir à déclencher une exception non gérée à l’aide de la classe `ErrorSignal` et de sa méthode `Raise`. La méthode `Raise` reçoit un objet `Exception` et le journalise comme si cette exception avait été levée et avait atteint le runtime ASP.NET sans être géré. Toutefois, la différence réside dans le fait que la requête continue de s’exécuter normalement après l’appel de la méthode `Raise`, alors qu’une exception levée, non gérée interrompt l’exécution normale de la demande et oblige le runtime ASP.NET à afficher la page d’erreurs configurée.

La classe `ErrorSignal` est utile dans les situations où une action peut échouer, mais son échec n’est pas catastrophique pour l’opération globale effectuée. Par exemple, un site Web peut contenir un formulaire qui prend l’entrée de l’utilisateur, le stocke dans une base de données, puis envoie à l’utilisateur un e-mail les informant qu’il a été traité. Que se passe-t-il si les informations sont correctement enregistrées dans la base de données, mais qu’une erreur se produit lors de l’envoi du message électronique ? Une option consisterait à lever une exception et à envoyer l’utilisateur à la page d’erreur. Toutefois, cela peut dérouter l’utilisateur pour penser que les informations qu’il a entrées n’ont pas été enregistrées. Une autre approche consisterait à consigner l’erreur liée à la messagerie, mais sans modifier l’expérience de l’utilisateur de quelque manière que ce soit. C’est là que la classe `ErrorSignal` est utile.

[!code-csharp[Main](logging-error-details-with-elmah-cs/samples/sample6.cs)]

## <a name="error-notification-via-email"></a>Notification d’erreur par E-mail

En plus de la journalisation des erreurs dans une base de données, ELMAH peut également être configuré pour envoyer par courrier électronique les détails des erreurs à un destinataire spécifié. Cette fonctionnalité est fournie par le module `ErrorMailModule` HTTP. par conséquent, vous devez inscrire ce module HTTP dans `Web.config` pour pouvoir envoyer les détails de l’erreur par courrier électronique.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample7.xml)]

Ensuite, spécifiez les informations relatives à l’e-mail d’erreur dans la section `<errorMail>` de l’élément `<elmah>`, indiquant l’expéditeur et le destinataire de l’e-mail, le sujet et si l’e-mail est envoyé de manière asynchrone.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample8.xml)]

Avec les paramètres ci-dessus en place, chaque fois qu’une erreur d’exécution se produit, ELMAH envoie un e-mail à support@example.com avec les détails de l’erreur. L’e-mail d’erreur de ELMAH comprend les mêmes informations que celles affichées dans la page Web détails de l’erreur, à savoir le message d’erreur, la trace de la pile et les variables de serveur (reportez-vous aux **figures 4** et **5**). L’e-mail d’erreur comprend également les détails de l’exception écran jaune de contenu de décès en tant que pièce jointe (`YSOD.html`).

La **figure 8** illustre l’e-mail d’erreur de ELMAH généré en visitant `Genre.aspx?ID=foo`. Bien que la **figure 8** affiche uniquement le message d’erreur et la trace de la pile, les variables de serveur sont incluses plus loin dans le corps de l’e-mail.

[![](logging-error-details-with-elmah-cs/_static/image21.png)](logging-error-details-with-elmah-cs/_static/image20.png)

**Figure 8**: vous pouvez configurer ELMAH pour envoyer les détails de l’erreur par courrier électronique  
([Cliquez pour afficher l’image en taille réelle](logging-error-details-with-elmah-cs/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Journalisation des erreurs d’intérêt uniquement

Par défaut, ELMAH journalise les détails de chaque exception non gérée, y compris 404 et d’autres erreurs HTTP. Vous pouvez ordonner à ELMAH d’ignorer ces types d’erreurs ou d’autres types d’erreurs à l’aide du filtrage des erreurs. La logique de filtrage est exécutée par le module `ErrorFilterModule` HTTP de ELMAH, que vous devrez inscrire dans `Web.config` pour pouvoir utiliser la logique de filtrage. Les règles de filtrage sont spécifiées dans la section `<errorFilter>`.

Le balisage suivant indique à ELMAH de ne pas enregistrer les erreurs 404.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample9.xml)]

> [!NOTE]
> N’oubliez pas que pour pouvoir utiliser le filtrage des erreurs, vous devez inscrire le module HTTP `ErrorFilterModule`.

L’élément `<equal>` à l’intérieur de la section `<test>` est désigné sous le terme d’assertion. Si l’assertion prend la valeur true, l’erreur est filtrée à partir du journal de ELMAH. D’autres assertions sont disponibles, notamment : `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`, etc. Vous pouvez également combiner des assertions à l’aide des opérateurs booléens `<and>` et `<or>`. De plus, vous pouvez même inclure une expression JavaScript simple en tant qu’assertion, ou écrire vos propres assertions C# dans ou Visual Basic.

Pour plus d’informations sur les fonctionnalités de filtrage d’erreurs de ELMAH, reportez-vous à la [section filtrage des erreurs](https://code.google.com/p/elmah/wiki/ErrorFiltering) dans le [wiki ELMAH](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Récapitulatif

ELMAH fournit un mécanisme simple, mais puissant pour la journalisation des erreurs dans une application Web ASP.NET. À l’instar du système de contrôle d’intégrité de Microsoft, ELMAH peut consigner les erreurs dans une base de données et envoyer les détails de l’erreur à un développeur par courrier électronique. Contrairement au système de contrôle d’intégrité, ELMAH inclut une prise en charge prête à l’emploi pour un éventail plus large de banques de données de journaux d’erreurs, notamment : Microsoft SQL Server, Microsoft Access, Oracle, les fichiers XML et plusieurs autres. En outre, ELMAH offre un mécanisme intégré permettant d’afficher le journal des erreurs et des informations sur une erreur spécifique à partir d’une page Web, `elmah.axd`. La page `elmah.axd` peut également afficher des informations sur les erreurs sous forme de flux RSS ou de fichier de valeurs séparées par des virgules (CSV), que vous pouvez lire à l’aide de Microsoft Excel. Vous pouvez également demander à ELMAH de filtrer les erreurs du journal à l’aide d’assertions déclaratives ou par programmation. Et ELMAH peuvent être utilisés avec les applications ASP.NET version 1. x.

Chaque application déployée doit avoir un mécanisme de journalisation automatique des exceptions non gérées et d’envoi de notifications à l’équipe de développement. L’utilisation de la surveillance de l’intégrité ou de ELMAH est secondaire. En d’autres termes, cela n’a pas vraiment d’importance, que vous utilisiez la surveillance de l’intégrité ou ELMAH. Évaluez les deux systèmes, puis choisissez celui qui répond le mieux à vos besoins. Ce qui est essentiel, c’est qu’un mécanisme est mis en place pour consigner les exceptions non gérées dans l’environnement de production.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [ELMAH-erreurs de modules et gestionnaires de journalisation des erreurs](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Page projet ELMAH](https://code.google.com/p/elmah/) (code source, exemples, wiki)
- [Branchement de ELMAH dans une application Web pour intercepter les exceptions non gérées](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (vidéo)
- [Pages du journal des erreurs de sécurité](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Utilisation de modules et de gestionnaires HTTP pour créer des composants ASP.NET enfichables](https://msdn.microsoft.com/library/aa479332.aspx)
- [Didacticiels sur la sécurité de site Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Précédent](logging-error-details-with-asp-net-health-monitoring-cs.md)
> [Suivant](precompiling-your-website-cs.md)
