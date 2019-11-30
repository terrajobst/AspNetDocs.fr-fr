---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Créer le projet | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 62918b17f42e54dfe4e45a08927b1039dcbb7012
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74576064"
---
# <a name="create-the-project"></a>Créer le projet

par [Erik Reitan](https://github.com/Erikre)

[Télécharger l’exemple de projet WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Télécharger le livre électronique (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour le Web. Un [projet Visual Studio 2013 avec C# le code source](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.

Dans ce didacticiel, vous allez créer, examiner et exécuter le projet par défaut dans Visual Studio, ce qui vous permettra de vous familiariser avec les fonctionnalités de ASP.NET. En outre, vous allez examiner l’environnement Visual Studio.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment créer un projet de Web Forms.
- Structure de fichiers du projet Web Forms.
- Comment exécuter le projet dans Visual Studio.
- Les différentes fonctionnalités de l’application Web Forms par défaut.
- Quelques notions de base sur l’utilisation de l’environnement Visual Studio.

## <a name="creating-the-project"></a>Création du projet

1. Ouvrez Visual Studio.
2. Sélectionnez **nouveau projet** dans le menu **fichier** de Visual Studio. 

    ![Créer l’élément de menu projet-nouveau projet](create-the-project/_static/image1.png)
3. Sélectionnez les **modèles** -&gt; le groupe **Visual C#**  -&gt; de modèles **Web** sur la gauche.
4. Choisissez le modèle **application Web ASP.net** dans la colonne centrale.  
 Cette série de didacticiels utilise .NET Framework 4.5.2.
5. Nommez votre projet *WingtipToys* et choisissez le bouton **OK** . 

    ![Créer la boîte de dialogue projet-nouveau projet](create-the-project/_static/image2.png)

    > [!NOTE]
    > Le nom du projet dans cette série de didacticiels est **WingtipToys**. Il est recommandé d’utiliser ce nom de projet *exact* afin que le code fourni dans la série de didacticiels fonctionne comme prévu.

6. Cliquez sur le bouton **modifier l’authentification** . Sélectionnez **des comptes d’utilisateur individuels** , puis cliquez sur le bouton **OK** .

7. Sélectionnez le modèle **Web Forms** , puis cliquez sur le bouton **OK** .

    ![Créer le modèle projet-nouveau projet](create-the-project/_static/image3.png)

La création du projet prend un peu de temps. Quand elle est prête, ouvrez la page **default. aspx** .

![Créer le modèle projet-nouveau projet](create-the-project/_static/image4.png)

Vous pouvez basculer entre le mode **création** et le mode **source** en sélectionnant une option en bas de la fenêtre centrale. Le mode **Design** affiche les pages Web ASP.net, les pages maîtres, les pages de contenu, les pages HTML et les contrôles utilisateur à l’aide d’un affichage proche de WYSIWYG. Le mode **source** affiche le balisage HTML de votre page Web, que vous pouvez modifier.

> [!TIP] 
> 
> **Fonctionnement des frameworks ASP.NET**
> 
> ASP.NET Web Forms permet de créer des sites web dynamiques par glisser-déposer, selon un modèle répondant en fonction des événements. Une aire de conception et des centaines de contrôles et de composants vous permettent de créer rapidement des sites sophistiqués et puissants, gérés par interface utilisateur avec accès aux données. Le magasin Wingtip Toys est basé sur ASP.NET Web Forms, mais un grand nombre des concepts que vous avez appris dans cette série de didacticiels s’appliquent à tous les ASP.NET.
> 
> ASP.NET propose quatre infrastructures de développement principales :
> 
> - [ASP.NET Web Forms](../../../index.md)  
>  Le Web Forms Framework cible les développeurs qui préfèrent la programmation déclarative et basée sur le contrôle, comme Microsoft Windows Forms (WinForms) et WPF/XAML/Silverlight. Il offre un modèle de développement WYSIWYG piloté par le concepteur. il est donc très populaire avec les développeurs qui cherchent un environnement de développement rapide d’applications (RAD) pour le développement Web. Si vous n’êtes pas familiarisé avec la programmation Web et que vous connaissez les outils de développement du client RAD Microsoft traditionnels (par exemple C#, pour Visual Basic et Visual), vous pouvez rapidement créer une application Web sans avoir à vous familiariser avec HTML et JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC cible les développeurs qui s’intéressent à des modèles et à des principes tels que le développement piloté par test, la séparation des préoccupations, l’inversion de contrôle (IoC) et l’injection de dépendances (DI). Cette infrastructure encourage la séparation de la couche de logique métier d’une application Web de sa couche de présentation.
> - [Pages Web ASP.NET](../../../../web-pages/index.md)  
>  Pages Web ASP.NET cible les développeurs qui souhaitent un simple développement Web, sur les lignes de PHP. Dans le modèle Web pages, vous créez des pages HTML, puis vous ajoutez du code basé sur le serveur à la page afin de contrôler de manière dynamique le mode de rendu de ce balisage. Web pages est spécifiquement conçue pour être une infrastructure légère, et il s’agit du point d’entrée le plus simple dans ASP.NET pour les personnes qui connaissent le HTML, mais qui n’ont peut-être pas une expérience de programmation étendue (par exemple, les élèves ou les amateurs). C’est également un bon moyen pour les développeurs Web qui connaissent PHP ou des infrastructures similaires de commencer à utiliser ASP.NET.
> - [Application à page unique ASP.NET](../../../../single-page-application/index.md)  
>  L’application à page unique ASP.NET (SPA) vous aide à créer des applications qui incluent des interactions majeures côté client à l’aide de HTML 5, CSS 3 et JavaScript. La mise à jour ASP.NET et Web Tools 2012,2 fournit un nouveau modèle pour créer des applications à page unique à l’aide de Knockout. js et API Web ASP.NET. Outre le nouveau modèle SPA, les nouveaux modèles SPA créés par la Communauté sont également disponibles en téléchargement.
> 
> En plus des quatre principales infrastructures de développement, ASP.NET offre également des technologies supplémentaires qui sont importantes à connaître et à connaître, mais qui ne sont pas abordées dans cette série de didacticiels :
> 
> - [API Web ASP.net](../../../../web-api/index.md) -une infrastructure pour la création de services http qui atteignent un large éventail de clients, y compris des navigateurs et des appareils mobiles.
> - [ASP.net signalr](../../../../signalr/index.md) -une bibliothèque qui facilite le développement de fonctionnalités Web en temps réel.

### <a name="reviewing-the-project"></a>Examen du projet

Dans Visual Studio, la fenêtre de **Explorateur de solutions** vous permet de gérer des fichiers pour le projet. Jetons un coup d’œil aux dossiers qui ont été ajoutés à votre application dans **Explorateur de solutions**. Le modèle d’application Web ajoute une structure de dossier de base :

![Créer le projet-Explorateur de solutions](create-the-project/_static/image5.png)

Visual Studio crée des dossiers et des fichiers initiaux pour votre projet. Les premiers fichiers que vous utiliserez plus loin dans ce didacticiel sont les suivants :

| **Fichier** | **Fonction** |
| --- | --- |
| *Default. aspx* | En général, première page affichée lorsque l’application est exécutée dans un navigateur. |
| *Site. Master* | Page qui vous permet de créer une disposition cohérente et d’utiliser le comportement standard pour les pages de votre application. |
| *Global. asax* | Fichier facultatif qui contient du code pour répondre aux événements au niveau de l’application et au niveau de la session déclenchés par ASP.NET ou par les modules HTTP. |
| *Web. config* | Données de configuration pour une application. |

### <a name="running-the-default-web-application"></a>Exécution de l’application Web par défaut

L’application Web par défaut offre une expérience enrichie basée sur les fonctionnalités intégrées et la prise en charge. Sans aucune modification du projet Web Forms par défaut, l’application est prête à s’exécuter sur votre navigateur Web local.

1. Appuyez sur la touche ***F5*** dans Visual Studio.   
 L’application est générée et s’affiche dans votre navigateur Web.  

    ![Créer le projet-page par défaut](create-the-project/_static/image6.png)
2. Une fois que vous avez terminé la vérification de l’application en cours d’exécution, fermez la fenêtre du navigateur.

Il existe trois pages principales dans cette application Web par défaut : *default. aspx* (accueil), *about. aspx*et *contact. aspx*. Chacune de ces pages est accessible à partir de la barre de navigation supérieure. Il y a également deux pages supplémentaires contenues dans le dossier Account, la page Register. aspx et la page Login. aspx. Ces deux pages vous permettent d’utiliser les fonctionnalités d’appartenance de ASP.NET pour créer, stocker et valider les informations d’identification de l’utilisateur.

## <a name="aspnet-web-forms-background"></a>ASP.NET Web Forms arrière-plan

ASP.NET Web Forms sont des pages basées sur Microsoft ASP.NET technologie, dans lesquelles le code qui s’exécute sur le serveur génère dynamiquement une sortie de page Web sur le navigateur ou le périphérique client. Une page de Web Forms ASP.NET affiche automatiquement le code HTML conforme au navigateur pour les fonctionnalités telles que les styles, la disposition, etc. Les Web Forms sont compatibles avec les langages pris en charge par le common language runtime .NET, tels que Microsoft C#Visual Basic et Microsoft Visual. En outre, les Web Forms s’appuient sur l' [infrastructure de Microsoft .net](https://msdn.microsoft.com/vstudio/aa496123), qui offre des avantages tels qu’un environnement géré, la sécurité de type et l’héritage.

Quand une page de Web Forms ASP.NET s’exécute, la page passe par un cycle de vie dans lequel elle effectue une série d’étapes de traitement. Ces étapes incluent l’initialisation, l’instanciation des contrôles, la restauration et la gestion de l’État, l’exécution du code du gestionnaire d’événements et le rendu. À mesure que vous vous familiariserez avec la puissance de ASP.NET Web Forms, il est important que vous compreniez le cycle de vie de la [page ASP.net](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) pour pouvoir écrire du code à l’étape de cycle de vie appropriée pour l’effet que vous envisagez.

Lorsqu’un serveur Web reçoit une demande pour une page, il trouve la page, le traite, l’envoie au navigateur, puis ignore toutes les informations de la page. Si l’utilisateur demande de nouveau la même page, le serveur répète l’intégralité de la séquence et retraite la page à partir de zéro. En d’autres termes, un serveur n’a pas de mémoire pour les pages traitées. les pages sont sans État. L’infrastructure de page ASP.NET gère automatiquement la tâche de maintenance de l’état de votre page et de ses contrôles, et vous offre des moyens explicites de conserver l’état des informations spécifiques à l’application.

> [!TIP] 
> 
> **Fonctionnalités de l’application Web dans le modèle d’application Web Forms**
> 
> Le modèle d’application ASP.NET Web Forms fournit un ensemble complet de fonctionnalités intégrées. Elle vous fournit non seulement une page main *. aspx* , une page *about. aspx* , une page *contact. aspx* , mais inclut également la fonctionnalité d’appartenance qui inscrit les utilisateurs et enregistre leurs informations d’identification afin qu’ils puissent se connecter à votre site Web. Cette vue d’ensemble fournit plus d’informations sur certaines des fonctionnalités contenues dans le modèle d’application ASP.NET Web Forms et sur la façon dont elles sont utilisées dans l’application Wingtip Toys.
> 
> **Appartenance**
> 
> [ASP.net](https://msdn.microsoft.com/library/yh26yfzy.aspx) Identity stocke les informations d’identification de vos utilisateurs dans une base de données créée par l’application. Lorsque vos utilisateurs se connectent, l’application valide leurs informations d’identification en lisant la base de données. Le dossier de *compte* de votre projet contient les fichiers qui implémentent les différentes parties de l’appartenance : l’inscription, la connexion, la modification d’un mot de passe et l’autorisation d’accès. En outre, ASP.NET Web Forms prend en charge OAuth et OpenID. Ces améliorations d’authentification permettent aux utilisateurs de se connecter à votre site à l’aide des informations d’identification existantes, à partir de comptes Facebook, Twitter, Windows Live et Google.
> 
> ![Créer le projet-Explorateur de solutions (ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> Par défaut, le modèle crée une base de données d’appartenance à l’aide d’un nom de base de données par défaut sur une instance de SQL Server Express de base de données locale, le serveur de base de données de développement fourni avec Visual Studio Express 2013 pour le Web.
> 
> **SQL Server Express de base de données locale**
> 
> [SQL Server Express](https://technet.microsoft.com/library/hh510202.aspx) la base de données locale est une version allégée de SQL Server qui possède de nombreuses fonctionnalités de programmabilité d’une base de données SQL Server. SQL Server Express la base de données locale s’exécute en mode utilisateur et dispose d’une installation rapide et sans configuration qui dispose d’une liste succincte des conditions préalables à l’installation. Dans Microsoft SQL Server, toute base de données ou tout code Transact-SQL peut être déplacé de SQL Server Express base de données locale vers SQL Server et SQL Azure sans aucune étape de mise à niveau. Ainsi, SQL Server Express base de données locale peut être utilisée en tant qu’environnement de développement pour les applications ciblant toutes les éditions de SQL Server. SQL Server Express base de données locale active des fonctionnalités telles que les procédures stockées, les fonctions définies par l’utilisateur et les agrégats, l’intégration .NET Framework, les types spatiaux et d’autres qui ne sont pas disponibles dans SQL Server Compact.
> 
> **Pages maîtres**
> 
> Une [page maître ASP.net](https://msdn.microsoft.com/library/wtxbf3hh.aspx) définit une apparence et un comportement cohérents pour toutes les pages de votre application. La disposition de la page maître fusionne avec le contenu d’une page de contenu individuelle pour produire la dernière page visible par l’utilisateur. Dans l’application Wingtip Toys, vous modifiez la page maître *site. Master* afin que toutes les pages du site Web Wingtip Toys partagent le même logo distinctif et la même barre de navigation.
> 
> **HTML5**
> 
> Le modèle d’application ASP.NET Web Forms prend en charge [HTML5](http://www.w3schools.com/html/html5_intro.asp), qui est la dernière version du langage de balisage HTML. HTML5 prend en charge de nouveaux éléments et fonctionnalités qui facilitent la création de sites Web.
> 
> **Modernizr**
> 
> Pour les navigateurs qui ne prennent pas en charge HTML5, vous pouvez utiliser [Modernizr](http://www.modernizr.com/). Modernizr est une bibliothèque JavaScript open source qui peut détecter si un navigateur prend en charge les fonctionnalités HTML5 et les active si ce n’est pas le cas. Dans le modèle d’application ASP.NET Web Forms, Modernizr est installé en tant que package NuGet.
> 
> **Bootstrap**
> 
> Les modèles de projet Visual Studio 2013 utilisent [bootstrap](http://getbootstrap.com/), une disposition et une infrastructure de thèmes créés par Twitter. Bootstrap utilise CSS3 pour fournir une conception réactive, ce qui signifie que les dispositions peuvent s’adapter de manière dynamique à différentes tailles de fenêtre de navigateur. Vous pouvez également utiliser la fonctionnalité de thèmes d’amorçage pour effectuer facilement une modification de l’apparence de l’application. Par défaut, le modèle d’application Web ASP.NET dans Visual Studio 2013 comprend le démarrage en tant que package NuGet.
> 
> **Packages NuGet**
> 
> Le modèle d’application ASP.NET Web Forms comprend un ensemble de packages [NuGet](http://www.nuget.org/) . Ces packages fournissent des fonctionnalités de composants sous la forme de bibliothèques et d’outils open source. Il existe une grande variété de packages pour vous aider à créer et tester vos applications. Visual Studio facilite l’ajout, la suppression et la mise à jour des packages NuGet. Les développeurs peuvent également créer et ajouter des packages à NuGet.
> 
> ![Créer la boîte de dialogue Project-NuGet](create-the-project/_static/image8.png)
> 
> Lorsque vous installez un package, NuGet copie les fichiers dans votre solution et effectue automatiquement toutes les modifications nécessaires, telles que l’ajout de références et la modification de la configuration associée à votre application Web. Si vous décidez de supprimer la bibliothèque, NuGet supprime les fichiers et annule les modifications apportées à votre projet afin qu’il ne reste aucun encombrement. NuGet est disponible dans le menu **Outils** de Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) est une bibliothèque JavaScript rapide et concise qui simplifie le parcours de documents html, la gestion des événements, l’animation et les interactions AJAX pour le développement Web rapide. La bibliothèque Java jQuery est incluse dans le modèle d’application ASP.NET Web Forms sous la forme d’un package NuGet.
> 
> **Validation discrète**
> 
> Les contrôles validateurs intégrés ont été configurés pour utiliser un JavaScript discret pour la logique de validation côté client. Cela réduit considérablement la quantité de JavaScript rendue inline dans le balisage de la page et réduit la taille globale de la page. Une validation discrète est ajoutée globalement au modèle d’application ASP.NET Web Forms en fonction du paramètre de l’élément&gt; &lt;appSettings du fichier *Web. config* à la racine de l’application.
> 
> **Entity Framework Code First**
> 
> Outre les fonctionnalités du modèle d’application ASP.NET Web Forms, l’application Wingtip Toys utilise [Entity Framework code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), qui est une bibliothèque NuGet qui permet le développement centré sur le code lorsque vous travaillez avec des données. En d’autres termes, il crée la partie base de données de votre application pour vous en fonction du code que vous écrivez. À l’aide de la Entity Framework, vous récupérez et manipulez des données en tant qu’objets fortement typés. Cela vous permet de vous concentrer sur la logique métier dans votre application plutôt que sur les détails de l’accès aux données.
> 
> Pour plus d’informations sur les bibliothèques et les packages installés inclus dans le modèle ASP.NET Web Forms, consultez la liste des packages NuGet installés. Pour ce faire, dans Visual Studio, créez un nouveau projet de Web Forms, sélectionnez **outils** > **Gestionnaire de package NuGet** > **gérer les packages NuGet pour la solution**, puis sélectionnez **packages installés** dans la boîte de dialogue **gérer les packages NuGet** .

### <a name="touring-visual-studio"></a>Cyclotourisme dans Visual Studio

Les fenêtres principales de Visual Studio incluent l' **Explorateur de solutions**, la **Explorateur de serveurs** (**Explorateur de base de données** dans Express), la **fenêtre Propriétés**, la **boîte à**outils, la **barre d’outils**et la **fenêtre de document**.

![Créer la boîte de dialogue Project-NuGet](create-the-project/_static/image9.png)

Pour plus d’informations sur Visual Studio, consultez [Guide visuel pour Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez créé, révisé et exécuté l’application par défaut Web Forms. Vous avez examiné les différentes fonctionnalités de l’application Web Forms par défaut et appris quelques notions de base sur l’utilisation de l’environnement Visual Studio. Dans les didacticiels suivants, vous allez créer la couche d’accès aux données.

## <a name="additional-resources"></a>Ressources supplémentaires

[Choix du modèle de programmation approprié](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Projets d’application Web et projets de site web](https://msdn.microsoft.com/library/dd547590.aspx)   
[Vue d’ensemble des pages de Web Forms ASP.NET](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Précédent](introduction-and-overview.md)
> [Suivant](create_the_data_access_layer.md)
