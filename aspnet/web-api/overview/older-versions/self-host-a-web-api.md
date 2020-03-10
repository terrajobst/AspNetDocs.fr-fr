---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Auto-hébergement API Web ASP.NET 1 (C#)-ASP.net 4. x
author: MikeWasson
description: Le didacticiel avec code montre comment héberger une API Web dans une application console.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525087"
---
# <a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="3ad47-103">Auto-hébergement API Web ASP.NET 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="3ad47-103">Self-Host ASP.NET Web API 1 (C#)</span></span>

<span data-ttu-id="3ad47-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3ad47-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="3ad47-105">Ce didacticiel montre comment héberger une API Web dans une application console.</span><span class="sxs-lookup"><span data-stu-id="3ad47-105">This tutorial shows how to host a web API inside a console application.</span></span> <span data-ttu-id="3ad47-106">API Web ASP.NET ne requiert pas IIS.</span><span class="sxs-lookup"><span data-stu-id="3ad47-106">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="3ad47-107">Vous pouvez auto-héberger une API Web dans votre propre processus hôte.</span><span class="sxs-lookup"><span data-stu-id="3ad47-107">You can self-host a web API in your own host process.</span></span> 
> 
> <span data-ttu-id="3ad47-108">**Les nouvelles applications doivent utiliser OWIN pour l’API Web auto-hébergée.**</span><span class="sxs-lookup"><span data-stu-id="3ad47-108">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="3ad47-109">Voir [utiliser OWIN pour l’auto-hébergement API Web ASP.NET 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="3ad47-109">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3ad47-110">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="3ad47-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3ad47-111">API web 1</span><span class="sxs-lookup"><span data-stu-id="3ad47-111">Web API 1</span></span>
> - <span data-ttu-id="3ad47-112">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="3ad47-112">Visual Studio 2012</span></span>

## <a name="create-the-console-application-project"></a><span data-ttu-id="3ad47-113">Créer le projet d’application console</span><span class="sxs-lookup"><span data-stu-id="3ad47-113">Create the Console Application Project</span></span>

<span data-ttu-id="3ad47-114">Démarrez Visual Studio et sélectionnez **nouveau projet** dans la page de **démarrage** .</span><span class="sxs-lookup"><span data-stu-id="3ad47-114">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="3ad47-115">Ou, dans le menu **fichier** , sélectionnez **nouveau** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-115">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="3ad47-116">Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  .</span><span class="sxs-lookup"><span data-stu-id="3ad47-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="3ad47-117">Sous **visuel C#** , sélectionnez **Windows**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-117">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="3ad47-118">Dans la liste des modèles de projet, sélectionnez **application console**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-118">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="3ad47-119">Nommez le projet &quot;&quot; SelfHost, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-119">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="3ad47-120">Définir la version cible de .NET Framework (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="3ad47-120">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="3ad47-121">Si vous utilisez Visual Studio 2010, remplacez la version cible de .NET Framework par .NET Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="3ad47-121">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="3ad47-122">(Par défaut, le modèle de projet cible le [.NET Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="3ad47-122">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="3ad47-123">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-123">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="3ad47-124">Dans la liste déroulante **Framework cible** , remplacez la version cible de .net framework par .NET Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="3ad47-124">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="3ad47-125">Lorsque vous êtes invité à appliquer la modification, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-125">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="3ad47-126">Installer le gestionnaire de package NuGet</span><span class="sxs-lookup"><span data-stu-id="3ad47-126">Install NuGet Package Manager</span></span>

<span data-ttu-id="3ad47-127">Le gestionnaire de package NuGet est le moyen le plus simple d’ajouter les assemblys d’API Web à un projet non-ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3ad47-127">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="3ad47-128">Pour vérifier si le gestionnaire de package NuGet est installé, cliquez sur le menu **Outils** dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3ad47-128">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="3ad47-129">Si vous voyez un élément de menu appelé **Gestionnaire de package NuGet**, vous disposez du gestionnaire de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="3ad47-129">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="3ad47-130">Pour installer le gestionnaire de package NuGet :</span><span class="sxs-lookup"><span data-stu-id="3ad47-130">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="3ad47-131">Démarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3ad47-131">Start Visual Studio.</span></span>
2. <span data-ttu-id="3ad47-132">Dans le menu **Outils**, sélectionnez **Extensions et mises à jour**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-132">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="3ad47-133">Dans la boîte de dialogue **extensions et mises à jour** , sélectionnez **en ligne**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-133">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="3ad47-134">Si vous ne voyez pas « gestionnaire de package NuGet », tapez « gestionnaire de package NuGet » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="3ad47-134">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="3ad47-135">Sélectionnez le gestionnaire de package NuGet, puis cliquez sur **Télécharger**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-135">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="3ad47-136">Une fois le téléchargement terminé, vous êtes invité à installer.</span><span class="sxs-lookup"><span data-stu-id="3ad47-136">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="3ad47-137">Une fois l’installation terminée, vous pouvez être invité à redémarrer Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3ad47-137">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="3ad47-138">Ajouter le package NuGet de l’API Web</span><span class="sxs-lookup"><span data-stu-id="3ad47-138">Add the Web API NuGet Package</span></span>

<span data-ttu-id="3ad47-139">Une fois le gestionnaire de package NuGet installé, ajoutez le package d’hébergement d’API Web à votre projet.</span><span class="sxs-lookup"><span data-stu-id="3ad47-139">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="3ad47-140">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-140">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="3ad47-141">*Remarque*: Si vous ne voyez pas cet élément de menu, vérifiez que le gestionnaire de package NuGet est correctement installé.</span><span class="sxs-lookup"><span data-stu-id="3ad47-141">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="3ad47-142">Sélectionnez **gérer les packages NuGet pour la solution**</span><span class="sxs-lookup"><span data-stu-id="3ad47-142">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="3ad47-143">Dans la boîte de dialogue **gérer les packages pépite** , sélectionnez **en ligne**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-143">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="3ad47-144">Dans la zone de recherche, tapez &quot;Microsoft. AspNet. WebApi. SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="3ad47-144">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="3ad47-145">Sélectionnez le API Web ASP.NET package auto-hôte, puis cliquez sur **installer**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-145">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="3ad47-146">Une fois le package installé, cliquez sur **Fermer** pour fermer la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="3ad47-146">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="3ad47-147">Veillez à installer le package nommé Microsoft. AspNet. WebApi. SelfHost, et non AspNetWebApi. SelfHost.</span><span class="sxs-lookup"><span data-stu-id="3ad47-147">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="3ad47-148">Créer le modèle et le contrôleur</span><span class="sxs-lookup"><span data-stu-id="3ad47-148">Create the Model and Controller</span></span>

<span data-ttu-id="3ad47-149">Ce didacticiel utilise les mêmes classes de modèle et de contrôleur que le didacticiel [prise en main](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="3ad47-149">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="3ad47-150">Ajoutez une classe publique nommée `Product`.</span><span class="sxs-lookup"><span data-stu-id="3ad47-150">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="3ad47-151">Ajoutez une classe publique nommée `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="3ad47-151">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="3ad47-152">Dérivez cette classe de **System. Web. http. ApiController**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-152">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="3ad47-153">Pour plus d’informations sur le code de ce contrôleur, reportez-vous au didacticiel [prise en main](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="3ad47-153">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="3ad47-154">Ce contrôleur définit trois actions d’extraction :</span><span class="sxs-lookup"><span data-stu-id="3ad47-154">This controller defines three GET actions:</span></span>

| <span data-ttu-id="3ad47-155">URI</span><span class="sxs-lookup"><span data-stu-id="3ad47-155">URI</span></span> | <span data-ttu-id="3ad47-156">Description</span><span class="sxs-lookup"><span data-stu-id="3ad47-156">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3ad47-157">/api/products</span><span class="sxs-lookup"><span data-stu-id="3ad47-157">/api/products</span></span> | <span data-ttu-id="3ad47-158">Obtenir la liste de tous les produits.</span><span class="sxs-lookup"><span data-stu-id="3ad47-158">Get a list of all products.</span></span> |
| <span data-ttu-id="3ad47-159">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="3ad47-159">/api/products/*id*</span></span> | <span data-ttu-id="3ad47-160">Obtenir un produit par ID.</span><span class="sxs-lookup"><span data-stu-id="3ad47-160">Get a product by ID.</span></span> |
| <span data-ttu-id="3ad47-161">/API/products/ ? category =*catégorie*</span><span class="sxs-lookup"><span data-stu-id="3ad47-161">/api/products/?category=*category*</span></span> | <span data-ttu-id="3ad47-162">Obtenir une liste de produits par catégorie.</span><span class="sxs-lookup"><span data-stu-id="3ad47-162">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="3ad47-163">Héberger l’API Web</span><span class="sxs-lookup"><span data-stu-id="3ad47-163">Host the Web API</span></span>

<span data-ttu-id="3ad47-164">Ouvrez le fichier Program.cs et ajoutez les instructions using suivantes :</span><span class="sxs-lookup"><span data-stu-id="3ad47-164">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="3ad47-165">Ajoutez le code suivant à la classe **Program** .</span><span class="sxs-lookup"><span data-stu-id="3ad47-165">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="3ad47-166">Facultatif Ajouter une réservation d’espace de noms d’URL HTTP</span><span class="sxs-lookup"><span data-stu-id="3ad47-166">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="3ad47-167">Cette application écoute les `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="3ad47-167">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="3ad47-168">Par défaut, l’écoute d’une adresse HTTP particulière requiert des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="3ad47-168">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="3ad47-169">Par conséquent, lorsque vous exécutez le didacticiel, vous pouvez recevoir cette erreur : « HTTP n’a pas pu inscrire l’URL http://+:8080/» il existe deux façons d’éviter cette erreur :</span><span class="sxs-lookup"><span data-stu-id="3ad47-169">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="3ad47-170">Exécuter Visual Studio avec des autorisations d’administrateur élevées ou</span><span class="sxs-lookup"><span data-stu-id="3ad47-170">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="3ad47-171">Utilisez netsh. exe pour accorder à votre compte les autorisations nécessaires pour réserver l’URL.</span><span class="sxs-lookup"><span data-stu-id="3ad47-171">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="3ad47-172">Pour utiliser Netsh. exe, ouvrez une invite de commandes avec des privilèges d’administrateur, puis entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="3ad47-172">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="3ad47-173">où *ordinateur\nom* est votre compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3ad47-173">where *machine\username* is your user account.</span></span>

<span data-ttu-id="3ad47-174">Une fois l’auto-hébergement terminé, veillez à supprimer la réservation :</span><span class="sxs-lookup"><span data-stu-id="3ad47-174">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="3ad47-175">Appeler l’API Web à partir d’une applicationC#cliente ()</span><span class="sxs-lookup"><span data-stu-id="3ad47-175">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="3ad47-176">Nous allons écrire une application console simple qui appelle l’API Web.</span><span class="sxs-lookup"><span data-stu-id="3ad47-176">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="3ad47-177">Ajoutez un nouveau projet d’application console à la solution :</span><span class="sxs-lookup"><span data-stu-id="3ad47-177">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="3ad47-178">Dans Explorateur de solutions, cliquez avec le bouton droit sur la solution et sélectionnez **Ajouter nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-178">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="3ad47-179">Créez une nouvelle application console nommée &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="3ad47-179">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="3ad47-180">Utilisez le gestionnaire de package NuGet pour ajouter le package de bibliothèques principales API Web ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="3ad47-180">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="3ad47-181">Dans le menu Outils, sélectionnez **Gestionnaire de package NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-181">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="3ad47-182">Sélectionnez **gérer les packages NuGet pour la solution**</span><span class="sxs-lookup"><span data-stu-id="3ad47-182">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="3ad47-183">Dans la boîte de dialogue **gérer les packages NuGet** , sélectionnez **en ligne**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-183">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="3ad47-184">Dans la zone de recherche, tapez &quot;Microsoft. AspNet. WebApi. client&quot;.</span><span class="sxs-lookup"><span data-stu-id="3ad47-184">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="3ad47-185">Sélectionnez le package bibliothèques clientes de l’API Web Microsoft ASP.NET, puis cliquez sur **installer**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-185">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="3ad47-186">Ajoutez une référence dans ClientApp au projet SelfHost :</span><span class="sxs-lookup"><span data-stu-id="3ad47-186">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="3ad47-187">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet ClientApp.</span><span class="sxs-lookup"><span data-stu-id="3ad47-187">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="3ad47-188">Sélectionnez **Ajouter une référence**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-188">Select **Add Reference**.</span></span>
- <span data-ttu-id="3ad47-189">Dans la boîte de dialogue **Gestionnaire de références** , sous **solution**, sélectionnez **projets**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-189">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="3ad47-190">Sélectionnez le projet SelfHost.</span><span class="sxs-lookup"><span data-stu-id="3ad47-190">Select the SelfHost project.</span></span>
- <span data-ttu-id="3ad47-191">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-191">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="3ad47-192">Ouvrez le fichier client/programme. cs.</span><span class="sxs-lookup"><span data-stu-id="3ad47-192">Open the Client/Program.cs file.</span></span> <span data-ttu-id="3ad47-193">Ajoutez les instructions **using** suivantes :</span><span class="sxs-lookup"><span data-stu-id="3ad47-193">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="3ad47-194">Ajoutez une instance **httpclient** statique :</span><span class="sxs-lookup"><span data-stu-id="3ad47-194">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="3ad47-195">Ajoutez les méthodes suivantes pour répertorier tous les produits, répertorier un produit par ID et répertorier les produits par catégorie.</span><span class="sxs-lookup"><span data-stu-id="3ad47-195">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="3ad47-196">Chacune de ces méthodes suit le même modèle :</span><span class="sxs-lookup"><span data-stu-id="3ad47-196">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="3ad47-197">Appelez **httpclient. GetAsync** pour envoyer une requête d’extraction à l’URI approprié.</span><span class="sxs-lookup"><span data-stu-id="3ad47-197">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="3ad47-198">Appelez **HttpResponseMessage. EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-198">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="3ad47-199">Cette méthode lève une exception si l’état de la réponse HTTP est un code d’erreur.</span><span class="sxs-lookup"><span data-stu-id="3ad47-199">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="3ad47-200">Appelez **ReadAsAsync&lt;t&gt;** pour désérialiser un type CLR à partir de la réponse http.</span><span class="sxs-lookup"><span data-stu-id="3ad47-200">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="3ad47-201">Cette méthode est une méthode d’extension, définie dans **System .net. http. HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="3ad47-201">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="3ad47-202">Les méthodes **GetAsync** et **ReadAsAsync** sont toutes deux asynchrones.</span><span class="sxs-lookup"><span data-stu-id="3ad47-202">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="3ad47-203">Elles retournent des objets de **tâche** qui représentent l’opération asynchrone.</span><span class="sxs-lookup"><span data-stu-id="3ad47-203">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="3ad47-204">L’obtention de la propriété **result** bloque le thread jusqu’à ce que l’opération se termine.</span><span class="sxs-lookup"><span data-stu-id="3ad47-204">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="3ad47-205">Pour plus d’informations sur l’utilisation de HttpClient, notamment sur la façon d’effectuer des appels non bloquants, consultez [appel d’une API Web à partir d’un client .net](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="3ad47-205">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="3ad47-206">Avant d’appeler ces méthodes, définissez la propriété BaseAddress sur l’instance HttpClient sur «`http://localhost:8080`».</span><span class="sxs-lookup"><span data-stu-id="3ad47-206">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="3ad47-207">Exemple :</span><span class="sxs-lookup"><span data-stu-id="3ad47-207">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="3ad47-208">Cela doit générer le résultat suivant.</span><span class="sxs-lookup"><span data-stu-id="3ad47-208">This should output the following.</span></span> <span data-ttu-id="3ad47-209">(N’oubliez pas d’exécuter d’abord l’application SelfHost.)</span><span class="sxs-lookup"><span data-stu-id="3ad47-209">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
