---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-Host ASP.NET Web API 1 (C#)-ASP.NET 4.x
author: MikeWasson
description: Didacticiel avec le code montre comment héberger une API web à l’intérieur d’une application console.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65134760"
---
# <a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="e0eee-103">Self-Host ASP.NET Web API 1 (c#)</span><span class="sxs-lookup"><span data-stu-id="e0eee-103">Self-Host ASP.NET Web API 1 (C#)</span></span>

<span data-ttu-id="e0eee-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e0eee-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e0eee-105">Ce didacticiel montre comment héberger une API web à l’intérieur d’une application console.</span><span class="sxs-lookup"><span data-stu-id="e0eee-105">This tutorial shows how to host a web API inside a console application.</span></span> <span data-ttu-id="e0eee-106">API Web ASP.NET ne nécessite pas d’IIS.</span><span class="sxs-lookup"><span data-stu-id="e0eee-106">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="e0eee-107">Vous pouvez Self-host une API web dans votre propre processus hôte.</span><span class="sxs-lookup"><span data-stu-id="e0eee-107">You can self-host a web API in your own host process.</span></span> 
> 
> <span data-ttu-id="e0eee-108">**Nouvelles applications doivent utiliser OWIN pour auto-héberger API Web.**</span><span class="sxs-lookup"><span data-stu-id="e0eee-108">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="e0eee-109">Consultez [utiliser OWIN pour auto-héberger ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e0eee-109">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e0eee-110">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="e0eee-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e0eee-111">Web API 1</span><span class="sxs-lookup"><span data-stu-id="e0eee-111">Web API 1</span></span>
> - <span data-ttu-id="e0eee-112">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e0eee-112">Visual Studio 2012</span></span>

## <a name="create-the-console-application-project"></a><span data-ttu-id="e0eee-113">Créer le projet d’Application Console</span><span class="sxs-lookup"><span data-stu-id="e0eee-113">Create the Console Application Project</span></span>

<span data-ttu-id="e0eee-114">Démarrez Visual Studio et sélectionnez **nouveau projet** à partir de la **Démarrer** page.</span><span class="sxs-lookup"><span data-stu-id="e0eee-114">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="e0eee-115">Ou, à partir de la **fichier** menu, sélectionnez **New** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-115">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="e0eee-116">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud.</span><span class="sxs-lookup"><span data-stu-id="e0eee-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="e0eee-117">Sous **Visual C#**, sélectionnez **Windows**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-117">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="e0eee-118">Dans la liste des modèles de projet, sélectionnez **Application Console**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-118">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="e0eee-119">Nommez le projet &quot;SelfHost&quot; et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-119">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="e0eee-120">Définir le Framework cible (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="e0eee-120">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="e0eee-121">Si vous utilisez Visual Studio 2010, modifier le framework cible .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="e0eee-121">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="e0eee-122">(Par défaut, le modèle de projet cible le [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="e0eee-122">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="e0eee-123">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-123">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="e0eee-124">Dans le **framework cible** liste déroulante liste, modifier le framework cible .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="e0eee-124">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="e0eee-125">Lorsque vous y êtes invité pour appliquer la modification, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-125">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="e0eee-126">Installer le Gestionnaire de Package NuGet</span><span class="sxs-lookup"><span data-stu-id="e0eee-126">Install NuGet Package Manager</span></span>

<span data-ttu-id="e0eee-127">Le Gestionnaire de Package NuGet est le moyen le plus simple pour ajouter les assemblys de l’API Web à un projet non-ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e0eee-127">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="e0eee-128">Pour vérifier si le Gestionnaire de Package NuGet est installé, cliquez sur le **outils** menu dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0eee-128">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="e0eee-129">Si vous voyez un menu appelé **Gestionnaire de Package NuGet**, vous devez le Gestionnaire de Package NuGet.</span><span class="sxs-lookup"><span data-stu-id="e0eee-129">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="e0eee-130">Pour installer le Gestionnaire de Package NuGet :</span><span class="sxs-lookup"><span data-stu-id="e0eee-130">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="e0eee-131">Démarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0eee-131">Start Visual Studio.</span></span>
2. <span data-ttu-id="e0eee-132">À partir de la **outils** menu, sélectionnez **Extensions et mises à jour**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-132">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="e0eee-133">Dans le **Extensions et mises à jour** boîte de dialogue, sélectionnez **Online**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-133">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="e0eee-134">Si vous ne voyez pas « Gestionnaire de Package NuGet », tapez « Gestionnaire de package nuget » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="e0eee-134">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="e0eee-135">Sélectionnez le Gestionnaire de Package NuGet, cliquez sur **télécharger**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-135">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="e0eee-136">Une fois le téléchargement terminé, vous seront invités à installer.</span><span class="sxs-lookup"><span data-stu-id="e0eee-136">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="e0eee-137">Une fois l’installation terminée, vous pouvez être invité à redémarrer Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0eee-137">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="e0eee-138">Ajouter le Package API NuGet Web</span><span class="sxs-lookup"><span data-stu-id="e0eee-138">Add the Web API NuGet Package</span></span>

<span data-ttu-id="e0eee-139">Une fois que le Gestionnaire de Package NuGet est installé, ajoutez le package d’auto-héberger des API Web à votre projet.</span><span class="sxs-lookup"><span data-stu-id="e0eee-139">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="e0eee-140">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-140">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="e0eee-141">*Remarque* : Si ne vous voyez pas ce menu item, assurez-vous que ce gestionnaire de Package NuGet installé correctement.</span><span class="sxs-lookup"><span data-stu-id="e0eee-141">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="e0eee-142">Sélectionnez **gérer les Packages NuGet pour la Solution**</span><span class="sxs-lookup"><span data-stu-id="e0eee-142">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="e0eee-143">Dans le **gérer les Packages NuGet** boîte de dialogue, sélectionnez **Online**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-143">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="e0eee-144">Dans la zone de recherche, tapez &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="e0eee-144">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="e0eee-145">Sélectionnez le package ASP.NET Web API Self hôte et cliquez sur **installer**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-145">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="e0eee-146">Une fois le package installé, cliquez sur **fermer** pour fermer la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="e0eee-146">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="e0eee-147">Veillez à installer le package nommé Microsoft.AspNet.WebApi.SelfHost, AspNetWebApi.SelfHost pas.</span><span class="sxs-lookup"><span data-stu-id="e0eee-147">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="e0eee-148">Créer le modèle et le contrôleur</span><span class="sxs-lookup"><span data-stu-id="e0eee-148">Create the Model and Controller</span></span>

<span data-ttu-id="e0eee-149">Ce didacticiel utilise les mêmes classes de modèle et du contrôleur en tant que le [mise en route](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) didacticiel.</span><span class="sxs-lookup"><span data-stu-id="e0eee-149">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="e0eee-150">Ajoutez une classe publique nommée `Product`.</span><span class="sxs-lookup"><span data-stu-id="e0eee-150">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="e0eee-151">Ajoutez une classe publique nommée `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="e0eee-151">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="e0eee-152">Cette classe dérive **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-152">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="e0eee-153">Pour plus d’informations sur le code dans ce contrôleur, consultez le [mise en route](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) didacticiel.</span><span class="sxs-lookup"><span data-stu-id="e0eee-153">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="e0eee-154">Ce contrôleur définit trois actions GET :</span><span class="sxs-lookup"><span data-stu-id="e0eee-154">This controller defines three GET actions:</span></span>

| <span data-ttu-id="e0eee-155">URI</span><span class="sxs-lookup"><span data-stu-id="e0eee-155">URI</span></span> | <span data-ttu-id="e0eee-156">Description</span><span class="sxs-lookup"><span data-stu-id="e0eee-156">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e0eee-157">/ api/produits</span><span class="sxs-lookup"><span data-stu-id="e0eee-157">/api/products</span></span> | <span data-ttu-id="e0eee-158">Obtenir une liste de tous les produits.</span><span class="sxs-lookup"><span data-stu-id="e0eee-158">Get a list of all products.</span></span> |
| <span data-ttu-id="e0eee-159">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="e0eee-159">/api/products/*id*</span></span> | <span data-ttu-id="e0eee-160">Obtenir un produit par ID.</span><span class="sxs-lookup"><span data-stu-id="e0eee-160">Get a product by ID.</span></span> |
| <span data-ttu-id="e0eee-161">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="e0eee-161">/api/products/?category=*category*</span></span> | <span data-ttu-id="e0eee-162">Obtenir la liste des produits par catégorie.</span><span class="sxs-lookup"><span data-stu-id="e0eee-162">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="e0eee-163">Héberger l’API Web</span><span class="sxs-lookup"><span data-stu-id="e0eee-163">Host the Web API</span></span>

<span data-ttu-id="e0eee-164">Ouvrez le fichier Program.cs et ajoutez le code suivant à l’aide d’instructions :</span><span class="sxs-lookup"><span data-stu-id="e0eee-164">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="e0eee-165">Ajoutez le code suivant à la **programme** classe.</span><span class="sxs-lookup"><span data-stu-id="e0eee-165">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="e0eee-166">(Facultatif) Ajouter une réservation Namespace d’URL HTTP</span><span class="sxs-lookup"><span data-stu-id="e0eee-166">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="e0eee-167">Cette application écoute `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="e0eee-167">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="e0eee-168">Par défaut, à l’écoute sur une adresse HTTP particulière nécessite des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="e0eee-168">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="e0eee-169">Par conséquent, lorsque vous exécutez le didacticiel, vous pouvez recevoir cette erreur : « HTTP n’a pas pu inscrire URL http://+:8080/« il existe deux façons d’éviter cette erreur :</span><span class="sxs-lookup"><span data-stu-id="e0eee-169">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="e0eee-170">Exécuter Visual Studio avec des autorisations d’administrateur avec élévation de privilèges, ou</span><span class="sxs-lookup"><span data-stu-id="e0eee-170">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="e0eee-171">Utilisez Netsh.exe pour accorder des autorisations de votre compte pour réserver l’URL.</span><span class="sxs-lookup"><span data-stu-id="e0eee-171">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="e0eee-172">Pour utiliser Netsh.exe, ouvrez une invite de commandes avec des privilèges d’administrateur et entrez la commande suivante de la commande : suivantes :</span><span class="sxs-lookup"><span data-stu-id="e0eee-172">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="e0eee-173">où *ORDINATEUR\nom d’utilisateur* est votre compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e0eee-173">where *machine\username* is your user account.</span></span>

<span data-ttu-id="e0eee-174">Lorsque vous avez terminé d’auto-hébergement, veillez à supprimer la réservation :</span><span class="sxs-lookup"><span data-stu-id="e0eee-174">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="e0eee-175">Appeler l’API Web à partir d’une Application cliente (c#)</span><span class="sxs-lookup"><span data-stu-id="e0eee-175">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="e0eee-176">Nous allons écrire une application console simple qui appelle l’API web.</span><span class="sxs-lookup"><span data-stu-id="e0eee-176">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="e0eee-177">Ajouter un nouveau projet d’application console à la solution :</span><span class="sxs-lookup"><span data-stu-id="e0eee-177">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="e0eee-178">Dans l’Explorateur de solutions, cliquez sur la solution et sélectionnez **ajouter un nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-178">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="e0eee-179">Créer une nouvelle application console nommée &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="e0eee-179">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="e0eee-180">Utilisez le Gestionnaire de Package NuGet pour ajouter le package de bibliothèques de Core ASP.NET Web API :</span><span class="sxs-lookup"><span data-stu-id="e0eee-180">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="e0eee-181">Dans le menu Outils, sélectionnez **Gestionnaire de Package NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-181">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="e0eee-182">Sélectionnez **gérer les Packages NuGet pour la Solution**</span><span class="sxs-lookup"><span data-stu-id="e0eee-182">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="e0eee-183">Dans le **gérer les Packages NuGet** boîte de dialogue, sélectionnez **Online**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-183">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="e0eee-184">Dans la zone de recherche, tapez &quot;Microsoft.AspNet.WebApi.Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="e0eee-184">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="e0eee-185">Sélectionnez le package de bibliothèques clientes de Microsoft ASP.NET Web API et cliquez sur **installer**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-185">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="e0eee-186">Ajoutez une référence dans ClientApp au projet SelfHost :</span><span class="sxs-lookup"><span data-stu-id="e0eee-186">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="e0eee-187">Dans l’Explorateur de solutions, cliquez sur le projet ClientApp.</span><span class="sxs-lookup"><span data-stu-id="e0eee-187">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="e0eee-188">Sélectionnez **Ajouter une référence**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-188">Select **Add Reference**.</span></span>
- <span data-ttu-id="e0eee-189">Dans le **Gestionnaire de références** boîte de dialogue, sous **Solution**, sélectionnez **projets**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-189">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="e0eee-190">Sélectionnez le projet SelfHost.</span><span class="sxs-lookup"><span data-stu-id="e0eee-190">Select the SelfHost project.</span></span>
- <span data-ttu-id="e0eee-191">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-191">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="e0eee-192">Ouvrez le fichier Client/Program.cs.</span><span class="sxs-lookup"><span data-stu-id="e0eee-192">Open the Client/Program.cs file.</span></span> <span data-ttu-id="e0eee-193">Ajoutez le code suivant **à l’aide de** instruction :</span><span class="sxs-lookup"><span data-stu-id="e0eee-193">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="e0eee-194">Ajouter un mappage statique **HttpClient** instance :</span><span class="sxs-lookup"><span data-stu-id="e0eee-194">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="e0eee-195">Ajoutez les méthodes suivantes pour répertorier tous les produits, un produit par ID de liste et répertorient les produits par catégorie.</span><span class="sxs-lookup"><span data-stu-id="e0eee-195">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="e0eee-196">Chacune de ces méthodes suit le même modèle :</span><span class="sxs-lookup"><span data-stu-id="e0eee-196">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="e0eee-197">Appelez **HttpClient.GetAsync** pour envoyer une demande GET à l’URI approprié.</span><span class="sxs-lookup"><span data-stu-id="e0eee-197">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="e0eee-198">Appelez **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-198">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="e0eee-199">Cette méthode lève une exception si l’état de réponse HTTP est un code d’erreur.</span><span class="sxs-lookup"><span data-stu-id="e0eee-199">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="e0eee-200">Appelez **ReadAsAsync&lt;T&gt;**  pour désérialiser un type CLR de la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="e0eee-200">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="e0eee-201">Cette méthode est une méthode d’extension définie dans **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="e0eee-201">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="e0eee-202">Le **GetAsync** et **ReadAsAsync** méthodes sont toutes les deux asynchrones.</span><span class="sxs-lookup"><span data-stu-id="e0eee-202">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="e0eee-203">Elles retournent **tâche** objets qui représentent l’opération asynchrone.</span><span class="sxs-lookup"><span data-stu-id="e0eee-203">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="e0eee-204">Obtention de la **résultat** propriété bloque le thread jusqu'à ce que l’opération se termine.</span><span class="sxs-lookup"><span data-stu-id="e0eee-204">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="e0eee-205">Pour plus d’informations sur l’utilisation de HttpClient, y compris comment effectuer des appels non bloquant, consultez [appelant une Web API à partir d’un Client .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="e0eee-205">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="e0eee-206">Avant d’appeler ces méthodes, définissez la propriété BaseAddress sur l’instance de HttpClient pour «`http://localhost:8080`».</span><span class="sxs-lookup"><span data-stu-id="e0eee-206">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="e0eee-207">Exemple :</span><span class="sxs-lookup"><span data-stu-id="e0eee-207">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="e0eee-208">Cela doit générer ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="e0eee-208">This should output the following.</span></span> <span data-ttu-id="e0eee-209">(N’oubliez pas d’exécuter d’abord l’application SelfHost).</span><span class="sxs-lookup"><span data-stu-id="e0eee-209">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
