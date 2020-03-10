---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notes de publication pour ASP.NET et Web Tools 2013,1 pour Visual Studio 2012 | Microsoft Docs
author: microsoft
description: Ce document décrit la version de ASP.NET et Web Tools 2013,1 pour Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 260af1018064d60d80cbb1002001f28c4daffffd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578441"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>ASP.NET et Web Tools 2013.1 pour Visual Studio 2012 - Notes de publication

par [Microsoft](https://github.com/microsoft)

> Ce document décrit la version de ASP.NET et Web Tools 2013,1 pour Visual Studio 2012.

## <a name="contents"></a>Contenu

- [Notes d’installation](#install)
- [Configuration logicielle requise](#requirements)
- Nouvelles fonctionnalités de ASP.NET et Web Tools 2013,1 pour Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Modèles](#templates)

        - [Modèle ASP.NET MVC 5](#mvc5template)
        - [Modèle API Web ASP.NET 2](#apitemplate)
        - [Modèles d’élément](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [Génération de modèles automatique ASP.NET](#scaffold)
    - [Éditeur Razor](#razor)
    - [NuGet 2.7](#nuget)
- Problèmes connus et modifications avec rupture

    - [Génération de modèles automatique ASP.NET](#issuescaffolding)

        - [Génération de modèles automatique MVC et API Web-erreur HTTP 404, introuvable](#404issue)
        - [Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un élément de génération de modèles automatique](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [L’affichage du fichier cshtml avec Browse avec ou F5 provoque une erreur de serveur](#browseissue)
        - [Réécriture d’URL et tilde (~)](#rewriteissue)
    - [Modèles](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Notes d'installation

[Installer](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET et Web Tools 2013,1 pour Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Configuration logicielle

Vous devez disposer de Visual Studio 2012 ou Visual Studio Express 2012 pour le Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nouvelles fonctionnalités de ASP.NET et Web Tools 2013,1 pour Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Lorsque vous générez des contrôleurs et des vues MVC 5, le balisage des vues utilise [bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Modèles

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Modèle ASP.NET MVC 5

Nous avons ajouté un nouveau modèle MVC 5. Il référence les derniers packages NuGet MVC 5, et vous pouvez utiliser la génération de modèles automatique pour ajouter des contrôleurs et des vues.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Modèle API Web ASP.NET 2

Nous avons ajouté un nouveau modèle Web API 2. Il fait référence aux derniers packages NuGet de l’API Web 2, et vous pouvez utiliser la génération de modèles automatique pour ajouter des contrôleurs et des vues.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Modèles d'élément

Nous avons ajouté de nouveaux modèles d’élément pour les vues MVC 5, les pages Web (Razor 3) et les contrôleurs Web API 2. Ils installent les packages NuGet associés dans le projet lors de l’ajout de nouveaux éléments.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Lors de la génération de modèles automatique d’un contrôleur MVC ou d’API Web à l’aide de Entity Framework, nous utilisons Framework 6. Pour plus d’informations sur Entity Framework, consultez l' [historique des versions Entity Framework](https://msdn.com/data/jj574253).

Vous pouvez également télécharger et installer le Entity Framework 6 Tools pour Visual Studio 2012. Consultez la [Entity Framework obtenir](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Génération de modèles automatique ASP.NET

La génération de modèles automatique ASP.NET est une infrastructure de génération de code pour les applications Web ASP.NET. Il facilite l’ajout de code réutilisable à votre projet qui interagit avec un modèle de données.

Dans les versions précédentes de Visual Studio, la génération de modèles automatique était limitée aux projets ASP.NET MVC. Avec cette mise à jour, vous pouvez désormais utiliser la génération de modèles automatique pour tous les projets ASP.NET, y compris Web Forms. Cette mise à jour ne prend pas en charge la génération de pages pour un projet de Web Forms, mais vous pouvez toujours utiliser la génération de modèles automatique avec Web Forms en ajoutant des dépendances MVC au projet. La prise en charge de la génération de pages pour Web Forms sera ajoutée dans une prochaine mise à jour.

Lors de l’utilisation de la génération de modèles automatique, nous garantissons que toutes les dépendances requises sont installées dans le projet. Par exemple, si vous démarrez avec un projet ASP.NET Web Forms et que vous utilisez ensuite la génération de modèles automatique pour ajouter un contrôleur d’API Web, les références et packages NuGet requis sont ajoutés automatiquement à votre projet.

Pour ajouter la génération de modèles automatique MVC à un projet Web Forms, ajoutez un **nouvel élément de génération de modèles** automatique et sélectionnez **dépendances MVC 5** dans la fenêtre de dialogue. Il existe deux options pour la génération de modèles automatique MVC ; Minimale et complète. Si vous sélectionnez minimal, seuls les packages NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet. Si vous sélectionnez l’option complète, les dépendances minimales sont ajoutées, ainsi que les fichiers de contenu requis pour un projet MVC.

La prise en charge de la génération de modèles automatique des contrôleurs asynchrones utilise les nouvelles fonctionnalités Async de Entity Framework 6.

Pour plus d’informations et des didacticiels, consultez [vue d’ensemble](../2013/aspnet-scaffolding-overview.md)de la génération de modèles automatique ASP.net. Ces didacticiels illustrent la génération de modèles automatique avec Visual Studio 2013, mais ils s’appliquent également à ASP.NET et Web Tools 2013,1 pour Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Éditeur Razor

Avec cette mise à jour, Visual Studio 2012 prend désormais en charge les outils/édition Razor 3.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2,7 comprend un ensemble complet de nouvelles fonctionnalités qui sont décrites en détail dans les [notes de publication de nuget 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Cette version de NuGet évite aux utilisateurs d’autoriser explicitement NuGet à restaurer les packages manquants. Lors de l’installation de NuGet 2,7, les utilisateurs consentent implicitement à restaurer automatiquement les packages manquants. Les utilisateurs peuvent explicitement refuser la restauration des packages à l’aide des paramètres NuGet dans Visual Studio. Cette modification simplifie le fonctionnement de la restauration des packages.

## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et modifications avec rupture

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>Génération de modèles automatique ASP.NET

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>Génération de modèles automatique MVC et API Web-erreur HTTP 404, introuvable

Si vous rencontrez une erreur lors de l’ajout d’un élément de génération de modèles automatique à un projet, il est possible que votre projet reste dans un état incohérent. Certaines des modifications apportées à la génération de modèles automatique seront annulées, mais d’autres modifications, telles que les packages NuGet installés, ne seront pas annulées. Si les modifications de configuration de routage sont annulées, les utilisateurs reçoivent une erreur HTTP 404 lors de la navigation vers des éléments de génération de modèles automatique.

Pour corriger cette erreur pour MVC, ajoutez un nouvel élément de génération de modèles automatique et sélectionnez dépendances MVC 5 (minimale ou complète). Ce processus ajoutera toutes les modifications requises à votre projet.

Pour corriger cette erreur pour l’API Web :

1. Ajoutez la classe WebApiConfig suivante à votre projet.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Configurez WebApiConfig. Register dans l’application\_méthode Start dans global. asax comme suit :

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un élément de génération de modèles automatique

Si Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un élément de génération de modèles automatique avec Entity Framework (par exemple, un contrôleur d’API Web 2 avec des actions, à l’aide d’Entity Framework), il est possible que Visual Studio Express n’ait pas pu charger l’image native d’un assembly dépend de System. Web. extensions.

Pour corriger ce problème, configurez Visual Studio Express pour qu’il fonctionne avec l’image MSIL de System. Web. extensions :

1. Ouvrez l’invite de commandes en mode administrateur.
2. Accédez à%ProgramFiles%\Microsoft Visual Studio 11.0 \ Common7\IDE ou% ProgramFiles (x86)% \ Microsoft Visual Studio 11.0 \ Common7\IDE (pour Windows 64 bits).
3. Ouvrez VWDExpress. exe. config dans un éditeur de texte.
4. Ajoutez la ligne suivante sous l’élément de configuration &lt;&gt;/&lt;Runtime&gt; :  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Redémarrez Visual Studio Express 2012 pour le Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a>L’affichage du fichier cshtml avec Browse avec ou F5 provoque une erreur de serveur

Quand vous créez un projet MVC 5 dans Visual Studio 2012 (ou que vous ouvrez dans Visual Studio 2012 un projet MVC 5 créé dans Visual Studio 2013) et que vous tentez d’afficher un fichier cshtml à l’aide de la fonction parcourir avec ou F5, vous recevez un message d’erreur indiquant que le serveur indique une **erreur de serveur dans l’application'/'** . Le serveur tente de naviguer jusqu’à `http://localhost:XXXX/Views/../XXXX.cshtml`

Pour résoudre ce problème, changez le paramètre d' **action de démarrage** de votre projet en **page spécifique**. Vous n’avez pas besoin de fournir une valeur pour la page.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Après avoir apporté cette modification, la sélection de la touche F5 permet de naviguer jusqu’à la racine de votre application (`http://localhost:XXXX`). Ce comportement est différent du comportement pour les projets MVC 5 dans Visual Studio 2013, où le paramètre de la **page active** lance la page ouverte.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Réécriture d’URL et tilde (~)

Après la mise à niveau vers ASP.NET Razor 3 ou ASP.NET MVC 5, la notation tilde (~) risque de ne plus fonctionner correctement si vous utilisez la réécriture d’URL. La réécriture d’URL affecte la notation tilde (~) dans les éléments HTML tels que &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;lien/&gt;, et par conséquent, le tilde n’est plus mappé au répertoire racine.

Par exemple, si vous réécrivez les demandes pour **ASP.net/content** à **ASP.net**, l’attribut href dans &lt;A href = "~/Content/"/&gt; se résout en **/content/content/** au lieu de **/** . Pour supprimer cette modification, vous pouvez définir le contexte **WasUrlRewritten IIS\_** sur false dans chaque page Web ou dans l' **application\_BeginRequest** dans global. asax.

<a id="templateissue"></a>
### <a name="templates"></a>Modèles

Quand vous créez des projets MVC ASP.NET avec Visual Studio 2012 sur Windows 8.1 ou Windows Server 2012 R2, Visual Studio affiche un message d’erreur indiquant « la configuration du Web [URL] pour ASP.NET 4,5 a échoué ».

![erreur de configuration](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Vous voyez cette erreur, car Visual Studio 2012 n’active pas la fonctionnalité ASP.NET 4,5 lorsqu’elle est installée sur ces versions de Windows. Pour activer ASP.NET 4,5, effectuez les étapes décrites dans [activer ou désactiver des fonctionnalités Windows](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![activer ou désactiver les fonctionnalités Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Vous pouvez également activer ASP.NET 4,5 via la ligne de commande.

1. Ouvrez l’invite de commandes en mode administrateur.
2. Exécutez la commande suivante pour activer ASP.NET 4,5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
