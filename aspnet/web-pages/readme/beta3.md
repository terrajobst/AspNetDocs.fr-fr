---
uid: web-pages/readme/beta3
title: Fichier Lisez-moi de la version bêta 3 de Web Matrix et de pages Web ASP.NET (Razor) | Microsoft Docs
author: rick-anderson
description: Fichier Lisez-moi de WebMatrix et ASP.NET Web Pages (Razor) Bêta 3
ms.author: riande
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: dc1d9237c04a7fcdbf4db6ccc8c36d255f6de003
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628897"
---
# <a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Fichier Lisez-moi de WebMatrix et ASP.NET Web Pages (Razor) Bêta 3

> Fichier Lisez-moi de WebMatrix et ASP.NET Web Pages (Razor) Bêta 3

9 novembre 2010

## <a name="contents"></a>Contenu

- [Vue d’ensemble](#Overview)
- [Installation](#Installation_Notes)
- [Nouvelles fonctionnalités, modifications et problèmes connus dans la version bêta 3](#Known_Issues)

    - [Problèmes d’installation de WebMatrix](#Known_Issues_Installation)
    - [Pages Web ASP.NET](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Installation d’applications](#Known_Issues_Installing_Applications)
    - [Publication d’applications](#Known_Issues_Publishing_Applications)
    - [Autres problèmes](#Known_Issues_Other_Issues)
- [Pour plus d'informations](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Présentation

> Microsoft WebMatrix Beta est une pile de développement Web gratuite qui s’installe en quelques minutes. Il intègre un serveur Web aux infrastructures de base de données et de programmation pour créer une expérience unique et intégrée. Vous pouvez utiliser la version bêta de WebMatrix pour simplifier la façon dont vous codez, testez et publiez votre propre site Web ASP.NET ou PHP, ou utilisez WebMatrix bêta pour démarrer un nouveau site Web à l’aide d’applications open source populaires, telles que DotNetNuke, Umbraco, WordPress ou Joomla. WebMatrix Beta utilise le même serveur Web, le même moteur de base de données et le même environnement de frameworks que ceux qui exécutent votre site Web sur Internet, ce qui rend le passage du développement à la production fluide et transparent.

<a id="Installation_Notes"></a>

## <a name="installation"></a>Installation

> Pour installer WebMatrix bêta 3, vous utilisez [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638). Une fois que vous avez installé le Web Platform Installer, vous pouvez l’utiliser pour installer la version bêta 3 de WebMatrix.
> 
> Si vous rencontrez des problèmes lors de l’installation, reportez-vous à [résolution des problèmes avec Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).

<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Instructions pour la publication d’applications

> Consultez [les instructions pas à pas pour la publication d’applications](https://go.microsoft.com/fwlink/?LinkID=196149)

<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Nouvelles fonctionnalités, modifications, problèmes andKnown

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Installation de WebMatrix bêta 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problème : WebMatrix bêta 3 est disponible uniquement sur les plateformes qui prennent en charge Microsoft .NET Framework 4

> Le .NET Framework version 4 est requis pour WebMatrix Beta. Dans certains cas, le programme d’installation bêta WebMatrix vous permet d’essayer d’installer sur une plateforme qui ne fait pas partie du jeu de configuration pris en charge. En particulier, Windows Vista sans la mise à jour SP1 vous permet de commencer l’installation de WebMatrix bêta, mais le composant .NET Framework 4 échoue et bloque votre installation.
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

#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problème : impossible d’installer WebMatrix bêta 3 Si Microsoft Visual Studio 2008 est installé sans Microsoft Visual Studio 2008 SP1

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

Cette section du document décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de la version bêta 3 de pages Web ASP.NET avec syntaxe Razor.

- [Nouvelles fonctionnalités](#NewFeatures)
- [Modifications](#Changes)
- [Problèmes](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Nouvelles fonctionnalités de la version bêta 3 pour pages Web ASP.NET avec la syntaxe Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Nouveau : la méthode « html. RAW » restitue le balisage non encodé

> La nouvelle méthode `Html.Raw` vous permet de restituer le balisage HTML en tant que balisage au lieu de restituer la sortie encodée. (Par défaut, ASP.NET Razor encode les chaînes avant de les restituer.) La syntaxe est la suivante :
> 
> `Html.Raw(value)`
> 
> L'exemple suivant montre comment utiliser `Html.Raw` :
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]

<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Modifications apportées à la version bêta 3 pour pages Web ASP.NET avec la syntaxe Razor

#### <a name="change-hrefattribute-method-removed"></a>Modification : méthode « HrefAttribute » supprimée

> La méthode `HrefAttribute` de la classe `WebPage` a été supprimée. Cette application d’assistance a été utilisée pour encoder des caractères non sécurisés dans les URL. Elle n’est plus nécessaire, car ASP.NET Razor encode automatiquement les chaînes. (Utilisez la nouvelle méthode `Html.Raw` pour afficher les chaînes non encodées.)

#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Modification : la syntaxe des applications auxiliaires «@helper» déclaratives a changé

> Dans la version bêta 3, ASP.NET modifie la manière dont il analyse les applications auxiliaires créées à l’aide de la syntaxe `@helper`. En résumé, la syntaxe de `@helper` est maintenant analysée comme un bloc de code plutôt que comme un bloc de balisage pouvant inclure du code. Par conséquent, le code à l’intérieur de l’application d’assistance n’a pas besoin d’être placé entre des blocs `@{ }`. Inversement, le balisage à l’intérieur du programme d’assistance doit être inclus explicitement dans des éléments HTML ou dans des balises Razor `<text></text>` ASP.NET.
> 
> Par exemple, la syntaxe de `@helper` suivante fonctionne dans la version bêta 3 :
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> Dans la version bêta 3, cette application d’assistance doit être modifiée pour ressembler à l’exemple suivant :
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Notez que le `@{ }` caractères autour du code initial dans le programme d’assistance n’est plus utilisé. Cela est dû au fait que le contenu des applications auxiliaires est traité comme un bloc de code par défaut. L’application d’assistance restitue le balisage, qui commence par la balise d’ouverture `<a>`. Si le programme d’assistance doit restituer du texte brut ou des balises qui n’incluent pas de balise de fermeture (par exemple, des balises `<meta>`), le contenu à restituer doit se trouver dans des balises `<text></text>`.

#### <a name="change-webpagecontexthttpcontext-removed"></a>Modification : « WebPageContext. HttpContext » a été supprimé

> La propriété `WebPageContext.HttpContext` a été supprimée. Utilisez plutôt `HttpContext.Current`. (La propriété `WebPageContext.HttpContext` a simplement encapsulé ce.)

#### <a name="change-facebook-helper-moved-to-new-package"></a>Modification : l’assistance « Facebook » a été déplacée vers le nouveau package

> Le programme d’assistance de `Facebook` a été déplacé vers la bibliothèque *Facebook. Helper* , qui inclut le programme d’assistance `Facebook` et des fonctionnalités supplémentaires. Vous devez installer cette bibliothèque en tant que package distinct, comme décrit dans la section « Installation des applications auxiliaires avec le gestionnaire de package » dans le didacticiel [prise en main avec les Pages ASP.net](https://go.microsoft.com/fwlink/?LinkId=202889).

#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Modification : l’appartenance, le rôle et les types de sécurité sont déplacés vers un nouvel assembly

> Les types suivants ont été déplacés vers l’assembly `WebMatrix.WebData` :
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`

#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Modification : la classe "TagBuilder" a été déplacée dans l’assembly System. Web. webpages. dll

> La classe `TagBuilde` r a été déplacée vers l’assembly System. Web. webpages. dll. Auparavant, il était dans un assembly qui faisait partie d’ASP.NET MVC. Cette modification signifie que vous n’avez pas besoin d’installer ASP.NET MVC pour pouvoir utiliser la classe `TagBuilder`.
> 
> Toutefois, la classe est toujours dans l’espace de noms `System.Web.Mvc`. Pour pouvoir utiliser la classe `TagBuilder` (par exemple, dans un programme d’assistance ASP.NET Razor personnalisé), vous devez référencer l’espace de noms (par exemple, en ajoutant `@using System.Web.Mvc` à votre code).

#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Modification : la syntaxe de validation de la demande a changé ; Classe « validation » supprimée

> Dans la version bêta 3, pour désactiver la validation d’un champ ou d’un ensemble de champs individuels, vous pouvez appeler la méthode `Validation.Exclude`, en passant le nom ou les noms des champs à exclure de la validation. Une nouvelle syntaxe est disponible dans la version bêta 3 pour ignorer la validation. La méthode `Validation` utilisée dans la version bêta 3 a été supprimée.
> 
> > [!NOTE]
> > Si vous ne désactivez pas la validation des demandes, si les utilisateurs essaient de télécharger le balisage HTML (par exemple, à l’aide d’un éditeur de texte enrichi sur une page), le site Web signale une erreur telle qu' *une requête. un formulaire potentiellement dangereux a été détecté à partir du client* et l’entrée utilisateur n’est pas acceptée. Si vous désactivez la validation de demande, vous devez vérifier manuellement l’entrée d’utilisateur pour vous assurer qu’elle ne contient pas de balisage ou de script potentiellement dangereux à l’aide d’éléments tels que [Microsoft Anti-Cross site Scripting Library v 4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Pour désactiver la validation de demande automatique, appelez la méthode `Request.Unvalidated`, en lui transmettant le nom du champ ou d’un autre objet de publication pour lequel vous souhaitez ignorer la validation de la demande. Vous pouvez utiliser cette méthode pour contourner la validation pour tous les éléments dans les collections `Form`, `QueryString`, `Cookies`et `ServerVariables`. Les exemples suivants montrent comment utiliser la méthode `Unvalidated` :
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]

<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Problèmes connus pour pages Web ASP.NET avec la syntaxe Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problème : comportement inattendu lors de l’utilisation d’une table utilisateur personnalisée pour l’appartenance

> Pour initialiser le fournisseur d’appartenances pour un site Web Razor ASP.NET, vous appelez la méthode `WebSecurity.InitializeDatabaseConnection`. (Dans WebMatrix, le modèle Starter site comprend un appel à cette méthode dans le *\_fichier AppStart. cshtml* .) Si le paramètre `autoCreateTables` de cette méthode a la valeur true (par défaut, il a la valeur true dans le modèle Starter Site) et si un nom de table non reconnu est passé à la méthode (le deuxième paramètre), la méthode ne génère pas d’erreur. Au lieu de cela, il crée automatiquement la table.
> 
> Cela peut être un problème si vous avez l’intention d’utiliser une table utilisateur personnalisée pour l’appartenance, mais de transmettre un nom de table incorrect à la méthode `WebSecurity.InitializeDatabaseConnection`. Étant donné que la méthode ne génère pas par défaut une erreur si la table que vous spécifiez n’existe pas et qu’elle crée à la place une nouvelle table, l’application peut sembler fonctionner. Toutefois, le code d’application qui s’appuie sur votre table utilisateur personnalisée (et sur les champs qu’il contient) peut finir par échouer avec des erreurs inattendues.
> 
> **Solution de contournement**  
> Assurez-vous que le nom passé dans la méthode `InitializeDatabaseConnection` correspond à la table de profil utilisateur dans la base de données d’appartenance, ou assurez-vous que le paramètre `autoCreateTables` a la valeur false.

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problème : erreur « Impossible de générer une instance utilisateur de SQL Server »

> Si une application Web WebMatrix utilise SQL Server Express et exécute IIS 7,5 sur Windows 7 ou Windows Server 2008 R2, vous pouvez voir une erreur qui indique que SQL Server ne peut pas récupérer le chemin d’accès de l’application locale de l’utilisateur au moment de l’exécution.
> 
> **Solution de contournement** Assurez-vous que le compte Windows sous lequel s’exécute l’application (en général SERVICE réseau) dispose des autorisations de lecture/écriture pour les dossiers racine de l’application et pour les sous-dossiers tels que les *données d'\_d’application*. Des informations plus détaillées sont disponibles dans l’article de la base de connaissances [problèmes avec SQL Server Express les projets d’application Web instanciation utilisateur et ASP.net](https://support.microsoft.com/kb/2002980).

#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problème : dans Visual Studio, les espaces de noms pour les assemblys personnalisés (dll) ne sont pas importés automatiquement

> Si vous utilisez des assemblys personnalisés dans un projet dans Visual Studio, les espaces de noms déclarés dans ces assemblys ne sont pas importés automatiquement au moment de la conception. Par conséquent, les références aux types personnalisés peuvent ne pas être reconnues au moment du design et sont marquées comme non reconnues dans Visual Studio (à l’aide d’un « tilde »). Ce problème se produit uniquement au moment de la conception dans Visual Studio. l’application elle-même s’exécute correctement.
> 
> **Solution de contournement**  
> Incluez une instruction `using` (`imports` dans Visual Basic) qui fait référence aux entités qui ne sont pas reconnues au moment de la conception.

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problème : Visual Studio IntelliSense et les modèles de projet disponibles uniquement dans ASP.NET MVC version 3

> L’installation de pages Web ASP.NET n’installe pas également les outils pour Visual Studio, tels que IntelliSense et les modèles de projet pour les applications pages Web ASP.NET.
> 
> **Solution de contournement** Pour utiliser IntelliSense et des modèles de projet pour pages Web ASP.NET des applications dans Visual Studio, installez ASP.NET MVC 3 RC par le biais du Web Platform Installer ou du [programme d’installation autonome](https://go.microsoft.com/fwlink/?LinkID=191797).

#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problème : « erreur de&lt;&gt; classe introuvable »

> Après la mise à niveau vers la version bêta 3, vous pouvez voir une erreur indiquant qu’une classe d’assistance (par exemple, la classe `Facebook`) est introuvable. À partir de la version bêta 2 et en continuant dans la version bêta 3, les programmes d’assistance ont été déplacés vers des packages que vous devez installer explicitement. Les sites existants ne sont pas mis à niveau pour inclure ces packages. Cela comprend les sites des dossiers *\Mes Documents\IISExpress* ou *\Mes Documents\Mes sites Web* . En particulier, cette erreur s’affiche si vous utilisez le site par défaut dans *mes sites* (WebSite1), qui comprend une référence à l’assistance `Twitter`.
> 
> **Solution de contournement**  
> Commentez les appels vers les programmes d’assistance du site, exécutez la page d' *administration\_* et installez le ou les packages qui incluent les programmes d’assistance que vous souhaitez utiliser. Une fois que vous avez installé le package, vous pouvez supprimer les marques de commentaire des lignes qui font référence aux applications auxiliaires.

#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problème : le déploiement des assemblys Razor bêta 3 ASP.NET dans le dossier bin peut ne pas fonctionner sur les sites d’hébergement

> Si vous déployez un site Web pages Web ASP.NET sur un site d’hébergement et que vous déployez les assemblys ASP.NET Razor bêta 3 dans le dossier *bin* du site, vous risquez de rencontrer des erreurs, notamment les suivantes :
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Cela peut se produire si le fournisseur d’hébergement a installé les assemblys pages Web ASP.NET bêta 1 dans le GAC (Global application cache) du serveur. Les assemblys dans le GAC sont prioritaires sur les assemblys installés localement dans le dossier *bin* .
> 
> **Solution de contournement** Contactez votre fournisseur d’hébergement pour confirmer que les erreurs que vous voyez sont dues à un conflit entre les versions du fournisseur des assemblys et les vôtres. Dans ce cas, demandez à ce que le fournisseur d’hébergement met à jour les assemblys dans le GAC du serveur.

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problème : lecture de flux ou d’autres données externes via un serveur proxy

> Si le serveur qui exécute le site se trouve derrière un serveur proxy, vous devrez peut-être configurer les informations de proxy dans le fichier *Web. config* afin de pouvoir lire les informations provenant de l’extérieur de votre site. Par exemple, si vous utilisez le programme d’assistance `ReCaptcha`, le programme d’assistance communique avec le service reCAPTCHA, mais peut être bloqué par votre serveur proxy. De même, les flux utilisés dans pages Web ASP.NET, tels que le flux utilisé par le gestionnaire de package, peuvent nécessiter une configuration de proxy.
> 
> Si vous rencontrez des problèmes lors de l’utilisation d’un service externe ou de l’utilisation du flux de package, placez les éléments suivants dans le fichier *Web. config* racine de votre application :
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Pour plus d’informations sur la configuration d’un serveur proxy, consultez [&lt;&gt; proxy, élément (paramètres réseau)](https://msdn.microsoft.com/library/sa91de1e.aspx) sur le site Web MSDN.

#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problème : erreur « Microsoft. Web. infrastructure. dll ne peut pas être chargée »

> Si vous avez précédemment installé la version bêta 1 de pages Web ASP.NET avec syntaxe Razor puis installé la version bêta 3, tous les assemblys appropriés sont installés dans le GAC, à l’exception de *Microsoft. Web. infrastructure. dll*. Par conséquent, lorsque vous exécutez des pages Razor ASP.NET, vous voyez une erreur qui indique que *Microsoft. Web. infrastructure. dll* n’a pas pu être chargé.
> 
> Ce problème ne se produit pas si vous avez chargé la version bêta 3 sur un ordinateur propre.
> 
> **Solution de contournement**  
> Dans le panneau de configuration, désinstallez pages Web ASP.NET. Réinstallez ensuite la version bêta 3.

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problème : la désinstallation du .NET Framework version 4 désactive pages Web ASP.NET avec la syntaxe Razor

> Si vous désinstallez le .NET Framework version 4, puis le réinstallez, pages Web ASP.NET avec syntaxe Razor est désactivé. Les pages avec l’extension *. cshtml* ne s’exécutent pas correctement. Pages Web ASP.NET inscrit un assembly dans le fichier *Web. config* racine de l’ordinateur, et la suppression du .NET Framework supprime ce fichier. La réinstallation du .NET Framework installe une nouvelle version du fichier de configuration, mais n’ajoute pas la référence pour l’assembly pages Web ASP.NET.
> 
> **Solution de contournement** Après la réinstallation du .NET Framework, réinstallez pages Web ASP.NET avec syntaxe Razor. L’élément suivant est ajouté au fichier *Web. config* à la racine de l’ordinateur, qui se trouve généralement à l’emplacement suivant :  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]

#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problème : les applications précédemment déployées avec des assemblys ASP.NET dans le dossier bin rencontrent des erreurs

> Pendant le déploiement, les copies des assemblys pages Web ASP.NET (par exemple, *Microsoft. webpages. dll*) dans le dossier *bin* du site Web sur le serveur. (Cela peut être dû à un déploiement automatique ou au fait que le développeur a explicitement copié les assemblys.) Toutefois, lorsque la version bêta 3 est installée, des erreurs se produisent, telles que des erreurs signalant que certains types sont introuvables. Cela est dû au fait que plusieurs types de pages Web ASP.NET ont été déplacés dans différents espaces de noms pour la version bêta 3.
> 
> **Solution de contournement**   
> Effacez le dossier *bin* de l’application déployée, copiez les nouveaux assemblys dans le dossier (ou redéployez l’application), puis redémarrez l’application.

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
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]

#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problème : utilisation d’un projet d’application Web ou de pages Web ASP.NET MVC et ASP.NET dans la même application

> Si vous utilisez pages Web ASP.NET dans un projet d’application Web ou une application ASP.NET MVC, vous pouvez voir une erreur indiquant que *WebPageHttpApplication* est introuvable.
> 
> **Solution de contournement**  
> Si vous obtenez cette erreur, modifiez la classe de base à partir de laquelle dérive l’application. Dans le fichier *global. asax* , modifiez la ligne suivante :
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> Par ceci :
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Cela a pour effet d’annuler une modification introduite pour la version bêta 1 de pages Web ASP.NET avec syntaxe Razor.

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problème : déploiement d’une application sur un ordinateur sur lequel SQL Server Compact n’est pas installé

> Les applications qui incluent des bases de données SQL Server Compact peuvent s’exécuter sur un ordinateur sur lequel SQL Server Compact n’est pas installé. Microsoft WebMatrix bêta 3 copie automatiquement ces fichiers binaires pour vous et exécute les transformations de fichier *Web. config* appropriées.
> 
> **Solution de contournement** Si vous devez copier ces fichiers et modifier manuellement le fichier *Web. config* , procédez comme suit :
> 
> 1. Copiez les assemblys du moteur de base de données dans le dossier *bin* (et sous-dossiers) de l’application sur l’ordinateur cible : 
> 
>     - Copiez *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **dans** *\Bin*
>     - Copiez *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\* * **vers** *\Bin\x86*
>     - Copiez *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* * **vers** *\Bin\amd64*
> 2. Dans le dossier racine du site Web, créez ou ouvrez un fichier *Web. config* . (Dans WebMatrix bêta 3, ce type de fichier est disponible si vous cliquez sur **tout** dans la boîte de dialogue **choisir un type de fichier** .)
> 3. Ajoutez l’élément suivant en tant qu’enfant de l’élément de **&gt;de configuration&lt;** (et non à l’intérieur de l’élément **&lt;system. Web&gt;** ) :
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problème : les applications auxiliaires de base de données et WebGrid ne fonctionnent pas dans une confiance moyenne dans Visual Basic

> Si vous utilisez Visual Basic (en créant des fichiers *. vbhtml* ), les applications d’assistance `Database` et `WebGrid` ne fonctionneront pas si l’application est configurée pour utiliser une confiance moyenne.
> 
> **Solution de contournement**  
> Définissez temporairement l’application pour qu’elle utilise la confiance totale.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problème : la propriété « Encrypt » n’est pas reconnue

> SQL Server Compact 4,0 ne reconnaît pas la propriété `Encrypt` de la classe `SqlCeConnection`. Vous ne devez pas utiliser cette propriété pour chiffrer les fichiers de base de données. La propriété `Encrypt` a été dépréciée dans SQL Server Compact version 3,5 et n’a été conservée que pour la compatibilité descendante. 
> 
> **Solution de contournement**  
> Utilisez la propriété `Encryption Mode` de la classe `SqlCeConnection` pour chiffrer les fichiers de base de données SQL Server Compact 4,0. L’exemple suivant montre comment créer une base de données SQL Server Compact 4,0 chiffrée à l’aide de la propriété `Encryption Mode` :
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Pour modifier le mode de chiffrement d’une base de données SQL Server Compact 4,0 existante, procédez comme suit :
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Pour chiffrer une base de données SQL Server Compact 4,0 non chiffrée, procédez comme suit :
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]

#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problème : les bibliothèques C++ Runtime Microsoft Visual 2008 sont requises

> Les DLL natives de SQL Server Compact 4,0 requièrent les C++ bibliothèques Runtime Microsoft Visual 2008 (x86, ia64 et x64), Service Pack 1.
> 
> **Solution de contournement**  
> Installez le .NET Framework 3,5 SP1. Cela installe également les bibliothèques C++ Runtime Visual 2008 SP1. Vous pouvez télécharger les bibliothèques à partir de l’emplacement suivant :   
>   
> [Mise à C++ jour de sécurité ATL pour le package redistribuable Microsoft Visual 2008 Service Pack 1](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Notez que l’installation de la .NET Framework 2,0, 3,0 ou 4 n’installe pas C++ les bibliothèques Runtime Visual 2008 SP1.

#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problème : si SQL Server Compact est installé avant l’installation de .NET Framework sur l’ordinateur, son nom invariant de fournisseur n’est pas enregistré dans le fichier machine. config .NET Framework

> SQL Server Compact peut être installé sur un ordinateur sur lequel .NET Framework n’est pas installé, car SQL Server Compact requiert le .NET Framework. Si aucune .NET Framework version 3,5 ou 4 n’est installée avant l’installation de SQL Server Compact, le programme d’installation de SQL Server Compact n’inscrit pas son nom invariant de fournisseur dans le fichier *machine. config* . Toutes les applications qui s’appuient sur l’entrée SQL Server Compact dans le fichier *machine. config* échouent. L’entrée d’inscription de nom invariant dans *machine. config* ressemble à l’exemple suivant :
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Solution de contournement**  
> Désinstallez SQL Server Compact 4,0 CTP1. Téléchargez et installez les versions complètes de la .NET Framework à partir de l’emplacement suivant :
> 
> [Microsoft .NET Framework 3,5 Service Pack 1 (package complet)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft .NET Framework 4,0 (package complet)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Réinstallez ensuite [SQL Server Compact 4,0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).

<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Installation d'applications

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problème : l’installation d’une application peut prendre beaucoup de temps si le dossier Mes documents de l’utilisateur est redirigé vers un partage réseau.

> **Solution de contournement**  
> Aucun. L’installation de l’application peut prendre un certain temps, mais elle sera correctement installée.

<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Publication d’applications

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

<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Autres problèmes

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problème : la recherche/le filtre ne fonctionne pas dans les rapports pour regrouper par : type de problème

> Lorsque vous exécutez un rapport pour un site, si vous entrez du texte dans la zone *Filtrer par URL* et que vous cliquez sur *Rechercher*, rien ne se passe. Cela est dû au fait que ce contrôle n’est pas fonctionnel alors que l’état *regrouper par* du rapport a la valeur *type de problème*, qui est la valeur par défaut.
> 
> **Solution de contournement** Sous l’onglet *Grouper par* du ruban, cliquez sur *URL* pour regrouper les entrées en fonction de leur URL source. La zone de texte et le bouton pour filtrer les entrées sont fonctionnels dans cet État.

#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problème : les applications WCF ne peuvent pas s’exécuter avec IIS Express

> L’accès à une application WCF génère une erreur semblable à celle-ci :
> 
> *Impossible de charger le fichier ou l’assembly’Microsoft. Web. Administration, version = 7.0.0.0, culture = neutral, PublicKeyToken = 31bf3856ad364e35 'ou l’une de ses dépendances. Le système ne peut pas trouver le fichier spécifié.*
> 
> Cela est dû au fait que IIS Express version bêta ne prend pas en charge WCF par défaut.
> 
> **Solution de contournement** Utilisez l’une des solutions de contournement suivantes (solution #2 nécessite Microsoft Windows Vista ou version ultérieure) :
> 
> 
> 1. Copiez les assemblys *Microsoft. Web. dll* et *Microsoft. Web. Administration. dll* à partir de l’emplacement d’installation de WebMatrix dans le répertoire *bin* de l’application WCF. Par défaut, WebMatrix est installé dans le sous-dossier *Microsoft WebMatrix* sous le dossier *Program Files* du système.
> 2. Sur Microsoft Windows Vista ou version ultérieure, créez un lien symbolique vers les assemblys dans le répertoire *bin* à l’aide des commandes suivantes. (Cette approche présente l’avantage de ne pas créer de copie des assemblys.)
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Installez les deux assemblys dans le GAC. À partir d’une invite avec élévation de privilèges, exécutez les commandes suivantes :
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]

#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problème : WebMatrix bêta 3 ne peut pas effectuer certaines tâches qui nécessitent une élévation

> WebMatrix bêta 3 ne peut pas effectuer certaines tâches qui nécessitent une élévation, telles que l’installation de composants supplémentaires dans les situations suivantes :
> 
> - Sur Windows Vista ou Windows 7, vous êtes connecté avec un compte qui ne dispose pas de privilèges d’administration et que le contrôle de compte d’utilisateur (UAC) est désactivé.
> - Vous utilisez Microsoft Windows XP ou Microsoft Windows Server 2003.
> 
> **Solution de contournement**  
> La plupart des tâches de WebMatrix bêta 3 ne nécessitent pas d’autorisation d’administration. Pour ceux qui le font, vous pouvez effectuer l’opération en tant qu’administrateur ou suivre les étapes suivantes :
> 
> - Sur Windows Vista ou Windows 7, activez le contrôle de compte d’utilisateur.
> - Sur Windows XP, ajoutez l’utilisateur au groupe de sécurité administrateurs.

#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problème : « le site à partir de la Galerie Web » est désactivé

> L’option **site à partir de la Galerie Web** est désactivée si le Web Platform Installer 3,0 n’est pas installé.
> 
> **Solution de contournement**  
> Installez le [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).

#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problème : sur Windows Server 2003, IIS Express ne démarre pas pour un utilisateur non-administrateur

> Sur Windows Server 2003, lorsque vous lancez une page ou démarrez IIS Express, IIS Express ne démarre pas. Pour les pages Web, une erreur s’affiche indiquant que l’application a été démarrée par un utilisateur non-administrateur.
> 
> **Solution de contournement**  
> Démarrez WebMatrix bêta 3 en tant qu’utilisateur administratif. Pour plus d’informations, consultez l’article de la base de connaissances suivant :  
>   
> [Une application démarrée par un utilisateur non-administrateur ne peut pas écouter le trafic HTTP de l’ordinateur sur lequel l’application s’exécute dans Windows Vista, Windows Server 2003 ou Windows XP.](https://support.microsoft.com/kb/939786)

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

#### <a name="issue-the-relationships-button-is-disabled"></a>Problème : le bouton « relations » est désactivé

> Le bouton **relations** sous l’onglet **table** de l’espace de travail **bases de données** est désactivé pour les bases de données de SQL Server Compact.
> 
> **Solution de contournement**  
> Aucun. SQL Server Compact ne prend pas en charge les relations entre les tables.

#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problème : les requêtes SQL paramétrées lèvent des exceptions

> Dans SQL Server Compact 4,0, si vous ne spécifiez pas de type de données comme `SqlDbType` ou `DbType` pour les paramètres dans les requêtes paramétrables, une exception est levée lors de l’exécution de la requête.
> 
> **Solution de contournement**  
> Définissez explicitement le type de données pour les paramètres tels que `SqlDbType` ou `DbType`. Cela est essentiel dans le cas des types de données BLOB (`image` et `ntext`). Utilisez un code similaire à ce qui suit :
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]

<a id="More_Info"></a>

## <a name="for-more-information"></a>Pour plus d'informations

Pour plus d’informations sur WebMatrix bêta 3, consultez les sites Web suivants :

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

---

© 2010 Microsoft Corporation. Tous droits réservés. [Conditions d’utilisation](https://msdn.microsoft.cos/cc300389.aspx).
