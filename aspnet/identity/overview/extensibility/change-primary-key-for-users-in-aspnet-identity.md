---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Modifier la clé primaire pour les utilisateurs dans ASP.NET Identity-ASP.NET 4. x
author: Rick-Anderson
description: Dans Visual Studio 2013, l’application Web par défaut utilise une valeur de chaîne pour la clé pour les comptes d’utilisateur. ASP.NET Identity vous permet de modifier le type de...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0afea8eacfc646f1489b87629fdb2d437815d88c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584461"
---
# <a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="14637-104">Changer la clé principale pour les utilisateurs dans ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="14637-104">Change Primary Key for Users in ASP.NET Identity</span></span>

<span data-ttu-id="14637-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="14637-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="14637-106">Dans Visual Studio 2013, l’application Web par défaut utilise une valeur de chaîne pour la clé pour les comptes d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="14637-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="14637-107">ASP.NET Identity vous permet de modifier le type de la clé pour répondre à vos besoins en matière de données.</span><span class="sxs-lookup"><span data-stu-id="14637-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="14637-108">Par exemple, vous pouvez modifier le type de la clé d’une chaîne en entier.</span><span class="sxs-lookup"><span data-stu-id="14637-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="14637-109">Cette rubrique montre comment démarrer avec l’application Web par défaut et remplacer la clé de compte d’utilisateur par un entier.</span><span class="sxs-lookup"><span data-stu-id="14637-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="14637-110">Vous pouvez utiliser les mêmes modifications pour implémenter n’importe quel type de clé dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="14637-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="14637-111">Il montre comment effectuer ces modifications dans l’application Web par défaut, mais vous pouvez appliquer des modifications similaires à une application personnalisée.</span><span class="sxs-lookup"><span data-stu-id="14637-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="14637-112">Il présente les modifications nécessaires lors de l’utilisation de MVC ou de Web Forms.</span><span class="sxs-lookup"><span data-stu-id="14637-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="14637-113">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="14637-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="14637-114">Visual Studio 2013 avec Update 2 (ou version ultérieure)</span><span class="sxs-lookup"><span data-stu-id="14637-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="14637-115">ASP.NET Identity 2,1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="14637-115">ASP.NET Identity 2.1 or later</span></span>

<span data-ttu-id="14637-116">Pour effectuer les étapes de ce didacticiel, vous devez disposer de Visual Studio 2013 Update 2 (ou version ultérieure) et d’une application Web créée à partir du modèle d’application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="14637-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="14637-117">Le modèle a été modifié dans Update 3.</span><span class="sxs-lookup"><span data-stu-id="14637-117">The template changed in Update 3.</span></span> <span data-ttu-id="14637-118">Cette rubrique montre comment modifier le modèle dans Update 2 et Update 3.</span><span class="sxs-lookup"><span data-stu-id="14637-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="14637-119">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="14637-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="14637-120">Modifier le type de la clé dans la classe d’utilisateur d’identité</span><span class="sxs-lookup"><span data-stu-id="14637-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="14637-121">Ajouter des classes d’identité personnalisées qui utilisent le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="14637-122">Modifier la classe de contexte et le gestionnaire des utilisateurs pour utiliser le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="14637-123">Modifier la configuration de démarrage pour utiliser le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="14637-124">Pour MVC avec Update 2, modifiez AccountController pour qu’il passe le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="14637-125">Pour MVC avec Update 3, modifiez AccountController et ManageController pour qu’il passe le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="14637-126">Pour Web Forms avec Update 2, modifiez les pages de compte pour qu’elles passent le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="14637-127">Pour Web Forms avec Update 3, modifiez les pages de compte pour qu’elles passent le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="14637-128">Exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="14637-128">Run application</span></span>](#run)
- [<span data-ttu-id="14637-129">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="14637-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="14637-130">Modifier le type de la clé dans la classe d’utilisateur d’identité</span><span class="sxs-lookup"><span data-stu-id="14637-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="14637-131">Dans votre projet créé à partir du modèle d’application Web ASP.NET, spécifiez que la classe ApplicationUser utilise un entier pour la clé pour les comptes d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="14637-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="14637-132">Dans IdentityModels.cs, modifiez la classe ApplicationUser pour qu’elle hérite de IdentityUser qui a un type **int** pour le paramètre générique TKey.</span><span class="sxs-lookup"><span data-stu-id="14637-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="14637-133">Vous devez également transmettre les noms de trois classes personnalisées que vous n’avez pas encore implémentées.</span><span class="sxs-lookup"><span data-stu-id="14637-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="14637-134">Vous avez modifié le type de la clé, mais par défaut, le reste de l’application suppose toujours que la clé est une chaîne.</span><span class="sxs-lookup"><span data-stu-id="14637-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="14637-135">Vous devez indiquer explicitement le type de la clé dans le code qui suppose une chaîne.</span><span class="sxs-lookup"><span data-stu-id="14637-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="14637-136">Dans la classe **ApplicationUser** , modifiez la méthode **GenerateUserIdentityAsync** pour inclure int, comme indiqué dans le code en surbrillance ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="14637-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="14637-137">Cette modification n’est pas nécessaire pour les projets Web Forms avec le modèle Update 3.</span><span class="sxs-lookup"><span data-stu-id="14637-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="14637-138">Ajouter des classes d’identité personnalisées qui utilisent le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="14637-139">Les autres classes d’identité, telles que IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, au rolestore, sont toujours configurées pour utiliser une clé de chaîne.</span><span class="sxs-lookup"><span data-stu-id="14637-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="14637-140">Créez de nouvelles versions de ces classes qui spécifient un entier pour la clé.</span><span class="sxs-lookup"><span data-stu-id="14637-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="14637-141">Vous n’avez pas besoin de fournir un code d’implémentation très important dans ces classes, vous définissez principalement int comme clé.</span><span class="sxs-lookup"><span data-stu-id="14637-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="14637-142">Ajoutez les classes suivantes à votre fichier IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="14637-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="14637-143">Modifier la classe de contexte et le gestionnaire des utilisateurs pour utiliser le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="14637-144">Dans IdentityModels.cs, modifiez la définition de la classe **ApplicationDbContext** pour utiliser vos nouvelles classes personnalisées et un **int** pour la clé, comme indiqué dans le code en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="14637-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="14637-145">Le paramètre ThrowIfV1Schema n’est plus valide dans le constructeur.</span><span class="sxs-lookup"><span data-stu-id="14637-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="14637-146">Modifiez le constructeur pour qu’il ne passe pas une valeur ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="14637-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="14637-147">Ouvrez IdentityConfig.cs, puis modifiez la classe **ApplicationUserManger** pour utiliser votre nouvelle classe de magasin utilisateur pour rendre les données persistantes et un **int** pour la clé.</span><span class="sxs-lookup"><span data-stu-id="14637-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="14637-148">Dans le modèle Update 3, vous devez modifier la classe ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="14637-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="14637-149">Modifier la configuration de démarrage pour utiliser le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="14637-150">Dans Startup.Auth.cs, remplacez le code OnValidateIdentity, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="14637-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="14637-151">Notez que la définition getUserIdCallback analyse la valeur de chaîne en un entier.</span><span class="sxs-lookup"><span data-stu-id="14637-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="14637-152">Si votre projet ne reconnaît pas l’implémentation générique de la méthode **GetUserId** , vous devrez peut-être mettre à jour le package NuGet ASP.net Identity vers la version 2,1</span><span class="sxs-lookup"><span data-stu-id="14637-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="14637-153">Vous avez apporté de nombreuses modifications aux classes d’infrastructure utilisées par ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="14637-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="14637-154">Si vous essayez de compiler le projet, vous remarquerez un grand nombre d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="14637-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="14637-155">Heureusement, les erreurs restantes sont toutes similaires.</span><span class="sxs-lookup"><span data-stu-id="14637-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="14637-156">La classe Identity attend un entier pour la clé, mais le contrôleur (ou le Web Form) passe une valeur de chaîne.</span><span class="sxs-lookup"><span data-stu-id="14637-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="14637-157">Dans chaque cas, vous devez convertir une chaîne en et un entier en appelant **GetUserId&lt;int&gt;** .</span><span class="sxs-lookup"><span data-stu-id="14637-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="14637-158">Vous pouvez soit utiliser la liste d’erreurs à partir de la compilation, soit suivre les modifications ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="14637-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="14637-159">Les modifications restantes dépendent du type de projet que vous créez et de la mise à jour que vous avez installée dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="14637-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="14637-160">Vous pouvez accéder directement à la section appropriée à l’aide des liens suivants</span><span class="sxs-lookup"><span data-stu-id="14637-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="14637-161">Pour MVC avec Update 2, modifiez AccountController pour qu’il passe le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="14637-162">Pour MVC avec Update 3, modifiez AccountController et ManageController pour qu’il passe le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="14637-163">Pour Web Forms avec Update 2, modifiez les pages de compte pour qu’elles passent le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="14637-164">Pour Web Forms avec Update 3, modifiez les pages de compte pour qu’elles passent le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="14637-165">Pour MVC avec Update 2, modifiez AccountController pour qu’il passe le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="14637-166">Ouvrez le fichier AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="14637-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="14637-167">Vous devez modifier les méthodes suivantes.</span><span class="sxs-lookup"><span data-stu-id="14637-167">You need to change the following methods.</span></span>

<span data-ttu-id="14637-168">Méthode **ConfirmEmail**</span><span class="sxs-lookup"><span data-stu-id="14637-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="14637-169">**Dissocier** la méthode</span><span class="sxs-lookup"><span data-stu-id="14637-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="14637-170">Méthode **Manage (ManageUserViewModel)**</span><span class="sxs-lookup"><span data-stu-id="14637-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="14637-171">Méthode **LinkLoginCallback**</span><span class="sxs-lookup"><span data-stu-id="14637-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="14637-172">Méthode **RemoveAccountList**</span><span class="sxs-lookup"><span data-stu-id="14637-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="14637-173">Méthode **HasPassword**</span><span class="sxs-lookup"><span data-stu-id="14637-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="14637-174">Vous pouvez maintenant [exécuter l’application](#run) et inscrire un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="14637-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="14637-175">Pour MVC avec Update 3, modifiez AccountController et ManageController pour qu’il passe le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="14637-176">Ouvrez le fichier AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="14637-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="14637-177">Vous devez modifier la méthode suivante.</span><span class="sxs-lookup"><span data-stu-id="14637-177">You need to change the following method.</span></span>

<span data-ttu-id="14637-178">Méthode **ConfirmEmail**</span><span class="sxs-lookup"><span data-stu-id="14637-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="14637-179">Méthode **SendCode**</span><span class="sxs-lookup"><span data-stu-id="14637-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="14637-180">Ouvrez le fichier ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="14637-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="14637-181">Vous devez modifier les méthodes suivantes.</span><span class="sxs-lookup"><span data-stu-id="14637-181">You need to change the following methods.</span></span>

<span data-ttu-id="14637-182">**Index** , méthode</span><span class="sxs-lookup"><span data-stu-id="14637-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="14637-183">Méthodes **RemoveLogin**</span><span class="sxs-lookup"><span data-stu-id="14637-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="14637-184">Méthode **AddPhoneNumber**</span><span class="sxs-lookup"><span data-stu-id="14637-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="14637-185">Méthode **EnableTwoFactorAuthentication**</span><span class="sxs-lookup"><span data-stu-id="14637-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="14637-186">Méthode **DisableTwoFactorAuthentication**</span><span class="sxs-lookup"><span data-stu-id="14637-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="14637-187">Méthodes **VerifyPhoneNumber**</span><span class="sxs-lookup"><span data-stu-id="14637-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="14637-188">Méthode **RemovePhoneNumber**</span><span class="sxs-lookup"><span data-stu-id="14637-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="14637-189">**ChangePassword** , méthode</span><span class="sxs-lookup"><span data-stu-id="14637-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="14637-190">**SetPassword** , méthode</span><span class="sxs-lookup"><span data-stu-id="14637-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="14637-191">Méthode **ManageLogins**</span><span class="sxs-lookup"><span data-stu-id="14637-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="14637-192">Méthode **LinkLoginCallback**</span><span class="sxs-lookup"><span data-stu-id="14637-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="14637-193">Méthode **HasPassword**</span><span class="sxs-lookup"><span data-stu-id="14637-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="14637-194">Méthode **HasPhoneNumber**</span><span class="sxs-lookup"><span data-stu-id="14637-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="14637-195">Vous pouvez maintenant [exécuter l’application](#run) et inscrire un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="14637-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="14637-196">Pour Web Forms avec Update 2, modifiez les pages de compte pour qu’elles passent le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="14637-197">Pour Web Forms avec Update 2, vous devez modifier les pages suivantes.</span><span class="sxs-lookup"><span data-stu-id="14637-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="14637-198">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="14637-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="14637-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="14637-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="14637-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="14637-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="14637-201">Vous pouvez maintenant [exécuter l’application](#run) et inscrire un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="14637-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="14637-202">Pour Web Forms avec Update 3, modifiez les pages de compte pour qu’elles passent le type de clé</span><span class="sxs-lookup"><span data-stu-id="14637-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="14637-203">Pour Web Forms avec Update 3, vous devez modifier les pages suivantes.</span><span class="sxs-lookup"><span data-stu-id="14637-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="14637-204">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="14637-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="14637-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="14637-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="14637-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="14637-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="14637-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="14637-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="14637-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="14637-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="14637-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="14637-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="14637-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="14637-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="14637-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="14637-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="14637-212">Exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="14637-212">Run application</span></span>

<span data-ttu-id="14637-213">Vous avez terminé toutes les modifications requises pour le modèle d’application Web par défaut.</span><span class="sxs-lookup"><span data-stu-id="14637-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="14637-214">Exécutez l’application et inscrivez un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="14637-214">Run the application and register a new user.</span></span> <span data-ttu-id="14637-215">Après avoir inscrit l’utilisateur, vous remarquerez que la table AspNetUsers a une colonne ID qui est un entier.</span><span class="sxs-lookup"><span data-stu-id="14637-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![nouvelle clé primaire](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="14637-217">Si vous avez déjà créé les tables ASP.NET Identity avec une clé primaire différente, vous devez apporter des modifications supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="14637-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="14637-218">Si possible, supprimez simplement la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="14637-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="14637-219">La base de données sera recréée avec la conception correcte lorsque vous exécuterez l’application Web et ajouterez un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="14637-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="14637-220">Si la suppression n’est pas possible, exécutez code First migrations pour modifier les tables.</span><span class="sxs-lookup"><span data-stu-id="14637-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="14637-221">Toutefois, la nouvelle clé primaire d’entier n’est pas configurée en tant que propriété d’identité SQL dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="14637-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="14637-222">Vous devez définir manuellement la colonne ID en tant qu’identité.</span><span class="sxs-lookup"><span data-stu-id="14637-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="14637-223">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="14637-223">Other resources</span></span>

- [<span data-ttu-id="14637-224">Vue d’ensemble des fournisseurs de stockage personnalisés pour ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="14637-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="14637-225">Migration d’un site web existant de l’appartenance SQL vers ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="14637-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="14637-226">Migration des données du fournisseur universel pour les profils d’appartenance et utilisateur vers ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="14637-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="14637-227">[Exemple d’application](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) avec la clé primaire modifiée</span><span class="sxs-lookup"><span data-stu-id="14637-227">[Sample application](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) with changed primary key</span></span>
