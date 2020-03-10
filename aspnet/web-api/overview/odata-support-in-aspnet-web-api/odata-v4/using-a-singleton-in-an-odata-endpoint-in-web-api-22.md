---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Créer un singleton dans OData v4 à l’aide de l’API Web 2,2 | Microsoft Docs
author: rick-anderson
description: Cette rubrique montre comment définir un singleton dans un point de terminaison OData dans l’API Web 2,2.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 218449c18759b306e425c55f8e7b573d837b4658
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622086"
---
# <a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="1e14d-103">Créer un singleton dans OData v4 à l’aide de l’API Web 2,2</span><span class="sxs-lookup"><span data-stu-id="1e14d-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="1e14d-104">par Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="1e14d-104">by Zoe Luo</span></span>

> <span data-ttu-id="1e14d-105">Traditionnellement, une entité n’est accessible que si elle était encapsulée dans un jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="1e14d-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="1e14d-106">Toutefois, OData v4 fournit deux options supplémentaires, Singleton et relation contenant-contenu, que WebAPI 2,2 prend en charge.</span><span class="sxs-lookup"><span data-stu-id="1e14d-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="1e14d-107">Cet article explique comment définir un singleton dans un point de terminaison OData dans l’API Web 2,2.</span><span class="sxs-lookup"><span data-stu-id="1e14d-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="1e14d-108">Pour plus d’informations sur ce qu’est un singleton et sur la façon dont vous pouvez l’utiliser, consultez [utilisation d’un singleton pour définir votre entité spéciale](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e14d-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="1e14d-109">Pour créer un point de terminaison OData v4 dans l’API Web, consultez [créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 2,2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="1e14d-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="1e14d-110">Nous allons créer un singleton dans votre projet d’API Web à l’aide du modèle de données suivant :</span><span class="sxs-lookup"><span data-stu-id="1e14d-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Modèle de données](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="1e14d-112">Un singleton nommé `Umbrella` sera défini en fonction du type `Company`, et un jeu d’entités nommé `Employees` sera défini en fonction du type `Employee`.</span><span class="sxs-lookup"><span data-stu-id="1e14d-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="1e14d-113">La solution utilisée dans ce didacticiel peut être téléchargée à partir de [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="1e14d-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="1e14d-114">Définir le modèle de données</span><span class="sxs-lookup"><span data-stu-id="1e14d-114">Define the data model</span></span>

1. <span data-ttu-id="1e14d-115">Définissez les types CLR.</span><span class="sxs-lookup"><span data-stu-id="1e14d-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="1e14d-116">Générez le modèle EDM basé sur les types CLR.</span><span class="sxs-lookup"><span data-stu-id="1e14d-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="1e14d-117">Ici, `builder.Singleton<Company>("Umbrella")` indique au générateur de modèles de créer un singleton nommé `Umbrella` dans le modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="1e14d-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="1e14d-118">Les métadonnées générées se présentent comme suit :</span><span class="sxs-lookup"><span data-stu-id="1e14d-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="1e14d-119">À partir des métadonnées, nous pouvons voir que la propriété de navigation `Company` dans le jeu d’entités `Employees` est liée à la `Umbrella`Singleton.</span><span class="sxs-lookup"><span data-stu-id="1e14d-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="1e14d-120">La liaison est effectuée automatiquement par `ODataConventionModelBuilder`, puisque seul `Umbrella` a le type de `Company`.</span><span class="sxs-lookup"><span data-stu-id="1e14d-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="1e14d-121">En cas d’ambiguïté dans le modèle, vous pouvez utiliser `HasSingletonBinding` pour lier explicitement une propriété de navigation à un singleton ; `HasSingletonBinding` a le même effet que l’utilisation de l’attribut `Singleton` dans la définition de type CLR :</span><span class="sxs-lookup"><span data-stu-id="1e14d-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="1e14d-122">Définir le contrôleur Singleton</span><span class="sxs-lookup"><span data-stu-id="1e14d-122">Define the singleton controller</span></span>

<span data-ttu-id="1e14d-123">À l’instar du contrôleur EntitySet, le contrôleur Singleton hérite de `ODataController`, et le nom du contrôleur Singleton doit être `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="1e14d-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="1e14d-124">Afin de gérer différents types de requêtes, les actions doivent être prédéfinies dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="1e14d-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="1e14d-125">Le **routage des attributs** est activé par défaut dans WebApi 2,2.</span><span class="sxs-lookup"><span data-stu-id="1e14d-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="1e14d-126">Par exemple, pour définir une action pour gérer l’interrogation des `Revenue` à partir d' `Company` à l’aide du routage d’attributs, utilisez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="1e14d-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="1e14d-127">Si vous n’êtes pas disposé à définir des attributs pour chaque action, définissez simplement vos actions en suivant les [conventions de routage OData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="1e14d-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="1e14d-128">Étant donné qu’une clé n’est pas requise pour l’interrogation d’un singleton, les actions définies dans le contrôleur Singleton diffèrent légèrement des actions définies dans le contrôleur EntitySet.</span><span class="sxs-lookup"><span data-stu-id="1e14d-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="1e14d-129">Pour référence, les signatures de méthode pour chaque définition d’action dans le contrôleur Singleton sont répertoriées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="1e14d-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="1e14d-130">Fondamentalement, c’est tout ce que vous devez faire côté service.</span><span class="sxs-lookup"><span data-stu-id="1e14d-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="1e14d-131">L' [exemple de projet](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contient tout le code de la solution et le client OData qui montre comment utiliser le singleton.</span><span class="sxs-lookup"><span data-stu-id="1e14d-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="1e14d-132">Le client est créé en suivant les étapes décrites dans [créer une application cliente OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="1e14d-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="1e14d-133">.</span><span class="sxs-lookup"><span data-stu-id="1e14d-133">.</span></span> 

<span data-ttu-id="1e14d-134">*Merci à Leo HU pour le contenu d’origine de cet article.*</span><span class="sxs-lookup"><span data-stu-id="1e14d-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
