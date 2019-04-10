---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Création du chapitre téléchargements pour le MVC EF 5 4 didacticiels | Microsoft Docs
author: Rick-Anderson
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: e90eebaba3645802f318dbf449c3ec734265a092
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59381943"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="67650-103">Création du chapitre téléchargements pour le MVC EF 5 4 didacticiels</span><span class="sxs-lookup"><span data-stu-id="67650-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>

<span data-ttu-id="67650-104">par [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="67650-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[<span data-ttu-id="67650-105">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="67650-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="67650-106">L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="67650-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="67650-107">Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="67650-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="67650-108">Téléchargements pour la génération correspondant aux chapitres</span><span class="sxs-lookup"><span data-stu-id="67650-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="67650-109">Téléchargez et décompressez le fichier zip d’exemple de projet.</span><span class="sxs-lookup"><span data-stu-id="67650-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="67650-110">Dans le package de téléchargement décompressé, vous trouverez les fichiers zip supplémentaires, une pour la fin de chaque chapitre.</span><span class="sxs-lookup"><span data-stu-id="67650-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="67650-111">Cliquez avec le bouton droit sur le fichier zip de votre choix, cliquez sur **propriétés**, puis cliquez sur le **Unblock** bouton.</span><span class="sxs-lookup"><span data-stu-id="67650-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="67650-112">Décompressez le fichier.</span><span class="sxs-lookup"><span data-stu-id="67650-112">Unzip the file.</span></span>
4. <span data-ttu-id="67650-113">Double-cliquez sur le *CUx.sln* fichier pour lancer Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="67650-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="67650-114">À partir de la **outils** menu, cliquez sur **Gestionnaire de Package NuGet**, puis **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="67650-114">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="67650-115">Dans le Package Manager de la console, cliquez sur **restaurer**.</span><span class="sxs-lookup"><span data-stu-id="67650-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="67650-116">Quittez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="67650-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="67650-117">Redémarrez Visual Studio, en ouvrant le fichier de solution que vous avez fermé à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="67650-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="67650-118">Dans le Package Manager de la console, entrez le `Update-Database` commande :</span><span class="sxs-lookup"><span data-stu-id="67650-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="67650-119">Si vous obtenez l’erreur suivante :</span><span class="sxs-lookup"><span data-stu-id="67650-119">If you get the following error:</span></span>  
    >   
    >  *<span data-ttu-id="67650-120">Le terme « Update-Database » n’est pas reconnu comme le nom de l’applet de commande, fonction, fichier de script ou programme exécutable.</span><span class="sxs-lookup"><span data-stu-id="67650-120">The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program.</span></span> <span data-ttu-id="67650-121">Vérifiez l’orthographe du nom, ou si un chemin d’accès a été inclus, vérifiez que le chemin d’accès est correct et réessayez.</span><span class="sxs-lookup"><span data-stu-id="67650-121">Check the spelling of the name, or if a path was included, verify that the path is correct and try again.</span></span>*  
    > <span data-ttu-id="67650-122">Quittez et redémarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="67650-122">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="67650-123">Chaque migration s’exécute, puis la méthode seed s’exécute.</span><span class="sxs-lookup"><span data-stu-id="67650-123">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="67650-124">Vous pouvez maintenant exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="67650-124">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="67650-125">Précédent</span><span class="sxs-lookup"><span data-stu-id="67650-125">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
