---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/06/2010
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 46d051a5eba6501cf36910b7674ce6400597de8a
ms.sourcegitcommit: 295cf898a4c87e264b0c35c7254b0fa4169f2278
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057014"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC

- [Vue d’ensemble](#overview)
- [Notes d’installation](#installation-notes)
- [Configuration logicielle requise](#software-requirements)
- [Documentation](#documentation)
- [Support](#support)
- [Mise à niveau d’un projet ASP.NET MVC 2 vers ASP.NET MVC 3 Tools Update](#upgrading)
- [Mise à jour des outils ASP.NET MVC 3 (12 avril 2011)](#tu-changes)

    - [La boîte de dialogue « Ajouter un contrôleur » peut désormais générer des contrôleurs d’échafaudages avec des vues et du code d’accès aux données](#tu-AddControllerDialog)
    - [Améliorations apportées à la boîte de dialogue « Nouveau projet ASP.NET MVC 3 »](#tu-ImprovementsNewDialogBox)
    - [Les modèles de projet incluent à présent Modernizr 1,7](#tu-Modernizr)
    - [Les modèles de projet incluent des versions mises à jour de jQuery, de jQuery UI et de jQuery validation](#tu-UpdatedJQuery)
    - [Les modèles de projet incluent désormais ADO.NET Entity Framework 4,1 en tant que package NuGet préinstallé](#tu-EF)
    - [Les modèles de projet incluent des bibliothèques JavaScript comme packages NuGet préinstallés](#tu-JavaScriptLibsNuget)
    - [Problèmes connus](#tu-KI)
- [ASP.NET MVC 3 RTM (13 janvier 2011)](#MVC3RTM)

    - [Modification : mise à jour de la version de jQuery UI sur 1.8.7](#RTM-1)
    - [Modification : le ModelMetadataProvider par défaut a été remplacé par DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Problème résolu : le collage d’une partie d’une expression Razor qui contient des espaces blancs entraîne sa contrepassation](#RTM-3)
    - [Problème résolu : le fait de renommer un fichier Razor ouvert dans l’éditeur désactive la coloration syntaxique et IntelliSense](#RTM-4)
    - [Problèmes connus](#RTM-KI)
    - [Modifications avec rupture](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 (10 décembre 2010)](#_Toc2)

    - [Les modèles de projet ont été modifiés pour inclure jQuery 1.4.4, jQuery validation 1,7 et jQuery UI 1.8.6 y UI 1.8.6](#_Toc2_1)
    - [Ajout de la classe « AdditionalMetadataAttribute »](#_Toc2_2)
    - [Amélioration de la génération de modèles automatique](#_Toc2_3)
    - [Ajout de la méthode html. RAW](#_Toc2_3)
    - [La propriété « Controller. ViewModel » et la propriété « View » ont été renommées en « ViewBag »](#_Toc2_4)
    - [La classe « ControllerSessionStateAttribute » a été renommée en « SessionStateAttribute »](#_Toc2_5)
    - [La propriété « Fields » de RemoteAttribute a été renommée en « AdditionalFields »](#_Toc2_6)
    - [« SkipRequestValidationAttribute » a été renommé en « AllowHtmlAttribute »](#_Toc2_7)
    - [Modification de la méthode « html. ValidationMessage » pour afficher le premier message d’erreur utile](#_Toc2_8)
    - [Correction de la déclaration de @model pour ne pas ajouter d’espace blanc au document](#_Toc2_9)
    - [Ajout de la propriété « FileExtensions » pour afficher les moteurs afin de prendre en charge les noms de fichiers spécifiques au moteur](#_Toc2_10)
    - [Correction du programme d’assistance « LabelFor » pour émettre la valeur correcte pour l’attribut « for »](#_Toc2_11)
    - [Correction de la méthode « RenderAction » pour fournir une précédence des valeurs explicites lors de la liaison de modèle](#_Toc2_12)
    - [Modifications avec rupture](#_Toc2_BC)
    - [Problèmes connus](#_Toc2_KI)
- [ASP.NET MVC 3 Release Candidate (9 novembre, 2010)](#TOC_ASP_NET_3_RC)

    - [Nouvelles fonctionnalités dans ASP.NET MVC 3 RC](#_Toc276711785)
    - [Gestionnaire de package NuGet](#_Toc276711786)
    - [Amélioration de la boîte de dialogue « Nouveau projet »](#_Toc276711787)
    - [Contrôleurs de session](#_Toc276711788)
    - [Nouveaux attributs de validation](#_Toc276711789)
    - [Nouvelles surcharges pour les méthodes « LabelFor » et « LabelForModel »](#_Toc276711790)
    - [Mise en cache de sortie des actions enfants](#_Toc276711791)
    - [Améliorations de la boîte de dialogue « Ajouter une vue »](#_Toc276711792)
    - [Validation granulaire des demandes](#_Toc276711793)
    - [Modifications avec rupture](#_Toc276711794)
    - [Problèmes connus](#_Toc276711795)
- [ASP. Notes bêta de MVC 3 (6 octobre 2010)](#TOC_ASP_NET_3_Beta)

    - [Nouvelles fonctionnalités dans ASP.NET MVC 3 Beta](#0.1__Toc274034215)
    - [Gestionnaire de package NuPack](#0.1__Toc274034216)
    - [Boîte de dialogue Nouveau projet amélioré](#0.1__Toc274034217)
    - [Méthode simplifiée pour spécifier des modèles fortement typés dans les vues Razor](#0.1__Toc274034218)
    - [Prise en charge des nouvelles méthodes d’assistance pages Web ASP.NET](#0.1__Toc274034219)
    - [Prise en charge supplémentaire de l’injection de dépendance](#0.1__Toc274034220)
    - [Nouvelle prise en charge d’Ajax basé sur jQuery discrète](#0.1__Toc274034221)
    - [Nouvelle prise en charge de la validation jQuery discrète](#0.1__Toc274034222)
    - [Nouveaux indicateurs à l’ensemble de l’application pour la validation du client et JavaScript discret](#0.1__Toc274034223)
    - [Nouvelle prise en charge du code qui s’exécute avant l’exécution des vues](#0.1__Toc274034224)
    - [Nouvelle prise en charge de la syntaxe Razor VBHTML](#0.1__Toc274034225)
    - [Contrôle plus granulaire sur ValidateInputAttribute](#0.1__Toc274034226)
    - [Les applications auxiliaires convertissent les traits de soulignement en tirets pour les noms d’attributs HTML spécifiés à l’aide d’objets anonymes](#0.1__Toc274034227)
    - [Résolutions de bogues](#0.1__Toc274034228)
    - [Modifications avec rupture](#0.1__Toc274034229)
    - [Problèmes connus](#0.1__Toc274034230)
- [AVERTISSEMENT](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Vue d'ensemble

Ce document décrit la version de ASP.NET MVC 3 RTM pour Visual Studio 2010. ASP.NET MVC est une infrastructure de développement d’applications Web qui utilise le modèle MVC (Model-View-Controller). Le programme d’installation de ASP.NET MVC 3 comprend les composants suivants :

- Composants d’exécution ASP.NET MVC 3
- Outils ASP.NET MVC 3 Visual Studio 2010
- Composants pages Web ASP.NET Runtime
- Pages Web ASP.NET Visual Studio 2010 Tools
- Gestionnaire de package Microsoft pour .NET (NuGet)
- Une mise à jour pour Visual Studio 2010 qui permet la prise en charge de syntaxe Razor. (Pour plus d’informations, consultez l’article 2483190 de la base de connaissances.)

L’ensemble complet des notes de publication de chaque version préliminaire de ASP.NET MVC 3 se trouve sur le site Web ASP.NET à l’adresse suivante :

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Notes d’installation

Pour installer ASP.NET MVC 3 RTM à l’aide du Web Platform Installer (Web PI), visitez la page suivante :

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Vous pouvez également télécharger le programme d’installation de ASP.NET MVC 3 RTM pour Visual Studio 2010 à partir de la page suivante :

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 peut être installé et peut s’exécuter côte à côte avec ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Configuration logicielle

Les composants d’exécution ASP.NET MVC 3 requièrent les logiciels suivants :

- .NET Framework version 4. 

    Les outils ASP.NET MVC 3 Visual Studio 2010 requièrent les logiciels suivants :
- Visual Studio 2010 ou Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Documentation

La documentation relative à ASP.NET MVC est disponible sur le site Web MSDN à l’adresse suivante :

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Des didacticiels et d’autres informations sur ASP.NET MVC sont disponibles sur la page MVC du site Web ASP.NET à l’adresse suivante :

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Assistance

Il s’agit d’une version entièrement prise en charge. Pour plus d’informations sur l’obtention d’un support technique, consultez le [site web support Microsoft](https://support.microsoft.com/).

N’hésitez pas à poser des questions sur cette version sur le Forum ASP.NET MVC, où les membres de la communauté ASP.NET sont souvent en mesure de fournir un support informel :

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Mise à niveau d’un projet ASP.NET MVC 2 vers ASP.NET MVC 3 Tools Update

ASP.NET MVC 3 peut être installé côte à côte avec ASP.NET MVC 2 sur le même ordinateur, ce qui vous donne la possibilité de choisir quand mettre à niveau une application ASP.NET MVC 2 vers ASP.NET MVC 3.

Pour mettre à niveau manuellement une application ASP.NET MVC 2 existante vers la version 3, procédez comme suit :

1. Créez un nouveau projet ASP.NET MVC 3 vide sur votre ordinateur. Ce projet contient certains fichiers nécessaires à la mise à niveau.
2. Copiez les fichiers suivants du projet ASP.NET MVC 3 dans l’emplacement correspondant de votre projet ASP.NET MVC 2. Vous devez mettre à jour toutes les références à la bibliothèque jQuery pour prendre en compte le nouveau nom de fichier (jQuery-1.5.1. js) : 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*. js
    - /Content/themes/\*.\*
3. Copiez le dossier *packages* à la racine de la solution de projet ASP.NET MVC 3 vide dans la racine de votre solution, qui se trouve dans le répertoire où se trouve le fichier. sln de la solution.
4. Si votre projet ASP.NET MVC 2 contient des zones, copiez le fichier/Views/Web.config dans le dossier *views* de chaque zone.
5. Dans les deux fichiers Web. config du projet ASP.NET MVC 2, recherchez et remplacez globalement la version de ASP.NET MVC. Recherchez les éléments suivants : 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Remplacez-le par ce qui suit :

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. Dans Explorateur de solutions, supprimez la référence à *System. Web. Mvc* (qui pointe vers la dll de la version 2), puis ajoutez une référence à *System. Web. Mvc* (v 3.0.0.0).
7. Ajoutez une référence à System. Web. webpages. dll et System. Web. helpers. dll. Ces assemblys se trouvent dans les dossiers suivants : 

    - % ProgramFiles% \ Assemblys Microsoft ASP. NET\ASP.NET MVC 3 \
    - % ProgramFiles% \ Pages\v1.0\Assemblies Web Microsoft ASP. NET\ASP.NET
8. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet, puis sélectionnez décharger le projet. Cliquez ensuite avec le bouton droit sur le nom du projet, puis sélectionnez Modifier *NomProjet*. csproj.
9. Recherchez l’élément *ProjectTypeGuids* et remplacez {F85E285D-A4E0-4152-9332-AB1D724D3325} par {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Enregistrez les modifications, cliquez avec le bouton droit sur le projet, puis sélectionnez recharger le projet.
11. Dans le fichier Web. config racine de l’application, ajoutez les paramètres suivants à la section *Assemblies* . 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Si le projet fait référence à des bibliothèques tierces qui sont compilées à l’aide de ASP.NET MVC 2, ajoutez l’élément *bindingRedirect* en surbrillance suivant au fichier Web. config dans la racine de l’application, sous la section de *configuration* : 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Modifications apportées à la mise à jour des outils ASP.NET MVC 3

Cette section décrit les modifications apportées à la version de mise à jour des outils ASP.NET MVC 3 depuis la version RTM ASP.NET MVC 3.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>La boîte de dialogue « Ajouter un contrôleur » peut désormais générer des contrôleurs d’échafaudages avec des vues et du code d’accès aux données

La génération de modèles automatique est un moyen de générer rapidement un contrôleur et des vues pour votre application. Une fois que le code a été généré, vous pouvez le modifier en fonction des exigences de votre projet.

Pour lancer la boîte de dialogue *Ajouter un contrôleur* dans ASP.NET MVC 3, cliquez avec le bouton droit sur le dossier *Controllers* dans *Explorateur de solutions*, cliquez sur *Ajouter*, puis sur *contrôleur*. La boîte de dialogue a été améliorée pour offrir des options d’échafaudage supplémentaires.

![](mvc3-release-notes/_static/image1.png)

Trois modèles de génération de modèles automatique sont disponibles par défaut.

#### <a name="empty-controller"></a>Contrôleur vide

Ce modèle génère un fichier de contrôleur vide. Ce modèle équivaut à ne pas cocher *Ajouter des actions pour créer, modifier, détails et supprimer des scénarios dans les* versions précédentes de ASP.NET MVC. Si vous choisissez cette option, aucune autre option n’est disponible.

#### <a name="controller-with-empty-readwrite-actions"></a>Contrôleur avec des actions de lecture/écriture vides

Ce modèle génère un fichier de contrôleur qui contient toutes les méthodes d’action requises, mais aucun code d’implémentation dans les méthodes. Ce modèle équivaut à cocher *Ajouter des actions pour créer, modifier, détails, supprimer des scénarios dans les* versions précédentes de ASP.NET MVC. Si vous choisissez cette option, aucune autre option n’est disponible.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Contrôleur avec des actions et des vues en lecture/écriture, à l’aide de Entity Framework

Ce modèle vous permet de créer rapidement une interface utilisateur de saisie de données de travail. Il génère du code qui gère une série de scénarios et exigences courants, tels que les suivants :

- *Accès aux données*. Le code généré lit et écrit des entités dans une base de données. Elle fonctionne avec l’approche Entity Framework Code First si vous choisissez une classe de contexte de données existante ou si vous laissez le modèle générer une nouvelle classe *DbContext* . Elle fonctionne également avec l’approche Entity Framework Database First ou Model First si vous choisissez une classe *ObjectContext* existante.
- *Validation*. Le code généré utilise des fonctionnalités de métadonnées et de liaison de modèle ASP.NET MVC afin que les envois de formulaire soient validés conformément aux règles déclarées dans votre classe de modèle. Cela comprend des règles de validation intégrées, telles que les attributs *requis* et *StringLength* , ainsi que des règles de validation personnalisées.
- *Relations un-à-plusieurs*. Si vous définissez des relations de clé étrangère un-à-plusieurs entre vos classes de modèle, le code généré produira des listes déroulantes pour sélectionner les entités associées. Par exemple, vous pouvez définir les classes de modèle suivantes en Entity Framework Code First conventions : 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Lorsque vous générez automatiquement un modèle de contrôleur pour la classe *Product* , ses vues permettent aux utilisateurs de choisir un objet *Category* pour chaque instance de *produit* .

    Ce modèle active des options supplémentaires dans la boîte de dialogue *Ajouter un contrôleur* . Pour la *classe de modèle*, vous pouvez choisir n’importe quelle classe de modèle dans votre solution, qui détermine le type de données que les utilisateurs seront en mesure de créer ou de modifier :
- Si vous souhaitez utiliser Entity Framework Code First, vous pouvez choisir n’importe quelle classe de modèle.
- Si vous utilisez Entity Framework Database First ou Entity Framework Model First, veillez à choisir une classe d’entité définie dans votre modèle conceptuel.

Pour la *classe de contexte de données*, vous pouvez effectuer les choix suivants :

- Si vous souhaitez utiliser Code First et que vous n’avez pas de classe de contexte de données existante, choisissez * * nouveau contexte de données * *. Une classe de contexte de données sera alors générée automatiquement.
- Si vous souhaitez utiliser Code First et que vous avez une classe de contexte de données existante, choisissez-la ici. Il sera mis à jour pour conserver la classe de modèle que vous avez sélectionnée.
- Si vous utilisez Database First ou Model First, choisissez votre classe de contexte d’objet ici.

Pour les affichages, choisissez le moteur d’affichage que vous souhaitez utiliser ou cliquez sur aucun si vous ne souhaitez pas générer de structure pour les vues.

Vous pouvez sélectionner des consulter avancés pour spécifier des options supplémentaires pour les vues générées. Par exemple, vous pouvez choisir la disposition ou la page maître à utiliser.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Améliorations apportées à la boîte de dialogue « Nouveau projet ASP.NET MVC 3 »

La boîte de dialogue que vous utilisez pour créer de nouveaux projets ASP.NET MVC 3 comprend plusieurs améliorations, comme indiqué ci-dessous.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Nouveau modèle « projet intranet »

La liste des modèles de projet comprend un nouveau modèle d’application intranet. Ce modèle contient des paramètres pour la génération d’une application Web à l’aide de l’authentification Windows au lieu de l’authentification par formulaire. Étant donné qu’une application intranet requiert certains paramètres IIS qui ne peuvent pas être encapsulés dans un modèle de projet, le modèle comprend un fichier Readme contenant des instructions sur la façon de faire fonctionner le modèle de projet dans IIS. La documentation du nouveau modèle d’application intranet est disponible sur le site Web MSDN à l’adresse suivante :

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Les modèles de projet sont désormais HTML5 activés

La boîte de dialogue Nouveau projet contient désormais une option permettant d’ajouter des fonctionnalités propres à HTML5 aux modèles de projet. La sélection de l’option entraîne la génération de vues qui contiennent les nouveaux éléments HTML5 `<header>`, `<footer>`et `<navigation>`.

Notez que les versions antérieures des navigateurs ne prennent pas en charge les balises spécifiques à HTML5. Pour répondre à cette limitation, les modèles de projet HTML5 incluent une référence à la bibliothèque Modernizr. (Voir la section suivante.)

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Les modèles de projet incluent à présent Modernizr 1,7

Modernizr est une bibliothèque JavaScript qui permet la prise en charge de CSS 3 et HTML5 dans les navigateurs qui ne prennent pas encore en charge ces fonctionnalités. Cette bibliothèque est incluse en tant que package NuGet préinstallé dans les modèles pour les projets ASP.NET MVC 3. Pour plus d’informations sur Modernizr, consultez [http://www.modernizr.com/](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Les modèles de projet incluent des versions mises à jour de jQuery, de jQuery UI et de jQuery validation

Les modèles de projet incluent désormais les versions suivantes des scripts jQuery :

- jQuery 1.5.1
- jQuery validation 1,8
- jQuery UI 1.8.11

Ces bibliothèques sont incluses en tant que packages NuGet préinstallés.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Les modèles de projet incluent désormais ADO.NET Entity Framework 4,1 en tant que package NuGet préinstallé

ADO.NET Entity Framework 4,1 comprend la fonctionnalité Code First. Code First est un nouveau modèle de développement pour le Entity Framework ADO.NET qui fournit une alternative aux modèles Database First et Model First existants.

Code First est axé sur la définition de votre modèle à l’aide de classes POCO (« Plain Old CLR Objects C#») écrites en Visual Basic ou. Ces classes peuvent ensuite être mappées à une base de données existante ou être utilisées pour générer un schéma de base de données. Une configuration supplémentaire peut être fournie à l’aide des attributs *DataAnnotations* ou à l’aide des API Fluent.

La documentation relative à l’utilisation de code Firstwith ASP.NET MVC est disponible sur le site Web ASP.NET à l’adresse URL suivante :

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Les modèles de projet incluent des bibliothèques JavaScript comme packages NuGet préinstallés

Lorsque vous créez un nouveau projet ASP.NET MVC 3, le projet comprend les fichiers JavaScript mentionnés précédemment (par exemple, la bibliothèque Modernizr) en les installant à l’aide de NuGet au lieu d’ajouter directement les scripts au dossier scripts dans le modèle de projet. matières. Cela vous permet d’utiliser NuGet pour mettre à jour les scripts avec la dernière version lors de la publication de nouvelles versions des scripts.

Par exemple, étant donné la fréquence des nouvelles versions jQuery, la version de jQuery incluse dans le modèle de projet sera à un moment donné obsolète. Toutefois, étant donné que jQuery est inclus en tant que package NuGet installé, vous êtes averti dans la boîte de dialogue NuGet lorsque des versions plus récentes de jQuery sont disponibles.

Étant donné que jQuery comprend le numéro de version dans le nom de fichier, la mise à jour de jQuery vers la dernière version nécessite également la mise à jour de la balise `<script>` qui fait référence au fichier jQuery pour utiliser le nouveau nom de fichier. Les autres bibliothèques de scripts incluses n’incluent pas le numéro de version dans le nom du script. elles peuvent donc être plus facilement mises à jour vers leurs versions les plus récentes.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Problèmes connus

- Dans certains cas, l’installation peut échouer avec le message d’erreur « Échec de l’installation avec le code d’erreur (0x80070643) ». Pour plus d’informations sur la façon de contourner ce problème, consultez [l’article 2531566](https://support.microsoft.com/kb/2531566)de la base de connaissances.
- La génération de modèles automatique pour l’ajout d’un contrôleur n’effectue pas l’échafaudage des entités qui tirent parti de la prise en charge de l’héritage d’entité dans Entity Framework Par exemple, pour une classe *Person* de base héritée par une classe *Student* , la génération de modèles automatique de la classe *Student* génère du code qui ne se compile pas.
- La création d’un nouveau projet ASP.NET MVC 3 dans un dossier de solution provoque une erreur *NullReferenceException* . La solution consiste à créer le projet ASP.NET MVC 3 à la racine de la solution, puis à le déplacer dans le dossier de solution.
- IntelliSense pour syntaxe Razor ne fonctionne pas quand resharper est installé. Si resharper est installé et que vous souhaitez tirer parti de la prise en charge de Razor IntelliSense dans ASP.NET MVC 3, consultez l’entrée [Razor IntelliSense et resharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) sur le blog de hadi Hariri, qui explique comment les utiliser ensemble aujourd’hui.
- Pendant l’installation, la boîte de dialogue acceptation du CLUF affiche les termes du contrat de licence dans une fenêtre plus petite que prévu.
- Lorsque vous modifiez une vue Razor (. cshtml ou. *fichier vbhtml* ), vues. ASP.NET MVC 3 n’inclut aucun extrait de code pour les vues Razor. aspxselecting un extrait de code pour ASP.NET MVC affiche des extraits de code pour
- Si vous installez ASP.NET MVC 3 pour Visual Web Developer Express sur un ordinateur sur lequel Visual Studio n’est pas installé, puis installez Visual Studio ultérieurement, vous devez réinstaller ASP.NET MVC 3. Visual Studio et Visual Web Developer Express partagent des composants qui sont mis à niveau par le programme d’installation de ASP.NET MVC 3. Le même problème s’applique si vous installez ASP.NET MVC 3 pour Visual Studio sur un ordinateur sur lequel Visual Web Developer Express n’est pas installé et que vous installez ensuite Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Modifications dans ASP.NET MVC 3 RTM

Cette section décrit les modifications et les correctifs de bogues apportés dans la version de ASP.NET MVC 3 RTM depuis la version RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Modification : mise à jour de la version de jQuery UI sur 1.8.7

Les modèles de projet MVC ASP.NET pour Visual Studio ont été mis à jour pour inclure la dernière version de la bibliothèque de l’interface utilisateur jQuery. Les modèles incluent également l’ensemble minimal de fichiers de ressources requis par jQuery UI, tels que les fichiers d’image et CSS associés.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Modification : le ModelMetadataProvider par défaut a été remplacé par DataAnnotationsModelMetadataProvider

La version RC2 de ASP.NET MVC 3 a introduit une classe *CachedDataAnnotationsMetadataProvider* qui offrait une mise en cache au-dessus de la classe *DataAnnotationsModelMetadataProvider* existante pour améliorer les performances. Toutefois, certains bogues ont été signalés avec cette implémentation, ce qui signifie que la modification a été annulée et déplacée dans le projet MVC futures, qui est disponible sur [ASP.net webstack](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Problème résolu : le collage d’une partie d’une expression Razor qui contient des espaces blancs entraîne sa contrepassation

Dans les versions préliminaires de ASP.NET MVC 3, lorsque vous collez une partie d’une expression Razor qui contient un espace blanc dans un fichier Razor, l’expression résultante est inversée. Par exemple, considérez le bloc de code Razor suivant :

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Si vous sélectionnez le texte « First param » dans la première méthode et que vous le collez comme argument dans la seconde méthode, le résultat est le suivant :

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Le comportement correct est que l’opération de collage doit aboutir à ce qui suit :

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Ce problème a été résolu dans la version RTM afin que l’expression soit conservée correctement pendant l’opération de collage.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Problème résolu : le fait de renommer un fichier Razor ouvert dans l’éditeur désactive la coloration syntaxique et IntelliSense

Si vous renommez un fichier Razor à l’aide d’Explorateur de solutions alors que le fichier est ouvert dans la fenêtre de l’éditeur, la mise en surbrillance de la syntaxe et IntelliSense cessent de fonctionner pour ce fichier. Cela a été corrigé de façon à ce que la mise en surbrillance et la fonctionnalité IntelliSense soient conservées après un changement de nom.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Problèmes connus

- Si vous fermez Visual Studio 2010 SP1 bêta alors que la console du gestionnaire de package NuGet est ouverte, Visual Studio se bloque et tente de redémarrer. Ce problème sera résolu dans la version RTM de Visual Studio 2010 SP1.
- Le programme d’installation de ASP.NET MVC 3 ne peut installer qu’une version initiale du gestionnaire de package NuGet. Une fois la version initiale installée, NuGet peut être installé et mis à jour à l’aide du gestionnaire d’extensions Visual Studio. Si vous avez déjà installé NuGet, accédez à la Galerie d’extensions de Visual Studio pour effectuer une mise à jour vers la dernière version de NuGet.
- La création d’un nouveau projet ASP.NET MVC 3 dans un dossier de solution provoque une erreur *NullReferenceException* . La solution consiste à créer le projet ASP.NET MVC 3 à la racine de la solution, puis à le déplacer dans le dossier de solution.
- Le programme d’installation peut prendre plus de temps que les versions précédentes de ASP.NET MVC pour s’exécuter. Cela est dû au fait qu’il met à jour les composants de Visual Studio 2010.
- IntelliSense pour syntaxe Razor ne fonctionne pas quand resharper est installé. Si resharper est installé et que vous souhaitez tirer parti de la prise en charge de Razor IntelliSense dans ASP.NET MVC 3, consultez l’entrée [Razor IntelliSense et resharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) sur le blog de hadi Hariri, qui explique comment les utiliser ensemble aujourd’hui.
- Les vues CCSHTML et VBHTML créées avec la version bêta de ASP.NET MVC 3 n’ont pas leur action de génération correctement définie, avec pour résultat que ces types d’affichages sont omis lorsque le projet est publié. La valeur de l’action de génération pour ces fichiers doit être définie sur « contenu ». ASP.NET MVC 3 RTM corrige ce problème pour les nouveaux fichiers, mais ne corrige pas le paramètre pour les fichiers existants d’un projet créé avec les versions préliminaires.
- ![](mvc3-release-notes/_static/image3.png)
- Pendant l’installation, la boîte de dialogue acceptation du CLUF affiche les termes du contrat de licence dans une fenêtre plus petite que prévu.
- Lorsque vous modifiez une vue Razor (fichier. cshtml), l’élément de menu atteindre le contrôleur dans Visual Studio n’est pas disponible, et il n’existe aucun extrait de code.
- Si vous installez ASP.NET MVC 3 pour Visual Web Developer Express sur un ordinateur sur lequel Visual Studio n’est pas installé, puis installez Visual Studio ultérieurement, vous devez réinstaller ASP.NET MVC 3. Visual Studio et Visual Web Developer Express partagent des composants qui sont mis à niveau par le programme d’installation de ASP.NET MVC 3. Le même problème s’applique si vous installez ASP.NET MVC 3 pour Visual Studio sur un ordinateur sur lequel Visual Web Developer Express n’est pas installé et que vous installez ensuite Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Modifications avec rupture

- Dans les versions précédentes de ASP.NET MVC, les filtres d’action sont créés par requête, sauf dans quelques cas. Ce comportement n’a jamais été garanti, mais simplement un détail d’implémentation et le contrat pour les filtres était de les considérer comme sans État. Dans ASP.NET MVC 3, les filtres sont mis en cache de manière plus agressive. Par conséquent, les filtres d’action personnalisés qui stockent de manière incorrecte l’état de l’instance peuvent être rompus.
- L’ordre d’exécution des filtres d’exception a changé pour les filtres d’exception qui ont la même valeur d' *ordre* . Dans ASP.NET MVC 2 et versions antérieures, les filtres d’exception sur le contrôleur qui ont la même valeur d' *ordre* que ceux sur une méthode d’action sont exécutés avant les filtres d’exception sur la méthode d’action. C’est généralement le cas lorsque des filtres d’exception sont appliqués sans une valeur d' *ordre* spécifiée. Dans ASP.NET MVC 3, cet ordre a été inversé afin que le gestionnaire d’exceptions le plus spécifique s’exécute en premier. Comme dans les versions antérieures, si la propriété *Order* est explicitement spécifiée, les filtres sont exécutés dans l’ordre spécifié.
- Une nouvelle propriété nommée *FileExtensions* a été ajoutée à la classe de base *VirtualPathProviderViewEngine* . Quand ASP.NET recherche une vue par chemin d’accès (et non par nom), seules les vues avec une extension de fichier figurant dans la liste spécifiée par cette nouvelle propriété sont prises en compte. Il s’agit d’une modification avec rupture dans les applications où un fournisseur de générations personnalisé est inscrit afin d’activer une extension de fichier personnalisée pour les vues Web Form et dans laquelle le fournisseur fait référence à ces vues à l’aide d’un chemin d’accès complet plutôt que d’un nom. La solution de contournement consiste à modifier la valeur de la propriété *FileExtensions* pour inclure l’extension de fichier personnalisée.
- Les implémentations de fabrique de contrôleurs personnalisées qui implémentent directement l’interface *IControllerFactory* doivent fournir une implémentation de la nouvelle méthode *GetControllerSessionBehavior* qui a été ajoutée à l’interface dans cette version. En général, il est recommandé de ne pas implémenter cette interface directement et de dériver plutôt votre classe de *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Modifications dans ASP.NET MVC 3 RC2

Cette section décrit les modifications (nouvelles fonctionnalités et correctifs de bogues) effectuées dans la version ASP.NET MVC 3 RC2 depuis la version RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Les modèles de projet ont été modifiés pour inclure jQuery 1.4.4, jQuery validation 1,7 et jQuery UI 1.8.6

Les modèles de projet pour ASP.NET MVC 3 incluent désormais les versions les plus récentes de jQuery, de jQuery validation et de jQuery UI. jQuery UI est un nouvel ajout aux modèles de projet et fournit des widgets d’interface utilisateur utiles. Pour plus d’informations sur l’interface utilisateur jQuery, visitez la page d’accueil suivante : [http://jqueryui.com/](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Ajout de la classe « AdditionalMetadataAttribute »

Vous pouvez utiliser la classe *AdditionalMetadataAttribute* pour remplir le dictionnaire *ModelMetadata. additionalValues* pour une propriété de modèle.

Par exemple, supposons qu’un modèle de vue possède des propriétés qui doivent être affichées uniquement par un administrateur. Ce modèle peut être annoté avec le nouvel attribut à l’aide de AdminOnly comme clé et True comme valeur, comme dans l’exemple suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Ces métadonnées sont mises à la disposition de tout modèle d’affichage ou d’éditeur lorsqu’un modèle de vue de produit est rendu. C’est à vous, en tant que développeur d’applications, d’interpréter les informations de métadonnées.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Amélioration de la génération de modèles automatique

Les modèles T4 utilisés pour les vues de génération de modèles automatique génèrent désormais des appels aux méthodes d’assistance de modèle, telles que *EditorFor* , au lieu d’applications auxiliaires telles que *TextBoxFor*. Cette modification améliore la prise en charge des métadonnées sur le modèle sous forme d’attributs d’annotation de données lorsque la boîte de dialogue Ajouter une vue génère une vue.

L’ajout de la génération de modèles automatique de vue comprend également une détection et une utilisation améliorées des informations de clé primaire sur le modèle, en fonction de la Convention. Par exemple, la boîte de dialogue Ajouter une vue utilise ces informations pour s’assurer que la valeur de clé primaire n’est pas échafaudée en tant que champ de formulaire modifiable.

Les modèles de modification et de création par défaut incluent des références aux scripts jQuery nécessaires à la validation du client.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Ajout de la méthode html. RAW

Par défaut, le moteur d’affichage Razor encode toutes les valeurs. Par exemple, l’extrait de code suivant encode le code HTML à l’intérieur de la variable Greeting afin qu’il s’affiche dans la page comme `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

La nouvelle méthode *html. RAW* offre un moyen simple d’afficher du code HTML non encodé lorsque le contenu est reconnu comme sécurisé. L’exemple suivant affiche la même chaîne, mais la chaîne est restituée en tant que balisage :

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>La propriété « Controller. ViewModel » et la propriété « View » ont été renommées en « ViewBag »

Auparavant, la propriété *ViewModel* du *contrôleur* correspond à la propriété *View* d’une vue. Ces deux propriétés offrent un moyen d’accéder aux valeurs de l’objet *ViewDataDictionary* à l’aide de la syntaxe d’accesseur de propriété dynamique. Les deux propriétés ont été renommées pour être identiques afin d’éviter toute confusion et d’être plus cohérentes.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>La classe « ControllerSessionStateAttribute » a été renommée en « SessionStateAttribute »

La classe *ControllerSessionStateAttribute* a été introduite dans la version RC de ASP.NET MVC 3. La propriété a été renommée pour être plus concise.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>La propriété « Fields » de RemoteAttribute a été renommée en « AdditionalFields »

La propriété des *champs* de la classe *RemoteAttribute* a provoqué une certaine confusion parmi les utilisateurs. L’attribution d’un nouveau nom à cette propriété *AdditionalFields* clarifie son intention.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>« SkipRequestValidationAttribute » a été renommé en « AllowHtmlAttribute »

L’attribut *SkipRequestValidationAttribute* a été renommé *AllowHtmlAttribute* afin de mieux représenter son utilisation prévue.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Modification de la méthode « html. ValidationMessage » pour afficher le premier message d’erreur utile

La méthode *html. ValidationMessage* a été corrigée pour afficher le premier message d’erreur utile au lieu d’afficher simplement la première erreur.

Pendant la liaison de modèle, le dictionnaire *ModelState* peut être rempli à partir de plusieurs sources avec des messages d’erreur sur la propriété, y compris du modèle lui-même (s’il implémente *IValidatableObject*), des attributs de validation appliqués à la propriété et des exceptions levées pendant l’accès à la propriété.

Lorsque la méthode *html. ValidationMessage* affiche un message de validation, elle ignore les entrées d’état de modèle qui incluent une exception, car celles-ci ne sont généralement pas destinées à l’utilisateur final. Au lieu de cela, la méthode recherche le premier message de validation qui n’est pas associé à une exception et affiche ce message. Si aucun message de ce type n’est trouvé, il s’agit par défaut d’un message d’erreur générique associé à la première exception.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Correction de la déclaration de @model pour ne pas ajouter d’espace blanc au document

Dans les versions antérieures, la déclaration de `@model` en haut d’une vue ajoutait une ligne vide à la sortie HTML rendue. Cela a été résolu afin que la déclaration n’introduise pas d’espace blanc.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Ajout de la propriété « FileExtensions » pour afficher les moteurs afin de prendre en charge les noms de fichiers spécifiques au moteur

Un moteur d’affichage peut retourner une vue à l’aide d’un chemin d’accès de vue explicite, comme dans l’exemple suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

Le premier moteur d’affichage tente toujours de restituer la vue. Par défaut, le moteur d’affichage Web Forms est le premier moteur d’affichage ; étant donné que le moteur de Web Forms ne peut pas restituer une vue Razor, une erreur se produit. Les moteurs d’affichage ont maintenant une propriété *FileExtensions* qui est utilisée pour spécifier les extensions de fichier qu’ils prennent en charge. Cette propriété est vérifiée lorsque ASP.NET détermine si un moteur d’affichage peut restituer un fichier. Il s’agit d’une modification avec rupture et d’autres détails sont inclus dans la section [modifications avec rupture](#_Toc2_BC) de ce document.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Correction du programme d’assistance « LabelFor » pour émettre la valeur correcte pour l’attribut « for »

Un bogue a été résolu lorsque la méthode *LabelFor* rend un attribut *for* qui correspond à l’attribut *Name* de l’élément *Input* au lieu de son ID. Selon le W3C, l’attribut *for* doit correspondre à l’ID de l’élément *Input* .

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Correction de la méthode « RenderAction » pour fournir une précédence des valeurs explicites lors de la liaison de modèle

Dans les versions antérieures, les valeurs explicites qui ont été passées à la méthode *RenderAction* étaient ignorées au profit des valeurs de formulaire actuelles pendant la liaison de modèle à l’intérieur d’une action enfant. Le correctif garantit que les valeurs explicites sont prioritaires lors de la liaison de modèle.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Modifications avec rupture

- Dans les versions précédentes de ASP.NET MVC, les filtres d’action étaient créés par requête, sauf dans certains cas. Ce comportement n’a jamais été garanti, mais simplement un détail d’implémentation et le contrat pour les filtres était de les considérer comme sans État. Dans ASP.NET MVC 3, les filtres sont mis en cache de manière plus agressive. Par conséquent, les filtres d’action personnalisés qui stockent de manière incorrecte l’état de l’instance peuvent être rompus.
- L’ordre d’exécution des filtres d’exception a changé pour les filtres d’exception qui ont la même valeur d' *ordre* . Dans ASP.NET MVC 2 et versions antérieures, les filtres d’exception sur le contrôleur ayant la même valeur d' *ordre* que ceux sur une méthode d’action ont été exécutés avant les filtres d’exception sur la méthode d’action. C’est généralement le cas lorsque des filtres d’exception ont été appliqués sans valeur d' *ordre* spécifiée. Dans ASP.NET MVC 3, cet ordre a été inversé afin que le gestionnaire d’exceptions le plus spécifique s’exécute en premier. Comme dans les versions antérieures, si la propriété *Order* est explicitement spécifiée, les filtres sont exécutés dans l’ordre spécifié.
- Une nouvelle propriété nommée *FileExtensions* a été ajoutée à la classe de base *VirtualPathProviderViewEngine* . Quand ASP.NET recherche une vue par chemin d’accès (et non par nom), seules les vues avec une extension de fichier figurant dans la liste spécifiée par cette nouvelle propriété sont prises en compte. Il s’agit d’une modification avec rupture dans les applications où un fournisseur de générations personnalisé est inscrit afin d’activer une extension de fichier personnalisée pour les vues Web Form et dans laquelle le fournisseur fait référence à ces vues à l’aide d’un chemin d’accès complet plutôt que d’un nom. La solution de contournement consiste à modifier la valeur de la propriété *FileExtensions* pour inclure l’extension de fichier personnalisée.
- Les implémentations de fabrique de contrôleurs personnalisées qui implémentent directement l’interface *IControllerFactory* doivent fournir une implémentation de la nouvelle méthode *GetControllerSessionBehavior* qui a été ajoutée à l’interface dans cette version. En général, il est recommandé de ne pas implémenter cette interface directement et de dériver plutôt votre classe de *DefaultControllerFactory*.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Problèmes connus

- Le programme d’installation de ASP.NET MVC 3 ne peut installer qu’une version initiale du gestionnaire de package NuGet. Une fois la version initiale installée, NuGet peut être installé et mis à jour à l’aide du gestionnaire d’extensions Visual Studio. Si vous avez déjà installé NuGet, accédez à la Galerie d’extensions de Visual Studio pour effectuer une mise à jour vers la dernière version de NuGet.
- La création d’un nouveau projet ASP.NET MVC 3 dans un dossier de solution provoque une erreur *NullReferenceException* . La solution consiste à créer le projet ASP.NET MVC 3 à la racine de la solution, puis à le déplacer dans le dossier de solution.
- Le programme d’installation peut prendre plus de temps que les versions précédentes de ASP.NET MVC pour s’exécuter. Cela est dû au fait qu’il met à jour les composants de Visual Studio 2010.
- IntelliSense pour syntaxe Razor ne fonctionne pas quand resharper est installé. Si resharper est installé et que vous souhaitez tirer parti de la prise en charge de Razor IntelliSense dans ASP.NET MVC 3 RC2, consultez l’entrée [Razor IntelliSense et resharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) sur le blog de hadi Hariri, qui explique comment les utiliser ensemble aujourd’hui.
- Les vues CSHTML et VBHTML créées avec la version bêta de ASP.NET MVC 3 n’ont pas leur action de génération correctement définie, avec pour résultat que ces types d’affichages sont omis lorsque le projet est publié. La valeur de l' *action de génération* pour ces fichiers doit être définie sur contenu». ASP.NET MVC 3 RC2 résout ce problème pour les nouveaux fichiers, mais ne corrige pas le paramètre pour les fichiers existants d’un projet créé avec la version bêta.![](mvc3-release-notes/_static/image4.png)
- Pendant l’installation, la boîte de dialogue acceptation du CLUF affiche les termes du contrat de licence dans une fenêtre plus petite que prévu.
- Lorsque vous modifiez une vue Razor (fichier. cshtml), l’élément de menu atteindre le contrôleur dans Visual Studio n’est pas disponible, et il n’existe aucun extrait de code.
- Si vous installez ASP.NET MVC 3 pour Visual Web Developer Express sur un ordinateur sur lequel Visual Studio n’est pas installé, puis installez Visual Studio ultérieurement, vous devez réinstaller ASP.NET MVC 3. Visual Studio et Visual Web Developer Express partagent des composants qui sont mis à niveau par le programme d’installation de ASP.NET MVC 3. Le même problème s’applique si vous installez ASP.NET MVC 3 pour Visual Studio sur un ordinateur sur lequel Visual Web Developer Express n’est pas installé et que vous installez ensuite Visual Web Developer Express.
- L’installation de ASP.NET MVC 3 RC 2 ne met pas à jour NuGet si vous l’avez déjà installé. Pour mettre à niveau NuGet, accédez au gestionnaire d’extensions Visual Studio. il doit apparaître comme une mise à jour disponible. Vous pouvez effectuer la mise à niveau de NuGet vers la dernière version.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 Release candidate

La version Release candidate de ASP.NET MVC a été publiée le 9 novembre 2010.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Nouvelles fonctionnalités dans ASP.NET MVC 3 RC

Cette section décrit les fonctionnalités qui ont été introduites dans la version ASP.NET MVC 3 RC depuis la version bêta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>NuGet Package Manager

ASP.NET MVC 3 comprend le gestionnaire de package NuGet (précédemment appelé NuPack), qui est un outil de gestion de package intégré pour l’ajout de bibliothèques et d’outils aux projets Visual Studio. Cet outil automatise les étapes que les développeurs prennent aujourd’hui pour obtenir une bibliothèque dans leur arborescence source.

Vous pouvez utiliser NuGet en tant qu’outil en ligne de commande, comme une fenêtre de console intégrée dans Visual Studio 2010, à partir du menu contextuel de Visual Studio et comme un ensemble d’applets de commande PowerShell.

Pour plus d’informations sur NuGet, consultez la [documentation NuGet](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Amélioration de la boîte de dialogue « Nouveau projet »

Lorsque vous créez un projet, la boîte de dialogue Nouveau projet vous permet à présent de spécifier le moteur d’affichage, ainsi qu’un type de projet MVC ASP.NET.

![](mvc3-release-notes/_static/image5.png)

La prise en charge de la modification de la liste des modèles et des moteurs d’affichage répertoriés dans la boîte de dialogue est incluse dans cette version.

Les modèles par défaut sont les suivants :

Vide : Contient un ensemble minimal de fichiers pour un projet MVC ASP.NET, y compris la structure de répertoire par défaut pour les projets MVC ASP.NET, un fichier site. css contenant les styles ASP.NET MVC par défaut et un répertoire de scripts qui contient les fichiers JavaScript par défaut.

Application Internet. Contient des exemples de fonctionnalités qui montrent comment utiliser le fournisseur d’appartenances avec ASP.NET MVC.

La liste des modèles de projet qui s’affiche dans la boîte de dialogue est spécifiée dans le Registre Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Contrôleurs de session

Le nouveau *ControllerSessionStateAttribute* vous donne davantage de contrôle sur le comportement de l’état de session pour les contrôleurs en spécifiant une valeur d’énumération [System. Web. SessionState. SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) .

L’exemple suivant montre comment désactiver l’état de session pour toutes les demandes adressées à un contrôleur.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

L’exemple suivant montre comment définir l’état de session en lecture seule pour toutes les demandes adressées à un contrôleur.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Nouveaux attributs de validation

#### <a name="compareattribute"></a>CompareAttribute

Le nouvel attribut de validation *CompareAttribute* vous permet de comparer les valeurs de deux propriétés différentes d’un modèle. Dans l’exemple suivant, la propriété *ComparePassword* doit correspondre au champ de *mot de passe* afin d’être valide.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

Le nouvel attribut de validation *RemoteAttribute* tire parti du validateur distant du plug-in de validation jQuery, qui permet à la validation côté client d’appeler une méthode sur le serveur qui exécute la logique de validation réelle.

Dans l’exemple suivant, le *RemoteAttribute* est appliqué à la propriété *username* . Lors de la modification de cette propriété dans une vue Edit, la validation client appelle une action nommée *UserNameAvailable* sur la classe *UsersController* afin de valider ce champ.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

L’exemple suivant montre le contrôleur correspondant.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Par défaut, le nom de la propriété à laquelle l’attribut est appliqué est envoyé à la méthode d’action sous la forme d’un paramètre de chaîne de requête.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Nouvelles surcharges pour les méthodes « LabelFor » et « LabelForModel »

De nouvelles surcharges ont été ajoutées pour les méthodes *LabelFor* et *LabelForModel* qui vous permettent de spécifier le texte de l’étiquette. L’exemple suivant montre comment utiliser ces surcharges.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Mise en cache de sortie des actions enfants

*OutputCacheAttribute* prend en charge la mise en cache de sortie des actions enfants appelées à l’aide des méthodes d’assistance *html. RenderAction* ou *html. action* . L’exemple suivant montre une vue qui appelle une autre action.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

L’action *GETDATE* est annotée avec *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Lorsque ce code s’exécute, le résultat de l’appel à html. action (« GetDate ») est mis en cache pendant 100 secondes.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>Améliorations de la boîte de dialogue « Ajouter une vue »

Lorsque vous ajoutez une vue fortement typée, la boîte de dialogue Ajouter une vue filtre maintenant les types non applicables que dans les versions précédentes, comme de nombreux types de .NET Framework de base. En outre, la liste est désormais triée par le nom de la classe et non par le nom de type complet, ce qui facilite la recherche des types. Par exemple, le nom du type s’affiche maintenant comme dans l’exemple suivant :

ClassName (espace de noms)

Dans les versions antérieures, cela aurait été affiché comme suit :

Namespace. ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Validation granulaire des demandes

La propriété *Exclude* de *ValidateInputAttribute* n’existe plus. Au lieu de cela, pour que la validation de la requête soit ignorée pour les propriétés spécifiques d’un modèle pendant la liaison de modèle, utilisez la nouvelle *SkipRequestValidationAttribute*.

Par exemple, supposons qu’une méthode d’action est utilisée pour modifier un billet de blog :

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

L’exemple suivant montre le modèle de vue pour un billet de blog.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Lorsqu’un utilisateur soumet un balisage pour la propriété Description, la liaison de modèle échouera en raison de la validation de la demande. Pour désactiver la validation de la demande pendant la liaison de modèle pour la description du billet de blog, appliquez le *SkipRequpestValidationAttribute* à la propriété, comme illustré dans cet exemple :.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Pour désactiver la validation de la demande pour chaque propriété du modèle, vous pouvez également appliquer *ValidateInputAttribute* avec la valeur *false* à la méthode d’action :

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Modifications avec rupture

- L’ordre d’exécution des filtres d’exception a changé pour les filtres d’exception qui ont la même valeur d' *ordre* . Dans ASP.NET MVC 2 et versions antérieures, les filtres d’exception sur le contrôleur qui avaient le même *ordre* que ceux sur une méthode d’action ont été exécutés avant les filtres d’exception sur la méthode d’action. C’est généralement le cas lorsque des filtres d’exception ont été appliqués sans valeur d' *ordre* spécifiée. Dans ASP.NET MVC 3, cet ordre a été inversé afin que le gestionnaire d’exceptions le plus spécifique s’exécute en premier. Comme dans les versions antérieures, si la propriété *Order* est explicitement spécifiée, les filtres sont exécutés dans l’ordre spécifié.
- Ajout d’une nouvelle propriété nommée *FileExtensions* à la classe de base *VirtualPathProviderViewEngine* . Lors de la recherche d’une vue par chemin d’accès (et non par nom), seules les vues avec une extension de fichier figurant dans la liste spécifiée par cette nouvelle propriété sont prises en compte. Il s’agit d’une modification avec rupture pour les utilisateurs qui inscrivent un fournisseur de générations personnalisées pour activer une extension de fichier personnalisée pour les vues Web Form et qui référencent ces vues à l’aide d’un chemin d’accès complet plutôt que d’un nom. La solution de contournement consiste à modifier la valeur de la propriété *FileExtensions* pour inclure l’extension de fichier personnalisée.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Problèmes connus

- Le programme d’installation peut prendre plus de temps que les versions précédentes de ASP.NET MVC pour s’exécuter car il met à jour les composants de Visual Studio 2010.
- L’option Ajouter la génération de modèles automatique lors de la sélection des modèles de vue astrongly typés génère des propriétés en écriture seule. Celles-ci doivent toujours être ignorées par la génération de modèles automatique. La boîte de dialogue Ajouter une vue génère également des modèles de propriétés en lecture seule lors de la génération d’une vue « modifier » ou « créer ». Les propriétés en lecture seule doivent uniquement être échafaudées pour les affichages de liste et d’affichage.
- Le débogage ne fonctionne pas quand ASP.NET MVC 3 est installé avec le CTP Async. ASP.NET MVC 3 ne peut pas être installé côte à côte avec le CTP Async. Désinstallez le CTP Async pour réparer le débogage. Pour plus d’informations, lisez ce billet de [blog](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) sur la désinstallation de tous les éléments de ASP.NET MVC 3 RC.
- Razor IntelliSense ne fonctionne pas quand resharper est installé. Si resharper est installé et que vous souhaitez tirer parti de la prise en charge de Razor IntelliSense dans ASP.NET MVC 3 RC, veuillez lire ce billet de [blog](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) de JetBrains qui explique comment les utiliser ensemble aujourd’hui.
- Les vues CSHTML et VBHTML créées avec la version bêta de ASP.NET MVC 3 n’ont pas leur action de génération correcte, ce qui les omet de publier. L' *action de génération* pour ces fichiers doit être définie sur « contenu ». ASP.NET MVC 3 RC résout ce problème pour les nouveaux fichiers, mais ne corrige pas le paramètre pour les fichiers existants d’un projet créé avec la version bêta.
- Le programme d’installation peut prendre plus de temps que les versions précédentes de ASP.NET MVC pour s’exécuter car il met à jour les composants de Visual Studio 2010.
- L’option Ajouter la génération de modèles automatique lors de la sélection des propriétés de lecture seule des modèles d’affichage fortement typés « Edit ». De même, les propriétés en écriture seule sont échafaudées pour les affichages « d’affichage ».
- Pendant l’installation, la boîte de dialogue acceptation du CLUF affiche les termes du contrat de licence dans une fenêtre plus petite que prévu.
- L’installation de Visual Studio Async CTP provoque un conflit avec la version Razor incluse dans le cadre de l’installation des outils ASP.NET MVC 3. Veillez à ne pas essayer d’installer à la fois Visual Studio Async CTP et la version Razor sur le même ordinateur.
- Lorsque vous modifiez une vue Razor (fichier. cshtml), l’élément de menu atteindre le contrôleur dans Visual Studio n’est pas disponible, et il n’existe aucun extrait de code.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 bêta

La version bêta de ASP.NET MVC 3 a été publiée le 6 octobre 2010. Les remarques suivantes sont spécifiques à la version bêta et sont soumises à toute mise à jour ou modification référencée dans la section Release candidate ASP.NET MVC 3 ci-dessus.

## <a id="0.1__Toc274034215"></a>Nouvelles fonctionnalités de ASP.NET MVC 3 Beta

<a id="0.1__Default_validation_system"></a>Cette section décrit les fonctionnalités qui ont été introduites dans la version bêta de ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>Gestionnaire de package NuGet

ASP.NET MVC 3 comprend le gestionnaire de package NuGet, qui est un outil de gestion de packages intégré pour l’ajout de bibliothèques et d’outils aux projets Visual Studio. Pour l’essentiel, il automatise les étapes que les développeurs prennent aujourd’hui pour obtenir une bibliothèque dans leur arborescence source.

Vous pouvez utiliser NuGet en tant qu’outil en ligne de commande, en tant que fenêtre de console intégrée dans Visual Studio 2010, à partir du menu contextuel de Visual Studio et en tant que jeu d’applets de commande PowerShell.

Pour plus d’informations sur NuGet, consultez la [documentation NuGet](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>Boîte de dialogue Nouveau projet amélioré

Lorsque vous créez un projet, la boîte de dialogue Nouveau projet vous permet à présent de spécifier le moteur d’affichage, ainsi qu’un type de projet MVC ASP.NET.

![](mvc3-release-notes/_static/image6.png)

La prise en charge de la modification de la liste des modèles et des moteurs d’affichage répertoriés dans la boîte de dialogue n’est pas incluse dans cette version.

Les modèles par défaut sont les suivants :

Vide : Contient un ensemble minimal de fichiers pour un projet MVC ASP.NET, y compris la structure de répertoire par défaut pour les projets MVC ASP.NET, un petit fichier. CSS de site contenant les styles ASP.NET MVC par défaut et un répertoire de scripts qui contient les fichiers JavaScript par défaut.

Application Internet. Contient des exemples de fonctionnalités qui montrent comment utiliser le fournisseur d’appartenances dans ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>Méthode simplifiée pour spécifier des modèles fortement typés dans les vues Razor

La façon de spécifier le type de modèle pour les vues Razor fortement typées a été simplifiée à l’aide de la nouvelle directive @model pour les vues CSHTML et @ModelType directive pour les vues VBHTML. Dans les versions antérieures de ASP.NET MVC, vous deviez spécifier un modèle fortement typé pour les vues Razor de cette façon :

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

Dans cette version, vous pouvez utiliser la syntaxe suivante :

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>Prise en charge des nouvelles méthodes d’assistance pages Web ASP.NET

La nouvelle technologie de pages Web ASP.NET comprend un ensemble de méthodes d’assistance qui sont utiles pour ajouter des fonctionnalités couramment utilisées aux vues et aux contrôleurs. ASP.NET MVC 3 prend en charge l’utilisation de ces méthodes d’assistance dans les contrôleurs et les vues (le cas échéant). Ces méthodes sont contenues dans l’assembly System. Web. helpers. Le tableau suivant répertorie quelques-unes des méthodes d’assistance pages Web ASP.NET.

| **D’assistance** | **Description** |
| --- | --- |
| Graphique | Génère le rendu d’un graphique dans une vue. Contient des méthodes telles que Chart. ToWebImage, Chart. Save et Chart. Write. |
| Crypto | Utilise des algorithmes de hachage pour créer des mots de passe correctement salés et hachés. |
| WebGrid | Génère le rendu d’une collection d’objets (en général, des données d’une base de données) sous la forme d’une grille. Prend en charge la pagination et le tri. |
| WebImage | Génère le rendu d’une image. |
| Messagerie Web | Envoie un e-mail. |

Une rubrique de référence rapide qui répertorie les applications auxiliaires et la syntaxe de base est disponible dans la documentation ASP.NET syntaxe Razor à l’adresse suivante :

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>Prise en charge supplémentaire de l’injection de dépendance

En s’appuyant sur la version ASP.NET MVC 3 Preview 1, la version actuelle offre une prise en charge supplémentaire de deux nouveaux services et de quatre services existants, ainsi qu’une prise en charge améliorée de la résolution des dépendances et du localisateur de service commun.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nouvelle interface IControllerActivator pour l’instanciation de contrôleur affiné

La nouvelle interface IControllerActivator offre un contrôle plus précis de la façon dont les contrôleurs sont instanciés par le biais de l’injection de dépendances. L’exemple suivant illustre l’interface :

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Comparez ceci au rôle de la fabrique de contrôleur. Une fabrique de contrôleur est une implémentation de l’interface IControllerFactory, qui est responsable de la localisation d’un type de contrôleur et de l’instanciation d’une instance de ce type de contrôleur.

Les activateurs de contrôleur sont responsables uniquement de l’instanciation d’une instance d’un type de contrôleur. Ils n’effectuent pas la recherche de type de contrôleur. Après avoir localisé un type de contrôleur approprié, les fabriques de contrôleurs doivent déléguer à une instance de IControllerActivator pour gérer l’instanciation réelle du contrôleur.

La classe DefaultControllerFactory a un nouveau constructeur qui accepte une instance IControllerFactory. Cela vous permet d’appliquer l’injection de dépendances pour gérer cet aspect de la création du contrôleur sans avoir à remplacer le comportement de recherche par défaut de type contrôleur.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Interface IServiceLocator remplacée par IDependencyResolver

D’après les commentaires de la Communauté, la version bêta de ASP.NET MVC 3 a remplacé l’utilisation de l’interface IServiceLocator par une interface IDependencyResolver allégée, spécifique aux besoins de ASP.NET MVC. L’exemple suivant illustre la nouvelle interface :

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Dans le cadre de cette modification, la classe ServiceLocator a également été remplacée par la classe DependencyResolver. L’inscription d’un programme de résolution de dépendances est similaire à celle des versions antérieures de ASP.NET MVC :

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Les implémentations de cette interface doivent simplement déléguer au conteneur d’injection de dépendances sous-jacent pour fournir le service inscrit pour le type demandé.

Lorsqu’il n’existe aucun service inscrit du type demandé, ASP.NET MVC s’attend à ce que les implémentations de cette interface retournent Null de GetService et retournent une collection vide à partir de GetServices.

La nouvelle classe DependencyResolver vous permet d’inscrire des classes qui implémentent la nouvelle interface IDependencyResolver ou l’interface common service Locator (IServiceLocator). Pour plus d’informations sur le localisateur de service courant, consultez [CommonServiceLocator sur GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nouvelle interface IViewActivator pour l’instanciation de page de vue fine

La nouvelle interface IViewPageActivator fournit un contrôle plus précis sur la façon dont les pages de vue sont instanciées via l’injection de dépendances. Cela s’applique aux instances WebFormView et RazorView. L’exemple suivant illustre la nouvelle interface :

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Ces classes acceptent désormais un argument de constructeur IViewPageActivator, qui vous permet d’utiliser l’injection de dépendances pour contrôler la manière dont les types ViewPage, ViewUserControl et WebViewPage sont instanciés.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Nouvelle prise en charge du programme de résolution des dépendances pour les services existants

La nouvelle version comprend la prise en charge de la résolution des dépendances pour les services suivants :

- Fournisseurs de validation de modèle. Les classes qui implémentent ModelValidatorProvider peuvent être inscrites dans le programme de résolution des dépendances, et le système les utilisera pour prendre en charge la validation côté client et côté serveur.
- Fournisseur de métadonnées de modèle. Une classe unique qui implémente ModelMetadataProvider peut être inscrite dans le programme de résolution des dépendances, et le système l’utilisera pour fournir des métadonnées pour les systèmes de création de modèles et de validation.
- Fournisseurs de valeurs. Les classes qui implémentent ValueProviderFactory peuvent être inscrites dans le programme de résolution des dépendances, et le système les utilisera pour créer des fournisseurs de valeurs qui sont consommés par le contrôleur et Pendant la liaison de modèle.
- Classeurs de modèles. Les classes qui implémentent IModelBinderProvider peuvent être inscrites dans le programme de résolution des dépendances, et le système les utilisera pour créer des classeurs de modèles qui sont consommés par le système de liaison de modèle.

### <a id="0.1__Toc274034221"></a>Nouvelle prise en charge d’Ajax basé sur jQuery discrète

ASP.NET MVC comprend des méthodes d’assistance AJAX comme les suivantes :

- Ajax. ActionLink
- Ajax. RouteLink
- Ajax. BeginForm
- Ajax. BeginRouteForm

Ces méthodes utilisent JavaScript pour appeler une méthode d’action sur le serveur au lieu d’utiliser une publication (postback) complète. Cette fonctionnalité a été mise à jour pour tirer parti de jQuery de manière discrète. Au lieu d’émettre indiscrètement des scripts clients Inline, ces méthodes d’assistance séparent le comportement du balisage en émettant des attributs HTML5 à l’aide du préfixe *Data-Ajax* . Le comportement est ensuite appliqué au balisage en référençant les fichiers JavaScript appropriés. Assurez-vous que les fichiers JavaScript suivants sont référencés :

- jQuery-1.4.1. js
- jQuery. discrète. Ajax. js

Cette fonctionnalité est activée par défaut dans le fichier Web. config dans les nouveaux modèles de projet ASP.NET MVC 3, mais elle est désactivée par défaut pour les projets existants. Pour plus d’informations, consultez [Ajout d’indicateurs à l’ensemble de l’application pour la validation du client et JavaScript discret](#0.1_AddedApplicationWideFlagsForClientValida) plus loin dans ce document.

### <a id="0.1__Toc274034222"></a>Nouvelle prise en charge de la validation jQuery discrète

Par défaut, ASP.NET MVC 3 Beta utilise la validation jQuery de manière discrète afin d’effectuer la validation côté client. Pour activer la validation client discrète, effectuez un appel similaire à ce qui suit à partir d’une vue :

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Cela nécessite que la propriété ViewContext. UnobtrusiveJavaScriptEnabled ait la valeur true, ce que vous pouvez faire en effectuant l’appel suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Assurez-vous également que les fichiers JavaScript suivants sont référencés.

- jQuery-1.4.1. js
- jQuery. Validate. js
- jQuery. Validate. undiscret. js

Cette fonctionnalité est activée par défaut dans le fichier Web. config dans les nouveaux modèles de projet ASP.NET MVC 3, mais elle est désactivée par défaut pour les projets existants. Pour plus d’informations, consultez [nouveaux indicateurs à l’ensemble de l’application pour la validation du client et JavaScript discret](#0.1_AddedApplicationWideFlagsForClientValida) plus loin dans ce document.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>Nouveaux indicateurs à l’ensemble de l’application pour la validation du client et JavaScript discret

Vous pouvez activer ou désactiver la validation du client et le JavaScript discrètement à l’aide des membres statiques de la classe HtmlHelper, comme dans l’exemple suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Les modèles de projet par défaut activent le JavaScript discrète par défaut. Vous pouvez également activer ou désactiver ces fonctionnalités dans le fichier Web. config racine de votre application à l’aide des paramètres suivants :

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Étant donné que vous pouvez activer ces fonctionnalités par défaut, de nouvelles surcharges ont été introduites dans la classe HtmlHelper qui vous permettent de remplacer les paramètres par défaut, comme indiqué dans les exemples suivants :

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Pour la compatibilité descendante, ces deux fonctionnalités sont désactivées par défaut.

### <a id="0.1__Toc274034224"></a>Nouvelle prise en charge du code qui s’exécute avant l’exécution des vues

Vous pouvez maintenant placer un fichier nommé \_ViewStart. cshtml (ou \_ViewStart. vbhtml) dans le répertoire views et y ajouter du code qui sera partagé entre plusieurs vues de ce répertoire et de ses sous-répertoires. Par exemple, vous pouvez placer le code suivant dans la page \_ViewStart. cshtml dans le dossier ~/Views :

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Cela permet de définir la page de disposition pour chaque vue dans le dossier Views et dans tous ses sous-dossiers de manière récursive. Lorsqu’une vue est rendue, le code du fichier \_ViewStart. cshtml s’exécute avant l’exécution du code de la vue. Le code \_ViewStart. cshtml s’applique à chaque vue de ce dossier.

Par défaut, le code dans le fichier \_ViewStart. cshtml s’applique également aux vues dans n’importe quel sous-dossier. Toutefois, les sous-dossiers individuels peuvent avoir leur propre version du fichier \_ViewStart. cshtml ; dans ce cas, la version locale est prioritaire. Par exemple, pour exécuter du code commun à toutes les vues du HomeController, placez un \_fichier ViewStart. cshtml dans le dossier ~/Views/Home.

### <a id="0.1__Toc274034225"></a>Nouvelle prise en charge de la syntaxe Razor VBHTML

La version préliminaire de ASP.NET MVC précédente comprenait la prise en charge C#des vues à l’aide de syntaxe Razor basées sur. Ces vues utilisent l’extension de fichier. cshtml. Dans le cadre du travail en cours de prise en charge de Razor, ASP.NET MVC 3 Beta introduit la prise en charge de l’syntaxe Razor dans Visual Basic, qui utilise l’extension de fichier. vbhtml.

Pour une introduction à l’utilisation de la syntaxe Visual Basic dans les pages VBHTML, consultez le didacticiel à l’adresse suivante :

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>Contrôle plus granulaire sur ValidateInputAttribute

ASP.NET MVC a toujours inclus la classe ValidateInputAttribute, qui appelle l’infrastructure de validation de requête ASP.NET principale pour s’assurer que la demande entrante ne contient pas d’entrée potentiellement malveillante. Par défaut, la validation d’entrée est activée. Il est possible de désactiver la validation de la demande à l’aide de l’attribut ValidateInputAttribute, comme dans l’exemple suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Toutefois, de nombreuses applications Web ont des champs de formulaire individuels qui doivent autoriser le code HTML, alors que les autres champs ne le sont pas. La classe ValidateInputAttribute vous permet à présent de spécifier une liste de champs qui ne doivent pas être inclus dans la validation de la demande.

Par exemple, si vous développez un moteur de blog, vous souhaiterez peut-être autoriser le balisage dans les champs corps et résumé. Ces champs peuvent être représentés par deux éléments d’entrée, chacun avec un attribut name correspondant au nom de la propriété (« Body » et « Summary »). Pour désactiver la validation de la demande pour ces champs uniquement, spécifiez les noms (séparés par des virgules) dans la propriété Exclude de la classe ValidateInput, comme dans l’exemple suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>Les applications auxiliaires convertissent les traits de soulignement en tirets pour les noms d’attributs HTML spécifiés à l’aide d’objets anonymes

Les méthodes d’assistance vous permettent de spécifier des paires nom/valeur d’attribut à l’aide d’un objet anonyme, comme dans l’exemple suivant :

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Cette approche ne vous permet pas d’utiliser des traits d’Union dans le nom de l’attribut, car un trait d’Union ne peut pas être utilisé pour un nom de propriété dans ASP.NET. Toutefois, les traits d’Union sont importants pour les attributs HTML5 personnalisés. par exemple, HTML5 utilise le préfixe « Data- ».

En même temps, les traits de soulignement ne peuvent pas être utilisés pour les noms d’attributs en HTML, mais ils sont valides dans les noms de propriété. Par conséquent, si vous spécifiez des attributs à l’aide d’un objet anonyme et si les noms d’attributs incluent un trait de soulignement, les méthodes d’assistance convertissent les traits de soulignement en traits d’Union. Par exemple, la syntaxe d’assistance suivante utilise un trait de soulignement :

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

L’exemple précédent restitue le balisage suivant lors de l’exécution de l’application auxiliaire :

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>Résolutions de bogues

Le modèle d’objet par défaut pour les applications d’assistance de modèle EditorFor et DisplayFor prend désormais en charge le classement spécifié dans la propriété DisplayAttribute. Order. (Dans les versions précédentes, le paramètre de commande n’était pas utilisé.)

La validation client prend désormais en charge la validation des propriétés substituées auxquelles des attributs de validation sont appliqués.

JsonValueProviderFactory est désormais inscrit par défaut.

## <a id="0.1__Toc274034229"></a>Modifications avec rupture

L’ordre d’exécution des filtres d’exception a changé pour les filtres d’exception qui ont la même valeur d’ordre. Dans ASP.NET MVC 2 et versions antérieures, les filtres d’exception sur le contrôleur avec le même ordre que ceux sur une méthode d’action ont été exécutés avant les filtres d’exception sur la méthode d’action. C’est généralement le cas lorsque des filtres d’exception ont été appliqués sans valeur d’ordre spécifiée. Dans ASP.NET MVC 3, cet ordre a été inversé afin que le gestionnaire d’exceptions le plus spécifique s’exécute en premier. Comme dans les versions antérieures, si la propriété Order est explicitement spécifiée, les filtres sont exécutés dans l’ordre spécifié.

## <a id="0.1__Toc274034230"></a>Problèmes connus

Pendant l’installation, la boîte de dialogue acceptation du CLUF affiche les termes du contrat de licence dans une fenêtre plus petite que prévu.

Les vues Razor ne prennent pas en charge IntelliSense ni la mise en surbrillance syntaxique. Il est prévu que la prise en charge de syntaxe Razor dans Visual Studio sera incluse dans le cadre d’une version ultérieure.

Lorsque vous modifiez une vue Razor (fichier cshtml), l’élément <a id="0.1__Toc224729061"></a> <a id="0.1__Toc238051347"></a> de menu atteindre le contrôleur dans Visual Studio n’est pas disponible, et il n’existe aucun extrait de code.

Lorsque vous utilisez la syntaxe @model pour spécifier une vue CSHTML fortement typée, les raccourcis spécifiques à la langue pour les types ne sont pas reconnus. Par exemple, @model int ne fonctionnera pas, mais @model Int32 fonctionnera. La solution de contournement pour ce bogue consiste à utiliser le nom de type réel lorsque vous spécifiez le type de modèle.

Lors de l’utilisation de la syntaxe @model pour spécifier une vue CSHTML fortement typée (ou @ModelType pour spécifier une vue VBHTML fortement typée), les types Nullable et les déclarations de tableau ne sont pas pris en charge. Par exemple, @model int ? n’est pas pris en charge. Utilisez plutôt `@model Nullable<Int32>`. La syntaxe @model String [] n’est pas non plus prise en charge ; Utilisez plutôt `@model IList<string>`.

Quand vous mettez à niveau un projet ASP.NET MVC 2 vers ASP.NET MVC 3, veillez à ajouter le code suivant à la section appSettings du fichier Web. config :

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Il existe un problème connu qui amène l’authentification des formulaires à toujours rediriger les utilisateurs non authentifiés vers ~/Account/Login, en ignorant le paramètre d’authentification par formulaire utilisé dans Web. config. La solution de contournement consiste à ajouter le paramètre d’application suivant.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>AVERTISSEMENT

© 2011 Microsoft Corporation. Tous droits réservés. Ce document est fourni « tel quel ». Les informations et les vues exprimées dans ce document, y compris les URL et autres références à des sites Web Internet, peuvent changer sans préavis. Vous assumez tous les risques liés à leur utilisation.

Ce document ne vous donne aucun droit légal de propriété intellectuelle quant aux produits Microsoft. Vous pouvez copier et utiliser ce document à titre de référence interne.
