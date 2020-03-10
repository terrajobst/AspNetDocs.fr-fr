---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Utilisation de HoverMenu avec un contrôle Repeater (VB) | Microsoft Docs
author: wenz
description: 'Le contrôle HoverMenu dans la boîte à outils de contrôle AJAX fournit un effet de contextuel simple : lorsque le pointeur de la souris se trouve au-dessus d’un élément, une fenêtre contextuelle s’affiche à la suite d’une spécification...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 9386aa2fe3a6174bbed52218337107733cb1fa99
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621575"
---
# <a name="using-hovermenu-with-a-repeater-control-vb"></a>Utilisation de HoverMenu avec un contrôle Repeater (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)

> Le contrôle HoverMenu dans la boîte à outils de contrôle AJAX fournit un effet de contextuel simple : lorsque le pointeur de la souris est placé sur un élément, une fenêtre contextuelle s’affiche à la position spécifiée. Il est également possible d’utiliser ce contrôle dans un Repeater.

## <a name="overview"></a>Présentation

Le contrôle `HoverMenu` dans la boîte à outils de contrôle AJAX fournit un effet de contextuel simple : lorsque le pointeur de la souris se trouve au-dessus d’un élément, une fenêtre contextuelle s’affiche à la position spécifiée. Il est également possible d’utiliser ce contrôle dans un Repeater.

## <a name="steps"></a>Étapes

Tout d’abord, une source de données est requise. Cet exemple utilise la base de données AdventureWorks et la Microsoft SQL Server édition Express 2005. La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition Express) et est également disponible sous forme de téléchargement distinct sous [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de données AdventureWorks fait partie des exemples SQL Server 2005 et des exemples de bases de données (téléchargez à l’adresse [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)en). Le moyen le plus simple de définir la base de données consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)en) et à attacher le fichier de base de données de `AdventureWorks.mdf`.

Pour cet exemple, nous partons du principe que l’instance de la SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur Web. Il s’agit également de l’installation par défaut. Si votre installation diffère, vous devez adapter les informations de connexion de la base de données.

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

Ajoutez ensuite une source de données à la page. Afin d’utiliser une quantité limitée de données, seules les cinq premières entrées de la table Vendor de la base de données AdventureWorks sont sélectionnées. Si vous utilisez l’Assistant Visual Studio pour créer la source de données, n’oubliez pas qu’un bogue dans la version actuelle ne précède pas le nom de la table (`Vendor`) avec `Purchasing`. Le balisage suivant illustre la syntaxe correcte :

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

Ensuite, ajoutez un panneau qui sert de fenêtre contextuelle modale :

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

À présent, le `HoverMenuExtender` entre en lecture. Pour que chaque élément de la source de données dispose de son propre menu contextuel, l’extendeur doit être placé dans la section de `<ItemTemplate>` du répéteur. Voici le code XAML :

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

À présent, chaque élément de la source de données affiche une fenêtre contextuelle à droite (`PopupPosition` attribut) après un délai de 50 millisecondes (attribut`PopDelay`).

[![le menu sensitif s’affiche en regard de chaque élément dans le répéteur](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)

Le menu sensitif apparaît en regard de chaque élément dans le répéteur ([cliquez pour afficher l’image en taille réelle](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-hovermenu-with-a-repeater-control-cs.md)
