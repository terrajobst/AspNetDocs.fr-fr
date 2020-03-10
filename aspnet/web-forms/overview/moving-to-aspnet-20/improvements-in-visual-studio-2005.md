---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Améliorations apportées à Visual Studio 2005 | Microsoft Docs
author: microsoft
description: Visual Studio 2005 fournit aux développeurs d’applications Web une longue liste d’améliorations et d’améliorations apportées aux projets Web.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 64215d556ded0850537a13856fe69b094116ebca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575578"
---
# <a name="improvements-in-visual-studio-2005"></a>Améliorations de Visual Studio 2005

par [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 fournit aux développeurs d’applications Web une longue liste d’améliorations et d’améliorations apportées aux projets Web.

Visual Studio 2005 fournit aux développeurs d’applications Web une longue liste d’améliorations et d’améliorations apportées aux projets Web. Aussi puissant que Visual Studio .NET 2002 et 2003, il y avait de nombreuses plaintes dans la façon dont les projets Web étaient gérés. Visual Studio 2005 ajoute un nombre significatif de nouvelles fonctionnalités pour traiter ces plaintes. Pour ceux qui préfèrent la manière dont Visual Studio .NET 2003 a géré la compilation des applications Web, consultez [projets d’application Web](https://go.microsoft.com/fwlink/?LinkId=57870).

Dans ce module, nous allons améliorer la création, la gestion et le développement d’un projet Web. Dans un module ultérieur, nous aborderons les améliorations apportées à la création de projets Web et à leur déploiement.

## <a name="frontpage-server-extensions"></a>FrontPage Server Extensions

Visual Studio .NET 2002 et 2003 requis FrontPage Server Extensions sur la boîte afin de créer ou de générer des projets Web. Les développeurs ont choisi de choisir entre deux modes d’accès (FrontPage Server Extensions ou mode d’accès aux fichiers), tous deux utilisés FrontPage Server Extensions pour effectuer des tâches telles que la définition de la racine de l’application dans IIS, etc.

Visual Studio 2005 supprime la dépendance vis-à-vis des FrontPage Server Extensions pour les projets locaux. Visual Studio 2005 accède désormais directement à la métabase IIS plutôt qu’à l’aide de l’FrontPage Server Extensions. Visual Studio 2005 ajoute également la prise en charge de FTP qui permet l’accès aux projets distants sans nécessiter de FrontPage Server Extensions.

Pour les développeurs qui souhaitent utiliser FrontPage Server Extensions dans leurs projets, l’option est toujours disponible. Toutefois, en raison de commentaires forts de la communauté de développeurs ASP.NET, ce n’est pas obligatoire.

> [!NOTE]
> FrontPage Server Extensions sont toujours nécessaires pour la création, l’ouverture, etc. des projets distants.

## <a name="aspnet-development-server"></a>serveur de développement ASP.NET

Visual Studio 2005 est fourni avec un nouveau serveur Web appelé Serveur de développement ASP.NET. (Ce serveur Web était auparavant appelé Cassini.)

Le Serveur de développement ASP.NET présente plusieurs avantages.

- Il est désormais possible pour les non-administrateurs de développer et déboguer sur un serveur Web.
- La Serveur de développement ASP.NET mappe dynamiquement les répertoires virtuels à n’importe quel emplacement dans le système de fichiers, ce qui permet d’obtenir des emplacements de projet flexibles.
- Les utilisateurs de Windows XP professionnel qui utilisent déjà IIS peuvent désormais créer des applications Web qui n’affecteront pas la structure de fichiers ou de dossiers de leur site Web par défaut dans IIS.

Aucune configuration spéciale n’est requise pour tirer parti de la Serveur de développement ASP.NET. Quand un projet Web hébergé sur le système de fichiers est débogué ou parcouru, Visual Studio 2005 démarre automatiquement une instance du Serveur de développement ASP.NET sur un port aléatoire pour traiter la demande.

Pour plus d’informations, reportez-vous à la Serveur de développement ASP.NET plus loin dans ce module.

## <a name="improved-file-management"></a>Amélioration de la gestion des fichiers

Dans Visual Studio 2002 et 2003, un fichier projet (. vbproj pour VB.NET et. csproj pour C#) a stocké des informations sur tous les fichiers de l’application Web. L’affichage de Explorateur de solutions est basé sur les informations de fichier dans le fichier projet. Pour cette raison, le Explorateur de solutions afficherait souvent des informations inexactes dans les cas où des éditeurs externes étaient utilisés. Visual Studio 2002 et 2003 remplacent souvent les modifications apportées aux fichiers ou n’affichent pas la version la plus récente des fichiers.

Visual Studio 2005 s’éloigne avec le fichier projet. Au lieu de cela, il lit les informations de fichier et de dossier directement à partir du disque, ce qui donne un affichage précis des fichiers dans votre projet. Étant donné que le dossier références dans Visual Studio 2002 et 2003 ne représente pas un dossier réel dans votre application Web, Visual Studio 2005 supprime également le dossier références de Explorateur de solutions. Pour accéder aux références de votre projet dans Visual Studio 2005, vous devez utiliser les pages de propriétés du projet.

## <a name="creating-web-projects"></a>Création de projets Web

Les développeurs Web ont de nombreuses nouvelles options disponibles pour la création de projets dans Visual Studio 2005. Les sites Web peuvent désormais être créés n’importe où dans le système de fichiers et peuvent ensuite être débogués ou parcourus à l’aide de la nouvelle Serveur de développement ASP.NET. Les développeurs peuvent également créer des sites Web à l’aide de FTP.

Cliquez ici pour afficher une vidéo pas à pas sur la création de projets Web dans Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image1.png)

[Ouvrir la vidéo en plein écran](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)

### <a name="file-system-projects"></a>Projets de système de fichiers

Comme vous l’avez vu dans la procédure pas à pas, vous pouvez choisir de créer des sites Web sur le système de fichiers, soit sur l’ordinateur local, soit sur un emplacement distant via un partage de fichiers. Les sites Web créés sur le système de fichiers sont explorés et débogués à l’aide de la Serveur de développement ASP.NET.

> [!NOTE]
> La Serveur de développement ASP.NET peut entraîner une certaine confusion pour les clients. Si un projet Web est créé sur le système de fichiers dans la structure de répertoire IISs (par exemple, c:/inetpub/wwwroot), le site Web est toujours parcouru via le Serveur de développement ASP.NET lorsqu’il est lancé à partir de Visual Studio 2005. Par conséquent, toute configuration IIS (par exemple, les méthodes d’authentification) n’est pas applicable.

Le projet Web par défaut supprime également une grande partie de la surcharge en n’incluant qu’une page default. aspx, un fichier default.cs et un dossier App/_Data. Le fichier Web. config et les dossiers spéciaux (par exemple application/_code) sont ajoutés lorsqu’ils sont nécessaires. Votre projet Web comprend uniquement les fichiers et dossiers dont vous avez besoin.

### <a name="http-projects"></a>Projets HTTP

Les projets HTTP peuvent être des projets créés sur un site Web IIS local ou sur un site Web distant. L’emplacement du projet par défaut est `http://localhost`. Si vous cliquez sur le bouton Parcourir, il existe deux options HTTP : IIS local et site distant. La principale différence entre ces deux options est la méthode dans laquelle les informations de site Web s’affichent dans la boîte de dialogue Choisir un emplacement et dans la manière dont les fichiers sont copiés sur le serveur Web.

L’option IIS local lit les informations de site à partir de la métabase sur l’ordinateur local et les fichiers sont copiés à l’aide du système de fichiers. L’option site distant utilise la FrontPage Server Extensions et les informations du site et les fichiers sont copiés à l’aide de HTTP et FrontPage Server Extensions appels RPC.

> [!NOTE]
> Le fichier vs # # #/_tmp. htm et obtenir/_aspx/_ver. aspx ne sont plus utilisés pour déterminer les informations de version.

L’option HTTP par défaut est IIS local. Cette option lit la métabase IIS pour déterminer les sites disponibles et l’emplacement dans lequel créer le contenu. Vous pouvez sélectionner un dossier ou un répertoire virtuel différent en le sélectionnant dans l’arborescence. Vous pouvez également créer un nouveau répertoire virtuel, marquer les dossiers en tant qu’applications et supprimer les répertoires virtuels existants de cette boîte de dialogue.

![Boîte de dialogue Choisir un emplacement](improvements-in-visual-studio-2005/_static/image1.gif)

**Figure 1**: boîte de dialogue Choisir un emplacement

Contrairement aux versions antérieures de Visual Studio, si vous activez la case à cocher **utiliser protocole SSL** et que le certificat SSL ne correspond pas à l’URL que vous explorez, une boîte de dialogue d’alerte de sécurité s’affiche pour vous demander si vous souhaitez continuer. À l’aide de Visual Studio .NET 2003, si le certificat n’était pas une correspondance, la création du projet échouerait.

![Alerte de sécurité concernant le certificat SSL](improvements-in-visual-studio-2005/_static/image2.gif)

**Figure 2**: alerte de sécurité concernant le certificat SSL

### <a name="note-on-host-headers"></a>Remarque sur les en-têtes d’hôte

Si vous créez une application Web sur un site lié à une adresse IP spécifique, vous devez vous assurer qu’un en-tête d’hôte est configuré. Dans le cas contraire, Visual Studio crée le site à `http://localhost`, mais l’adresse IP n’est pas résolue correctement lorsque le site est parcouru ou débogué à partir de l’IDE.

Si vous sélectionnez l’option site distant, la boîte de dialogue se transforme et vous permet d’entrer l’URL de destination du nouveau site Web. Cette URL doit être sur un serveur sur lequel l’FrontPage Server Extensions activée. Si vous souhaitez utiliser votre serveur Web local à l’aide de l’FrontPage Server Extensions, vous pouvez utiliser l’option site distant et spécifier une URL locale.

![Création d’un site Web sur un serveur distant](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figure 3**: création d’un site Web sur un serveur distant

Lorsque vous créez une application sur un site distant via SSL, si le certificat SSL ne correspond pas, la boîte de dialogue de confirmation est légèrement différente de la boîte de dialogue affichée lors de l’utilisation de l’option IIS locale.

![Alerte de sécurité du site distant](improvements-in-visual-studio-2005/_static/image3.gif)

**Figure 4**: alerte de sécurité du site distant

<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 introduit la possibilité de créer des sites Web via FTP. Lorsque vous utilisez cette option, l’IDE crée les fichiers localement dans le dossier Temp des utilisateurs, puis utilise FTP pour déplacer les fichiers vers l’emplacement FTP.

> [!NOTE]
> L’emplacement du dossier temporaire est c:/documents and Settings/&lt;user&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;nom de l’application&gt;

Lorsque vous utilisez l’option FTP, un dialogue Choisir un emplacement s’affiche. Vous entrez les informations de connexion FTP requises dans cette boîte de dialogue, comme indiqué ci-dessous.

![Boîte de dialogue Choisir un emplacement pour FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figure 5**: boîte de dialogue Choisir un emplacement pour FTP

## <a name="lab-setup-ftp-site-and-create-a-project"></a>Lab : configurer le site FTP et créer un projet

Les étapes suivantes configurent le site FTP afin qu’un utilisateur dispose d’un emplacement sur lequel il peut télécharger via FTP.

### <a name="install-the-ftp-service"></a>Installer le service FTP

1. Ouvrez ajout/suppression de programmes, puis sélectionnez Ajouter/supprimer des composants Windows.
2. Sélectionnez Internet Information Services (serveur d’applications sur Windows 2003), puis cliquez sur **Détails**.
3. Vérifiez le **Service protocole FTP (FTP)** , puis cliquez sur **OK**.
4. Cliquez sur **suivant** pour installer le service FTP.

### <a name="create-a-new-folder-for-content"></a>Créer un nouveau dossier pour le contenu

1. Dans l’Explorateur Windows, créez un dossier appelé **User1** à l’intérieur de c:/inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configurez les dossiers et les autorisations sur les dossiers.

1. Ouvrez le composant logiciel enfichable Internet Information Services à partir des outils d’administration. Vous avez maintenant un dossier sites FTP sous le nœud nom de l’ordinateur.
2. Développez **sites FTP**.
3. Cliquez avec le bouton droit sur le **site FTP par défaut**, sélectionnez **nouveau**, puis **répertoire virtuel**, puis cliquez sur **suivant**.
4. Entrez **User1** comme nom de répertoire virtuel, puis cliquez sur **suivant**.
5. Entrez **c:/inetpub/wwwroot/utilisateur1** pour le chemin d’accès et cliquez sur **suivant**.
6. Cliquez sur **suivant** , puis sur **Terminer** pour terminer l’Assistant.
7. Cliquez avec le bouton droit sur le répertoire virtuel **User1** sous site FTP par défaut, puis sélectionnez **Propriétés**.
8. Cochez la case **écrire** , puis cliquez sur **OK** pour fermer la boîte de dialogue.
9. Cliquez avec le bouton droit sur **site FTP par défaut** , puis sélectionnez **Propriétés**.
10. Sous l’onglet **comptes de sécurité** , désactivez l’option autoriser les **connexions anonymes**.
11. Cliquez sur **Oui** dans la boîte de dialogue qui vous demande si vous souhaitez continuer.
12. Cliquez sur **OK** pour fermer la boîte de dialogue.
13. Développez le **site Web par défaut** sous le nœud **sites Web** .
14. Cliquez avec le bouton droit sur le répertoire **User1** et sélectionnez **Propriétés** .
15. Dans la section paramètres de l' **application** , cliquez sur **créer** pour marquer le dossier en tant qu’application.
16. Cliquez sur **OK** pour fermer la boîte de dialogue.
17. Fermez le composant logiciel enfichable Internet Information Services.

### <a name="create-web-project"></a>Créer un projet Web

1. Ouvrez Visual Studio 2005.
2. Dans le menu **fichier** , sélectionnez **nouveau site Web**.
3. Dans la liste déroulante **emplacement** , sélectionnez **FTP**.
4. Cliquez sur **Parcourir**.
5. Entrez **localhost** dans la zone de texte **serveur** .
6. Entrez **User1** dans la zone de texte répertoire.
7. Cliquez sur **Ouvrir**. L’emplacement FTP est entré dans la boîte de dialogue Nouveau site Web.
8. Cliquez sur **OK**.
9. Désactivez la case à cocher **connexion anonyme** dans la boîte de dialogue connexion FTP, entrez vos informations d’identification, puis cliquez sur **OK**.
10. Quelle est l’URL du projet ? (L’URL du projet s’affiche dans Explorateur de solutions.)
11. Dans le menu **générer** , sélectionnez **générer le site Web** ou **générer la solution**.
12. Dans Explorateur de solutions, cliquez avec le bouton droit sur default. aspx, puis sélectionnez **afficher dans le navigateur**.
13. Dans la boîte de dialogue URL du site Web requise, entrez `http://localhost/user1` pour l’URL, puis cliquez sur **OK**.

> [!NOTE]
> Si vous recevez une erreur indiquant qu’il est impossible de charger le type/_Default, assurez-vous que vous exécutez ASP.NET 2,0 sur votre site Web et non une version antérieure. Vous pouvez le faire à partir de l’onglet ASP.NET dans Internet Information Services.

## <a name="opening-web-projects"></a>Ouverture de projets Web

L’ouverture de projets Web est similaire à la création de projets. Les sections suivantes démontrent les zones pour garder un œil sur tout en travaillant dans l’IDE. Il traite également de l’utilisation des projets Web à l’aide de HTTP et FTP.

Pour ouvrir un projet Web, sélectionnez Ouvrir le site Web dans le menu fichier. Vous serez invité à sélectionner la même boîte de dialogue Choisir un emplacement que celle abordée précédemment. les quatre options disponibles sont les suivantes : système de fichiers, IIS local, FTP et site distant.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Système de fichiers

Comme indiqué précédemment dans ce module, Visual Studio n’utilise plus un fichier projet. Par conséquent, si vous choisissez d’ouvrir un site Web à partir du système de fichiers, vous avez en fait la possibilité de choisir n’importe quel dossier, même si le dossier que vous choisissez n’a pas été créé initialement en tant que projet Web dans Visual Studio. Par exemple, vous pouvez choisir d’ouvrir le dossier Mes documents en tant que site Web et Visual Studio l’ouvrira et affichera vos fichiers comme indiqué ci-dessous.

![Mes documents ouverts en tant que site Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figure 6**: *Mes documents* ouverts en tant que site Web

Étant donné que Visual Studio crée uniquement des fichiers et dossiers supplémentaires si nécessaire, aucun fichier ou dossier supplémentaire n’est ajouté à l’emplacement que vous ouvrez. Un effet secondaire de cette architecture est qu’elle vous empêche d’imbriquer des sites Web sur le système de fichiers. Par exemple, considérez la structure de répertoires suivante.

Projet Web sur C:/monsiteweb

Un autre projet Web sur C:/monsiteweb/imbriqué

Lorsque vous ouvrez le site Web à l’adresse c:/monsiteweb, le dossier imbriqué s’affiche sous la forme d’un sous-dossier de cette application.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Lors de l’ouverture de sites Web via HTTP, les paramètres sont lus à partir de la métabase IIS (IIS local) ou à l’aide de FrontPage Server Extensions (site distant). S’il existe des applications Web imbriquées, celles-ci sont également affichées avec une icône qui les identifie comme une application. Si vous êtes familiarisé avec l’utilisation des applications Web dans FrontPage, le comportement de Visual Studio 2005 est similaire.

Même si Visual Studio affiche une icône pour les applications imbriquées sous l’application qui est actuellement ouverte dans l’IDE, il ne vous permet pas de les développer pour afficher leur contenu. Toutefois, vous pouvez double-cliquer dessus pour les ouvrir. Dans ce cas, une boîte de dialogue s’affiche pour vous inviter à ouvrir l’application Web (et à remplacer la solution actuellement ouverte) ou à ajouter l’application Web à votre solution actuelle.

![Si vous double-cliquez sur une icône d’application imbriquée, cette boîte de dialogue s’affiche](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figure 7**: Si vous double-cliquez sur une icône d’application imbriquée, vous voyez cette boîte de dialogue

<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Site FTP

Lorsque vous ouvrez un site via FTP, tous les fichiers sont copiés localement dans votre dossier Temp. Le chemin d’accès complet de l’emplacement de stockage local est affiché dans le volet Propriétés du projet et est créé au format suivant.

C:/documents and Settings/&lt;utilisateur&gt;/local paramètres/Temp/VWDWebCache/&lt;Server&gt;/_&lt;nom de l’application&gt;

Lorsque vous utilisez FTP, Visual Studio doit spécifier l’URL de base de votre projet afin que vous puissiez le parcourir comme indiqué ci-dessous. Si vous ne spécifiez pas d’URL de base, Visual Studio vous le demande la première fois que vous tentez de parcourir une page du site Web.

![Spécification d’une URL de base pour les sites FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figure 8**: spécification d’une URL de base pour les sites FTP

## <a name="improvements-in-compilation"></a>Améliorations de la compilation

L’utilisation des applications Web dans Visual Studio 2005 est sensiblement plus rapide que les versions précédentes. Cela est dû à une petite partie des modifications apportées à l’architecture de compilation.

Dans Visual Studio 2002 et 2003, les applications Web ont été compilées en un assembly principal résidant dans le dossier/bin. Dans Visual Studio 2005, un dossier App/_Code a été ajouté. Les classes et le code non-interface utilisateur sont ajoutés au dossier App/_Code. Lorsque Visual Studio génère le projet, tous les fichiers du dossier App/_Code sont compilés en un seul fichier app/_Code. dll. Le résultat de cette modification est que les builds suivantes sont beaucoup plus rapides que dans les versions précédentes.

> [!NOTE]
> L’utilitaire de ligne de commande MSBuild peut également être utilisé pour générer des applications Web ASP.NET. Cet outil sera couvert dans le module 9.

Une autre amélioration de la compilation est l’option nouvelle page de génération du menu Générer. Cette fonctionnalité permet à un développeur de reconstruire uniquement la page actuelle (avec, bien évidemment, les dépendances) afin que les modifications puissent être compilées plus rapidement. Étant C# donné que n’offre pas de compilation en arrière-plan à des fins de mise à jour d’IntelliSense, etc., elles bénéficient énormément de cette fonctionnalité, car elle permet la mise à jour rapide d’IntelliSense en reconstruisant simplement une seule page.

Les propriétés de génération d’un projet vous permettent de configurer le type de build qui se produit avant l’exécution de la page de démarrage. Les développeurs peuvent choisir de générer uniquement la page actuelle afin que Visual Studio puisse commencer à déboguer les applications plus rapidement après les modifications du code.

![Action de démarrage de la page de génération](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figure 9**: action de démarrage de la page de génération

Une autre excellente amélioration de Visual Studio et de l’architecture ASP.NET est dans le domaine de la fonctionnalité Modifier & continuer. Dans Visual Studio 2005, les développeurs peuvent commencer à déboguer un projet et apporter des modifications au code du projet sans détacher le débogueur. En fait, vous pouvez littéralement commencer à déboguer un projet, ajouter une nouvelle classe, ajouter du code à cette classe, ajouter du code à votre page qui crée une instance de cette classe et exécuter une méthode de la classe, tout cela sans détacher le débogueur. L’exécution du nouveau code est littéralement aussi simple que l’actualisation du navigateur !

Cliquez ici pour afficher une vidéo pas à pas de la fonctionnalité Modifier & continuer dans Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image2.png)

[Ouvrir la vidéo en plein écran](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)

La fonctionnalité de modification et de continuation robuste dans ASP.NET 2,0 et Visual Studio 2005 est due à une modification architecturale des applications ASP.NET. Dans ASP.NET 1. x, les applications créées dans Visual Studio 2002/2003 ont été compilées dans un assembly principal qui était stocké dans le dossier/bin. Toutes les classes, pages, etc. de l’application ont été compilées dans cette DLL. Ensuite, au moment de l’exécution, ASP.NET compile tous les contrôles, le balisage et le code ASP.NET dans les pages et copie ces dll dans le dossier temporaire ASP.NET.

Dans Visual Studio 2005 à l’aide de ASP.NET 2,0, les deux modèles de compilation ci-dessus (un pour Visual Studio et un pour ASP.NET au moment de l’exécution) ont été fusionnés dans un modèle de compilation commun. Cela signifie que tous les problèmes de compilation sont maintenant détectés au cours de l’étape de développement et non au moment de l’exécution. Il permet également la prise en charge par le concepteur et IntelliSense des fonctionnalités telles que les contrôles utilisateur et les pages maîtres.

Cliquez ici pour afficher une vidéo pas à pas de la prise en charge du concepteur pour les contrôles utilisateur.

![](improvements-in-visual-studio-2005/_static/image3.png)

[Ouvrir la vidéo en plein écran](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)

> [!NOTE]
> Quand un contrôle utilisateur est supprimé d’une page, la directive @Register reste dans le balisage et doit être supprimée manuellement afin d’éviter les erreurs d’analyse si le contrôle utilisateur est supprimé du site Web.

Une autre amélioration apportée au modèle de compilation Visual Studio est la fonctionnalité publier le site Web. Étant donné que la fonctionnalité de publication précompile le site Web, les développeurs peuvent profiter de la performance supplémentaire de n’avoir à compiler rien à la demande. Il précompile également tout le code source dans le dossier App/_Code dans une DLL afin qu’aucun code source ne soit déployé.

![Boîte de dialogue publier le site Web](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figure 10**: boîte de dialogue publier le site Web

> [!NOTE]
> L’utilitaire ASPNET/_compile. exe peut également être utilisé pour précompiler une application Web ASP.NET. Cet outil sera couvert dans le module 9.

Lorsque vous publiez un site Web, les fichiers précompilés sont stockés dans le dossier Temporary ASP.NET Files, comme indiqué ci-dessous. Les fichiers avec une extension *. Compiled* sont des fichiers XML qui définissent des dépendances pour des dll particulières. Tous les contrôles WebForm ou utilisateur sont compilés en dll aléatoires qui commencent par *app/_Web/_* .

Si vous laissez la case à cocher *autoriser ce site précompilé à être mis à jour* activée, le balisage dans vos formulaires Web et contrôles utilisateur n’est pas précompilé dans une dll, ce qui vous permet d’apporter des modifications après le déploiement. Si vous préférez verrouiller le balisage afin que les modifications apportées au contenu déployé ne soient pas autorisées, décochez cette case.

La case à cocher *utiliser la dénomination fixe et les assemblys d’une seule page* vous permet de désactiver la compilation par lots afin que chaque page soit compilée dans un assembly avec nom fixe. La désactivation de cette case à cocher vous permet de tirer parti de la compilation par lots.

La case à cocher *activer l’affectation de noms forts sur les assemblys précompilés* vous permet d’attribuer un nom fort à vos assemblys précompilés.

> [!NOTE]
> Dans ASP.NET 1. x, les assemblys avec nom fort devaient être installés dans le global assembly cache (GAC). Dans ASP.NET 2,0, vous n’êtes pas obligé d’installer des assemblys avec nom fort dans le GAC.

![Fichiers précompilés d’applications ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figure 11**: fichiers ASP.NET applications précompilées

> [!NOTE]
> Dans l’application ci-dessus, il n’existait aucun fichier Web. config. Si tel était le cas, il aurait été appelé *PrecompiledApp. config* après le processus de publication du site Web.

## <a name="improvements-in-deployment"></a>Améliorations du déploiement

Comme avec Visual Studio 2002 et 2003, Visual Studio 2005 offre une fonctionnalité de copie de projet. Toutefois, la fonctionnalité a été mise en concurrence dans Visual Studio 2005 et est maintenant appelée copier le site Web.

La boîte de dialogue Copier le site Web est scindée en un frame gauche et un cadre droit. Le cadre de gauche est appelé le site Web source et le cadre droit est appelé site Web distant. Une chose qui peut tromper certains développeurs est que le site affiché dans le cadre de droite n’est pas nécessairement un site distant. Il peut s’agir d’un site sur le système de fichiers local ou sur l’instance locale d’IIS. En outre, le site affiché dans le cadre de gauche n’est pas nécessairement le site Web source, car la boîte de dialogue vous permet de publier à partir du site Web distant *vers* le site Web source.

Si vous copiez un projet sur un site Web distant, le FrontPage Server Extensions doit être installé sur ce site. Si ce n’est pas le cas, vous devez vous connecter à l’aide du protocole FTP. En revanche, si vous copiez un projet vers l’instance IIS locale, FrontPage Server Extensions ne sont pas nécessaires.

> [!NOTE]
> Si vous essayez de créer un nouveau site Web sur l’instance IIS locale et que les extensions serveur FrontPage 2002 sont installées, vous obtiendrez un message d’erreur indiquant que la création de sites Web n’est pas prise en charge sur un serveur SharePoint. Dans ce cas, vous avez la possibilité d’installer les extensions serveur FrontPage 2000 ou de supprimer les FrontPage Server Extensions.

Cliquez ici pour obtenir une vidéo pas à pas de la fonctionnalité copier le site Web.

![](improvements-in-visual-studio-2005/_static/image4.png)

[Ouvrir la vidéo en plein écran](improvements-in-visual-studio-2005/_static/copysite1.wmv)

## <a name="improvements-in-debugging"></a>Améliorations du débogage

Il existe quatre améliorations clés du débogage dans Visual Studio 2005.

- Le débogage local en tant que non-administrateur peut être prêt à l’emploi.
- L’attribut debug pour l’élément compilation est désormais false par défaut.
- L’installation et la configuration du débogage à distance sont plus faciles qu’auparavant.
- Vous pouvez désormais déboguer un site Web ouvert via un emplacement FTP.

## <a name="debugging-as-a-non-administrator"></a>Débogage en tant que non-administrateur

L’ajout du Serveur de développement ASP.NET permet aux non-administrateurs de déboguer facilement les applications ASP.NET immédiatement. Quand une application ASP.NET s’exécutant sur le système de fichiers local est déboguée, Visual Studio lance le Serveur de développement ASP.NET dans le contexte de l’utilisateur connecté. Cet utilisateur peut ensuite Déboguer cette application sans aucune configuration supplémentaire.

## <a name="debug-is-false-by-default"></a>Le débogage a la valeur false par défaut

Dans ASP.NET 1. x, l’attribut *Debug* de l’élément *compilation* du fichier Web. config a la valeur *true* par défaut. Il a toujours été recommandé que les développeurs attribuent à cet attribut la valeur *false* avant de déployer une application en production, mais comme la plupart des développeurs ne comprennent pas pleinement les conséquences de la définition de l’attribut debug sur true, ils l’ont simplement laissé tel quel.

Le problème le plus grave lié à l’utilisation de l’attribut debug avec la valeur true est qu’il désactive le modèle de compilation par lot ASP. Par conséquent, chaque page est compilée dans une DLL distincte. Si une application Web se compose de milliers de pages (pas inconnues de quelque manière que ce soit), cela signifie que plusieurs milliers de dll seront créées par cette application. Bien que la taille de ces dll soit faible, elles ne sont pas chargées dans un emplacement particulier en mémoire. Par conséquent, ils provoquent la fragmentation de la mémoire système et peuvent contribuer aux occurrences OutOfMemoryException.

Dans ASP.NET 2,0, l’attribut debug est défini sur false par défaut. Comme vous l’avez déjà vu, quand un développeur débogue une application ASP.NET dans Visual Studio 2005, il est invité à ajouter un fichier Web. config avec le débogage activé. Cela entraîne les mêmes inconvénients que ceux présents dans ASP.NET 1. x, mais à présent, le développeur est clairement informé que l’attribut doit être réinitialisé sur false avant de déplacer l’application en production.

## <a name="remote-debugging-setup-and-configuration"></a>Installation et configuration du débogage à distance

Dans Visual Studio 2002/2003, le débogage distant s’appuyait sur le gestionnaire de débogage d’ordinateur (MDM. exe) et le processus vs7jit. exe. Pour cette raison, le dépannage des problèmes de débogage à distance était souvent une boîte noire pour les clients et il n’était souvent pas bien plus adapté aux PSS.

Visual Studio 2005 supprime la dépendance vis-à-vis des processus MDM. exe et vs7jit. exe. Au lieu de cela, il utilise désormais le service Remote Debug Monitor (msvsmon. exe).

La configuration requise pour le débogage à distance dans Visual Studio 2005 est assez simple. Vous devez exécuter msvsmon. exe sur le serveur distant avant le débogage. Vous pouvez installer Remote Debug Monitor à partir du CD de Visual Studio ou vous pouvez simplement exécuter msvsmon. exe à partir d’un partage sans installer quoi que ce soit sur le serveur Web.

Quand vous exécutez msvsmon. exe, il est probable qu’il se plaindra des ports bloqués pour le débogage distant. Heureusement, vous pouvez facilement débloquer les ports à partir de la droite dans la boîte de dialogue d’avertissement, comme indiqué ci-dessous.

![Notification indiquant que le pare-feu Windows bloque le débogage distant](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figure 12**: notification indiquant que le pare-feu Windows bloque le débogage à distance

Une fois que vous avez débloqué les ports nécessaires pour le débogage, vous verrez le Remote Debugging Monitor comme indiqué ci-dessous. À partir de cette interface, vous pouvez surveiller les connexions et modifier facilement les autorisations de débogage.

![Remote Debugging Monitor](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figure 13**: Remote Debugging Monitor

Il est également possible de déboguer à distance une application Web ouverte via FTP. Les étapes sont les mêmes que celles précédemment traitées. Toutefois, vous devrez spécifier une URL de base pour parcourir le projet FTP comme indiqué plus haut dans ce module.

## <a name="lab-2"></a>Laboratoire 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Débogage à distance avec Visual Studio 2005

Ce laboratoire vous guidera tout au long du débogage à distance avec Visual Studio 2005.

Cliquez ici pour obtenir une vidéo pas à pas de ce laboratoire.

![](improvements-in-visual-studio-2005/_static/image5.png)

[Ouvrir la vidéo en plein écran](improvements-in-visual-studio-2005/_static/remdebug1.wmv)

Ce laboratoire nécessite que vous disposiez de deux machines, l’une exécutant Visual Studio 2005 et l’autre qui exécute IIS 5 ou version ultérieure.

1. Ouvrez Visual Studio 2005 et créez un nouveau site Web sur le serveur distant.

> [!NOTE]
> Vous pouvez créer le site Web sur une instance distante d’IIS ou via FTP.

1. À partir du serveur Web distant, localisez msvsmon. exe sur l’ordinateur de développement à l’aide d’un chemin d’accès UNC et exécutez-le.  
 L’emplacement par défaut de msvsmon. exe est//Server/c $/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Si vous êtes invité à débloquer les ports pour le débogage distant, faites-le.
3. À partir de l’ordinateur de développement, ouvrez le code-behind pour default. aspx et définissez un point d’arrêt dans la méthode page/_Load.
4. Démarrez le débogage à partir de l’ordinateur de développement.

Vous devez atteindre le point d’arrêt comme prévu.

## <a name="aspnet-development-server"></a>serveur de développement ASP.NET

Comme nous l’avons déjà vu, Visual Studio 2005 est livré avec un serveur Web appelé Serveur de développement ASP.NET. (Le Serveur de développement ASP.NET est parfois appelé Cassini.) Ce serveur Web est un moyen pratique de parcourir et de déboguer les applications Web qui s’exécutent sur le système de fichiers.

Le Serveur de développement ASP.NET est un serveur Web restreint. Elle n’autorise pas les connexions à distance, mais elle n’autorise pas les demandes de la part d’un utilisateur autre que l’utilisateur qui a démarré le serveur Web. Il n’a pas non plus la possibilité de servir des pages ASP. Seules les ressources ASP.NET et HTML (y compris les images, les fichiers CSS, etc.) sont prises en charge.

La Serveur de développement ASP.NET peut être lancée à l’aide de la ligne de commande en exécutant le fichier WebDev. webserver. exe situé dans c:/Windows/Microsoft. NET/Framework/v 2.0./ */* / */* /*. La boîte de dialogue suivante affiche les paramètres qui sont disponibles.

![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Figure 14**

> [!NOTE]
> La Serveur de développement ASP.NET n’est pas prise en charge lorsqu’elle est lancée explicitement via la ligne de commande.
