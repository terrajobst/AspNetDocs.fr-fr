---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Liaison de liaison à unC#accordéon () | Microsoft Docs
author: wenz
description: Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois. Les panneaux sont généralement déclarés w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c28cc958a1de9844627ae16175a5aed153993a8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607262"
---
# <a name="databinding-to-an-accordion-c"></a>Liaison de données à un Accordion (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois. Les panneaux sont généralement déclarés dans la page elle-même, mais la liaison à une source de données offre plus de flexibilité.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois. Les panneaux sont généralement déclarés dans la page elle-même, mais la liaison à une source de données offre plus de flexibilité.

## <a name="steps"></a>Étapes

Tout d’abord, une source de données est requise. Cet exemple utilise la base de données AdventureWorks et la Microsoft SQL Server édition Express 2005. La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition Express) et est également disponible sous forme de téléchargement distinct sous [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de données AdventureWorks fait partie des exemples SQL Server 2005 et des exemples de bases de données (téléchargez à l’adresse [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)en). Le moyen le plus simple de définir la base de données consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)en) et à attacher le fichier de base de données de `AdventureWorks.mdf`.

Pour cet exemple, nous partons du principe que l’instance de la SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur Web. Il s’agit également de l’installation par défaut. Si votre installation diffère, vous devez adapter les informations de connexion de la base de données.

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Ajoutez ensuite une source de données à la page. Afin d’utiliser une quantité limitée de données, seules les cinq premières entrées de la table Vendor de la base de données AdventureWorks sont sélectionnées. Si vous utilisez l’Assistant Visual Studio pour créer la source de données, n’oubliez pas qu’un bogue dans la version actuelle ne précède pas le nom de la table (`Vendor`) avec `Purchasing`. Le balisage suivant illustre la syntaxe correcte :

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

N’oubliez pas le nom (ID) de la source de données. Cette identification doit ensuite être utilisée dans la propriété `DataSourceID` du contrôle accordéon :

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

Dans le contrôle accordéon, vous pouvez fournir des modèles pour différentes parties du contrôle, y compris l’en-tête (`<HeaderTemplate>`) et le contenu (`<ContentTemplate>`). Au sein de ces éléments, il suffit de sortir les données de la source de données à l’aide de la méthode `DataBinder.Eval()` :

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Lorsque la page est chargée, la source de données doit être liée à l’accordéon avec ce code côté serveur :

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Pour conclure cet exemple, vous devez définir les deux classes CSS qui sont référencées dans le contrôle accordéon (dans ses propriétés `HeaderCssClass` et `ContentCssClass`). Placez le balisage suivant dans la section `<head>` de la page :

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]

[![les données de l’accordéon proviennent directement de la source de données](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Les données de l’accordéon proviennent directement de la source de données ([cliquez pour afficher l’image en taille réelle](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Suivant](dynamically-adding-an-accordion-pane-cs.md)
