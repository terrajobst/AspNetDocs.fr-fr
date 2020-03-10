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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78599462"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="4d173-103">Développement du chapitre téléchargements pour les didacticiels d’EF 5 MVC 4</span><span class="sxs-lookup"><span data-stu-id="4d173-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>

<span data-ttu-id="4d173-104">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4d173-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="4d173-105">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="4d173-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="4d173-106">L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et de Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="4d173-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="4d173-107">Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="4d173-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

## <a name="building-the-chapter-downloads"></a><span data-ttu-id="4d173-108">Téléchargements pour la génération correspondant aux chapitres</span><span class="sxs-lookup"><span data-stu-id="4d173-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="4d173-109">Téléchargez et décompressez le fichier zip de l’exemple de projet.</span><span class="sxs-lookup"><span data-stu-id="4d173-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="4d173-110">Dans le package de téléchargement décompressé, vous trouverez des fichiers zip supplémentaires, un pour la fin de chaque chapitre.</span><span class="sxs-lookup"><span data-stu-id="4d173-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="4d173-111">Cliquez avec le bouton droit sur le fichier zip de votre choix, cliquez sur **Propriétés**, puis sur le bouton **débloquer** .</span><span class="sxs-lookup"><span data-stu-id="4d173-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="4d173-112">Décompressez le fichier.</span><span class="sxs-lookup"><span data-stu-id="4d173-112">Unzip the file.</span></span>
4. <span data-ttu-id="4d173-113">Double-cliquez sur le fichier *CUx. sln* pour lancer Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4d173-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="4d173-114">Dans le menu **Outils** , cliquez sur **Gestionnaire de package NuGet**, puis sur **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="4d173-114">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="4d173-115">Dans la console du gestionnaire de package (PMC), cliquez sur **restaurer**.</span><span class="sxs-lookup"><span data-stu-id="4d173-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="4d173-116">Quittez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4d173-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="4d173-117">Redémarrez Visual Studio, en ouvrant le fichier solution que vous avez fermé à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="4d173-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="4d173-118">Dans la console du gestionnaire de package (PMC), entrez la commande `Update-Database` :</span><span class="sxs-lookup"><span data-stu-id="4d173-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="4d173-119">Si vous obtenez l’erreur suivante :</span><span class="sxs-lookup"><span data-stu-id="4d173-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="4d173-120">*Le terme « Update-Database » n’est pas reconnu comme le nom d’une applet de commande, d’une fonction, d’un fichier de script ou d’un programme exécutable. Vérifiez l’orthographe du nom, ou si un chemin d’accès a été inclus, vérifiez que le chemin d’accès est correct, puis réessayez.*</span><span class="sxs-lookup"><span data-stu-id="4d173-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="4d173-121">Quittez et redémarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4d173-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="4d173-122">Chaque migration est exécutée, puis la méthode Seed s’exécute.</span><span class="sxs-lookup"><span data-stu-id="4d173-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="4d173-123">Vous pouvez maintenant exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="4d173-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="4d173-124">Précédent</span><span class="sxs-lookup"><span data-stu-id="4d173-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
