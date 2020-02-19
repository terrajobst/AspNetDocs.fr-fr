---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Développement du chapitre téléchargements pour les didacticiels d’EF 5 MVC 4 | Microsoft Docs
author: Rick-Anderson
description: L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 10237af40e3914b65e5181f17555697e86adea4b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457854"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Développement du chapitre téléchargements pour les didacticiels d’EF 5 MVC 4

par [Rick Anderson](https://twitter.com/RickAndMSFT)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et de Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

## <a name="building-the-chapter-downloads"></a>Téléchargements pour la génération correspondant aux chapitres

1. Téléchargez et décompressez le fichier zip de l’exemple de projet. Dans le package de téléchargement décompressé, vous trouverez des fichiers zip supplémentaires, un pour la fin de chaque chapitre.
2. Cliquez avec le bouton droit sur le fichier zip de votre choix, cliquez sur **Propriétés**, puis sur le bouton **débloquer** .  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Décompressez le fichier.
4. Double-cliquez sur le fichier *CUx. sln* pour lancer Visual Studio.
5. Dans le menu **Outils** , cliquez sur **Gestionnaire de package NuGet**, puis sur **console du gestionnaire de package**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. Dans la console du gestionnaire de package (PMC), cliquez sur **restaurer**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Quittez Visual Studio.
8. Redémarrez Visual Studio, en ouvrant le fichier solution que vous avez fermé à l’étape précédente.
9. Dans la console du gestionnaire de package (PMC), entrez la commande `Update-Database` :  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Si vous obtenez l’erreur suivante :  
    >   
    >  *Le terme « Update-Database » n’est pas reconnu comme le nom d’une applet de commande, d’une fonction, d’un fichier de script ou d’un programme exécutable. Vérifiez l’orthographe du nom, ou si un chemin d’accès a été inclus, vérifiez que le chemin d’accès est correct, puis réessayez.*  
    > Quittez et redémarrez Visual Studio.

    Chaque migration est exécutée, puis la méthode Seed s’exécute. Vous pouvez maintenant exécuter l’application.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Précédent](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
