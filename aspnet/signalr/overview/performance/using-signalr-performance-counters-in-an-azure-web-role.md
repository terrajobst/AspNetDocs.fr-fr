---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Utilisation des compteurs de performance Signalr dans un rôle Web Azure | Microsoft Docs
author: guardrex
description: Comment installer et utiliser les compteurs de performance Signalr dans un rôle Web Azure.
keywords: ASP. NET, signalr, compteur de performances, rôle Web Azure
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 969a2ce43a7cb8d649555daf282f900401c0c914
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578980"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="234a0-104">Utilisation des compteurs de performance Signalr dans un rôle Web Azure</span><span class="sxs-lookup"><span data-stu-id="234a0-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="234a0-105">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="234a0-105">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="234a0-106">Les compteurs de performance signalr permettent de surveiller les performances de votre application dans un rôle Web Azure.</span><span class="sxs-lookup"><span data-stu-id="234a0-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="234a0-107">Les compteurs sont capturés par diagnostics Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="234a0-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="234a0-108">Vous installez les compteurs de performance Signalr sur Azure avec *signalr. exe*, le même outil que celui utilisé pour les applications autonomes ou locales.</span><span class="sxs-lookup"><span data-stu-id="234a0-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="234a0-109">Étant donné que les rôles Azure sont temporaires, vous configurez une application pour installer et enregistrer les compteurs de performance Signalr lors du démarrage.</span><span class="sxs-lookup"><span data-stu-id="234a0-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="234a0-110">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="234a0-110">Prerequisites</span></span>

* <span data-ttu-id="234a0-111">Visual Studio 2015 ou [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span><span class="sxs-lookup"><span data-stu-id="234a0-111">Visual Studio 2015 or [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span></span>
* <span data-ttu-id="234a0-112">[Microsoft Azure SDK pour Visual Studio](https://azure.microsoft.com/downloads/) **Remarque : redémarrez votre ordinateur après avoir installé le kit de développement logiciel (SDK).**</span><span class="sxs-lookup"><span data-stu-id="234a0-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="234a0-113">Abonnement Microsoft Azure : pour vous inscrire pour obtenir un compte d’évaluation Azure gratuit, consultez la page [version d’évaluation gratuite d’Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="234a0-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="234a0-114">Création d’une application de rôle Web Azure qui expose les compteurs de performance Signalr</span><span class="sxs-lookup"><span data-stu-id="234a0-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="234a0-115">Ouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="234a0-115">Open Visual Studio.</span></span>

2. <span data-ttu-id="234a0-116">Dans Visual Studio, sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="234a0-116">In Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="234a0-117">Dans la boîte de dialogue **nouveau projet** , sélectionnez la catégorie **visuel C#**  > **Cloud** sur la gauche, puis sélectionnez le modèle de **service Cloud Azure** .</span><span class="sxs-lookup"><span data-stu-id="234a0-117">In the **New Project** dialog box, select the **Visual C#** > **Cloud** category on the left, and then select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="234a0-118">Nommez l’application **SignalRPerfCounters** et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="234a0-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Nouvelle application Cloud](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > <span data-ttu-id="234a0-120">Si vous ne voyez pas la catégorie de modèle **Cloud** ou le modèle de **service Cloud Azure** , vous devez installer la charge de travail de **développement Azure** pour Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="234a0-120">If you don't see the **Cloud** template category or the **Azure Cloud Service** template, you need to install the **Azure development** workload for Visual Studio 2017.</span></span> <span data-ttu-id="234a0-121">Cliquez sur le lien **ouvrir le Visual Studio installer** en bas à gauche de la boîte de dialogue **nouveau projet** pour ouvrir Visual Studio installer.</span><span class="sxs-lookup"><span data-stu-id="234a0-121">Choose the **Open Visual Studio Installer** link on the bottom-left side of the **New Project** dialog to open Visual Studio Installer.</span></span> <span data-ttu-id="234a0-122">Sélectionnez la charge de travail **développement Azure** , puis choisissez **modifier** pour commencer l’installation de la charge de travail.</span><span class="sxs-lookup"><span data-stu-id="234a0-122">Select the **Azure development** workload, and then choose **Modify** to start installing the workload.</span></span>
   >
   > ![Charge de travail de développement Azure dans Visual Studio Installer](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. <span data-ttu-id="234a0-124">Dans la boîte de dialogue **nouveau service Cloud Microsoft Azure** , sélectionnez **rôle Web ASP.net** et sélectionnez le bouton > pour ajouter le rôle au projet.</span><span class="sxs-lookup"><span data-stu-id="234a0-124">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="234a0-125">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="234a0-125">Select **OK**.</span></span>

   ![Ajouter un rôle Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. <span data-ttu-id="234a0-127">Dans la boîte de dialogue **nouvelle application Web ASP.net-WebRole1** , sélectionnez le modèle **MVC** , puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="234a0-127">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Ajouter MVC et l’API Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. <span data-ttu-id="234a0-129">Dans **Explorateur de solutions**, ouvrez le fichier *Diagnostics. wadcfgx* sous **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="234a0-129">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Diagnostics de Explorateur de solutions. wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. <span data-ttu-id="234a0-131">Remplacez le contenu du fichier par la configuration suivante et enregistrez le fichier :</span><span class="sxs-lookup"><span data-stu-id="234a0-131">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. <span data-ttu-id="234a0-132">Ouvrez la **console du gestionnaire de package** dans **Outils** > **Gestionnaire de package NuGet**.</span><span class="sxs-lookup"><span data-stu-id="234a0-132">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="234a0-133">Entrez les commandes suivantes pour installer la dernière version de Signalr et du package d’utilitaires Signalr :</span><span class="sxs-lookup"><span data-stu-id="234a0-133">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. <span data-ttu-id="234a0-134">Configurez l’application pour installer les compteurs de performance Signalr dans l’instance de rôle lors du démarrage ou du recyclage.</span><span class="sxs-lookup"><span data-stu-id="234a0-134">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="234a0-135">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **WebRole1** et sélectionnez **Ajouter** > **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="234a0-135">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="234a0-136">Nommez le nouveau dossier *Startup*.</span><span class="sxs-lookup"><span data-stu-id="234a0-136">Name the new folder *Startup*.</span></span>

   ![Ajouter un dossier de démarrage](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. <span data-ttu-id="234a0-138">Copiez le fichier *signalr. exe* (ajouté avec le package **Microsoft. Aspnet. signalr. utils** ) à partir du dossier du projet \<>/SignalRPerfCounters/packages/Microsoft.Aspnet.SignalR.utils.\<version >/Tools dans le dossier de *démarrage* que vous avez créé à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="234a0-138">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="234a0-139">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Startup* , puis sélectionnez **Ajouter** > **élément existant**.</span><span class="sxs-lookup"><span data-stu-id="234a0-139">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="234a0-140">Dans la boîte de dialogue qui s’affiche, sélectionnez *signalr. exe* , puis sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="234a0-140">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Ajouter signalr. exe au projet](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. <span data-ttu-id="234a0-142">Cliquez avec le bouton droit sur le dossier de *démarrage* que vous avez créé.</span><span class="sxs-lookup"><span data-stu-id="234a0-142">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="234a0-143">Sélectionnez **Ajouter** > **Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="234a0-143">Select **Add** > **New Item**.</span></span> <span data-ttu-id="234a0-144">Sélectionnez le nœud **général** , sélectionnez **fichier texte**, puis nommez le nouvel élément *SignalRPerfCounterInstall. cmd*.</span><span class="sxs-lookup"><span data-stu-id="234a0-144">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="234a0-145">Ce fichier de commandes installe les compteurs de performance Signalr dans le rôle Web.</span><span class="sxs-lookup"><span data-stu-id="234a0-145">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Créer un fichier de commandes d’installation du compteur de performance Signalr](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. <span data-ttu-id="234a0-147">Lorsque Visual Studio crée le fichier *SignalRPerfCounterInstall. cmd* , il s’ouvre automatiquement dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="234a0-147">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="234a0-148">Remplacez le contenu du fichier par le script suivant, puis enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="234a0-148">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="234a0-149">Ce script exécute *signalr. exe*, qui ajoute les compteurs de performance signalr à l’instance de rôle.</span><span class="sxs-lookup"><span data-stu-id="234a0-149">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. <span data-ttu-id="234a0-150">Sélectionnez le fichier *signalr. exe* dans **Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="234a0-150">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="234a0-151">Dans les **Propriétés**du fichier, affectez à **copier dans le répertoire de sortie** la valeur **toujours copier**.</span><span class="sxs-lookup"><span data-stu-id="234a0-151">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Définir la copie dans le répertoire de sortie sur toujours copier](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. <span data-ttu-id="234a0-153">Répétez l’étape précédente pour le fichier *SignalRPerfCounterInstall. cmd* .</span><span class="sxs-lookup"><span data-stu-id="234a0-153">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

16. <span data-ttu-id="234a0-154">Cliquez avec le bouton droit sur le fichier *SignalRPerfCounterInstall. cmd* , puis sélectionnez **Ouvrir avec**.</span><span class="sxs-lookup"><span data-stu-id="234a0-154">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="234a0-155">Dans la boîte de dialogue qui s’affiche, sélectionnez **Éditeur binaire** et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="234a0-155">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Ouvrir avec l’éditeur binaire](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. <span data-ttu-id="234a0-157">Dans l’éditeur binaire, sélectionnez des octets de début dans le fichier et supprimez-les.</span><span class="sxs-lookup"><span data-stu-id="234a0-157">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="234a0-158">Enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="234a0-158">Save and close the file.</span></span>

    ![Supprimer les octets de début](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. <span data-ttu-id="234a0-160">Ouvrez *fichier ServiceDefinition. csdef* et ajoutez une tâche de démarrage qui exécute le fichier *SignalrPerfCounterInstall. cmd* au démarrage du service :</span><span class="sxs-lookup"><span data-stu-id="234a0-160">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. <span data-ttu-id="234a0-161">Ouvrez `Views/Shared/_Layout.cshtml` et supprimez le script d’offre groupée jQuery à la fin du fichier.</span><span class="sxs-lookup"><span data-stu-id="234a0-161">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. <span data-ttu-id="234a0-162">Ajoutez un client JavaScript qui appelle en continu la méthode `increment` sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="234a0-162">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="234a0-163">Ouvrez `Views/Home/Index.cshtml` et remplacez le contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="234a0-163">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. <span data-ttu-id="234a0-164">Créez un nouveau dossier dans le projet **WebRole1** nommé *hubs*.</span><span class="sxs-lookup"><span data-stu-id="234a0-164">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="234a0-165">Cliquez avec le bouton droit sur le dossier *hubs* dans **Explorateur de solutions** , puis sélectionnez **Ajouter** > **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="234a0-165">Right-click the *Hubs* folder in **Solution Explorer** and select **Add** > **New Item**.</span></span> <span data-ttu-id="234a0-166">Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez la catégorie **Web** > **signalr** , puis sélectionnez le modèle d’élément **classe de concentrateur signalr (v2)** .</span><span class="sxs-lookup"><span data-stu-id="234a0-166">In the **Add New Item** dialog box, select the **Web** > **SignalR** category, and then select the **SignalR Hub Class (v2)** item template.</span></span> <span data-ttu-id="234a0-167">Nommez le nouveau hub *MyHub.cs* et sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="234a0-167">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Ajout de la classe de concentrateur Signalr au dossier hubs dans la boîte de dialogue Ajouter un nouvel élément](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="234a0-169">*MyHub.cs* s’ouvre automatiquement dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="234a0-169">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="234a0-170">Remplacez le contenu par le code suivant, puis enregistrez et fermez le fichier :</span><span class="sxs-lookup"><span data-stu-id="234a0-170">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. <span data-ttu-id="234a0-171">*[Manivelle. exe](signalr-connection-density-testing-with-crank.md)* est un outil de test de densité de connexion fourni avec le code base de signalr.</span><span class="sxs-lookup"><span data-stu-id="234a0-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="234a0-172">Étant donné que manivelle requiert une connexion permanente, vous en ajoutez une à votre site pour l’utiliser lors du test.</span><span class="sxs-lookup"><span data-stu-id="234a0-172">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="234a0-173">Ajoutez un nouveau dossier au projet **WebRole1** appelé *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="234a0-173">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="234a0-174">Cliquez avec le bouton droit sur ce dossier, puis sélectionnez **ajouter** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="234a0-174">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="234a0-175">Nommez le nouveau fichier de classe *MyPersistentConnections.cs* et sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="234a0-175">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="234a0-176">Visual Studio ouvre le fichier *MyPersistentConnections.cs* dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="234a0-176">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="234a0-177">Remplacez le contenu par le code suivant, puis enregistrez et fermez le fichier :</span><span class="sxs-lookup"><span data-stu-id="234a0-177">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. <span data-ttu-id="234a0-178">À l’aide de la classe `Startup`, les objets Signalr démarrent au démarrage de OWIN.</span><span class="sxs-lookup"><span data-stu-id="234a0-178">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="234a0-179">Ouvrez ou créez *Startup.cs* et remplacez le contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="234a0-179">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    <span data-ttu-id="234a0-180">Dans le code ci-dessus, l’attribut `OwinStartup` marque cette classe pour démarrer OWIN.</span><span class="sxs-lookup"><span data-stu-id="234a0-180">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="234a0-181">La méthode `Configuration` démarre Signalr.</span><span class="sxs-lookup"><span data-stu-id="234a0-181">The `Configuration` method starts SignalR.</span></span>

26. <span data-ttu-id="234a0-182">Testez votre application dans le Microsoft Azure Emulator en appuyant sur **F5**.</span><span class="sxs-lookup"><span data-stu-id="234a0-182">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="234a0-183">Si vous rencontrez un **FileLoadException** sur **MapSignalR**, modifiez les redirections de liaison dans *Web. config* comme suit :</span><span class="sxs-lookup"><span data-stu-id="234a0-183">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. <span data-ttu-id="234a0-184">Attendez environ une minute.</span><span class="sxs-lookup"><span data-stu-id="234a0-184">Wait about one minute.</span></span> <span data-ttu-id="234a0-185">Ouvrez la fenêtre outil Cloud Explorer dans Visual Studio (**affichez** > **Cloud Explorer**) et développez le chemin `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="234a0-185">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="234a0-186">Double-cliquez sur **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="234a0-186">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="234a0-187">Vous devez voir les compteurs Signalr dans les données de la table.</span><span class="sxs-lookup"><span data-stu-id="234a0-187">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="234a0-188">Si vous ne voyez pas la table, vous devrez peut-être entrer à nouveau vos informations d’identification Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="234a0-188">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="234a0-189">Vous devrez peut-être sélectionner le bouton **Actualiser** pour afficher la table dans **Cloud Explorer** ou sélectionner le bouton **Actualiser** dans la fenêtre ouvrir une table pour afficher les données dans la table.</span><span class="sxs-lookup"><span data-stu-id="234a0-189">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Sélection de la table des compteurs de performance WAD dans Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Indication des compteurs collectés dans le tableau des compteurs de performance WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. <span data-ttu-id="234a0-192">Pour tester votre application dans le Cloud, mettez à jour le fichier **ServiceConfiguration. Cloud. cscfg** et définissez la `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` sur une chaîne de connexion de compte de stockage Azure valide.</span><span class="sxs-lookup"><span data-stu-id="234a0-192">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="234a0-193">Déployez l’application dans votre abonnement Azure.</span><span class="sxs-lookup"><span data-stu-id="234a0-193">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="234a0-194">Pour plus d’informations sur le déploiement d’une application sur Azure, consultez [comment créer et déployer un service Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="234a0-194">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="234a0-195">Patientez quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="234a0-195">Wait a few minutes.</span></span> <span data-ttu-id="234a0-196">Dans **Cloud Explorer**, recherchez le compte de stockage que vous avez configuré ci-dessus et recherchez la table `WADPerformanceCountersTable`.</span><span class="sxs-lookup"><span data-stu-id="234a0-196">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="234a0-197">Vous devez voir les compteurs Signalr dans les données de la table.</span><span class="sxs-lookup"><span data-stu-id="234a0-197">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="234a0-198">Si vous ne voyez pas la table, vous devrez peut-être entrer à nouveau vos informations d’identification Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="234a0-198">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="234a0-199">Vous devrez peut-être sélectionner le bouton **Actualiser** pour afficher la table dans **Cloud Explorer** ou sélectionner le bouton **Actualiser** dans la fenêtre ouvrir une table pour afficher les données dans la table.</span><span class="sxs-lookup"><span data-stu-id="234a0-199">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="234a0-200">Merci à [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) pour le contenu d’origine utilisé dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="234a0-200">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
