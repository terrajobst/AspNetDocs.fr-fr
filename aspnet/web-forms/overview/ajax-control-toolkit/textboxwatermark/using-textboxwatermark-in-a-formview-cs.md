---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
title: Utilisation de TextBoxWatermark dans un FormViewC#() | Microsoft Docs
author: wenz
description: Le contrôle TextBoxWatermark dans la boîte à outils du contrôle AJAX étend une zone de texte afin qu’un texte s’affiche dans la zone. Quand un utilisateur clique sur la zone, je vais...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e6ee90bf-32a5-4987-a384-15cc7dd30c8a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
msc.type: authoredcontent
ms.openlocfilehash: 13ac0da5ca53756aa7c660cdc47c96f0c865b006
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611315"
---
# <a name="using-textboxwatermark-in-a-formview-c"></a>Utilisation de TextBoxWatermark dans un FormView (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)

> Le contrôle TextBoxWatermark dans la boîte à outils du contrôle AJAX étend une zone de texte afin qu’un texte s’affiche dans la zone. Lorsqu’un utilisateur clique sur la zone, il est vidé. Si l’utilisateur quitte la zone sans entrer de texte, le texte prérempli réapparaît. Cela est également possible dans un contrôle FormView.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle `TextBoxWatermark` dans le kit d’outils de contrôle AJAX étend une zone de texte pour qu’un texte s’affiche dans la zone. Lorsqu’un utilisateur clique sur la zone, il est vidé. Si l’utilisateur quitte la zone sans entrer de texte, le texte prérempli réapparaît. Cela est également possible dans un contrôle de `FormView`.

## <a name="steps"></a>Étapes

Tout d’abord, une source de données est requise. Cet exemple utilise la base de données AdventureWorks et la Microsoft SQL Server édition Express 2005. La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition Express) et est également disponible sous forme de téléchargement distinct sous [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de données AdventureWorks fait partie des exemples SQL Server 2005 et des exemples de bases de données (téléchargez à l’adresse [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)en). Le moyen le plus simple de définir la base de données consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)en) et à attacher le fichier de base de données de `AdventureWorks.mdf`.

Pour cet exemple, nous partons du principe que l’instance de la SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur Web. Il s’agit également de l’installation par défaut. Si votre installation diffère, vous devez adapter les informations de connexion de la base de données.

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample1.aspx)]

Ajoutez ensuite une source de données à la page qui prend en charge les instructions SQL `DELETE`, `INSERT` et `UPDATE`. Si vous utilisez l’Assistant Visual Studio pour créer la source de données, n’oubliez pas qu’un bogue dans la version actuelle ne précède pas le nom de la table (`Vendor`) avec `Purchasing`. Le balisage suivant illustre la syntaxe correcte :

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample2.aspx)]

Souvenez-vous du nom (`ID`) de la source de données, car il sera utilisé dans la propriété `DataSourceID` du contrôle `FormView`. La section `<InsertItemTemplate>` du `FormView` contient une zone de texte qui est étendue par le contrôle `TextBoxWatermarkExtender`. Assurez-vous que l’extendeur réside dans le modèle et non en dehors de celui-ci.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample3.aspx)]

Désormais, lorsque l’utilisateur passe en mode insertion du contrôle `FormView`, le champ de texte du nouveau fournisseur est prérempli grâce au contrôle `TextBoxWatermarkExtender`. Un clic à l’intérieur de la zone de texte permet de disparaître le texte du remplissage.

[![le filigrane dans le champ provient de l’extendeur](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)

Le filigrane dans le champ provient de l’extendeur ([cliquez pour afficher l’image en taille réelle](using-textboxwatermark-in-a-formview-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Suivant](using-textboxwatermark-with-validation-controls-cs.md)
