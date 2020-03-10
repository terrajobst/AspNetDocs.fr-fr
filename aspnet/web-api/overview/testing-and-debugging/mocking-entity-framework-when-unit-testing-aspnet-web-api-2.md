---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simulation d’Entity Framework lors du test unitaire API Web ASP.NET 2 | Microsoft Docs
author: Rick-Anderson
description: Ce guide et cette application montrent comment créer des tests unitaires pour votre application Web API 2 qui utilise l’Entity Framework. Il montre comment modifier le...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 258450107ee7443c4efd43a3b8e4851249745227
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555089"
---
# <a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="30f77-104">Simulation d’Entity Framework lors du test unitaire API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="30f77-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>

<span data-ttu-id="30f77-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="30f77-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="30f77-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="30f77-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="30f77-107">Ce guide et cette application montrent comment créer des tests unitaires pour votre application Web API 2 qui utilise l’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="30f77-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="30f77-108">Il montre comment modifier le contrôleur de génération de modèles automatique pour permettre le passage d’un objet de contexte à des fins de test, et comment créer des objets de test qui fonctionnent avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="30f77-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
>
> <span data-ttu-id="30f77-109">Pour une introduction aux tests unitaires avec API Web ASP.NET, consultez [tests unitaires avec API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="30f77-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
>
> <span data-ttu-id="30f77-110">Ce didacticiel part du principe que vous êtes familiarisé avec les concepts de base de API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="30f77-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="30f77-111">Pour obtenir un didacticiel d’introduction, consultez [prise en main avec API Web ASP.NET 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="30f77-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="30f77-112">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="30f77-112">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="30f77-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="30f77-113">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="30f77-114">API Web 2</span><span class="sxs-lookup"><span data-stu-id="30f77-114">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="30f77-115">Dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="30f77-115">In this topic</span></span>

<span data-ttu-id="30f77-116">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="30f77-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="30f77-117">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="30f77-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="30f77-118">Télécharger le code</span><span class="sxs-lookup"><span data-stu-id="30f77-118">Download code</span></span>](#download)
- [<span data-ttu-id="30f77-119">Créer une application avec un projet de test unitaire</span><span class="sxs-lookup"><span data-stu-id="30f77-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="30f77-120">Créer la classe de modèle</span><span class="sxs-lookup"><span data-stu-id="30f77-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="30f77-121">Ajouter le contrôleur</span><span class="sxs-lookup"><span data-stu-id="30f77-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="30f77-122">Ajouter une injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="30f77-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="30f77-123">Installer des packages NuGet dans un projet de test</span><span class="sxs-lookup"><span data-stu-id="30f77-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="30f77-124">Créer un contexte de test</span><span class="sxs-lookup"><span data-stu-id="30f77-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="30f77-125">Créer des tests</span><span class="sxs-lookup"><span data-stu-id="30f77-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="30f77-126">Exécuter les tests</span><span class="sxs-lookup"><span data-stu-id="30f77-126">Run tests</span></span>](#runtests)

<span data-ttu-id="30f77-127">Si vous avez déjà effectué les étapes du [test unitaire avec API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md), vous pouvez passer à la section [Ajouter le contrôleur](#controller).</span><span class="sxs-lookup"><span data-stu-id="30f77-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="30f77-128">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="30f77-128">Prerequisites</span></span>

<span data-ttu-id="30f77-129">Visual Studio 2017, édition Professional ou Enterprise</span><span class="sxs-lookup"><span data-stu-id="30f77-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="30f77-130">Télécharger le code</span><span class="sxs-lookup"><span data-stu-id="30f77-130">Download code</span></span>

<span data-ttu-id="30f77-131">Téléchargez le [projet terminé](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="30f77-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="30f77-132">Le projet téléchargeable comprend du code de test unitaire pour cette rubrique et la rubrique [test unitaire API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="30f77-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="30f77-133">Créer une application avec un projet de test unitaire</span><span class="sxs-lookup"><span data-stu-id="30f77-133">Create application with unit test project</span></span>

<span data-ttu-id="30f77-134">Vous pouvez créer un projet de test unitaire lors de la création de votre application ou ajouter un projet de test unitaire à une application existante.</span><span class="sxs-lookup"><span data-stu-id="30f77-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="30f77-135">Ce didacticiel montre comment créer un projet de test unitaire lors de la création de l’application.</span><span class="sxs-lookup"><span data-stu-id="30f77-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="30f77-136">Créez une nouvelle application Web ASP.NET nommée **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="30f77-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="30f77-137">Dans les fenêtres nouveau projet ASP.NET, sélectionnez le modèle **vide** et ajoutez des dossiers et des références principales pour l’API Web.</span><span class="sxs-lookup"><span data-stu-id="30f77-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="30f77-138">Sélectionnez l’option **Ajouter des tests unitaires** .</span><span class="sxs-lookup"><span data-stu-id="30f77-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="30f77-139">Le projet de test unitaire est automatiquement nommé **StoreApp. tests**.</span><span class="sxs-lookup"><span data-stu-id="30f77-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="30f77-140">Vous pouvez conserver ce nom.</span><span class="sxs-lookup"><span data-stu-id="30f77-140">You can keep this name.</span></span>

![créer un projet de test unitaire](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="30f77-142">Après avoir créé l’application, vous verrez qu’elle contient deux projets : **StoreApp** et **StoreApp. tests**.</span><span class="sxs-lookup"><span data-stu-id="30f77-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="30f77-143">Créer la classe de modèle</span><span class="sxs-lookup"><span data-stu-id="30f77-143">Create the model class</span></span>

<span data-ttu-id="30f77-144">Dans votre projet StoreApp, ajoutez un fichier de classe au dossier **Models** nommé **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="30f77-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="30f77-145">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="30f77-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="30f77-146">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="30f77-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="30f77-147">Ajouter le contrôleur</span><span class="sxs-lookup"><span data-stu-id="30f77-147">Add the controller</span></span>

<span data-ttu-id="30f77-148">Cliquez avec le bouton droit sur le dossier contrôleurs et sélectionnez **Ajouter** et **nouvel élément de génération de modèles**automatique.</span><span class="sxs-lookup"><span data-stu-id="30f77-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="30f77-149">Sélectionnez contrôleur d’API Web 2 avec des actions, à l’aide de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="30f77-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Ajouter un nouveau contrôleur](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="30f77-151">Définissez les valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="30f77-151">Set the following values:</span></span>

- <span data-ttu-id="30f77-152">Nom du contrôleur : **ProductController**</span><span class="sxs-lookup"><span data-stu-id="30f77-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="30f77-153">Classe de modèle : **produit**</span><span class="sxs-lookup"><span data-stu-id="30f77-153">Model class: **Product**</span></span>
- <span data-ttu-id="30f77-154">Classe de contexte de données : [sélectionner **un nouveau bouton de contexte de données** qui renseigne les valeurs affichées ci-dessous]</span><span class="sxs-lookup"><span data-stu-id="30f77-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![spécifier le contrôleur](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="30f77-156">Cliquez sur **Ajouter** pour créer le contrôleur avec le code généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="30f77-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="30f77-157">Le code comprend des méthodes pour la création, la récupération, la mise à jour et la suppression d’instances de la classe Product.</span><span class="sxs-lookup"><span data-stu-id="30f77-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="30f77-158">Le code suivant illustre la méthode d’ajout d’un produit.</span><span class="sxs-lookup"><span data-stu-id="30f77-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="30f77-159">Notez que la méthode retourne une instance de **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="30f77-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="30f77-160">IHttpActionResult est l’une des nouvelles fonctionnalités de l’API Web 2 et simplifie le développement de tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="30f77-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="30f77-161">Dans la section suivante, vous allez personnaliser le code généré pour faciliter le passage d’objets de test au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="30f77-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="30f77-162">Ajouter une injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="30f77-162">Add dependency injection</span></span>

<span data-ttu-id="30f77-163">Actuellement, la classe ProductController est codée en dur pour utiliser une instance de la classe StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="30f77-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="30f77-164">Vous allez utiliser un modèle appelé injection de dépendances pour modifier votre application et supprimer cette dépendance codée en dur.</span><span class="sxs-lookup"><span data-stu-id="30f77-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="30f77-165">En rompant cette dépendance, vous pouvez passer un objet fictif lors du test.</span><span class="sxs-lookup"><span data-stu-id="30f77-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="30f77-166">Cliquez avec le bouton droit sur le dossier **Models** , puis ajoutez une nouvelle interface nommée **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="30f77-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="30f77-167">Remplacez le code par celui-ci :</span><span class="sxs-lookup"><span data-stu-id="30f77-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="30f77-168">Ouvrez le fichier StoreAppContext.cs et apportez les modifications suivantes en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="30f77-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="30f77-169">Les changements importants à noter sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="30f77-169">The important changes to note are:</span></span>

- <span data-ttu-id="30f77-170">La classe StoreAppContext implémente l’interface IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="30f77-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="30f77-171">La méthode MarkAsModified est implémentée</span><span class="sxs-lookup"><span data-stu-id="30f77-171">MarkAsModified method is implemented</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="30f77-172">Ouvrez le fichier ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="30f77-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="30f77-173">Modifiez le code existant pour qu’il corresponde au code mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="30f77-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="30f77-174">Ces modifications rompent la dépendance sur StoreAppContext et permettent à d’autres classes de passer un objet différent pour la classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="30f77-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="30f77-175">Cette modification vous permettra de passer un contexte de test pendant les tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="30f77-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="30f77-176">Vous devez apporter une autre modification dans ProductController.</span><span class="sxs-lookup"><span data-stu-id="30f77-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="30f77-177">Dans la méthode **PutProduct** , remplacez la ligne qui définit l’état de l’entité par modifié par un appel à la méthode MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="30f77-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="30f77-178">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="30f77-178">Build the solution.</span></span>

<span data-ttu-id="30f77-179">Vous êtes maintenant prêt à configurer le projet de test.</span><span class="sxs-lookup"><span data-stu-id="30f77-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="30f77-180">Installer des packages NuGet dans un projet de test</span><span class="sxs-lookup"><span data-stu-id="30f77-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="30f77-181">Quand vous utilisez le modèle vide pour créer une application, le projet de test unitaire (StoreApp. tests) n’inclut aucun package NuGet installé.</span><span class="sxs-lookup"><span data-stu-id="30f77-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="30f77-182">D’autres modèles, tels que le modèle d’API Web, incluent des packages NuGet dans le projet de test unitaire.</span><span class="sxs-lookup"><span data-stu-id="30f77-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="30f77-183">Pour ce didacticiel, vous devez inclure le package Entity Framework et le package Microsoft ASP.NET API Web 2 Core au projet de test.</span><span class="sxs-lookup"><span data-stu-id="30f77-183">For this tutorial, you must include the Entity Framework package and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="30f77-184">Cliquez avec le bouton droit sur le projet StoreApp. tests, puis sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="30f77-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="30f77-185">Vous devez sélectionner le projet StoreApp. tests pour ajouter les packages à ce projet.</span><span class="sxs-lookup"><span data-stu-id="30f77-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![gérer les packages](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="30f77-187">À partir des packages en ligne, recherchez et installez le package EntityFramework (version 6,0 ou ultérieure).</span><span class="sxs-lookup"><span data-stu-id="30f77-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="30f77-188">S’il apparaît que le package EntityFramework est déjà installé, vous avez peut-être sélectionné le projet StoreApp au lieu du projet StoreApp. tests.</span><span class="sxs-lookup"><span data-stu-id="30f77-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Ajouter Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="30f77-190">Recherchez et installez Microsoft ASP.NET package principal API Web 2.</span><span class="sxs-lookup"><span data-stu-id="30f77-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![installer le package d’API Web Core](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="30f77-192">Fermez la fenêtre gérer les packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="30f77-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="30f77-193">Créer un contexte de test</span><span class="sxs-lookup"><span data-stu-id="30f77-193">Create test context</span></span>

<span data-ttu-id="30f77-194">Ajoutez une classe nommée **TestDbSet** au projet de test.</span><span class="sxs-lookup"><span data-stu-id="30f77-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="30f77-195">Cette classe sert de classe de base pour votre jeu de données de test.</span><span class="sxs-lookup"><span data-stu-id="30f77-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="30f77-196">Remplacez le code par celui-ci :</span><span class="sxs-lookup"><span data-stu-id="30f77-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="30f77-197">Ajoutez une classe nommée **TestProductDbSet** au projet de test qui contient le code suivant.</span><span class="sxs-lookup"><span data-stu-id="30f77-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="30f77-198">Ajoutez une classe nommée **TestStoreAppContext** et remplacez le code existant par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="30f77-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="30f77-199">Créer des tests</span><span class="sxs-lookup"><span data-stu-id="30f77-199">Create tests</span></span>

<span data-ttu-id="30f77-200">Par défaut, votre projet de test contient un fichier de test vide nommé **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="30f77-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="30f77-201">Ce fichier montre les attributs que vous utilisez pour créer des méthodes de test.</span><span class="sxs-lookup"><span data-stu-id="30f77-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="30f77-202">Pour ce didacticiel, vous pouvez supprimer ce fichier, car vous allez ajouter une nouvelle classe de test.</span><span class="sxs-lookup"><span data-stu-id="30f77-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="30f77-203">Ajoutez une classe nommée **TestProductController** au projet de test.</span><span class="sxs-lookup"><span data-stu-id="30f77-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="30f77-204">Remplacez le code par celui-ci :</span><span class="sxs-lookup"><span data-stu-id="30f77-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="30f77-205">Exécuter les tests</span><span class="sxs-lookup"><span data-stu-id="30f77-205">Run tests</span></span>

<span data-ttu-id="30f77-206">Vous êtes maintenant prêt à exécuter les tests.</span><span class="sxs-lookup"><span data-stu-id="30f77-206">You are now ready to run the tests.</span></span> <span data-ttu-id="30f77-207">Toutes les méthodes marquées avec l’attribut **TestMethod** seront testées.</span><span class="sxs-lookup"><span data-stu-id="30f77-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="30f77-208">À partir de l’élément de menu **test** , exécutez les tests.</span><span class="sxs-lookup"><span data-stu-id="30f77-208">From the **Test** menu item, run the tests.</span></span>

![exécuter des tests](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="30f77-210">Ouvrez la fenêtre **Explorateur de tests** et observez les résultats des tests.</span><span class="sxs-lookup"><span data-stu-id="30f77-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![résultats des tests](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
