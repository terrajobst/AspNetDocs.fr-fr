---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Utilisation d’un ConfirmButton dans un Repeater (VB) | Microsoft Docs
author: wenz
description: L’extendeur ConfirmButton dans la boîte à outils de contrôle AJAX crée une fenêtre contextuelle oui/non quand l’utilisateur clique sur un bouton (y compris le contrôle LinkButton). Uniquement si oui est...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 001233d866d8a731d93d6900f714cd2060f3d08c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613952"
---
# <a name="using-a-confirmbutton-in-a-repeater-vb"></a>Utilisation d’un ConfirmButton dans un répéteur (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> L’extendeur ConfirmButton dans la boîte à outils de contrôle AJAX crée une fenêtre contextuelle oui/non quand l’utilisateur clique sur un bouton (y compris le contrôle LinkButton). Si l’utilisateur clique sur Oui, l’action du bouton est exécutée, sinon annulée. Cela est également possible dans un Repeater.

## <a name="overview"></a>Présentation

L’extendeur ConfirmButton dans la boîte à outils de contrôle AJAX crée une fenêtre contextuelle oui/non quand l’utilisateur clique sur un bouton (y compris le contrôle LinkButton). Si l’utilisateur clique sur Oui, l’action du bouton est exécutée, sinon annulée. Cela est également possible dans un Repeater.

## <a name="steps"></a>Étapes

Tout d’abord, une source de données est requise. Cet exemple utilise la base de données AdventureWorks et la Microsoft SQL Server édition Express 2005. La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition Express) et est également disponible sous forme de téléchargement distinct sous [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de données AdventureWorks fait partie des exemples SQL Server 2005 et des exemples de bases de données (téléchargez à l’adresse [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)en). Le moyen le plus simple de définir la base de données consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang =](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)en) et à attacher le fichier de base de données de `AdventureWorks.mdf`.

Pour cet exemple, nous partons du principe que l’instance de la SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur Web. Il s’agit également de l’installation par défaut. Si votre installation diffère, vous devez adapter les informations de connexion de la base de données.

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

Ensuite, une source de données est requise. Par souci de simplicité, seules les cinq premières entrées dans la table des fournisseurs AdventureWorks sont récupérées. Notez que lorsque vous utilisez l’Assistant Visual Studio pour créer la source de données, le nom de la table (`Vendors`) n’est pas correctement préfixé avec `Purchasing`. Le balisage suivant est correct :

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

Cette source de données peut ensuite être utilisée dans un Repeater. Comme d’habitude, la méthode `DataBinder.Eval()` récupère les données de la source de données. Le contrôle `ConfirmButtonExtender` doit ensuite être placé dans la section `<ItemTemplate>` du répéteur afin qu’il apparaisse pour chaque entrée dans la source de données.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]

[![le bouton confirmer apparaît en regard de chaque entrée de la source de données](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

Le bouton confirmer apparaît en regard de chaque entrée de la source de données ([cliquez pour afficher l’image en taille réelle](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-a-confirmbutton-in-a-repeater-cs.md)
