---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: Le Open Data Protocol (OData) est un protocole d’accès aux données pour le Web. OData offre une méthode uniforme pour interroger et manipuler des jeux de données à l’aide d’opérations CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 81d134cbd3231b9a0d5537ccbd1bbfe6419254af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598734"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a><span data-ttu-id="d28ec-104">Créer un point de terminaison OData v4 à l’aide de API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d28ec-104">Create an OData v4 Endpoint Using ASP.NET Web API</span></span> 

> <span data-ttu-id="d28ec-105">Le Open Data Protocol (OData) est un protocole d’accès aux données pour le Web.</span><span class="sxs-lookup"><span data-stu-id="d28ec-105">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="d28ec-106">OData offre un moyen uniforme d’interroger et de manipuler des jeux de données via des opérations CRUD (création, lecture, mise à jour et suppression).</span><span class="sxs-lookup"><span data-stu-id="d28ec-106">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
>
> <span data-ttu-id="d28ec-107">API Web ASP.NET prend en charge v3 et v4 du protocole.</span><span class="sxs-lookup"><span data-stu-id="d28ec-107">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="d28ec-108">Vous pouvez même avoir un point de terminaison v4 qui s’exécute côte à côte avec un point de terminaison v3.</span><span class="sxs-lookup"><span data-stu-id="d28ec-108">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
>
> <span data-ttu-id="d28ec-109">Ce didacticiel montre comment créer un point de terminaison OData v4 qui prend en charge les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="d28ec-109">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d28ec-110">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="d28ec-110">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="d28ec-111">API Web 5,2</span><span class="sxs-lookup"><span data-stu-id="d28ec-111">Web API 5.2</span></span>
> - <span data-ttu-id="d28ec-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="d28ec-112">OData v4</span></span>
> - <span data-ttu-id="d28ec-113">Visual Studio 2017 (Télécharger Visual Studio 2017 [ici](https://visualstudio.microsoft.com/downloads/))</span><span class="sxs-lookup"><span data-stu-id="d28ec-113">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/))</span></span>
> - <span data-ttu-id="d28ec-114">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="d28ec-114">Entity Framework 6</span></span>
> - <span data-ttu-id="d28ec-115">4\.7.2 .NET</span><span class="sxs-lookup"><span data-stu-id="d28ec-115">.NET 4.7.2</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="d28ec-116">Versions du didacticiel</span><span class="sxs-lookup"><span data-stu-id="d28ec-116">Tutorial versions</span></span>
>
> <span data-ttu-id="d28ec-117">Pour OData version 3, consultez [création d’un point de terminaison OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="d28ec-117">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="d28ec-118">Créer le projet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d28ec-118">Create the Visual Studio Project</span></span>

<span data-ttu-id="d28ec-119">Dans Visual Studio, dans le menu **fichier** , sélectionnez **nouveau** &gt; **projet**.</span><span class="sxs-lookup"><span data-stu-id="d28ec-119">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="d28ec-120">Développez **installé** &gt;  **C# Visual** &gt; **Web**, puis sélectionnez le modèle **application Web ASP.net (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="d28ec-120">Expand **Installed** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application (.NET Framework)** template.</span></span> <span data-ttu-id="d28ec-121">Nommez le projet &quot;&quot;ProductService.</span><span class="sxs-lookup"><span data-stu-id="d28ec-121">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

<span data-ttu-id="d28ec-122">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="d28ec-122">Select **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

<span data-ttu-id="d28ec-123">Sélectionnez le modèle **Vide**.</span><span class="sxs-lookup"><span data-stu-id="d28ec-123">Select the **Empty** template.</span></span> <span data-ttu-id="d28ec-124">Sous **Ajouter des dossiers et des références principales pour :** , sélectionnez **API Web**.</span><span class="sxs-lookup"><span data-stu-id="d28ec-124">Under **Add folders and core references for:**, select **Web API**.</span></span> <span data-ttu-id="d28ec-125">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="d28ec-125">Select **OK**.</span></span>

## <a name="install-the-odata-packages"></a><span data-ttu-id="d28ec-126">Installer les packages OData</span><span class="sxs-lookup"><span data-stu-id="d28ec-126">Install the OData packages</span></span>

<span data-ttu-id="d28ec-127">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** &gt; **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="d28ec-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="d28ec-128">Dans la fenêtre de la console du gestionnaire de package, tapez :</span><span class="sxs-lookup"><span data-stu-id="d28ec-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="d28ec-129">Cette commande installe les derniers packages NuGet OData.</span><span class="sxs-lookup"><span data-stu-id="d28ec-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="d28ec-130">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="d28ec-130">Add a model class</span></span>

<span data-ttu-id="d28ec-131">Un *modèle* est un objet qui représente une entité de données dans votre application.</span><span class="sxs-lookup"><span data-stu-id="d28ec-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="d28ec-132">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier Modèles.</span><span class="sxs-lookup"><span data-stu-id="d28ec-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="d28ec-133">Dans le menu contextuel, sélectionnez **ajouter** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="d28ec-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="d28ec-134">Par Convention, les classes de modèle sont placées dans le dossier Models, mais vous n’êtes pas obligé de suivre cette Convention dans vos propres projets.</span><span class="sxs-lookup"><span data-stu-id="d28ec-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>

<span data-ttu-id="d28ec-135">Nommez la classe `Product`.</span><span class="sxs-lookup"><span data-stu-id="d28ec-135">Name the class `Product`.</span></span> <span data-ttu-id="d28ec-136">Dans le fichier Product.cs, remplacez le code réutilisable par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="d28ec-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="d28ec-137">La propriété `Id` est la clé d’entité.</span><span class="sxs-lookup"><span data-stu-id="d28ec-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="d28ec-138">Les clients peuvent interroger les entités par clé.</span><span class="sxs-lookup"><span data-stu-id="d28ec-138">Clients can query entities by key.</span></span> <span data-ttu-id="d28ec-139">Par exemple, pour obtenir le produit avec l’ID 5, l’URI est `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="d28ec-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="d28ec-140">La propriété `Id` est également la clé primaire dans la base de données principale.</span><span class="sxs-lookup"><span data-stu-id="d28ec-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="d28ec-141">Activer Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d28ec-141">Enable Entity Framework</span></span>

<span data-ttu-id="d28ec-142">Pour ce didacticiel, nous allons utiliser Entity Framework (EF) Code First pour créer la base de données principale.</span><span class="sxs-lookup"><span data-stu-id="d28ec-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="d28ec-143">L’API Web OData ne requiert pas EF.</span><span class="sxs-lookup"><span data-stu-id="d28ec-143">Web API OData does not require EF.</span></span> <span data-ttu-id="d28ec-144">Utilisez n’importe quelle couche d’accès aux données capable de convertir les entités de base de données en modèles.</span><span class="sxs-lookup"><span data-stu-id="d28ec-144">Use any data-access layer that can translate database entities into models.</span></span>

<span data-ttu-id="d28ec-145">Tout d’abord, installez le package NuGet pour EF.</span><span class="sxs-lookup"><span data-stu-id="d28ec-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="d28ec-146">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** &gt; **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="d28ec-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="d28ec-147">Dans la fenêtre de la console du gestionnaire de package, tapez :</span><span class="sxs-lookup"><span data-stu-id="d28ec-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="d28ec-148">Ouvrez le fichier Web. config et ajoutez la section suivante à l’intérieur de l’élément de **configuration** , après l’élément **configSections** .</span><span class="sxs-lookup"><span data-stu-id="d28ec-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="d28ec-149">Ce paramètre ajoute une chaîne de connexion pour une base de données de base de données locale.</span><span class="sxs-lookup"><span data-stu-id="d28ec-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="d28ec-150">Cette base de données sera utilisée lorsque vous exécuterez l’application localement.</span><span class="sxs-lookup"><span data-stu-id="d28ec-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="d28ec-151">Ensuite, ajoutez une classe nommée `ProductsContext` au dossier Models :</span><span class="sxs-lookup"><span data-stu-id="d28ec-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="d28ec-152">Dans le constructeur, `"name=ProductsContext"` donne le nom de la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="d28ec-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="d28ec-153">Configurer le point de terminaison OData</span><span class="sxs-lookup"><span data-stu-id="d28ec-153">Configure the OData endpoint</span></span>

<span data-ttu-id="d28ec-154">Ouvrez le fichier d’application\_Start/WebApiConfig. cs.</span><span class="sxs-lookup"><span data-stu-id="d28ec-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="d28ec-155">Ajoutez les instructions **using** suivantes :</span><span class="sxs-lookup"><span data-stu-id="d28ec-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="d28ec-156">Ajoutez ensuite le code suivant à la méthode **Register** :</span><span class="sxs-lookup"><span data-stu-id="d28ec-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="d28ec-157">Ce code fait deux choses :</span><span class="sxs-lookup"><span data-stu-id="d28ec-157">This code does two things:</span></span>

- <span data-ttu-id="d28ec-158">Crée un Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="d28ec-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="d28ec-159">Ajoute un itinéraire.</span><span class="sxs-lookup"><span data-stu-id="d28ec-159">Adds a route.</span></span>

<span data-ttu-id="d28ec-160">Un modèle EDM est un modèle abstrait de données.</span><span class="sxs-lookup"><span data-stu-id="d28ec-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="d28ec-161">Le modèle EDM est utilisé pour créer le document de métadonnées du service.</span><span class="sxs-lookup"><span data-stu-id="d28ec-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="d28ec-162">La classe **ODataConventionModelBuilder** crée un modèle EDM à l’aide des conventions d’affectation des noms par défaut.</span><span class="sxs-lookup"><span data-stu-id="d28ec-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="d28ec-163">Cette approche nécessite le moins de code.</span><span class="sxs-lookup"><span data-stu-id="d28ec-163">This approach requires the least code.</span></span> <span data-ttu-id="d28ec-164">Si vous souhaitez davantage de contrôle sur le modèle EDM, vous pouvez utiliser la classe **ODataModelBuilder** pour créer le modèle EDM en ajoutant explicitement des propriétés, des clés et des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="d28ec-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="d28ec-165">Un *itinéraire* indique à l’API Web comment acheminer les requêtes http vers le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="d28ec-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="d28ec-166">Pour créer un itinéraire OData v4, appelez la méthode d’extension **MapODataServiceRoute** .</span><span class="sxs-lookup"><span data-stu-id="d28ec-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="d28ec-167">Si votre application comporte plusieurs points de terminaison OData, créez un itinéraire distinct pour chacun d’entre eux.</span><span class="sxs-lookup"><span data-stu-id="d28ec-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="d28ec-168">Donnez à chaque itinéraire un nom et un préfixe d’itinéraire uniques.</span><span class="sxs-lookup"><span data-stu-id="d28ec-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="d28ec-169">Ajouter le contrôleur OData</span><span class="sxs-lookup"><span data-stu-id="d28ec-169">Add the OData controller</span></span>

<span data-ttu-id="d28ec-170">Un *contrôleur* est une classe qui gère les requêtes http.</span><span class="sxs-lookup"><span data-stu-id="d28ec-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="d28ec-171">Vous créez un contrôleur distinct pour chaque jeu d’entités dans votre service OData.</span><span class="sxs-lookup"><span data-stu-id="d28ec-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="d28ec-172">Dans ce didacticiel, vous allez créer un contrôleur pour l’entité `Product`.</span><span class="sxs-lookup"><span data-stu-id="d28ec-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="d28ec-173">Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers et sélectionnez **ajouter** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="d28ec-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="d28ec-174">Nommez la classe `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d28ec-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="d28ec-175">La version de ce didacticiel pour OData v3 utilise la génération de modèles automatique de **contrôleur Add** .</span><span class="sxs-lookup"><span data-stu-id="d28ec-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="d28ec-176">Actuellement, il n’existe pas de génération de modèles automatique pour OData v4.</span><span class="sxs-lookup"><span data-stu-id="d28ec-176">Currently, there is no scaffolding for OData v4.</span></span>

<span data-ttu-id="d28ec-177">Remplacez le code réutilisable dans ProductsController.cs par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="d28ec-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="d28ec-178">Le contrôleur utilise la classe `ProductsContext` pour accéder à la base de données à l’aide d’EF.</span><span class="sxs-lookup"><span data-stu-id="d28ec-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="d28ec-179">Notez que le contrôleur remplace la méthode **dispose** pour supprimer **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="d28ec-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="d28ec-180">Il s’agit du point de départ du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d28ec-180">This is the starting point for the controller.</span></span> <span data-ttu-id="d28ec-181">Nous allons ensuite ajouter des méthodes pour toutes les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="d28ec-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="query-the-entity-set"></a><span data-ttu-id="d28ec-182">Interroger le jeu d’entités</span><span class="sxs-lookup"><span data-stu-id="d28ec-182">Query the entity set</span></span>

<span data-ttu-id="d28ec-183">Ajoutez les méthodes suivantes à `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d28ec-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="d28ec-184">La version sans paramètre de la méthode `Get` retourne l’ensemble de la collection Products.</span><span class="sxs-lookup"><span data-stu-id="d28ec-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="d28ec-185">La méthode `Get` avec un paramètre de *clé* recherche un produit par sa clé (dans ce cas, la propriété `Id`).</span><span class="sxs-lookup"><span data-stu-id="d28ec-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="d28ec-186">L’attribut **[EnableQuery]** permet aux clients de modifier la requête, en utilisant des options de requête telles que $filter, $sort et $page.</span><span class="sxs-lookup"><span data-stu-id="d28ec-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="d28ec-187">Pour plus d’informations, consultez [prise en charge des options de requête OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="d28ec-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="add-an-entity-to-the-entity-set"></a><span data-ttu-id="d28ec-188">Ajouter une entité au jeu d’entités</span><span class="sxs-lookup"><span data-stu-id="d28ec-188">Add an entity to the entity set</span></span>

<span data-ttu-id="d28ec-189">Pour permettre aux clients d’ajouter un nouveau produit à la base de données, ajoutez la méthode suivante à `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d28ec-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a><span data-ttu-id="d28ec-190">Mise à jour d'une entité</span><span class="sxs-lookup"><span data-stu-id="d28ec-190">Update an entity</span></span>

<span data-ttu-id="d28ec-191">OData prend en charge deux sémantiques différentes pour la mise à jour d’une entité, d’un correctif et d’un PUT.</span><span class="sxs-lookup"><span data-stu-id="d28ec-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="d28ec-192">PATCH effectue une mise à jour partielle.</span><span class="sxs-lookup"><span data-stu-id="d28ec-192">PATCH performs a partial update.</span></span> <span data-ttu-id="d28ec-193">Le client spécifie uniquement les propriétés à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="d28ec-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="d28ec-194">PUT remplace l’entité entière.</span><span class="sxs-lookup"><span data-stu-id="d28ec-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="d28ec-195">L’inconvénient de PUT est que le client doit envoyer des valeurs pour toutes les propriétés de l’entité, y compris les valeurs qui ne changent pas.</span><span class="sxs-lookup"><span data-stu-id="d28ec-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="d28ec-196">La [spécification OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) indique que le correctif est préféré.</span><span class="sxs-lookup"><span data-stu-id="d28ec-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="d28ec-197">Dans tous les cas, voici le code pour les méthodes PATCH et PUT :</span><span class="sxs-lookup"><span data-stu-id="d28ec-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="d28ec-198">Dans le cas de PATCH, le contrôleur utilise le type **Delta&lt;t&gt;** pour suivre les modifications.</span><span class="sxs-lookup"><span data-stu-id="d28ec-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="delete-an-entity"></a><span data-ttu-id="d28ec-199">Suppression d’une entité</span><span class="sxs-lookup"><span data-stu-id="d28ec-199">Delete an entity</span></span>

<span data-ttu-id="d28ec-200">Pour permettre aux clients de supprimer un produit de la base de données, ajoutez la méthode suivante à `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d28ec-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
