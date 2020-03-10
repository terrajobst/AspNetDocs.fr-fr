---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Mise à niveau d’une application ASP.NET MVC 1,0 vers ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Ce document décrit la procédure à suivre pour mettre à niveau manuellement et avec un Assistant une application ASP.NET MVC 1,0 sur ASP.NET MVC 2. Ce document est également disponible pour d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 27589f1b1c9d5038118e5ff0cc2e7cecae17d5ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637017"
---
# <a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Mise à niveau d’une application ASP.NET MVC 1.0 vers ASP.NET MVC 2

> Ce document décrit la procédure à suivre pour mettre à niveau manuellement et avec un Assistant une application ASP.NET MVC 1,0 sur ASP.NET MVC 2. Ce document est également disponible au [Téléchargement](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)

## <a name="introduction"></a>Introduction

ASP.NET MVC 2 peut être installé côte à côte avec ASP.NET MVC 1,0 sur le même serveur. Cela permet aux développeurs d’applications de choisir quand mettre à niveau une application ASP.NET MVC 1,0 vers ASP.NET MVC 2.

Visual Studio 2010 comprend un assistant qui met à niveau les projets ASP.NET MVC 1,0 existants créés avec Visual Studio 2008 vers ASP.NET MVC 2. L’Assistant Mise à niveau est lancé en ouvrant un projet ASP.NET MVC 1,0 dans Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Assistant Mise à niveau pour ASP.NET MVC 1,0 sur Visual Studio 2008 SP1

Pour mettre à niveau une application ASP.NET MVC 1,0 vers ASP.NET MVC 2 dans Visual Studio 2008 SP1, utilisez l’application MvcAppConverter (non prise en charge). Vous pouvez télécharger cette application à partir de l’URL suivante :

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Mise à niveau manuelle d’un projet ASP.NET MVC 1,0

Pour mettre à niveau manuellement une application ASP.NET MVC 1,0 existante vers la version 2, procédez comme suit :

1. Effectuez une sauvegarde du projet existant.
2. Dans un éditeur de texte, ouvrez le fichier projet (le fichier avec l’extension de fichier. csproj ou. vbproj) et recherchez l’élément ProjectTypeGuid. Remplacez le GUID {603c0e0b-db56-11dc-Be95-000d561079b0} par {F85E285D-A4E0-4152-9332-AB1D724D3325} comme valeur de cet élément. Lorsque vous avez terminé, la valeur de cet élément doit être la suivante : 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Dans le dossier racine de l’application Web, modifiez le fichier Web. config. Recherchez System. Web. Mvc, version = 1.0.0.0 et remplacez toutes les instances par System. Web. Mvc, version = 2.0.0.0.
4. Répétez l’étape précédente pour le fichier Web. config situé dans le dossier views.
5. Ouvrez le projet à l’aide de Visual Studio et, dans **Explorateur de solutions**, développez le nœud **références** . Supprimez la référence à System. Web. Mvc (qui pointe vers l’assembly version 1,0). Ajoutez une référence à System. Web. Mvc (v 2.0.0.0).
6. Ajoutez l’élément bindingRedirect suivant au fichier Web. config dans la racine de l’application, sous la section Configuration :   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Créez une nouvelle application ASP.NET MVC 2 vide. Copiez les fichiers à partir du dossier scripts de la nouvelle application dans le dossier scripts de l’application existante.
8. Mettez à jour le fichier CSS application-™ s existant avec les définitions de style CSS dans le fichier site. css.
9. Compilez l’application et exécutez-la. Si des erreurs se produisent, reportez-vous à la section modifications avec rupture de la page [Nouveautés dans ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) .
