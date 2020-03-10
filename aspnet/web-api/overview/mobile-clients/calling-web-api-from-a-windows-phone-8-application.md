---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Appel de l’API Web à partir d’uneC#application Windows Phone 8 ()-ASP.net 4. x
author: rmcmurray
description: 'Didacticiel avec code : créez une application API Web ASP.NET dans ASP.NET 4. x qui fournit un catalogue de livres à une application Windows Phone 8.'
ms.author: riande
ms.date: 10/09/2013
ms.custom: seoapril2019
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: c5da14a6856f551343b6fb14f0aedc659e792f6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614736"
---
# <a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="46717-103">Appel de l’API web à partir d’une application Windows Phone 8 (C#)</span><span class="sxs-lookup"><span data-stu-id="46717-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>

<span data-ttu-id="46717-104">par [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="46717-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="46717-105">Dans ce didacticiel, vous allez apprendre à créer un scénario complet de bout en bout constitué d’une application API Web ASP.NET qui fournit un catalogue de livres à une application Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="46717-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="46717-106">Présentation</span><span class="sxs-lookup"><span data-stu-id="46717-106">Overview</span></span>

<span data-ttu-id="46717-107">Les services RESTful comme API Web ASP.NET simplifier la création d’applications basées sur HTTP pour les développeurs en extrayant l’architecture pour les applications côté serveur et côté client.</span><span class="sxs-lookup"><span data-stu-id="46717-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="46717-108">Au lieu de créer un protocole propriétaire pour la communication, les développeurs d’API Web doivent simplement publier les méthodes HTTP nécessaires pour leur application (par exemple : obtenir, publier, PUT, supprimer) et les développeurs d’application cliente doivent uniquement utiliser méthodes HTTP nécessaires à leur application.</span><span class="sxs-lookup"><span data-stu-id="46717-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="46717-109">Dans ce didacticiel de bout en bout, vous allez apprendre à utiliser l’API Web pour créer les projets suivants :</span><span class="sxs-lookup"><span data-stu-id="46717-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="46717-110">Dans la [première partie de ce didacticiel](#STEP1), vous allez créer une application API Web ASP.net qui prend en charge toutes les opérations de création, lecture, mise à jour et suppression (CRUD) pour gérer un catalogue de livres.</span><span class="sxs-lookup"><span data-stu-id="46717-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="46717-111">Cette application utilisera l' [exemple de fichier XML (Books. Xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) à partir de MSDN.</span><span class="sxs-lookup"><span data-stu-id="46717-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="46717-112">Dans la [deuxième partie de ce didacticiel](#STEP2), vous allez créer une application Windows Phone 8 interactive qui récupère les données à partir de votre application API Web.</span><span class="sxs-lookup"><span data-stu-id="46717-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="46717-113">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="46717-113">Prerequisites</span></span>

- <span data-ttu-id="46717-114">Visual Studio 2013 avec le kit de développement logiciel (SDK) Windows Phone 8 installé</span><span class="sxs-lookup"><span data-stu-id="46717-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="46717-115">Windows 8 ou version ultérieure sur un système 64 bits avec Hyper-V installé</span><span class="sxs-lookup"><span data-stu-id="46717-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="46717-116">Pour obtenir la liste des conditions requises supplémentaires, consultez la section *configuration* requise de la page de téléchargement de [Windows Phone SDK 8,0](https://www.microsoft.com/download/details.aspx?id=35471) .</span><span class="sxs-lookup"><span data-stu-id="46717-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="46717-117">Si vous envisagez de tester la connectivité entre l’API Web et Windows Phone 8 projets sur votre système local, vous devez suivre les instructions de l’article *[connexion de l’émulateur Windows Phone 8 aux applications d’API Web sur un ordinateur local](https://go.microsoft.com/fwlink/?LinkId=324014)* pour configurer votre environnement de test.</span><span class="sxs-lookup"><span data-stu-id="46717-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="46717-118">Étape 1 : création du projet de librairie d’API Web</span><span class="sxs-lookup"><span data-stu-id="46717-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="46717-119">La première étape de ce didacticiel de bout en bout consiste à créer un projet d’API Web qui prend en charge toutes les opérations CRUD. Notez que vous allez ajouter le Windows Phone projet d’application à cette solution à l' [étape 2](#STEP2) de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="46717-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="46717-120">Ouvrez **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="46717-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="46717-121">Cliquez sur **fichier**, sur **nouveau**, puis sur **projet**.</span><span class="sxs-lookup"><span data-stu-id="46717-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="46717-122">Quand la **boîte de dialogue Nouveau projet** s’affiche, développez **installé**, puis **modèles**, puis **visuel C#** , puis **Web**.</span><span class="sxs-lookup"><span data-stu-id="46717-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="46717-123">Cliquez sur l’image à développer</span><span class="sxs-lookup"><span data-stu-id="46717-123">Click image to expand</span></span>                                                                |

4. <span data-ttu-id="46717-124">Mettez en surbrillance **application Web ASP.net**, entrez **Bookstore** comme nom de projet, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="46717-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="46717-125">Quand la boîte **de dialogue Nouveau projet ASP.net** s’affiche, sélectionnez le modèle **API Web** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="46717-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="46717-126">Cliquez sur l’image à développer</span><span class="sxs-lookup"><span data-stu-id="46717-126">Click image to expand</span></span>                                                                |

6. <span data-ttu-id="46717-127">Lorsque le projet d’API Web s’ouvre, supprimez l’exemple de contrôleur du projet :</span><span class="sxs-lookup"><span data-stu-id="46717-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="46717-128">Développez le dossier **contrôleurs** dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="46717-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="46717-129">Cliquez avec le bouton droit sur le fichier **ValuesController.cs** , puis cliquez sur **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="46717-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="46717-130">Cliquez sur **OK** lorsque vous êtes invité à confirmer la suppression.</span><span class="sxs-lookup"><span data-stu-id="46717-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="46717-131">Ajoutez un fichier de données XML au projet d’API Web. ce fichier contient le contenu du catalogue de la librairie :</span><span class="sxs-lookup"><span data-stu-id="46717-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="46717-132">Cliquez avec le bouton droit sur le dossier de données de l' **application\_** dans l’Explorateur de solutions, puis cliquez sur **Ajouter**et sur **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="46717-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="46717-133">Quand la boîte de dialogue **Ajouter un nouvel élément** s’affiche, mettez en surbrillance le modèle de **fichier XML** .</span><span class="sxs-lookup"><span data-stu-id="46717-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="46717-134">Nommez le fichier **books. xml**, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="46717-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="46717-135">Une fois le fichier **books. xml** ouvert, remplacez le code dans le fichier par le code XML du fichier Sample **books. xml** sur MSDN :</span><span class="sxs-lookup"><span data-stu-id="46717-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="46717-136">Enregistrez et fermez le fichier XML.</span><span class="sxs-lookup"><span data-stu-id="46717-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="46717-137">Ajoutez le modèle Bookstore au projet d’API Web. ce modèle contient la logique de création, lecture, mise à jour et suppression (CRUD) pour l’application Bookstore :</span><span class="sxs-lookup"><span data-stu-id="46717-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="46717-138">Cliquez avec le bouton droit sur le dossier **modèles** dans l’Explorateur de solutions, cliquez sur **Ajouter**, puis sur **classe**.</span><span class="sxs-lookup"><span data-stu-id="46717-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="46717-139">Quand la boîte de dialogue **Ajouter un nouvel élément** s’affiche, nommez le fichier de classe **BookDetails.cs**, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="46717-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="46717-140">Une fois le fichier **BookDetails.cs** ouvert, remplacez le code dans le fichier par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="46717-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="46717-141">Enregistrez et fermez le fichier **BookDetails.cs** .</span><span class="sxs-lookup"><span data-stu-id="46717-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="46717-142">Ajoutez le contrôleur Bookstore au projet d’API Web :</span><span class="sxs-lookup"><span data-stu-id="46717-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="46717-143">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier **Controllers** , puis cliquez sur **Ajouter**et sur **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="46717-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="46717-144">Quand la boîte de dialogue **Ajouter une structure** s’affiche, sélectionnez **contrôleur d’API Web 2-vide**, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="46717-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="46717-145">Quand la boîte de dialogue **Ajouter un contrôleur** s’affiche, nommez le contrôleur **BooksController**, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="46717-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="46717-146">Une fois le fichier **BooksController.cs** ouvert, remplacez le code dans le fichier par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="46717-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="46717-147">Enregistrez et fermez le fichier **BooksController.cs** .</span><span class="sxs-lookup"><span data-stu-id="46717-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="46717-148">Générez l’application API Web pour rechercher les erreurs.</span><span class="sxs-lookup"><span data-stu-id="46717-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="46717-149">Étape 2 : ajout du projet de catalogue Windows Phone 8 Bookstore</span><span class="sxs-lookup"><span data-stu-id="46717-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="46717-150">L’étape suivante de ce scénario de bout en bout consiste à créer l’application de catalogue pour Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="46717-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="46717-151">Cette application utilise le modèle d’application *Windows Phone lié* pour l’interface utilisateur par défaut, et elle utilise l’application API Web que vous avez créée à l' [étape 1](#STEP1) de ce didacticiel comme source de données.</span><span class="sxs-lookup"><span data-stu-id="46717-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="46717-152">Cliquez avec le bouton droit sur la solution **Bookstore** dans l’Explorateur de solutions, puis cliquez sur **Ajouter**, puis sur **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="46717-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="46717-153">Quand la boîte **de dialogue Nouveau projet** s’affiche, développez **installé**, puis **visuel C#** , puis **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="46717-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="46717-154">Mettez en surbrillance **Windows Phone application liée aux DataBound**, entrez **BookCatalog** pour le nom, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="46717-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="46717-155">Ajoutez le package NuGet Json.NET au projet **BookCatalog** :</span><span class="sxs-lookup"><span data-stu-id="46717-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="46717-156">Cliquez avec le bouton droit sur **références** pour le projet **BookCatalog** dans l’Explorateur de solutions, puis cliquez sur **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="46717-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="46717-157">Quand la boîte de dialogue **gérer les packages NuGet** s’affiche, développez la section **en ligne** et mettez en surbrillance **NuGet.org**.</span><span class="sxs-lookup"><span data-stu-id="46717-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="46717-158">Entrez **JSON.net** dans le champ de recherche, puis cliquez sur l’icône de recherche.</span><span class="sxs-lookup"><span data-stu-id="46717-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="46717-159">Mettez en surbrillance **JSON.net** dans les résultats de la recherche, puis cliquez sur **installer**.</span><span class="sxs-lookup"><span data-stu-id="46717-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="46717-160">Une fois l’installation terminée, cliquez sur **Fermer**.</span><span class="sxs-lookup"><span data-stu-id="46717-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="46717-161">Ajoutez le modèle **BookDetails** au projet **BookCatalog** ; contient un modèle générique de la classe Bookstore :</span><span class="sxs-lookup"><span data-stu-id="46717-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="46717-162">Cliquez avec le bouton droit sur le projet **BookCatalog** dans l’Explorateur de solutions, cliquez sur **Ajouter**, puis sur **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="46717-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="46717-163">Nommez le nouveau dossier **modèles**.</span><span class="sxs-lookup"><span data-stu-id="46717-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="46717-164">Cliquez avec le bouton droit sur le dossier **modèles** dans l’Explorateur de solutions, cliquez sur **Ajouter**, puis sur **classe**.</span><span class="sxs-lookup"><span data-stu-id="46717-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="46717-165">Quand la boîte de dialogue **Ajouter un nouvel élément** s’affiche, nommez le fichier de classe **BookDetails.cs**, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="46717-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="46717-166">Une fois le fichier **BookDetails.cs** ouvert, remplacez le code dans le fichier par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="46717-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="46717-167">Enregistrez et fermez le fichier **BookDetails.cs** .</span><span class="sxs-lookup"><span data-stu-id="46717-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="46717-168">Mettez à jour la classe **MainViewModel.cs** pour inclure la fonctionnalité de communication avec l’application de l’API Web Bookstore :</span><span class="sxs-lookup"><span data-stu-id="46717-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="46717-169">Développez le dossier **ViewModels** dans l’Explorateur de solutions, puis double-cliquez sur le fichier **MainViewModel.cs** .</span><span class="sxs-lookup"><span data-stu-id="46717-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="46717-170">Une fois le fichier **MainViewModel.cs** ouvert, remplacez le code dans le fichier par le code suivant : Notez que vous devrez mettre à jour la valeur de la constante `apiUrl` avec l’URL réelle de votre API Web :</span><span class="sxs-lookup"><span data-stu-id="46717-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="46717-171">Enregistrez et fermez le fichier **MainViewModel.cs** .</span><span class="sxs-lookup"><span data-stu-id="46717-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="46717-172">Mettez à jour le fichier **MainPage. Xaml** pour personnaliser le nom de l’application :</span><span class="sxs-lookup"><span data-stu-id="46717-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="46717-173">Double-cliquez sur le fichier **MainPage. Xaml** dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="46717-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="46717-174">Lorsque le fichier **MainPage. Xaml** est ouvert, recherchez les lignes de code suivantes :</span><span class="sxs-lookup"><span data-stu-id="46717-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="46717-175">Remplacez ces lignes par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="46717-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="46717-176">Enregistrez et fermez le fichier **MainPage. Xaml** .</span><span class="sxs-lookup"><span data-stu-id="46717-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="46717-177">Mettez à jour le fichier **DetailsPage. Xaml** pour personnaliser les éléments affichés :</span><span class="sxs-lookup"><span data-stu-id="46717-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="46717-178">Double-cliquez sur le fichier **DetailsPage. Xaml** dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="46717-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="46717-179">Lorsque le fichier **DetailsPage. Xaml** est ouvert, recherchez les lignes de code suivantes :</span><span class="sxs-lookup"><span data-stu-id="46717-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="46717-180">Remplacez ces lignes par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="46717-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="46717-181">Enregistrez et fermez le fichier **DetailsPage. Xaml** .</span><span class="sxs-lookup"><span data-stu-id="46717-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="46717-182">Générez l’application Windows Phone pour vérifier les erreurs.</span><span class="sxs-lookup"><span data-stu-id="46717-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="46717-183">Étape 3 : test de la solution de bout en bout</span><span class="sxs-lookup"><span data-stu-id="46717-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="46717-184">Comme mentionné dans la section *conditions préalables* de ce didacticiel, lorsque vous testez la connectivité entre l’API web et Windows Phone 8 projets sur votre système local, vous devez suivre les instructions de l’article *[connexion de l’émulateur Windows Phone 8 aux applications API Web sur un ordinateur local](https://go.microsoft.com/fwlink/?LinkId=324014)* pour configurer votre environnement de test.</span><span class="sxs-lookup"><span data-stu-id="46717-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="46717-185">Une fois que l’environnement de test est configuré, vous devez définir l’application Windows Phone comme projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="46717-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="46717-186">Pour ce faire, mettez en surbrillance l’application **BookCatalog** dans l’Explorateur de solutions, puis cliquez sur **définir comme projet de démarrage**:</span><span class="sxs-lookup"><span data-stu-id="46717-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="46717-187">Cliquez sur l’image à développer</span><span class="sxs-lookup"><span data-stu-id="46717-187">Click image to expand</span></span> |

<span data-ttu-id="46717-188">Quand vous appuyez sur F5, Visual Studio démarre à la fois l’émulateur Windows Phone, qui affiche une &quot;Veuillez patienter&quot; message pendant que les données d’application sont récupérées à partir de votre API Web :</span><span class="sxs-lookup"><span data-stu-id="46717-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="46717-189">Cliquez sur l’image à développer</span><span class="sxs-lookup"><span data-stu-id="46717-189">Click image to expand</span></span> |

<span data-ttu-id="46717-190">Si tout est correct, le catalogue doit s’afficher :</span><span class="sxs-lookup"><span data-stu-id="46717-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="46717-191">Cliquez sur l’image à développer</span><span class="sxs-lookup"><span data-stu-id="46717-191">Click image to expand</span></span> |

<span data-ttu-id="46717-192">Si vous appuyez sur n’importe quel titre de livre, l’application affiche la description du livre :</span><span class="sxs-lookup"><span data-stu-id="46717-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="46717-193">Cliquez sur l’image à développer</span><span class="sxs-lookup"><span data-stu-id="46717-193">Click image to expand</span></span> |

<span data-ttu-id="46717-194">Si l’application ne peut pas communiquer avec votre API Web, un message d’erreur s’affiche :</span><span class="sxs-lookup"><span data-stu-id="46717-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="46717-195">Cliquez sur l’image à développer</span><span class="sxs-lookup"><span data-stu-id="46717-195">Click image to expand</span></span> |

<span data-ttu-id="46717-196">Si vous appuyez sur le message d’erreur, tous les détails supplémentaires sur l’erreur s’affichent :</span><span class="sxs-lookup"><span data-stu-id="46717-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="46717-197">Cliquez sur l’image à développer</span><span class="sxs-lookup"><span data-stu-id="46717-197">Click image to expand</span></span>                                                                 |
