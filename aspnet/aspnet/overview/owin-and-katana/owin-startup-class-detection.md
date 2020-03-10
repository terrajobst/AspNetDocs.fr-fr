---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Détection de la classe de démarrage OWIN | Microsoft Docs
author: Praburaj
description: Ce didacticiel montre comment configurer la classe de démarrage OWIN chargée. Pour plus d’informations sur OWIN, consultez vue d’ensemble du projet Katana. Ce didacticiel était...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: e1670c32049ec33dd4d1941a091a429d3929c452
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617046"
---
# <a name="owin-startup-class-detection"></a><span data-ttu-id="464ed-105">Détection de classe de démarrage OWIN</span><span class="sxs-lookup"><span data-stu-id="464ed-105">OWIN Startup Class Detection</span></span>

> <span data-ttu-id="464ed-106">Ce didacticiel montre comment configurer la classe de démarrage OWIN chargée.</span><span class="sxs-lookup"><span data-stu-id="464ed-106">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="464ed-107">Pour plus d’informations sur OWIN, consultez [vue d’ensemble du projet Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="464ed-107">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="464ed-108">Ce didacticiel a été rédigé par Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan et Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="464ed-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
>
> ## <a name="prerequisites"></a><span data-ttu-id="464ed-109">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="464ed-109">Prerequisites</span></span>
>
> [<span data-ttu-id="464ed-110">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="464ed-110">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)

## <a name="owin-startup-class-detection"></a><span data-ttu-id="464ed-111">Détection de classe de démarrage OWIN</span><span class="sxs-lookup"><span data-stu-id="464ed-111">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="464ed-112">Chaque application OWIN a une classe Startup dans laquelle vous spécifiez les composants du pipeline d’application.</span><span class="sxs-lookup"><span data-stu-id="464ed-112">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="464ed-113">Vous pouvez connecter votre classe Startup à l’exécution de différentes manières, selon le modèle d’hébergement que vous choisissez (OwinHost, IIS et IIS-Express).</span><span class="sxs-lookup"><span data-stu-id="464ed-113">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="464ed-114">La classe Startup présentée dans ce didacticiel peut être utilisée dans toutes les applications d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="464ed-114">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="464ed-115">Vous connectez la classe Startup au runtime d’hébergement à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="464ed-115">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>

1. <span data-ttu-id="464ed-116">**Convention d’affectation de noms**: Katana recherche une classe nommée `Startup` dans l’espace de noms correspondant au nom de l’assembly ou à l’espace de noms global.</span><span class="sxs-lookup"><span data-stu-id="464ed-116">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="464ed-117">**Attribut OwinStartup**: il s’agit de l’approche adoptée par la plupart des développeurs pour spécifier la classe Startup.</span><span class="sxs-lookup"><span data-stu-id="464ed-117">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="464ed-118">L’attribut suivant définit la classe Startup sur la classe `TestStartup` de l’espace de noms `StartupDemo`.</span><span class="sxs-lookup"><span data-stu-id="464ed-118">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="464ed-119">L’attribut `OwinStartup` remplace la Convention d’affectation de noms.</span><span class="sxs-lookup"><span data-stu-id="464ed-119">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="464ed-120">Vous pouvez également spécifier un nom convivial avec cet attribut. Toutefois, l’utilisation d’un nom convivial nécessite également l’utilisation de l’élément `appSetting` dans le fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="464ed-120">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="464ed-121">**Élément AppSetting dans le fichier de configuration**: l’élément `appSetting` remplace l’attribut `OwinStartup` et la Convention d’affectation des noms.</span><span class="sxs-lookup"><span data-stu-id="464ed-121">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="464ed-122">Vous pouvez avoir plusieurs classes de démarrage (chacune utilisant un attribut `OwinStartup`) et configurer la classe de démarrage qui sera chargée dans un fichier de configuration à l’aide d’un balisage similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="464ed-122">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="464ed-123">La clé suivante, qui spécifie explicitement la classe de démarrage et l’assembly peut également être utilisée :</span><span class="sxs-lookup"><span data-stu-id="464ed-123">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="464ed-124">Le code XML suivant dans le fichier de configuration spécifie un nom de classe de démarrage convivial de `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="464ed-124">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="464ed-125">Le balisage ci-dessus doit être utilisé avec l’attribut `OwinStartup` suivant qui spécifie un nom convivial et provoque l’exécution de la classe `ProductionStartup2`.</span><span class="sxs-lookup"><span data-stu-id="464ed-125">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="464ed-126">Pour désactiver la découverte du démarrage OWIN, ajoutez la `appSetting owin:AutomaticAppStartup` avec la valeur `"false"` dans le fichier Web. config.</span><span class="sxs-lookup"><span data-stu-id="464ed-126">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="464ed-127">Créer une application Web ASP.NET à l’aide du démarrage OWIN</span><span class="sxs-lookup"><span data-stu-id="464ed-127">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="464ed-128">Créez une application Web Asp.Net vide et nommez-la **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="464ed-128">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="464ed-129">-Installer `Microsoft.Owin.Host.SystemWeb` à l’aide du gestionnaire de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="464ed-129">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="464ed-130">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="464ed-130">From the **Tools** menu, select **NuGet Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="464ed-131">Entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="464ed-131">Enter the following command:</span></span>

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="464ed-132">Ajoutez une classe de démarrage OWIN.</span><span class="sxs-lookup"><span data-stu-id="464ed-132">Add an OWIN startup class.</span></span> <span data-ttu-id="464ed-133">Dans Visual Studio 2017, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter une classe**.-dans la boîte de dialogue **Ajouter un nouvel élément** , entrez *OWIN* dans le champ de recherche, puis remplacez le nom par Startup.cs, puis sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="464ed-133">In Visual Studio 2017 right-click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then select **Add**.</span></span>

     ![](owin-startup-class-detection/_static/image1.png)

   <span data-ttu-id="464ed-134">La prochaine fois que vous voudrez ajouter une *classe de démarrage Owin*, celle-ci sera disponible dans le menu **Ajouter** .</span><span class="sxs-lookup"><span data-stu-id="464ed-134">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>

     ![](owin-startup-class-detection/_static/image2.png)

   <span data-ttu-id="464ed-135">Vous pouvez également cliquer avec le bouton droit sur le projet, sélectionner **Ajouter**, puis sélectionner **nouvel élément**, puis sélectionner la **classe de démarrage Owin**.</span><span class="sxs-lookup"><span data-stu-id="464ed-135">Alternatively, you can right-click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>

     ![](owin-startup-class-detection/_static/image3.png)

- <span data-ttu-id="464ed-136">Remplacez le code généré dans le fichier *Startup.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="464ed-136">Replace the generated code in the *Startup.cs* file with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  <span data-ttu-id="464ed-137">L’expression lambda `app.Use` est utilisée pour inscrire le composant d’intergiciel (middleware) spécifié dans le pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="464ed-137">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="464ed-138">Dans ce cas, nous allons configurer la journalisation des demandes entrantes avant de répondre à la demande entrante.</span><span class="sxs-lookup"><span data-stu-id="464ed-138">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="464ed-139">Le paramètre `next` est le délégué ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt;) au composant suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="464ed-139">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="464ed-140">L’expression lambda `app.Run` raccorde le pipeline aux demandes entrantes et fournit le mécanisme de réponse.</span><span class="sxs-lookup"><span data-stu-id="464ed-140">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="464ed-141">Dans le code ci-dessus, nous avons commenté l’attribut `OwinStartup` et nous nous appuyons sur la Convention d’exécution de la classe nommée `Startup`.-Appuyez sur ***F5*** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="464ed-141">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="464ed-142">L’actualisation a été atteinte plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="464ed-142">Hit refresh a few times.</span></span>

    <span data-ttu-id="464ed-143">![](owin-startup-class-detection/_static/image4.png) Remarque : le nombre indiqué dans les images de ce didacticiel ne correspond pas au nombre que vous voyez.</span><span class="sxs-lookup"><span data-stu-id="464ed-143">![](owin-startup-class-detection/_static/image4.png) Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="464ed-144">La chaîne de millisecondes est utilisée pour afficher une nouvelle réponse quand vous actualisez la page.</span><span class="sxs-lookup"><span data-stu-id="464ed-144">The millisecond string is used to show a new response when you refresh the page.</span></span>
  <span data-ttu-id="464ed-145">Vous pouvez voir les informations de trace dans la fenêtre **sortie** .</span><span class="sxs-lookup"><span data-stu-id="464ed-145">You can see the trace information in the **Output** window.</span></span>

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="464ed-146">Ajouter d’autres classes de démarrage</span><span class="sxs-lookup"><span data-stu-id="464ed-146">Add More Startup Classes</span></span>

<span data-ttu-id="464ed-147">Dans cette section, nous allons ajouter une autre classe de démarrage.</span><span class="sxs-lookup"><span data-stu-id="464ed-147">In this section we'll add another Startup class.</span></span> <span data-ttu-id="464ed-148">Vous pouvez ajouter plusieurs classes de démarrage OWIN à votre application.</span><span class="sxs-lookup"><span data-stu-id="464ed-148">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="464ed-149">Par exemple, vous souhaiterez peut-être créer des classes de démarrage pour le développement, les tests et la production.</span><span class="sxs-lookup"><span data-stu-id="464ed-149">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="464ed-150">Créez une nouvelle classe de démarrage OWIN et nommez-la `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="464ed-150">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="464ed-151">Remplacez le code généré par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="464ed-151">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="464ed-152">Appuyez sur CTRL + F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="464ed-152">Press Control F5 to run the app.</span></span> <span data-ttu-id="464ed-153">L’attribut `OwinStartup` spécifie la classe de démarrage de production qui est exécutée.</span><span class="sxs-lookup"><span data-stu-id="464ed-153">The `OwinStartup` attribute specifies the production startup class is run.</span></span>

    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="464ed-154">Créez une autre classe de démarrage OWIN et nommez-la `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="464ed-154">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="464ed-155">Remplacez le code généré par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="464ed-155">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="464ed-156">La surcharge d’attribut `OwinStartup` ci-dessus spécifie `TestingConfiguration` comme nom *convivial* de la classe Startup.</span><span class="sxs-lookup"><span data-stu-id="464ed-156">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="464ed-157">Ouvrez le fichier *Web. config* et ajoutez la clé de démarrage de l’application OWIN qui spécifie le nom convivial de la classe Startup :</span><span class="sxs-lookup"><span data-stu-id="464ed-157">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="464ed-158">Appuyez sur CTRL + F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="464ed-158">Press Control F5 to run the app.</span></span> <span data-ttu-id="464ed-159">L’élément paramètres de l’application prend l’antécédent et la configuration de test est exécutée.</span><span class="sxs-lookup"><span data-stu-id="464ed-159">The app settings element takes precedent, and the test configuration is run.</span></span>

    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="464ed-160">Supprimez le nom *convivial* de l’attribut `OwinStartup` dans la classe `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="464ed-160">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="464ed-161">Remplacez la clé de démarrage de l’application OWIN dans le fichier *Web. config* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="464ed-161">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="464ed-162">Rétablissez l’attribut `OwinStartup` de chaque classe sur le code d’attribut par défaut généré par Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="464ed-162">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="464ed-163">Chacune des clés de démarrage d’application OWIN ci-dessous entraîne l’exécution de la classe de production.</span><span class="sxs-lookup"><span data-stu-id="464ed-163">Each of the OWIN App startup keys below will cause the production class to run.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="464ed-164">La dernière clé de démarrage spécifie la méthode de configuration de démarrage.</span><span class="sxs-lookup"><span data-stu-id="464ed-164">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="464ed-165">La clé de démarrage de l’application OWIN suivante vous permet de modifier le nom de la classe de configuration pour `MyConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="464ed-165">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="464ed-166">Utilisation de Owinhost. exe</span><span class="sxs-lookup"><span data-stu-id="464ed-166">Using Owinhost.exe</span></span>

1. <span data-ttu-id="464ed-167">Remplacez le fichier Web. config par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="464ed-167">Replace the Web.config file with the following markup:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="464ed-168">La dernière clé gagne, donc dans ce cas `TestStartup` est spécifié.</span><span class="sxs-lookup"><span data-stu-id="464ed-168">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="464ed-169">Installez Owinhost à partir de l’PMC :</span><span class="sxs-lookup"><span data-stu-id="464ed-169">Install Owinhost from the PMC:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="464ed-170">Accédez au dossier d’application (le dossier contenant le fichier *Web. config* ) et, à l’invite de commandes, tapez :</span><span class="sxs-lookup"><span data-stu-id="464ed-170">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="464ed-171">La fenêtre de commande affiche :</span><span class="sxs-lookup"><span data-stu-id="464ed-171">The command window will show:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="464ed-172">Lancez un navigateur avec l’URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="464ed-172">Launch a browser with the URL `http://localhost:5000/`.</span></span>

    ![](owin-startup-class-detection/_static/image8.png)

   <span data-ttu-id="464ed-173">OwinHost a respecté les conventions de démarrage indiquées ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="464ed-173">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="464ed-174">Dans la fenêtre commande, appuyez sur entrée pour quitter OwinHost.</span><span class="sxs-lookup"><span data-stu-id="464ed-174">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="464ed-175">Dans la classe `ProductionStartup`, ajoutez l’attribut OwinStartup suivant qui spécifie un nom convivial de *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="464ed-175">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="464ed-176">À l’invite de commandes et tapez :</span><span class="sxs-lookup"><span data-stu-id="464ed-176">In the command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="464ed-177">La classe de démarrage de production est chargée.</span><span class="sxs-lookup"><span data-stu-id="464ed-177">The Production startup class is loaded.</span></span>
    ![](owin-startup-class-detection/_static/image9.png)

   <span data-ttu-id="464ed-178">Notre application possède plusieurs classes de démarrage et, dans cet exemple, nous avons différé la classe de démarrage à charger jusqu’à l’exécution.</span><span class="sxs-lookup"><span data-stu-id="464ed-178">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="464ed-179">Testez les options de démarrage du runtime suivantes :</span><span class="sxs-lookup"><span data-stu-id="464ed-179">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
