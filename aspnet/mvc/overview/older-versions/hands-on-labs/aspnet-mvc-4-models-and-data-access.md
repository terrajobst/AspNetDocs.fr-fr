---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Modèles ASP.NET MVC 4 et accès aux données | Microsoft Docs
author: rick-anderson
description: 'Remarque : ce laboratoire pratique suppose que vous avez une connaissance de base de ASP.NET MVC. Si vous n’avez pas encore utilisé ASP.NET MVC, nous vous recommandons de passer à ASP.NET MVC 4...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 90635b617930d0a9c126795f4c8790d542e33dc9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560199"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="a79e6-104">Modèles ASP.NET MVC 4 et accès aux données</span><span class="sxs-lookup"><span data-stu-id="a79e6-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="a79e6-105">par l' [équipe Web camps](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="a79e6-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="a79e6-106">Télécharger le kit de formation Web camps</span><span class="sxs-lookup"><span data-stu-id="a79e6-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="a79e6-107">Ce laboratoire pratique suppose que vous avez une connaissance de base de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="a79e6-108">Si vous n’avez pas encore utilisé **ASP.NET MVC** , nous vous recommandons de passer au-dessus d’un laboratoire pratique de **notions de base sur ASP.NET MVC 4** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="a79e6-109">Ce laboratoire vous guide tout au long des améliorations et des nouvelles fonctionnalités décrites précédemment en appliquant des modifications mineures à un exemple d’application Web fournie dans le dossier source.</span><span class="sxs-lookup"><span data-stu-id="a79e6-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="a79e6-110">Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="a79e6-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="a79e6-111">Le projet propre à ce laboratoire est disponible sur les [modèles ASP.NET MVC 4 et l’accès aux données](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="a79e6-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="a79e6-112">Dans le laboratoire pratique **ASP.NET MVC notions de base** , vous avez passé des données codées en dur des contrôleurs aux modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="a79e6-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="a79e6-113">Toutefois, pour générer une application Web réelle, vous souhaiterez peut-être utiliser une véritable base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="a79e6-114">Ce laboratoire pratique vous montrera comment utiliser un moteur de base de données pour stocker et récupérer les données nécessaires à l’application du Store musical.</span><span class="sxs-lookup"><span data-stu-id="a79e6-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="a79e6-115">Pour ce faire, vous allez démarrer avec une base de données existante et créer l’Entity Data Model à partir de celle-ci.</span><span class="sxs-lookup"><span data-stu-id="a79e6-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="a79e6-116">Tout au long de ce laboratoire, vous allez rencontrer l’approche **Database First** , ainsi que l’approche **Code First** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="a79e6-117">Toutefois, vous pouvez également utiliser l’approche **Model First** , créer le même modèle à l’aide des outils, puis générer la base de données à partir de celle-ci.</span><span class="sxs-lookup"><span data-stu-id="a79e6-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="a79e6-118">![Database First et Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First et Model First")</span><span class="sxs-lookup"><span data-stu-id="a79e6-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="a79e6-119">*Database First et Model First*</span><span class="sxs-lookup"><span data-stu-id="a79e6-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="a79e6-120">Après avoir généré le modèle, vous allez effectuer les ajustements appropriés dans le StoreController pour fournir les vues du magasin avec les données extraites de la base de données, au lieu d’utiliser des données codées en dur.</span><span class="sxs-lookup"><span data-stu-id="a79e6-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="a79e6-121">Vous n’avez pas besoin d’apporter des modifications aux modèles de vue, car le StoreController retourne les mêmes ViewModels aux modèles de vue, bien que cette fois-ci, les données proviennent de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="a79e6-122">**Approche Code First**</span><span class="sxs-lookup"><span data-stu-id="a79e6-122">**The Code First Approach**</span></span>

<span data-ttu-id="a79e6-123">L’approche Code First nous permet de définir le modèle à partir du code sans générer des classes qui sont généralement associées à l’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="a79e6-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="a79e6-124">Dans code First, les objets de modèle sont définis avec les POCO, &quot;les objets CLR plain&quot;.</span><span class="sxs-lookup"><span data-stu-id="a79e6-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="a79e6-125">Les POCO sont des classes simples simples qui n’ont pas d’héritage et n’implémentent pas d’interfaces.</span><span class="sxs-lookup"><span data-stu-id="a79e6-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="a79e6-126">Nous pouvons générer automatiquement la base de données à partir de ces bases de données ou utiliser une base de données existante et générer le mappage de la classe à partir du code.</span><span class="sxs-lookup"><span data-stu-id="a79e6-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="a79e6-127">L’utilisation de cette approche présente les avantages suivants : le modèle reste indépendant de l’infrastructure de persistance (dans ce cas, Entity Framework), car les classes POCO ne sont pas associées à l’infrastructure de mappage.</span><span class="sxs-lookup"><span data-stu-id="a79e6-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="a79e6-128">Ce laboratoire est basé sur ASP.NET MVC 4 et une version de l’exemple d’application Music Store personnalisée et réduite pour s’adapter uniquement aux fonctionnalités présentées dans ce laboratoire pratique.</span><span class="sxs-lookup"><span data-stu-id="a79e6-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="a79e6-129">Si vous souhaitez explorer l’ensemble de l’application didacticiel du **Store Music** , vous pouvez la trouver dans [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="a79e6-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="a79e6-130">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="a79e6-130">Prerequisites</span></span>

<span data-ttu-id="a79e6-131">Pour effectuer ce laboratoire, vous devez disposer des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="a79e6-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="a79e6-132">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe A](#AppendixA) pour obtenir des instructions sur son installation).</span><span class="sxs-lookup"><span data-stu-id="a79e6-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="a79e6-133">Installation</span><span class="sxs-lookup"><span data-stu-id="a79e6-133">Setup</span></span>

<span data-ttu-id="a79e6-134">**Installation d’extraits de code**</span><span class="sxs-lookup"><span data-stu-id="a79e6-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="a79e6-135">Pour plus de commodité, la majeure partie du code que vous allez gérer dans le cadre de ce laboratoire est disponible sous forme d’extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a79e6-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="a79e6-136">Pour installer les extraits de code, exécutez le fichier **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="a79e6-137">Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;l' [annexe C : utilisation d’extraits de Code](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="a79e6-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="a79e6-138">Exercices</span><span class="sxs-lookup"><span data-stu-id="a79e6-138">Exercises</span></span>

<span data-ttu-id="a79e6-139">Ce laboratoire pratique est constitué des exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="a79e6-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="a79e6-140">Exercice 1 : ajout d’une base de données</span><span class="sxs-lookup"><span data-stu-id="a79e6-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="a79e6-141">Exercice 2 : création d’une base de données à l’aide d’Code First</span><span class="sxs-lookup"><span data-stu-id="a79e6-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="a79e6-142">Exercice 3 : interrogation de la base de données avec des paramètres</span><span class="sxs-lookup"><span data-stu-id="a79e6-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="a79e6-143">Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="a79e6-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="a79e6-144">Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.</span><span class="sxs-lookup"><span data-stu-id="a79e6-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="a79e6-145">Durée estimée pour effectuer ce laboratoire : **35 minutes**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="a79e6-146">Exercice 1 : ajout d’une base de données</span><span class="sxs-lookup"><span data-stu-id="a79e6-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="a79e6-147">Dans cet exercice, vous allez apprendre à ajouter une base de données avec les tables de l’application MusicStore à la solution afin de consommer ses données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="a79e6-148">Une fois que la base de données est générée avec le modèle et ajoutée à la solution, vous allez modifier la classe StoreController pour fournir le modèle de vue avec les données extraites de la base de données, au lieu d’utiliser des valeurs codées en dur.</span><span class="sxs-lookup"><span data-stu-id="a79e6-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="a79e6-149">Tâche 1 : ajout d’une base de données</span><span class="sxs-lookup"><span data-stu-id="a79e6-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="a79e6-150">Dans cette tâche, vous allez ajouter une base de données déjà créée avec les tables principales de l’application MusicStore à la solution.</span><span class="sxs-lookup"><span data-stu-id="a79e6-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="a79e6-151">Ouvrez le **début** de la solution situé à l’emplacement **source/EX1-AddingADatabaseDBFirst/Begin/** Folder.</span><span class="sxs-lookup"><span data-stu-id="a79e6-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="a79e6-152">Vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="a79e6-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="a79e6-153">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="a79e6-154">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="a79e6-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="a79e6-155">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="a79e6-156">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="a79e6-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="a79e6-157">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="a79e6-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="a79e6-158">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="a79e6-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="a79e6-159">Ajoutez un fichier de base de données **MvcMusicStore** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="a79e6-160">Dans ce laboratoire pratique, vous allez utiliser une base de données déjà créée appelée **MvcMusicStore. mdf**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="a79e6-161">Pour ce faire, cliquez avec le bouton droit sur le dossier de données de l' **application\_** , pointez sur **Ajouter** , puis cliquez sur **élément existant**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="a79e6-162">Accédez à **\Source\Assets** et sélectionnez le fichier **MvcMusicStore. mdf** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="a79e6-163">![Ajout d’un élément existant](aspnet-mvc-4-models-and-data-access/_static/image2.png "Ajout d’un élément existant")</span><span class="sxs-lookup"><span data-stu-id="a79e6-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="a79e6-164">*Ajout d’un élément existant*</span><span class="sxs-lookup"><span data-stu-id="a79e6-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="a79e6-165">![Fichier de base de données MvcMusicStore. mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "Fichier de base de données MvcMusicStore. mdf")</span><span class="sxs-lookup"><span data-stu-id="a79e6-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="a79e6-166">*Fichier de base de données MvcMusicStore. mdf*</span><span class="sxs-lookup"><span data-stu-id="a79e6-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="a79e6-167">La base de données a été ajoutée au projet.</span><span class="sxs-lookup"><span data-stu-id="a79e6-167">The database has been added to the project.</span></span> <span data-ttu-id="a79e6-168">Même lorsque la base de données se trouve dans la solution, vous pouvez l’interroger et la mettre à jour en l’hébergeant sur un serveur de base de données différent.</span><span class="sxs-lookup"><span data-stu-id="a79e6-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="a79e6-169">![Base de données MvcMusicStore dans Explorateur de solutions](aspnet-mvc-4-models-and-data-access/_static/image4.png "Base de données MvcMusicStore dans Explorateur de solutions")</span><span class="sxs-lookup"><span data-stu-id="a79e6-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="a79e6-170">*Base de données MvcMusicStore dans Explorateur de solutions*</span><span class="sxs-lookup"><span data-stu-id="a79e6-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="a79e6-171">Vérifiez la connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-171">Verify the connection to the database.</span></span> <span data-ttu-id="a79e6-172">Pour ce faire, double-cliquez sur **MvcMusicStore. mdf** pour établir une connexion.</span><span class="sxs-lookup"><span data-stu-id="a79e6-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="a79e6-173">![Connexion à MvcMusicStore. mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connexion à MvcMusicStore. mdf")</span><span class="sxs-lookup"><span data-stu-id="a79e6-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="a79e6-174">*Connexion à MvcMusicStore. mdf*</span><span class="sxs-lookup"><span data-stu-id="a79e6-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="a79e6-175">Tâche 2 : création d’un modèle de données</span><span class="sxs-lookup"><span data-stu-id="a79e6-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="a79e6-176">Dans cette tâche, vous allez créer un modèle de données pour interagir avec la base de données ajoutée dans la tâche précédente.</span><span class="sxs-lookup"><span data-stu-id="a79e6-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="a79e6-177">Créez un modèle de données qui représente la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="a79e6-178">Pour ce faire, dans Explorateur de solutions cliquez avec le bouton droit sur le dossier **Models** , pointez sur **Ajouter** , puis cliquez sur **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="a79e6-179">Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez le modèle de **données** , puis l’élément **ADO.NET Entity Data Model** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="a79e6-180">Modifiez le nom du modèle de données en **StoreDB. edmx** , puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="a79e6-181">![Ajout de la Entity Data Model ADO.NET StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "Ajout de la Entity Data Model ADO.NET StoreDB")</span><span class="sxs-lookup"><span data-stu-id="a79e6-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="a79e6-182">*Ajout de la Entity Data Model ADO.NET StoreDB*</span><span class="sxs-lookup"><span data-stu-id="a79e6-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="a79e6-183">L' **assistant Entity Data Model** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="a79e6-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="a79e6-184">Cet Assistant va vous guider tout au long de la création de la couche de modèle.</span><span class="sxs-lookup"><span data-stu-id="a79e6-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="a79e6-185">Étant donné que le modèle doit être créé en fonction de la base de données existante récemment ajoutée, sélectionnez **générer à partir de la base de données** , puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-185">Since the model should be created based on the existing database recently added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="a79e6-186">![Choix du contenu du modèle](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choix du contenu du modèle")</span><span class="sxs-lookup"><span data-stu-id="a79e6-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="a79e6-187">*Choix du contenu du modèle*</span><span class="sxs-lookup"><span data-stu-id="a79e6-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="a79e6-188">Étant donné que vous générez un modèle à partir d’une base de données, vous devrez spécifier la connexion à utiliser.</span><span class="sxs-lookup"><span data-stu-id="a79e6-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="a79e6-189">Cliquez sur **nouvelle connexion**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="a79e6-190">Sélectionnez **Microsoft SQL Server fichier de base de données** , puis cliquez sur **Continuer**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="a79e6-191">![Choisir la source de données](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choisir la source de données")</span><span class="sxs-lookup"><span data-stu-id="a79e6-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="a79e6-192">*Boîte de dialogue Choisir la source de données*</span><span class="sxs-lookup"><span data-stu-id="a79e6-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="a79e6-193">Cliquez sur **Parcourir** et sélectionnez la base de données **MvcMusicStore. mdf** que vous avez située dans le dossier **application\_données** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="a79e6-194">![Propriétés de connexion](aspnet-mvc-4-models-and-data-access/_static/image9.png "Propriétés de connexion")</span><span class="sxs-lookup"><span data-stu-id="a79e6-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="a79e6-195">*Propriétés de connexion*</span><span class="sxs-lookup"><span data-stu-id="a79e6-195">*Connection properties*</span></span>
6. <span data-ttu-id="a79e6-196">La classe générée doit avoir le même nom que la chaîne de connexion de l’entité. par conséquent, remplacez son nom par **MusicStoreEntities** , puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="a79e6-197">![Choix de la connexion de données](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choix de la connexion de données")</span><span class="sxs-lookup"><span data-stu-id="a79e6-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="a79e6-198">*Choix de la connexion de données*</span><span class="sxs-lookup"><span data-stu-id="a79e6-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="a79e6-199">Choisissez les objets de base de données à utiliser.</span><span class="sxs-lookup"><span data-stu-id="a79e6-199">Choose the database objects to use.</span></span> <span data-ttu-id="a79e6-200">Étant donné que le modèle d’entité utilisera uniquement les tables de la base de données, sélectionnez l’option **tables** et assurez-vous que les options **inclure les colonnes clés étrangères dans le modèle** et mettre en **pluriel ou au singulier les noms d’objets générés** sont également sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="a79e6-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="a79e6-201">Remplacez l’espace de noms du modèle par **MvcMusicStore. Model** , puis cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="a79e6-202">![Choix des objets de base de données](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choix des objets de base de données")</span><span class="sxs-lookup"><span data-stu-id="a79e6-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="a79e6-203">*Choix des objets de base de données*</span><span class="sxs-lookup"><span data-stu-id="a79e6-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="a79e6-204">Si une boîte de dialogue d’avertissement de sécurité s’affiche, cliquez sur **OK** pour exécuter le modèle et générer les classes pour les entités du modèle.</span><span class="sxs-lookup"><span data-stu-id="a79e6-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="a79e6-205">Un diagramme d’entité pour la base de données s’affiche, tandis qu’une classe distincte qui mappe chaque table à la base de données est créée.</span><span class="sxs-lookup"><span data-stu-id="a79e6-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="a79e6-206">Par exemple, la table **albums** sera représentée par une classe **album** , où chaque colonne de la table sera mappée à une propriété de classe.</span><span class="sxs-lookup"><span data-stu-id="a79e6-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="a79e6-207">Cela vous permet d’interroger et d’utiliser des objets qui représentent des lignes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="a79e6-208">![Diagramme d’entité](aspnet-mvc-4-models-and-data-access/_static/image12.png "Diagramme des entités")</span><span class="sxs-lookup"><span data-stu-id="a79e6-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="a79e6-209">*Diagramme d’entité*</span><span class="sxs-lookup"><span data-stu-id="a79e6-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="a79e6-210">Les modèles T4 (. TT) exécutent du code pour générer les classes d’entités et remplacent les classes existantes portant le même nom.</span><span class="sxs-lookup"><span data-stu-id="a79e6-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="a79e6-211">Dans cet exemple, les classes &quot;&quot;d’album, &quot;genre&quot; et &quot;artiste&quot; ont été remplacées par le code généré.</span><span class="sxs-lookup"><span data-stu-id="a79e6-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="a79e6-212">Tâche 3 : création de l’application</span><span class="sxs-lookup"><span data-stu-id="a79e6-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="a79e6-213">Dans cette tâche, vous allez vérifier que, bien que la génération de modèle ait supprimé les classes de modèle **album**, **genre** et **artiste** , le projet est généré avec succès à l’aide des nouvelles classes de modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="a79e6-214">Générez le projet en sélectionnant l’élément de menu **générer** , puis **créez MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="a79e6-215">![Génération du projet](aspnet-mvc-4-models-and-data-access/_static/image13.png "Génération du projet")</span><span class="sxs-lookup"><span data-stu-id="a79e6-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="a79e6-216">*Génération du projet*</span><span class="sxs-lookup"><span data-stu-id="a79e6-216">*Building the project*</span></span>
2. <span data-ttu-id="a79e6-217">Le projet est correctement généré.</span><span class="sxs-lookup"><span data-stu-id="a79e6-217">The project builds successfully.</span></span> <span data-ttu-id="a79e6-218">Pourquoi fonctionne-t-il toujours ?</span><span class="sxs-lookup"><span data-stu-id="a79e6-218">Why does it still work?</span></span> <span data-ttu-id="a79e6-219">Cela fonctionne parce que les tables de base de données ont des champs qui incluent les propriétés que vous utilisiez dans l' **album** et le **genre**des classes supprimées.</span><span class="sxs-lookup"><span data-stu-id="a79e6-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="a79e6-220">![Builds réussies](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds réussies")</span><span class="sxs-lookup"><span data-stu-id="a79e6-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="a79e6-221">*Builds réussies*</span><span class="sxs-lookup"><span data-stu-id="a79e6-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="a79e6-222">Tandis que le concepteur affiche les entités dans un format de diagramme, C# il s’agit vraiment de classes.</span><span class="sxs-lookup"><span data-stu-id="a79e6-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="a79e6-223">Développez le nœud **StoreDB. edmx** dans le Explorateur de solutions puis **StoreDB.TT**, vous verrez les nouvelles entités générées.</span><span class="sxs-lookup"><span data-stu-id="a79e6-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="a79e6-224">![Fichiers générés](aspnet-mvc-4-models-and-data-access/_static/image15.png "Fichiers générés")</span><span class="sxs-lookup"><span data-stu-id="a79e6-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="a79e6-225">*Fichiers générés*</span><span class="sxs-lookup"><span data-stu-id="a79e6-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="a79e6-226">Tâche 4 : interrogation de la base de données</span><span class="sxs-lookup"><span data-stu-id="a79e6-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="a79e6-227">Dans cette tâche, vous allez mettre à jour la classe StoreController afin que, au lieu d’utiliser des données codées en dur, elle interroge la base de données pour récupérer les informations.</span><span class="sxs-lookup"><span data-stu-id="a79e6-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="a79e6-228">Ouvrez **Controllers\StoreController.cs** et ajoutez le champ suivant à la classe pour contenir une instance de la classe **MusicStoreEntities** , nommée **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="a79e6-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="a79e6-229">(Extrait de code- *modèles et accès aux données-EX1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="a79e6-230">La classe **MusicStoreEntities** expose une propriété de collection pour chaque table de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="a79e6-231">Mettez à jour la méthode d’action **Browse** pour récupérer un genre avec tous les **albums**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="a79e6-232">(Extrait de code- *modèles et accès aux données-navigation dans le magasin de EX1*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="a79e6-233">Vous utilisez une fonctionnalité de .NET appelée **LINQ** (Language-Integrated Query) pour écrire des expressions de requête fortement typées sur ces collections, qui exécutent du code sur la base de données et retournent des objets que vous pouvez programmer.</span><span class="sxs-lookup"><span data-stu-id="a79e6-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="a79e6-234">Pour plus d’informations sur LINQ, consultez le [site MSDN](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="a79e6-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="a79e6-235">Mettez à jour la méthode d’action d' **index** pour récupérer tous les genres.</span><span class="sxs-lookup"><span data-stu-id="a79e6-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="a79e6-236">(Extrait de code- *modèles et accès aux données-index du magasin EX1*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="a79e6-237">Mettez à jour la méthode d’action d' **index** pour récupérer tous les genres et transformer la collection en liste.</span><span class="sxs-lookup"><span data-stu-id="a79e6-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="a79e6-238">(Extrait de code- *modèles et accès aux données-EX1 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="a79e6-239">Tâche 5 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="a79e6-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="a79e6-240">Au cours de cette tâche, vous allez vérifier que la page d’index de magasin affiche désormais les genres stockés dans la base de données au lieu de ceux qui sont codés en dur.</span><span class="sxs-lookup"><span data-stu-id="a79e6-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="a79e6-241">Il n’est pas nécessaire de modifier le modèle de vue, car **StoreController** retourne les mêmes entités qu’auparavant, bien que cette fois-ci, les données proviennent de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="a79e6-242">Régénérez la solution et appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="a79e6-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="a79e6-243">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="a79e6-243">The project starts in the Home page.</span></span> <span data-ttu-id="a79e6-244">Vérifiez que le menu des **genres** n’est plus une liste codée en dur et que les données sont récupérées directement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="a79e6-246">*Exploration des genres de la base de données*</span><span class="sxs-lookup"><span data-stu-id="a79e6-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="a79e6-247">À présent, accédez à n’importe quel genre et vérifiez que les albums sont remplis à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="a79e6-248">![Exploration des albums de la base de données](aspnet-mvc-4-models-and-data-access/_static/image17.png "Exploration des albums de la base de données")</span><span class="sxs-lookup"><span data-stu-id="a79e6-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="a79e6-249">*Exploration des albums de la base de données*</span><span class="sxs-lookup"><span data-stu-id="a79e6-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="a79e6-250">Exercice 2 : création d’une base de données à l’aide d’Code First</span><span class="sxs-lookup"><span data-stu-id="a79e6-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="a79e6-251">Dans cet exercice, vous allez apprendre à utiliser l’approche Code First pour créer une base de données avec les tables de l’application MusicStore et comment accéder à ses données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="a79e6-252">Une fois le modèle généré, vous allez modifier le StoreController pour fournir le modèle de vue avec les données extraites de la base de données, au lieu d’utiliser des valeurs codées en dur.</span><span class="sxs-lookup"><span data-stu-id="a79e6-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="a79e6-253">Si vous avez terminé l’exercice 1 et que vous avez déjà travaillé avec l’approche Database First, vous allez maintenant apprendre à obtenir les mêmes résultats avec un processus différent.</span><span class="sxs-lookup"><span data-stu-id="a79e6-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="a79e6-254">Les tâches qui sont en commun avec l’exercice 1 ont été marquées pour faciliter votre lecture.</span><span class="sxs-lookup"><span data-stu-id="a79e6-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="a79e6-255">Si vous n’avez pas terminé l’exercice 1 mais que vous souhaitez connaître l’approche Code First, vous pouvez commencer par cet exercice et obtenir une couverture complète de la rubrique.</span><span class="sxs-lookup"><span data-stu-id="a79e6-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="a79e6-256">Tâche 1 : remplissage des exemples de données</span><span class="sxs-lookup"><span data-stu-id="a79e6-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="a79e6-257">Dans cette tâche, vous allez remplir la base de données avec des exemples de données lorsqu’elle est créée initialement à l’aide de code-First.</span><span class="sxs-lookup"><span data-stu-id="a79e6-257">In this task, you will populate the database with sample data when it is initially created using Code-First.</span></span>

1. <span data-ttu-id="a79e6-258">Ouvrez le **début** de la solution situé dans **source/EX2-CreatingADatabaseCodeFirst/Begin/** Folder.</span><span class="sxs-lookup"><span data-stu-id="a79e6-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="a79e6-259">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="a79e6-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="a79e6-260">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="a79e6-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="a79e6-261">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="a79e6-262">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="a79e6-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="a79e6-263">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="a79e6-264">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="a79e6-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="a79e6-265">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="a79e6-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="a79e6-266">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="a79e6-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="a79e6-267">Ajoutez le fichier **SampleData.cs** au dossier **Models** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="a79e6-268">Pour ce faire, cliquez avec le bouton droit sur le dossier **Models** , pointez sur **Ajouter** , puis cliquez sur **élément existant**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="a79e6-269">Accédez à **\Source\Assets** et sélectionnez le fichier **SampleData.cs** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="a79e6-270">![Code de remplissage des exemples de données](aspnet-mvc-4-models-and-data-access/_static/image18.png "Code de remplissage des exemples de données")</span><span class="sxs-lookup"><span data-stu-id="a79e6-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="a79e6-271">*Code de remplissage des exemples de données*</span><span class="sxs-lookup"><span data-stu-id="a79e6-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="a79e6-272">Ouvrez le fichier **global.asax.cs** et ajoutez les instructions *using* suivantes.</span><span class="sxs-lookup"><span data-stu-id="a79e6-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="a79e6-273">(Extrait de code- *modèles et accès aux données-EX2 global asax using*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="a79e6-274">Dans la méthode **Application\_Start ()** , ajoutez la ligne suivante pour définir l’initialiseur de base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="a79e6-275">(Extrait de code- *modèles et accès aux données-EX2 global asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="a79e6-276">Tâche 2 : configuration de la connexion à la base de données</span><span class="sxs-lookup"><span data-stu-id="a79e6-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="a79e6-277">Maintenant que vous avez déjà ajouté une base de données à notre projet, vous allez écrire dans le fichier **Web. config** la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="a79e6-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="a79e6-278">Ajoutez une chaîne de connexion à **Web. config**. Pour ce faire, ouvrez le **fichier Web. config** à la racine du projet et remplacez la chaîne de connexion nommée DefaultConnection par cette ligne dans la section **&lt;connectionStrings&gt;** :</span><span class="sxs-lookup"><span data-stu-id="a79e6-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="a79e6-279">![Emplacement du fichier Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "Emplacement du fichier Web. config")</span><span class="sxs-lookup"><span data-stu-id="a79e6-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="a79e6-280">*Emplacement du fichier Web. config*</span><span class="sxs-lookup"><span data-stu-id="a79e6-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="a79e6-281">Tâche 3 : utilisation du modèle</span><span class="sxs-lookup"><span data-stu-id="a79e6-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="a79e6-282">Maintenant que vous avez déjà configuré la connexion à la base de données, vous allez lier le modèle aux tables de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="a79e6-283">Dans cette tâche, vous allez créer une classe qui sera liée à la base de données avec Code First.</span><span class="sxs-lookup"><span data-stu-id="a79e6-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="a79e6-284">N’oubliez pas qu’il existe une classe de modèle POCO existante qui doit être modifiée.</span><span class="sxs-lookup"><span data-stu-id="a79e6-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="a79e6-285">Si vous avez terminé l’exercice 1, vous remarquerez que cette étape a été effectuée par un Assistant.</span><span class="sxs-lookup"><span data-stu-id="a79e6-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="a79e6-286">En procédant Code First, vous allez créer manuellement des classes qui seront liées aux entités de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="a79e6-287">Ouvrez le **genre** de classe de modèle POCO à partir du dossier de **projet Models** et incluez un ID.</span><span class="sxs-lookup"><span data-stu-id="a79e6-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="a79e6-288">Utilisez une propriété int portant le nom **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="a79e6-289">(Extrait de code- *modèles et accès aux données-Ex2 code First genre*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="a79e6-290">Pour utiliser des conventions de Code First, le genre de classe doit avoir une propriété de clé primaire qui sera détectée automatiquement.</span><span class="sxs-lookup"><span data-stu-id="a79e6-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="a79e6-291">Pour plus d’informations sur les conventions de Code First, consultez cet [article MSDN](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="a79e6-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="a79e6-292">À présent, ouvrez l' **album** de classe de modèle POCO à partir du dossier de **projet Models** et incluez les clés étrangères, puis créez des propriétés avec les noms **GenreId** et **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="a79e6-293">Cette classe a déjà le **GenreId** pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="a79e6-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="a79e6-294">(Extrait de code- *modèles et accès aux données-Ex2 code First album*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="a79e6-295">Ouvrez la classe de modèle POCO **Artist** et incluez la propriété **ArtistId** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="a79e6-296">(Extrait de code- *modèles et accès aux données-Ex2 code First Artist*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="a79e6-297">Cliquez avec le bouton droit sur le dossier du projet **Models** , puis sélectionnez **Ajouter | Classe**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="a79e6-298">Nommez le fichier **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="a79e6-299">Cliquez ensuite sur **Ajouter.**</span><span class="sxs-lookup"><span data-stu-id="a79e6-299">Then, click **Add.**</span></span>

    <span data-ttu-id="a79e6-300">![Ajout d’une classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "Ajout d’une classe")</span><span class="sxs-lookup"><span data-stu-id="a79e6-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="a79e6-301">*Ajout d’un nouvel élément*</span><span class="sxs-lookup"><span data-stu-id="a79e6-301">*Adding a new item*</span></span>

    <span data-ttu-id="a79e6-302">![Ajout d’un Class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Ajout d’un Class2")</span><span class="sxs-lookup"><span data-stu-id="a79e6-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="a79e6-303">*Ajout d’une classe*</span><span class="sxs-lookup"><span data-stu-id="a79e6-303">*Adding a class*</span></span>
5. <span data-ttu-id="a79e6-304">Ouvrez la classe que vous venez de créer, **MusicStoreEntities.cs**, et incluez les espaces de noms **System. Data. Entity** et **System. Data. Entity. infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="a79e6-305">Remplacez la déclaration de classe pour étendre la classe **DbContext** : déclarez une **DBSet** publique et substituez la méthode **OnModelCreating** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="a79e6-306">Après cette étape, vous obtiendrez une classe de domaine qui lie votre modèle au Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a79e6-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="a79e6-307">Pour ce faire, remplacez le code de la classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a79e6-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="a79e6-308">(Extrait de code- *modèles et accès aux données-Ex2 code First MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="a79e6-309">Avec Entity Framework **DbContext** et **DBSet** , vous serez en mesure d’interroger le genre de la classe POCO.</span><span class="sxs-lookup"><span data-stu-id="a79e6-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="a79e6-310">En étendant la méthode **OnModelCreating** , vous spécifiez dans le **code** comment le genre sera mappé à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="a79e6-311">Vous trouverez plus d’informations sur DBContext et DBSet dans cet article MSDN : [lien](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="a79e6-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="a79e6-312">Tâche 4 : interrogation de la base de données</span><span class="sxs-lookup"><span data-stu-id="a79e6-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="a79e6-313">Dans cette tâche, vous allez mettre à jour la classe StoreController afin que, au lieu d’utiliser des données codées en dur, elle la récupère de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="a79e6-314">Cette tâche est en commun avec l’exercice 1.</span><span class="sxs-lookup"><span data-stu-id="a79e6-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="a79e6-315">Si vous avez terminé l’exercice 1, vous noterez que ces étapes sont les mêmes dans les deux approches (Database First ou code First).</span><span class="sxs-lookup"><span data-stu-id="a79e6-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="a79e6-316">Elles diffèrent dans la façon dont les données sont liées au modèle, mais l’accès aux entités de données est encore transparent à partir du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a79e6-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>

1. <span data-ttu-id="a79e6-317">Ouvrez **Controllers\StoreController.cs** et ajoutez le champ suivant à la classe pour contenir une instance de la classe **MusicStoreEntities** , nommée **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="a79e6-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="a79e6-318">(Extrait de code- *modèles et accès aux données-EX1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="a79e6-319">La classe **MusicStoreEntities** expose une propriété de collection pour chaque table de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="a79e6-320">Mettez à jour la méthode d’action **Browse** pour récupérer un genre avec tous les **albums**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="a79e6-321">(Extrait de code- *modèles et accès aux données-EX2 Store Browse*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="a79e6-322">Vous utilisez une fonctionnalité de .NET appelée **LINQ** (Language-Integrated Query) pour écrire des expressions de requête fortement typées sur ces collections, qui exécutent du code sur la base de données et retournent des objets que vous pouvez programmer.</span><span class="sxs-lookup"><span data-stu-id="a79e6-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="a79e6-323">Pour plus d’informations sur LINQ, consultez le [site MSDN](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="a79e6-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="a79e6-324">Mettez à jour la méthode d’action d' **index** pour récupérer tous les genres.</span><span class="sxs-lookup"><span data-stu-id="a79e6-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="a79e6-325">(Extrait de code- *modèles et accès aux données-index EX2 Store*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="a79e6-326">Mettez à jour la méthode d’action d' **index** pour récupérer tous les genres et transformer la collection en liste.</span><span class="sxs-lookup"><span data-stu-id="a79e6-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="a79e6-327">(Extrait de code- *modèles et accès aux données-EX2 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="a79e6-328">Tâche 5 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="a79e6-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="a79e6-329">Au cours de cette tâche, vous allez vérifier que la page d’index de magasin affiche désormais les genres stockés dans la base de données au lieu de ceux qui sont codés en dur.</span><span class="sxs-lookup"><span data-stu-id="a79e6-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="a79e6-330">Il n’est pas nécessaire de modifier le modèle de vue, car **StoreController** retourne le même **StoreIndexViewModel** qu’auparavant, mais cette fois-ci, les données proviennent de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="a79e6-331">Régénérez la solution et appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="a79e6-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="a79e6-332">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="a79e6-332">The project starts in the Home page.</span></span> <span data-ttu-id="a79e6-333">Vérifiez que le menu des **genres** n’est plus une liste codée en dur et que les données sont récupérées directement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="a79e6-335">*Exploration des genres de la base de données*</span><span class="sxs-lookup"><span data-stu-id="a79e6-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="a79e6-336">À présent, accédez à n’importe quel genre et vérifiez que les albums sont remplis à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="a79e6-337">![Exploration des albums de la base de données](aspnet-mvc-4-models-and-data-access/_static/image23.png "Exploration des albums de la base de données")</span><span class="sxs-lookup"><span data-stu-id="a79e6-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="a79e6-338">*Exploration des albums de la base de données*</span><span class="sxs-lookup"><span data-stu-id="a79e6-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="a79e6-339">Exercice 3 : interrogation de la base de données avec des paramètres</span><span class="sxs-lookup"><span data-stu-id="a79e6-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="a79e6-340">Dans cet exercice, vous allez apprendre à interroger la base de données à l’aide de paramètres et à utiliser la mise en forme des résultats de requête, une fonctionnalité qui réduit le nombre d’accès à la base de données qui récupèrent les données de façon plus efficace.</span><span class="sxs-lookup"><span data-stu-id="a79e6-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="a79e6-341">Pour plus d’informations sur la mise en forme des résultats de la requête, consultez l' [article MSDN](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx)suivant.</span><span class="sxs-lookup"><span data-stu-id="a79e6-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="a79e6-342">Tâche 1 : modification de StoreController pour récupérer des albums de la base de données</span><span class="sxs-lookup"><span data-stu-id="a79e6-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="a79e6-343">Dans cette tâche, vous allez modifier la classe **StoreController** pour accéder à la base de données afin de récupérer les albums d’un genre spécifique.</span><span class="sxs-lookup"><span data-stu-id="a79e6-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="a79e6-344">Ouvrez la solution **Begin** située dans le dossier **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** si vous souhaitez utiliser l’approche code-First ou le dossier **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** si vous souhaitez utiliser l’approche basée sur la première base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="a79e6-345">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="a79e6-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="a79e6-346">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="a79e6-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="a79e6-347">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="a79e6-348">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="a79e6-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="a79e6-349">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="a79e6-350">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="a79e6-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="a79e6-351">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="a79e6-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="a79e6-352">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="a79e6-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="a79e6-353">Ouvrez la classe **StoreController** pour modifier la méthode d’action **Browse** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="a79e6-354">Pour ce faire, dans le **Explorateur de solutions**, développez le dossier **Controllers** , puis double-cliquez sur **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="a79e6-355">Modifiez la méthode d’action **Parcourir** pour récupérer des albums pour un genre spécifique.</span><span class="sxs-lookup"><span data-stu-id="a79e6-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="a79e6-356">Pour ce faire, remplacez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a79e6-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="a79e6-357">(Extrait de code- *modèles et accès aux données-EX3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="a79e6-358">Pour remplir une collection de l’entité, vous devez utiliser la méthode **include** pour spécifier que vous souhaitez également récupérer les albums.</span><span class="sxs-lookup"><span data-stu-id="a79e6-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="a79e6-359">Vous pouvez utiliser le. Extension **unique ()** dans LINQ, car dans ce cas, un seul genre est attendu pour un album.</span><span class="sxs-lookup"><span data-stu-id="a79e6-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="a79e6-360">La méthode **Single ()** prend une expression lambda en tant que paramètre, qui, dans ce cas, spécifie un objet de genre unique, de sorte que son nom correspond à la valeur définie.</span><span class="sxs-lookup"><span data-stu-id="a79e6-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="a79e6-361">Vous pouvez tirer parti d’une fonctionnalité qui vous permet d’indiquer d’autres entités associées que vous souhaitez charger également lorsque l’objet genre est récupéré.</span><span class="sxs-lookup"><span data-stu-id="a79e6-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="a79e6-362">Cette fonctionnalité est appelée mise en **forme des résultats**de la requête et vous permet de réduire le nombre de fois où l’accès à la base de données est nécessaire pour récupérer des informations.</span><span class="sxs-lookup"><span data-stu-id="a79e6-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="a79e6-363">Dans ce scénario, vous souhaiterez pré-extraire les albums pour le genre que vous récupérez.</span><span class="sxs-lookup"><span data-stu-id="a79e6-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="a79e6-364">La requête inclut **genres. include (&quot;les albums&quot;)** pour indiquer que vous souhaitez également obtenir des albums associés.</span><span class="sxs-lookup"><span data-stu-id="a79e6-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="a79e6-365">Cela aboutit à une application plus efficace, car elle récupère à la fois les données de genre et d’album dans une requête de base de données unique.</span><span class="sxs-lookup"><span data-stu-id="a79e6-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="a79e6-366">Tâche 2 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="a79e6-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="a79e6-367">Au cours de cette tâche, vous allez exécuter l’application et récupérer des albums d’un genre spécifique à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="a79e6-368">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="a79e6-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="a79e6-369">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="a79e6-369">The project starts in the Home page.</span></span> <span data-ttu-id="a79e6-370">Remplacez l’URL par **/Store/Browse ? genre = pop** pour vérifier que les résultats sont récupérés à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="a79e6-371">![Exploration par genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Exploration par genre")</span><span class="sxs-lookup"><span data-stu-id="a79e6-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="a79e6-372">*Exploration de/Store/Browse ? genre = pop*</span><span class="sxs-lookup"><span data-stu-id="a79e6-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="a79e6-373">Tâche 3 : accès aux albums par ID</span><span class="sxs-lookup"><span data-stu-id="a79e6-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="a79e6-374">Dans cette tâche, vous allez répéter la procédure précédente pour récupérer les albums en fonction de leur ID.</span><span class="sxs-lookup"><span data-stu-id="a79e6-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="a79e6-375">Fermez le navigateur si nécessaire pour revenir à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a79e6-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="a79e6-376">Ouvrez la classe **StoreController** pour modifier la méthode d’action **Details** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="a79e6-377">Pour ce faire, dans le **Explorateur de solutions**, développez le dossier **Controllers** , puis double-cliquez sur **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="a79e6-378">Modifiez la méthode d’action **Details** pour récupérer les détails des albums en fonction de leur **ID**. Pour ce faire, remplacez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a79e6-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="a79e6-379">(Extrait de code- *modèles et accès aux données-EX3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="a79e6-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="a79e6-380">Tâche 4 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="a79e6-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="a79e6-381">Dans cette tâche, vous allez exécuter l’application dans un navigateur Web et obtenir les détails de l’album en fonction de son ID.</span><span class="sxs-lookup"><span data-stu-id="a79e6-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="a79e6-382">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="a79e6-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="a79e6-383">Le projet démarre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="a79e6-383">The project starts in the Home page.</span></span> <span data-ttu-id="a79e6-384">Remplacez l’URL par **/Store/Details/51** ou parcourez les genres et sélectionnez un album pour vérifier que les résultats sont récupérés à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="a79e6-385">![Exploration des détails](aspnet-mvc-4-models-and-data-access/_static/image25.png "Exploration des détails")</span><span class="sxs-lookup"><span data-stu-id="a79e6-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="a79e6-386">*Navigation dans/Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="a79e6-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="a79e6-387">En outre, vous pouvez déployer cette application sur les sites Web Windows Azure à l' [annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="a79e6-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="a79e6-388">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="a79e6-388">Summary</span></span>

<span data-ttu-id="a79e6-389">En effectuant ce laboratoire pratique, vous avez appris les principes de base des modèles MVC ASP.NET et l’accès aux données, à l’aide de l’approche **Database First** , ainsi que de l’approche **Code First** :</span><span class="sxs-lookup"><span data-stu-id="a79e6-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="a79e6-390">Comment ajouter une base de données à la solution afin de consommer ses données</span><span class="sxs-lookup"><span data-stu-id="a79e6-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="a79e6-391">Comment mettre à jour des contrôleurs pour fournir des modèles d’affichage avec les données extraites de la base de données au lieu d’un en codage codé en dur</span><span class="sxs-lookup"><span data-stu-id="a79e6-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="a79e6-392">Comment interroger la base de données à l’aide de paramètres</span><span class="sxs-lookup"><span data-stu-id="a79e6-392">How to query the database using parameters</span></span>
- <span data-ttu-id="a79e6-393">Comment utiliser la mise en forme des résultats de la requête, une fonctionnalité qui réduit le nombre d’accès aux bases de données, la récupération des données de façon plus efficace</span><span class="sxs-lookup"><span data-stu-id="a79e6-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="a79e6-394">Comment utiliser les Database First et les approches de Code First dans Microsoft Entity Framework pour lier la base de données au modèle</span><span class="sxs-lookup"><span data-stu-id="a79e6-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="a79e6-395">Annexe A : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="a79e6-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="a79e6-396">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="a79e6-397">Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="a79e6-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="a79e6-398">Accédez à [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="a79e6-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="a79e6-399">Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="a79e6-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="a79e6-400">Cliquez sur **Installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-400">Click on **Install Now**.</span></span> <span data-ttu-id="a79e6-401">Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.</span><span class="sxs-lookup"><span data-stu-id="a79e6-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="a79e6-402">Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.</span><span class="sxs-lookup"><span data-stu-id="a79e6-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="a79e6-403">![Installer Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="a79e6-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="a79e6-404">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="a79e6-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="a79e6-405">Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="a79e6-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acceptation des termes du contrat de licence](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="a79e6-407">*Acceptation des termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="a79e6-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="a79e6-408">Attendez que le processus de téléchargement et d’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="a79e6-408">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="a79e6-410">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="a79e6-410">*Installation progress*</span></span>
6. <span data-ttu-id="a79e6-411">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-411">When the installation completes, click **Finish**.</span></span>

    ![Installation terminée](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="a79e6-413">*Installation terminée*</span><span class="sxs-lookup"><span data-stu-id="a79e6-413">*Installation completed*</span></span>
7. <span data-ttu-id="a79e6-414">Cliquez sur **quitter** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="a79e6-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="a79e6-415">Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Vignette VS Express pour le Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="a79e6-417">*Vignette VS Express pour le Web*</span><span class="sxs-lookup"><span data-stu-id="a79e6-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="a79e6-418">Annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="a79e6-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="a79e6-419">Cette annexe vous indique comment créer un nouveau site Web à partir de Windows Azure Portail de gestion et publier l’application que vous avez obtenue en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="a79e6-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="a79e6-420">Tâche 1 : création d’un nouveau site Web à partir du portail Windows Azure</span><span class="sxs-lookup"><span data-stu-id="a79e6-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="a79e6-421">Accédez au [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous à l’aide des informations d’identification Microsoft associées à votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="a79e6-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a79e6-422">Avec Windows Azure, vous pouvez héberger gratuitement 10 sites Web ASP.NET, puis mettre à l’échelle à mesure que votre trafic augmente.</span><span class="sxs-lookup"><span data-stu-id="a79e6-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="a79e6-423">Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="a79e6-423">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="a79e6-424">![Ouvrir une session sur Windows Portail Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Ouvrir une session sur Windows Portail Azure")</span><span class="sxs-lookup"><span data-stu-id="a79e6-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="a79e6-425">*Connectez-vous à Windows Azure Portail de gestion*</span><span class="sxs-lookup"><span data-stu-id="a79e6-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="a79e6-426">Cliquez sur **nouveau** dans la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="a79e6-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="a79e6-427">![Création d’un nouveau site Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "Création d’un nouveau site Web")</span><span class="sxs-lookup"><span data-stu-id="a79e6-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="a79e6-428">*Création d’un nouveau site Web*</span><span class="sxs-lookup"><span data-stu-id="a79e6-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="a79e6-429">Cliquez sur **compute** | **site Web**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="a79e6-430">Sélectionnez ensuite l’option **création rapide** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="a79e6-431">Fournissez une URL disponible pour le nouveau site Web, puis cliquez sur **créer un site Web**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a79e6-432">Un site Web Windows Azure est l’hôte d’une application Web s’exécutant dans le Cloud que vous pouvez contrôler et gérer.</span><span class="sxs-lookup"><span data-stu-id="a79e6-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="a79e6-433">L’option création rapide vous permet de déployer une application Web terminée sur le site Web Windows Azure depuis l’extérieur du portail.</span><span class="sxs-lookup"><span data-stu-id="a79e6-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="a79e6-434">Elle n’inclut pas les étapes de configuration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="a79e6-435">![Création d’un nouveau site Web à l’aide de la création rapide](aspnet-mvc-4-models-and-data-access/_static/image33.png "Création d’un nouveau site Web à l’aide de la création rapide")</span><span class="sxs-lookup"><span data-stu-id="a79e6-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="a79e6-436">*Création d’un nouveau site Web à l’aide de la création rapide*</span><span class="sxs-lookup"><span data-stu-id="a79e6-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="a79e6-437">Patientez jusqu’à la création du nouveau **site Web** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="a79e6-438">Une fois le site Web créé, cliquez sur le lien sous la colonne **URL** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="a79e6-439">Vérifiez que le nouveau site Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="a79e6-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="a79e6-440">![Navigation vers le nouveau site Web](aspnet-mvc-4-models-and-data-access/_static/image34.png "Navigation vers le nouveau site Web")</span><span class="sxs-lookup"><span data-stu-id="a79e6-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="a79e6-441">*Navigation vers le nouveau site Web*</span><span class="sxs-lookup"><span data-stu-id="a79e6-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="a79e6-442">![Site Web en cours d’exécution](aspnet-mvc-4-models-and-data-access/_static/image35.png "Site Web en cours d’exécution")</span><span class="sxs-lookup"><span data-stu-id="a79e6-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="a79e6-443">*Site Web en cours d’exécution*</span><span class="sxs-lookup"><span data-stu-id="a79e6-443">*Web site running*</span></span>
6. <span data-ttu-id="a79e6-444">Revenez au portail et cliquez sur le nom du site Web dans la colonne **nom** pour afficher les pages de gestion.</span><span class="sxs-lookup"><span data-stu-id="a79e6-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="a79e6-445">![Ouverture des pages de gestion de site Web](aspnet-mvc-4-models-and-data-access/_static/image36.png "Ouverture des pages de gestion de site Web")</span><span class="sxs-lookup"><span data-stu-id="a79e6-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="a79e6-446">*Ouverture des pages de gestion de site Web*</span><span class="sxs-lookup"><span data-stu-id="a79e6-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="a79e6-447">Dans la page **tableau de bord** , sous la section **Aperçu rapide** , cliquez sur le lien **Télécharger le profil de publication** .</span><span class="sxs-lookup"><span data-stu-id="a79e6-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a79e6-448">Le *profil de publication* contient toutes les informations requises pour publier une application Web sur un site Web Windows Azure pour chaque méthode de publication activée.</span><span class="sxs-lookup"><span data-stu-id="a79e6-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="a79e6-449">Le profil de publication contient les URL, les informations d'identification de l'utilisateur et les chaînes de base de données nécessaires pour la connexion et l'authentification auprès de chacun des points de terminaison pour lesquels une méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="a79e6-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="a79e6-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour le web** et **Microsoft Visual Studio 2012** prennent en charge la lecture des profils de publication pour automatiser la configuration de ces programmes pour la publication d’applications Web sur les sites Web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="a79e6-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="a79e6-451">![Téléchargement du profil de publication du site Web](aspnet-mvc-4-models-and-data-access/_static/image37.png "Téléchargement du profil de publication du site Web")</span><span class="sxs-lookup"><span data-stu-id="a79e6-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="a79e6-452">*Téléchargement du profil de publication du site Web*</span><span class="sxs-lookup"><span data-stu-id="a79e6-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="a79e6-453">Téléchargez le fichier de profil de publication dans un emplacement connu.</span><span class="sxs-lookup"><span data-stu-id="a79e6-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="a79e6-454">Dans cet exercice, vous allez apprendre à utiliser ce fichier pour publier une application Web sur un site Web Windows Azure à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a79e6-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="a79e6-455">![Enregistrement du fichier de profil de publication](aspnet-mvc-4-models-and-data-access/_static/image38.png "Enregistrement du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="a79e6-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="a79e6-456">*Enregistrement du fichier de profil de publication*</span><span class="sxs-lookup"><span data-stu-id="a79e6-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="a79e6-457">Tâche 2 : configuration du serveur de base de données</span><span class="sxs-lookup"><span data-stu-id="a79e6-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="a79e6-458">Si votre application utilise des bases de données SQL Server, vous devez créer un serveur SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a79e6-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="a79e6-459">Si vous souhaitez déployer une application simple qui n’utilise pas SQL Server vous pouvez ignorer cette tâche.</span><span class="sxs-lookup"><span data-stu-id="a79e6-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="a79e6-460">Vous aurez besoin d’un serveur de SQL Database pour stocker la base de données d’application.</span><span class="sxs-lookup"><span data-stu-id="a79e6-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="a79e6-461">Vous pouvez afficher les serveurs SQL Database à partir de votre abonnement dans le portail de gestion Windows Azure, à partir des **bases de données SQL** | **serveurs** | **tableau de bord du serveur**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="a79e6-462">Si vous n’avez pas de serveur créé, vous pouvez en créer un à l’aide du bouton **Ajouter** de la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="a79e6-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="a79e6-463">Notez le **nom et l’URL du serveur,** ainsi que le nom et le mot de passe de connexion de l’administrateur, car vous les utiliserez dans les tâches suivantes.</span><span class="sxs-lookup"><span data-stu-id="a79e6-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="a79e6-464">Ne créez pas encore la base de données, car elle sera créée dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="a79e6-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="a79e6-465">![Tableau de bord du serveur SQL Database](aspnet-mvc-4-models-and-data-access/_static/image39.png "Tableau de bord du serveur SQL Database")</span><span class="sxs-lookup"><span data-stu-id="a79e6-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="a79e6-466">*Tableau de bord du serveur SQL Database*</span><span class="sxs-lookup"><span data-stu-id="a79e6-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="a79e6-467">Dans la tâche suivante, vous allez tester la connexion à la base de données à partir de Visual Studio. pour cette raison, vous devez inclure votre adresse IP locale dans la liste des **adresses IP autorisées**du serveur.</span><span class="sxs-lookup"><span data-stu-id="a79e6-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="a79e6-468">Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de l' **adresse IP actuelle du client** et collez-la dans les zones de texte **adresse IP de début** et **adresse IP de fin** , puis cliquez sur le bouton ![ajouter-client-IP-adresse-OK-button](aspnet-mvc-4-models-and-data-access/_static/image40.png).</span><span class="sxs-lookup"><span data-stu-id="a79e6-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Ajout de l’adresse IP du client](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="a79e6-470">*Ajout de l’adresse IP du client*</span><span class="sxs-lookup"><span data-stu-id="a79e6-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="a79e6-471">Une fois l' **adresse IP du client** ajoutée à la liste des adresses IP autorisées, cliquez sur **Enregistrer** pour confirmer les modifications.</span><span class="sxs-lookup"><span data-stu-id="a79e6-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmer les modifications](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="a79e6-473">*Confirmer les modifications*</span><span class="sxs-lookup"><span data-stu-id="a79e6-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="a79e6-474">Tâche 3 : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="a79e6-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="a79e6-475">Revenez à la solution ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="a79e6-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="a79e6-476">Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet de site Web et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="a79e6-477">![Publication de l’application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publication de l'application")</span><span class="sxs-lookup"><span data-stu-id="a79e6-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="a79e6-478">*Publication du site Web*</span><span class="sxs-lookup"><span data-stu-id="a79e6-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="a79e6-479">Importez le profil de publication que vous avez enregistré dans la première tâche.</span><span class="sxs-lookup"><span data-stu-id="a79e6-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="a79e6-480">![Importation du profil de publication](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importation du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="a79e6-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="a79e6-481">*Importation du profil de publication*</span><span class="sxs-lookup"><span data-stu-id="a79e6-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="a79e6-482">Cliquez sur **valider la connexion**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-482">Click **Validate Connection**.</span></span> <span data-ttu-id="a79e6-483">Une fois la validation terminée, cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a79e6-484">La validation est terminée une fois que vous voyez une coche verte s’affiche en regard du bouton valider la connexion.</span><span class="sxs-lookup"><span data-stu-id="a79e6-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="a79e6-485">![Validation de la connexion](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validation de la connexion")</span><span class="sxs-lookup"><span data-stu-id="a79e6-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="a79e6-486">*Validation de la connexion*</span><span class="sxs-lookup"><span data-stu-id="a79e6-486">*Validating connection*</span></span>
4. <span data-ttu-id="a79e6-487">Dans la page **paramètres** , sous la section **bases de données** , cliquez sur le bouton en regard de la zone de texte de votre connexion à la base de données (par exemple, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="a79e6-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="a79e6-488">![Configuration de Web Deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "Configuration de Web Deploy")</span><span class="sxs-lookup"><span data-stu-id="a79e6-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="a79e6-489">*Configuration de Web Deploy*</span><span class="sxs-lookup"><span data-stu-id="a79e6-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="a79e6-490">Configurez la connexion de base de données comme suit :</span><span class="sxs-lookup"><span data-stu-id="a79e6-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="a79e6-491">Dans **nom du serveur** , tapez l’URL de votre serveur SQL Database à l’aide du préfixe *TCP :* .</span><span class="sxs-lookup"><span data-stu-id="a79e6-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="a79e6-492">Dans **nom d’utilisateur** , tapez le nom de connexion de l’administrateur du serveur.</span><span class="sxs-lookup"><span data-stu-id="a79e6-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="a79e6-493">Dans **mot de passe** , tapez le mot de passe de connexion de l’administrateur du serveur.</span><span class="sxs-lookup"><span data-stu-id="a79e6-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="a79e6-494">Tapez un nouveau nom de base de données.</span><span class="sxs-lookup"><span data-stu-id="a79e6-494">Type a new database name.</span></span>

     <span data-ttu-id="a79e6-495">![Configuration de la chaîne de connexion de destination](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuration de la chaîne de connexion de destination")</span><span class="sxs-lookup"><span data-stu-id="a79e6-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="a79e6-496">*Configuration de la chaîne de connexion de destination*</span><span class="sxs-lookup"><span data-stu-id="a79e6-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="a79e6-497">Cliquez ensuite sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-497">Then click **OK**.</span></span> <span data-ttu-id="a79e6-498">Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="a79e6-499">![Création de la base de données](aspnet-mvc-4-models-and-data-access/_static/image48.png "Création de la chaîne de base de données")</span><span class="sxs-lookup"><span data-stu-id="a79e6-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="a79e6-500">*Création de la base de données*</span><span class="sxs-lookup"><span data-stu-id="a79e6-500">*Creating the database*</span></span>
7. <span data-ttu-id="a79e6-501">La chaîne de connexion que vous allez utiliser pour vous connecter à SQL Database dans Windows Azure est affichée dans la zone de texte connexion par défaut.</span><span class="sxs-lookup"><span data-stu-id="a79e6-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="a79e6-502">Cliquez ensuite sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-502">Then click **Next**.</span></span>

    <span data-ttu-id="a79e6-503">![Chaîne de connexion pointant vers SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Chaîne de connexion pointant vers SQL Database")</span><span class="sxs-lookup"><span data-stu-id="a79e6-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="a79e6-504">*Chaîne de connexion pointant vers SQL Database*</span><span class="sxs-lookup"><span data-stu-id="a79e6-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="a79e6-505">Dans la page **Aperçu** , cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="a79e6-506">![Publication de l’application Web](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publication de l’application Web")</span><span class="sxs-lookup"><span data-stu-id="a79e6-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="a79e6-507">*Publication de l’application Web*</span><span class="sxs-lookup"><span data-stu-id="a79e6-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="a79e6-508">Une fois le processus de publication terminé, votre navigateur par défaut ouvre le site Web publié.</span><span class="sxs-lookup"><span data-stu-id="a79e6-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="a79e6-509">Annexe C : utilisation d’extraits de code</span><span class="sxs-lookup"><span data-stu-id="a79e6-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="a79e6-510">Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="a79e6-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="a79e6-511">Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.</span><span class="sxs-lookup"><span data-stu-id="a79e6-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="a79e6-512">![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-models-and-data-access/_static/image51.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="a79e6-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="a79e6-513">*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="a79e6-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="a79e6-514">***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***</span><span class="sxs-lookup"><span data-stu-id="a79e6-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="a79e6-515">Placez le curseur à l’endroit où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="a79e6-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="a79e6-516">Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).</span><span class="sxs-lookup"><span data-stu-id="a79e6-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="a79e6-517">Regarder comme IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="a79e6-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="a79e6-518">Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).</span><span class="sxs-lookup"><span data-stu-id="a79e6-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="a79e6-519">Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="a79e6-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="a79e6-520">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-models-and-data-access/_static/image52.png "Commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="a79e6-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="a79e6-521">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="a79e6-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="a79e6-522">![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](aspnet-mvc-4-models-and-data-access/_static/image53.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="a79e6-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="a79e6-523">*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="a79e6-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="a79e6-524">![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](aspnet-mvc-4-models-and-data-access/_static/image54.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")</span><span class="sxs-lookup"><span data-stu-id="a79e6-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="a79e6-525">*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*</span><span class="sxs-lookup"><span data-stu-id="a79e6-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="a79e6-526">***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0.</span><span class="sxs-lookup"><span data-stu-id="a79e6-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="a79e6-527">Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="a79e6-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="a79e6-528">Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.</span><span class="sxs-lookup"><span data-stu-id="a79e6-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="a79e6-529">Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="a79e6-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="a79e6-530">![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](aspnet-mvc-4-models-and-data-access/_static/image55.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="a79e6-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="a79e6-531">*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="a79e6-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="a79e6-532">![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](aspnet-mvc-4-models-and-data-access/_static/image56.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")</span><span class="sxs-lookup"><span data-stu-id="a79e6-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="a79e6-533">*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*</span><span class="sxs-lookup"><span data-stu-id="a79e6-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
