---
uid: web-pages/readme/overview
title: Lisez-moi WebMatrix | Microsoft Docs
author: rick-anderson
description: Fichier Lisez-moi de la version 1,0 de WebMatrix et de pages Web ASP.NET (Razor)
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: fac53e935860a90d8f2aa96699d56d66ade3a40f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563468"
---
# <a name="webmatrix-readme"></a>Fichier Lisez-moi de WebMatrix

13 janvier 2011

## <a name="contents"></a>Contenu

> [!NOTE]
> Ce fichier Lisez-moi s’applique à la version 1,0 de WebMatrix.

- [Vue d’ensemble](#Overview)
- [Installation](#Installation_Notes)
- [Comment publier des applications](#InstructionsForPublishingApplications)
- [Modifications et problèmes](#ChangesAndIssues)

    - [Installation de WebMatrix 1,0](#Known_Issues_Installation)
    - [Pages Web ASP.NET](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Installation d’applications](#Known_Issues_Installing_Applications)
    - [Publication d’applications](#Known_Issues_Publishing_Applications)
- [Pour plus d'informations](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Présentation

> Microsoft WebMatrix 1,0 est une pile de développement Web gratuite qui s’installe en quelques minutes. Il intègre un serveur Web aux infrastructures de base de données et de programmation pour créer une expérience unique et intégrée. Vous pouvez utiliser WebMatrix pour simplifier la façon dont vous codez, testez et publiez votre propre site Web ASP.NET ou PHP, ou utilisez WebMatrix pour démarrer un nouveau site Web à l’aide d’applications open source populaires, telles que DotNetNuke, Umbraco, WordPress ou Joomla. WebMatrix utilise le même serveur Web puissant, le même moteur de base de données et le même environnement d’infrastructure que celui qui exécutera votre site Web sur Internet, ce qui rend le passage du développement à la production fluide et transparent.

<a id="Installation_Notes"></a>

## <a name="installation"></a>Installation

> Pour installer WebMatrix 1,0, vous devez d’abord installer le [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638). Une fois que vous avez installé le Web Platform Installer, vous pouvez l’utiliser pour installer WebMatrix.
> 
> Si vous rencontrez des problèmes lors de l’installation, reportez-vous à [résolution des problèmes avec Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).

<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Comment publier des applications

> Consultez [les instructions pas à pas pour la publication d’applications](https://go.microsoft.com/fwlink/?LinkID=196149)

<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Modifications et problèmes

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problèmes d’installation de WebMatrix 1,0

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problème : WebMatrix 1,0 est disponible uniquement sur les plateformes qui prennent en charge Microsoft .NET Framework 4

> Le .NET Framework version 4 est requis pour WebMatrix. Dans certains cas, le programme d’installation de WebMatrix 1,0 vous permet d’essayer d’installer sur une plateforme qui ne fait pas partie du jeu de configuration pris en charge. En particulier, Windows Vista sans la mise à jour SP1 vous permet de commencer l’installation de WebMatrix, mais le composant .NET Framework 4 échoue et bloque votre installation.
> 
> **Solution de contournement**  
> Installez sur une plateforme prise en charge, notamment :
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2 SP2
> - Windows Vista SP1 ou ultérieur
> - Windows XP SP3
> - Windows Server 2003 SP2

#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problème : impossible d’installer WebMatrix 1,0 si Microsoft Visual Studio 2008 est installé sans Microsoft Visual Studio 2008 SP1

> **Solution de contournement**  
> Installez [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) à partir du centre de téléchargement Microsoft.

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problème : certains assemblys pour SQL Server Compact 4,0 ne sont pas installés dans le GAC

> Les assemblys managés pour SQL Server Compact 4,0 ne sont pas placés dans le Global Assembly Cache (GAC) lorsque vous installez SQL Server Compact 4,0 sur un ordinateur 64 bits et que seul le profil client .NET Framework 3,5 SP1 est installé sur l’ordinateur. Les assemblys managés qui ne sont pas installés dans le GAC sont :
> 
> - *System. Data. SqlServerCe. dll* (fournisseur ADO.net)
> - *System. Data. SqlServerCe. Entity. dll* (ADO.NET Entity Framework)
> 
> **Solution de contournement**  
> Désinstallez SQL Server Compact 4,0. Téléchargez et installez la version complète de .NET Framework 3,5 SP1 à partir de l’emplacement suivant :  
>   
> [Microsoft .NET Framework 3,5 Service Pack 1 (package complet)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Réinstallez ensuite SQL Server Compact 4,0.

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problème : impossible de désinstaller SQL Server Compact à l’aide de la ligne de commande

> La désinstallation de SQL Server Compact à l’aide des options de ligne de commande ne fonctionne pas dans cette version.
> 
> **Solution de contournement**  
> Utilisez *programmes et fonctionnalités* dans le panneau de configuration Windows pour désinstaller Microsoft SQL Server Compact 4,0.

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Pages web ASP.NET

Cette section du document décrit les nouvelles fonctionnalités, les modifications et les problèmes connus liés à la version 1,0 de pages Web ASP.NET avec syntaxe Razor.

- [Nouvelles fonctionnalités](#NewFeatures)
- [Modifications](#Changes)
- [Problèmes](#Issues)

#### <a id="NewFeatures"></a>Nouvelles fonctionnalités

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Nouveau : paramètre de configuration ajouté pour désactiver le gestionnaire de package

> Une nouvelle clé de `asp:AdminManagerEnabled` est disponible pour l’élément `<appSettings>` dans le fichier *Web. config* , ce qui vous permet de désactiver complètement le gestionnaire de package. La valeur par défaut de cet élément est true, ce qui signifie que s’il n’est pas inclus dans le fichier *Web. config* , le gestionnaire de package est activé. Pour désactiver le gestionnaire de package, ajoutez l’élément suivant au fichier *Web. config* à la racine du site Web :
> 
> [!code-xml[Main](overview/samples/sample1.xml)]

#### <a id="Changes"></a>Relatives

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Modification : « page Web : AdminFolderVirtualPath » renommée en « ASP : AdminFolderVirtualPath »

> La clé de `webPages:AdminFolderVirtualPath` qui peut être ajoutée au fichier *Web. config* pour spécifier l’emplacement du gestionnaire de package a été renommée pour utiliser l’espace de noms `asp:` au lieu de l’espace de noms `webPages`. Si vous avez utilisé cet élément, vous devez le renommer dans le fichier de configuration.

#### <a id="Issues"></a>  Problèmes connus

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problème : les mots de passe des utilisateurs d’appartenance ne sont plus reconnus

> L’algorithme pour la création et le stockage des mots de passe d’appartenance (login) a été modifié de façon à être plus sécurisé. Par conséquent, les mots de passe stockés pour les membres (utilisateurs) créés dans les versions bêta de ASP.NET Razor ne seront pas reconnus. 
> 
> **Solution de contournement** Si le site n’a pas encore été mis en production, supprimez les enregistrements utilisateur de la base de données d’appartenance. Si la base de données est active, régénérez par programmation les mots de passe existants dans la base de données d’appartenance.

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problème : comportement inattendu lors de l’utilisation d’une table utilisateur personnalisée pour l’appartenance

> Pour initialiser le fournisseur d’appartenances pour un site Web Razor ASP.NET, vous appelez la méthode `WebSecurity.InitializeDatabaseConnection`. (Dans WebMatrix, le modèle Starter site comprend un appel à cette méthode dans le *\_fichier AppStart. cshtml* .) Si le paramètre `autoCreateTables` de cette méthode a la valeur true (par défaut, il a la valeur true dans le modèle Starter Site) et si un nom de table non reconnu est passé à la méthode (le deuxième paramètre), la méthode ne génère pas d’erreur. Au lieu de cela, il crée automatiquement la table.
> 
> Cela peut être un problème si vous avez l’intention d’utiliser une table utilisateur personnalisée pour l’appartenance, mais de transmettre un nom de table incorrect à la méthode `WebSecurity.InitializeDatabaseConnection`. Étant donné que la méthode ne génère pas par défaut une erreur si la table que vous spécifiez n’existe pas et qu’elle crée à la place une nouvelle table, l’application peut sembler fonctionner. Toutefois, le code d’application qui s’appuie sur votre table utilisateur personnalisée (et sur les champs qu’il contient) peut finir par échouer avec des erreurs inattendues.
> 
> **Solution de contournement**  
> Assurez-vous que le nom passé dans la méthode `InitializeDatabaseConnection` correspond à la table de profil utilisateur dans la base de données d’appartenance, ou assurez-vous que le paramètre `autoCreateTables` a la valeur false.

#### <a name="issue-error-message-the-admin-module-requires-access-to-app_data"></a>Problème : message d’erreur « le module admin requiert l’accès à ~/App\_Data »

> Dans certains cas, si vous tentez de créer des utilisateurs ou si vous travaillez avec le système d’appartenance ASP.NET, la page affiche l’erreur. *le module admin requiert l’accès à ~/App\_données*. Cela se produit si le compte sous lequel IIS ou IIS Express s’exécute ne dispose pas des autorisations nécessaires pour créer et écrire dans le dossier de données de l' *application\_* sous la racine du site Web. 
> 
> **Solution de contournement** Créez manuellement un dossier de *données d’application\_* pour le site Web. Ensuite, assurez-vous que le compte Windows sous lequel s’exécute l’application (en général SERVICE réseau) dispose des autorisations de lecture/écriture pour les dossiers racine de l’application et pour les sous-dossiers tels que les données d'\_d’application. Des informations plus détaillées sont disponibles dans l’article de la base de connaissances [problèmes avec SQL Server Express les projets d’application Web instanciation utilisateur et ASP.net](https://support.microsoft.com/kb/2002980).

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problème : erreur « Impossible de générer une instance utilisateur de SQL Server »

> Si une application Web WebMatrix utilise SQL Server Express et exécute IIS 7,5 sur Windows 7 ou Windows Server 2008 R2, vous pouvez voir une erreur qui indique que SQL Server ne peut pas récupérer le chemin d’accès de l’application locale de l’utilisateur au moment de l’exécution.
> 
> **Solution de contournement** Assurez-vous que le compte Windows sous lequel s’exécute l’application (en général SERVICE réseau) dispose des autorisations de lecture/écriture pour les dossiers racine de l’application et pour les sous-dossiers tels que les *données d'\_d’application*. Des informations plus détaillées sont disponibles dans l’article de la base de connaissances [problèmes avec SQL Server Express les projets d’application Web instanciation utilisateur et ASP.net](https://support.microsoft.com/kb/2002980).

#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problème : les fichiers qui contiennent des ressources du gestionnaire de package ou des mots de passe de gestionnaire de package peuvent être réservés sous IIS 6,0 et versions antérieures

> Si vous déployez une application pages Web ASP.NET (Razor) qui a été créée à l’aide de la version RC2, et si l’application contient un fichier *Password. txt* ou *PackageSources. txt* sous */app\_données/admin*, IIS 6,0 traitera le fichier si nécessaire, exposant potentiellement les mots de passe de votre instance du gestionnaire de package. 
> 
> **Solution de contournement** Renommez le fichier *Password. txt* ou *PackageSources. txt* en *Password. config* ou *PackageSources. config*. Par défaut, IIS 6,0 ne traite pas les fichiers dotés de l’extension *. config* . (Dans IIS 7, aucun fichier du dossier de *\_de données* de l’application n’est pris en charge. vous n’avez donc pas besoin de renommer les fichiers.)

#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problème : la désinstallation des packages installés à l’aide de la version bêta 3 ne supprime pas complètement les composants du package

> Si vous avez installé un package à l’aide du gestionnaire de package dans la version bêta 3, puis que vous essayez de le désinstaller à l’aide de la version actuelle, le package n’est pas complètement désinstallé. L’utilisation du bouton **désinstaller** du gestionnaire de package supprime certains composants, mais laisse le code de bibliothèque du package et ne met pas à jour le fichier *Package. config* .
> 
> **Solution de contournement**   
> Procédez comme suit :  
> 1. Supprimez l' *application\_dossier Data\packages* . Cela supprime tous les packages.   
> 2. Supprimez le fichier *packages. config* à la racine du site Web.

#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problème : dans Visual Studio, l’appel du gestionnaire de package basé sur le Web met l’application hors connexion

> Si vous travaillez dans Visual Studio (non WebMatrix) et que vous utilisez la fonctionnalité d' *administration de\_* pour démarrer le gestionnaire de package, Visual Studio met l’application hors connexion et publie l’application *\_offline. htm* dans la racine du site Web, ce qui interrompt votre capacité à utiliser le gestionnaire de package.
> 
> [!NOTE]
> Même si vous constatez généralement ce comportement lorsque vous utilisez l’interface du gestionnaire de package basée sur le Web, le même comportement se produit si vous ajoutez, supprimez ou modifiez des fichiers dans le dossier de données de l' *application\_* .
> 
> **Solution de contournement**   
> Pour travailler avec des packages dans Visual Studio, utilisez l’extension NuGet au lieu du gestionnaire de package basé sur le Web. Pour plus d’informations, consultez la [documentation NuGet](https://docs.microsoft.com/nuget/). Si vous travaillez avec d’autres fichiers dans le dossier de données de l' *application\_* , envisagez de conserver les fichiers ailleurs afin d’éviter ce problème. Si ce n’est pas pratique, supprimez manuellement l' *application\_fichier. htm hors connexion* ou patientez jusqu’à ce que le site soit remis en ligne automatiquement (par défaut, après 30 secondes).

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problème : Visual Studio IntelliSense et les modèles de projet disponibles uniquement dans ASP.NET MVC version 3

> L’installation de pages Web ASP.NET n’installe pas également les outils pour Visual Studio, tels que IntelliSense et les modèles de projet pour les applications pages Web ASP.NET.
> 
> **Solution de contournement** Pour utiliser IntelliSense et des modèles de projet pour pages Web ASP.NET des applications dans Visual Studio, installez ASP.NET MVC 3 RC par le biais du Web Platform Installer ou du [programme d’installation autonome](https://go.microsoft.com/fwlink/?LinkID=191797).

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problème : lecture de flux ou d’autres données externes via un serveur proxy

> Si le serveur qui exécute le site se trouve derrière un serveur proxy, vous devrez peut-être configurer les informations de proxy dans le fichier *Web. config* afin de pouvoir lire les informations provenant de l’extérieur de votre site. Par exemple, si vous utilisez le programme d’assistance `ReCaptcha`, le programme d’assistance communique avec le service reCAPTCHA, mais peut être bloqué par votre serveur proxy. De même, les flux utilisés dans pages Web ASP.NET, tels que le flux utilisé par le gestionnaire de package, peuvent nécessiter une configuration de proxy.
> 
> Si vous rencontrez des problèmes lors de l’utilisation d’un service externe ou de l’utilisation du flux de package, placez les éléments suivants dans le fichier *Web. config* racine de votre application :
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Pour plus d’informations sur la configuration d’un serveur proxy, consultez [&lt;&gt; proxy, élément (paramètres réseau)](https://msdn.microsoft.com/library/sa91de1e.aspx) sur le site Web MSDN.

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problème : la désinstallation du .NET Framework version 4 désactive pages Web ASP.NET avec la syntaxe Razor

> Si vous désinstallez le .NET Framework version 4, puis le réinstallez, pages Web ASP.NET avec syntaxe Razor est désactivé. Les pages avec l’extension *. cshtml* ne s’exécutent pas correctement. Pages Web ASP.NET inscrit un assembly dans le fichier *Web. config* racine de l’ordinateur, et la suppression du .NET Framework supprime ce fichier. La réinstallation du .NET Framework installe une nouvelle version du fichier de configuration, mais n’ajoute pas la référence pour l’assembly pages Web ASP.NET.
> 
> **Solution de contournement** Après la réinstallation du .NET Framework, réinstallez pages Web ASP.NET avec syntaxe Razor. L’élément suivant est ajouté au fichier *Web. config* à la racine de l’ordinateur, qui se trouve généralement à l’emplacement suivant :  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problème : les URL sans extension ne trouvent pas les fichiers. cshtml/. vbhtml sur IIS 7 ou IIS 7,5

> Sur IIS 7 ou IIS 7,5, les demandes avec une URL telle que la suivante ne sont pas en mesure de trouver les pages qui ont l’extension *. cshtml* ou *. vbhtml* :  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Le problème survient parce que la réécriture d’URL n’est pas activée par défaut pour IIS 7 ou IIS 7,5. Le scénario probable est que vous ne voyez pas le problème lors du test local à l’aide de IIS Express, mais vous le rencontrez quand vous déployez votre site Web sur un site Web d’hébergement.
> 
> **Solution de contournement**
> 
> - Si vous avez le contrôle sur l’ordinateur serveur, sur l’ordinateur serveur, installez la mise à jour qui est décrite dans [une mise à jour disponible qui permet à certains gestionnaires iis 7,0 ou iis 7,5 de gérer les demandes dont les URL ne se terminent pas par un point](https://support.microsoft.com/kb/980368).
> - Si vous n’avez pas le contrôle sur l’ordinateur serveur (par exemple, si vous effectuez un déploiement sur un site Web d’hébergement), ajoutez ce qui suit au fichier *Web. config* du site Web : 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problème : déploiement d’une application sur un ordinateur sur lequel SQL Server Compact n’est pas installé

> Les applications qui incluent des bases de données SQL Server Compact peuvent s’exécuter sur un ordinateur sur lequel SQL Server Compact n’est pas installé. Microsoft WebMatrix 1,0 copie automatiquement ces fichiers binaires pour vous et exécute les transformations de fichier *Web. config* appropriées.
> 
> **Solution de contournement** Si vous devez copier ces fichiers et modifier manuellement le fichier *Web. config* , procédez comme suit :
> 
> 1. Copiez les assemblys du moteur de base de données dans le dossier *bin* (et sous-dossiers) de l’application sur l’ordinateur cible :  
> 
>    - Copiez *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>      **à** *\Bin*
>    - Copiez *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\* **sur** *\Bin\x86*
>    - Copiez *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* * **vers** *\Bin\amd64*
> 
> 2. Dans le dossier racine du site Web, créez ou ouvrez un fichier *Web. config* . (Dans WebMatrix 1,0, ce type de fichier est disponible si vous cliquez sur **tout** dans la boîte de dialogue **choisir un type de fichier** .)
> 3. Ajoutez l’élément suivant en tant qu’enfant de l’élément `<configuration>` (pas à l’intérieur de l’élément `<system.web>`) :
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problème : les applications auxiliaires « base de données » et « WebGrid » ne fonctionnent pas dans un niveau de confiance moyen dans Visual Basic

> Si vous utilisez Visual Basic (en créant des fichiers *. vbhtml* ), les applications d’assistance `Database` et `WebGrid` ne fonctionneront pas si l’application est configurée pour utiliser une confiance moyenne.
> 
> **Solution de contournement**  
> Si vous utilisez Visual Studio 2010, vous pouvez résoudre ce problème en installant la version de Service Pack 1. Tant que la version finale de la version SP1 n’est pas disponible, vous pouvez télécharger la version bêta de SP1 à partir de la page [Microsoft Visual Studio 2010 Service Pack 1 bêta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) du centre de téléchargement Microsoft.   
>   
> Si ce n’est pas pratique ou si vous n’utilisez pas Visual Studio 2010, vous pouvez définir temporairement l’application pour qu’elle utilise la confiance totale.

#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problème : les ressources « ApplicationPart » sont accessibles en externe

> Si un assembly contient des objets qui dérivent de la classe `ApplicationPart`, les ressources de cet assembly sont exposées par la classe `ResourceRouteHandler`. Considérez par exemple l’URL suivante :  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Cette requête télécharge toutes les chaînes de ressource dans l’assembly *System. Web. webpages. Administration. dll* . Toutes les ressources incorporées (même celles qui ne sont pas destinées à être traitées en tant que contenu statique) sont téléchargées. Si les ressources incorporées contiennent des informations sensibles, cela peut représenter un risque pour la sécurité. 
> 
> **Solution de contournement**   
> Si vous créez un objet **ApplicationPart** , assurez-vous que les ressources incorporées associées à l’assembly de cet objet **ApplicationPart** ne contiennent pas d’informations sensibles.

<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Pour plus d’informations sur les problèmes d’installation de WebMatrix, consultez [problèmes d’installation de WebMatrix](#Known_Issues_Installation) plus haut dans ce document.

Cette section du document décrit les problèmes connus liés à l’environnement de développement WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problème : les modifications apportées au nom d’utilisateur ou au mot de passe d’une chaîne de connexion de base de données dans un fichier Web. config ne sont pas reflétées dans l’espace de travail bases de données

> **Solution de contournement**  
> 
> 1. Dans le fichier *Web. config* , modifiez le nom de la base de données dans la chaîne de connexion (par exemple, ajoutez « 1 » à celle-ci).
> 2. Enregistrez le fichier *Web. config* .
> 3. Cliquez sur **bases de données** et actualiser.
> 4. Remplacez le nom de la base de données de la chaîne de connexion dans le fichier *Web. config* par le nom de la base de données d’origine.
> 5. Enregistrez le fichier *Web. config* .
> 6. Cliquez sur **bases de données** et actualiser.

#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problème : impossible de supprimer des dossiers créés par WebMatrix

> Si WebMatrix s’exécute à l’aide d’autorisations élevées (autrement dit, si vous avez démarré WebMatrix à l’aide de l’option **exécuter en tant qu’administrateur** dans Windows), les dossiers créés par WebMatrix ne peuvent pas être supprimés à l’aide de l’Explorateur Windows.
> 
> **Solution de contournement**  
> Exécutez l’Explorateur Windows en utilisant des autorisations élevées. Procédez comme suit :  
> 
> 1. Dans Windows, cliquez sur **Démarrer**.
> 2. Entrez « Explorateur Windows », puis cliquez avec le bouton droit sur l’entrée de l' **Explorateur Windows**.
> 3. Cliquez sur **exécuter en tant qu’administrateur**. Vous pouvez ensuite supprimer les dossiers.

#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problème : WebMatrix 1,0 ne peut pas effectuer certaines tâches qui nécessitent une élévation

> WebMatrix 1,0 ne peut pas effectuer certaines tâches qui nécessitent une élévation, telles que l’installation de composants supplémentaires dans les situations suivantes :
> 
> - Sur Windows Vista ou Windows 7, vous êtes connecté avec un compte qui ne dispose pas de privilèges d’administration et que le contrôle de compte d’utilisateur (UAC) est désactivé.
> - Vous utilisez Microsoft Windows XP ou Microsoft Windows Server 2003.
> 
> **Solution de contournement**  
> La plupart des tâches dans WebMatrix 1,0 ne nécessitent pas d’autorisation d’administration. Pour ceux qui le font, vous pouvez effectuer l’opération en tant qu’administrateur ou suivre les étapes suivantes :
> 
> - Sur Windows Vista ou Windows 7, activez le contrôle de compte d’utilisateur.
> - Sur Windows XP, ajoutez l’utilisateur au groupe de sécurité administrateurs.

#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problème : « le site à partir de la Galerie Web » est désactivé

> L’option **site à partir de la Galerie Web** est désactivée si le Web Platform Installer 3,0 n’est pas installé.
> 
> **Solution de contournement**  
> Installez le [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problème : Google Chrome n’est pas disponible en tant qu’option d’exécution

> Google Chrome n’est pas affiché dans la liste des navigateurs sous **exécuter** sous l’onglet dossier de **démarrage** .
> 
> **Solution de contournement**  
> Certaines versions de Google Chrome ne s’inscrivent pas correctement avec la fonctionnalité programmes par défaut de Windows. Pour résoudre ce problème, démarrez Google Chrome, cliquez sur le menu *personnaliser et contrôler Google Chrome* , cliquez sur *options*, puis sur *créer Google Chrome comme navigateur par défaut*.

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problème : la boîte de dialogue « clé étrangère » n’autorise pas l’entrée d’une clé primaire

> La boîte de dialogue **clé étrangère** ne vous permet pas d’entrer le nom de la clé primaire à partir de la table de clé primaire.
> 
> **Solution de contournement**  
> Cela est intentionnel. Vous n’avez pas besoin d’entrer le nom de la clé primaire dans la table de clé primaire.

#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problème : IntelliSense n’est pas disponible dans WebMatrix pour syntaxe Razor C#, ou Visual Basic

> IntelliSense est pris en charge dans WebMatrix pour HTML et CSS. Toutefois, il n’est pas disponible pour d’autres langages. 
> 
> **Solution de contournement**   
> Aucun.

#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problème : IntelliSense pour HTML et CSS suggère des éléments qui ne sont pas appropriés au contexte

> IntelliSense pour le balisage dans WebMatrix prend en charge HTML à l’aide du [schéma de transition XHTML 1,0](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) et CSS à l’aide du [schéma CSS 2,1](http://www.w3.org/TR/CSS2/). Étant donné qu’IntelliSense est basé sur ces schémas spécifiques, certaines balises, attributs ou propriétés peuvent être suggérés et ne conviennent pas à la définition de la page ou du style actuelle. Pour HTML, elle peut également entraîner des suggestions inattendues dans le contenu qui peuvent être interprétées comme du code XHTML mal formé (par exemple, lorsque les balises ne sont pas fermées). Ce problème peut être plus perceptible si le point d’insertion se trouve à l’intérieur d’une balise incomplète. dans ce cas, IntelliSense peut suggérer de nouvelles balises d’ouverture ou proposer d’autres suggestions incorrectes. 
> 
> **Solution de contournement**   
> Pour le format HTML, assurez-vous que vous travaillez dans une page XHTML bien formée. Pour CSS, il n’existe aucune solution de contournement.

#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problème : IntelliSense n’est pas appelé lorsque vous tapez

> Dans certains cas, IntelliSense n’est peut-être pas appelé au format HTML ou CSS dans l’éditeur. En particulier, cela peut se produire lorsque le point d’insertion se trouve directement à côté d’un autre élément ou à la fin d’un fichier. 
> 
> **Solution de contournement**   
> Assurez-vous qu’il existe un espace blanc autour du point d’insertion et que le point d’insertion ne se trouve pas à la fin d’un fichier. Vous pouvez également appeler manuellement IntelliSense en appuyant sur CTRL + espace.

#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problème : aucune interface utilisateur n’est disponible pour la désactivation d’IntelliSense

> WebMatrix 1,0 ne fournit aucune interface utilisateur ou aucun geste pour désactiver IntelliSense. 
> 
> **Solution de contournement**   
> Démarrez WebMatrix à l’aide de la commande suivante, qui comprend un commutateur qui désactive IntelliSense :  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`

<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express possède son propre fichier Lisez-moi, qui est disponible à l’adresse suivante :

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x40C](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact possède son propre fichier Lisez-moi, qui est disponible à l’adresse suivante :

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Pour plus d’informations sur les problèmes qui impliquent l’installation de SQL Server Compact dans le cadre de WebMatrix, consultez [problèmes d’installation de WebMatrix](#Known_Issues_Installation) plus haut dans ce document.

### <a id="Known_Issues_Installing_Applications"></a>Installation d’applications

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problème : l’installation d’une application peut prendre beaucoup de temps si le dossier Mes documents de l’utilisateur est redirigé vers un partage réseau.

> **Solution de contournement**  
> Aucun. L’installation de l’application peut prendre un certain temps, mais elle sera correctement installée.

### <a id="Known_Issues_Publishing_Applications"></a>Publication d’applications

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problème : erreur « des autorisations requises ne peuvent pas être acquises » lors de la publication d’une base de données SQL Compact

> WebMatrix ne prend pas entièrement en charge le déploiement de fichiers binaires de prise en charge pour SQL Server Compact sur un serveur qui exécute .NET Framework version 3,5 avec une configuration de confiance moyenne.
> 
> **Solution de contournement**  
> La solution de contournement recommandée consiste à installer le .NET Framework 4 sur le serveur. Vous pouvez également effectuer les opérations suivantes :
> 
> 1. Ajoutez les éléments suivants à la section `SecurityClasses` dans le fichier *Web\_MediumTrust. config* :
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Créez un jeu d’autorisations dans le fichier *Web\_MediumTrust. config* avec les autorisations requises suivantes :
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Appliquez le jeu d’autorisations à SQL Server Compact en plaçant les éléments suivants dans le fichier *Web\_MediumTrust. config* :
> 
>     [!code-html[Main](overview/samples/sample8.html)]

#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problème : les applications Web Galerie et PhpBB affichent une erreur « le service n’est pas disponible » après la publication

> Dans certains cas, la publication d’une application entraîne une erreur « le service n’est pas disponible ».
> 
> **Solution de contournement**  
> Dans WebMatrix, ajoutez une barre oblique inverse (\) à la fin du nom du serveur dans la fenêtre **paramètres de publication** , puis publiez à nouveau l’application.

#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problème : les liens et la disposition de site Web Moodle sont rompus après la publication

> Une fois que vous avez publié une application Moodle, l’application ne fonctionne pas correctement.
> 
> **Solution de contournement**  
> Dans WebMatrix, ajoutez une barre oblique (/) à la fin du champ **nom du site** dans la fenêtre Paramètres de **publication** , puis publiez à nouveau l’application.

#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problème : échec de la publication de nopCommerce avec une erreur de base de données

> La publication de nopCommerce échoue et signale une erreur de base de données comme « échec de l’insertion dans NOP\_table du journal ».
> 
> **Solution de contournement**  
> 
> 1. Dans WebMatrix, cliquez sur **exécuter** pour lancer nopCommerce localement.
> 2. Connectez-vous à la page d’administration.
> 3. Cliquez sur le menu **système** .
> 4. Cliquez sur l’option **Journal** .
> 5. Cliquez sur le bouton **Effacer le journal**.
> 6. Publiez à nouveau nopCommerce.

#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problème : SilverStripe CMS affiche une erreur « HTTP 500 PHP FCGI » lorsque vous téléchargez un site publié

> **Solution de contournement**  
> Après avoir cliqué sur **Télécharger le site publié**, ignorez `silverstripe-cache/manifest_main` dans l' **aperçu de publication**. Ce fichier est utilisé à des fins de mise en cache et est spécifique à chaque ordinateur.

#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problème : le sous-texte affiche « erreur de serveur dans l’application «/ » lorsque vous téléchargez un site publié

> **Solution de contournement**  
> Ouvrez le fichier *Web. config* du site et remplacez l’ID utilisateur et le mot de passe dans la chaîne de connexion de la base de données par les informations d’identification d’administrateur SQL Server (les informations d’identification « sa »).
> 
> Vous pouvez également suivre ces étapes pour fournir au compte d’utilisateur auquel vous êtes connecté avec `db_owner` autorisations :
> 
> 1. Installez SQL Server Management Studio à l’aide du Web Platform Installer.
> 2. Connectez-vous à l’instance de SQL Server Express locale (par défaut, `.\SQLEXPRESS`).
> 3. Cliquez sur **bases de données** &gt; *[LocalSubtextDatabase]* &gt; **sécurité** &gt; **utilisateurs** &gt; *[localSubtextUser*] (la valeur par défaut est `subtextuser`], cliquez avec le bouton droit, puis cliquez sur **Propriétés**.
> 4. Sélectionnez **db\_propriétaire** dans la section appartenance au rôle.

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problème : le site peut ne pas fonctionner après la publication si le champ « URL de destination » n’est pas préfixé avec http://ou https://

> Dans la boîte de dialogue **paramètres de publication** , si l’URL de destination ne commence pas par `http://` ou `https://`, le site peut ne pas fonctionner après le déploiement.
> 
> **Solution de contournement**  
> Avant de publier un site, assurez-vous que l’URL de destination dans la boîte de dialogue **paramètres de publication** commence par `http://` ou `https://`.

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problème : la publication d’une base de données MySQL échoue avec l’erreur «Échec de la publication de la base de données. Cela peut se produire si la base de données distante ne peut pas exécuter le script.»

> L’erreur peut se produire pour plusieurs raisons. Vous pouvez voir cette erreur si le script de base de données contient un guillemet simple (') et si le jeu de caractères par défaut de la base de données MySQL de destination n’est pas UTF-8.
> 
> **Solution de contournement**  
> Définissez UTF-8 comme jeu de caractères par défaut pour la base de données MySQL à distance.

#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problème : certains liens ne sont pas visibles dans DotNetNuke après la publication ou le téléchargement du site

> Si vous publiez ou téléchargez un site DotNetNuke, vous devrez peut-être effacer le cache pour afficher les nouveaux liens sur le site.
> 
> **Solution de contournement**
> 
> 1. Connectez-vous en tant que « Host ».
> 2. Accédez au menu hôte et sélectionnez Paramètres de l' **ordinateur hôte**.
> 3. Faites défiler la liste vers le-dessous, puis sous **Paramètres avancés**, développez **paramètres de performances**.
> 4. Cliquez sur le lien **effacer le cache** pour les pages.
> 5. Accédez au bas de la page et redémarrez l’application.

#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problème : certains liens dans AtomSite sont rompus après le téléchargement d’un site publié

> **Solution de contournement**  
> Dans le fichier *service. config* , le fichier *users. config* et tous les fichiers *. xml* , remplacez la chaîne d’URL (par exemple, `http://myhost.com/atomsite`) par la chaîne locale (par exemple, `http://localhost:1239`).

#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problème : les applications MySQL telles que WordPress ne parviennent pas à publier et à signaler une erreur de base de données

> Par défaut, WebMatrix installe MySQL avec le jeu de caractères UTF-8. Si vous installez MySQL par vous-même et si le jeu de caractères n’est pas UTF-8 (par exemple, il est Latin1), le processus de publication des bases de données peut échouer.
> 
> **Solution de contournement**
> 
> 1. Remplacez le jeu de caractères de MySQL par UTF-8. (Pour plus d’informations, consultez [jeu de caractères de serveur et classement](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) sur le site Web MySQL.)
> 2. Réinstallez l'application.
> 3. Republiez l’application.

#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problème : « Télécharger le site publié » échoue pour les applications qui ont une configuration basée sur un navigateur

> Certaines applications (par exemple, Kentico CMS) vous obligent à les lancer dans le navigateur pour effectuer une installation après l’installation, telle que la création d’une base de données. Si vous publiez une application comme celle-ci sans effectuer l’installation basée sur un navigateur, la tentative de téléchargement du même site à partir d’un serveur distant échouera.
> 
> **Solution de contournement**  
> Terminez l’installation basée sur un navigateur avant de publier le site.

#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problème : « Télécharger le site publié » échoue avec une erreur de base de données pour DotNetNuke et Kooboo CMS

> Si vous essayez de télécharger une application à partir d’un serveur et que vous avez des informations d’identification d’administrateur dans la chaîne de connexion de la base de données dans la boîte de dialogue **paramètres de publication** , l’erreur suivante peut s’afficher dans le journal de publication :
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Solution de contournement**  
> Si possible, republiez le site (ou publiez-le) à l’aide d’informations d’identification non-administrateur pour la base de données.

<a id="More_Info"></a>

## <a name="for-more-information"></a>Pour plus d'informations

Pour plus d’informations sur WebMatrix 1,0, consultez les sites Web suivants :

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Tous droits réservés. [Conditions d’utilisation](https://msdn.microsoft.cos/cc300389.aspx).
