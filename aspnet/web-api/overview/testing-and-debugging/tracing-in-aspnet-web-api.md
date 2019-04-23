---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Le traçage dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Montre comment activer le suivi dans l’API Web ASP.NET.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a01acb649556d06ab9828ceab0fcbdf363bbc0d1
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59405896"
---
# <a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="42271-103">Le traçage dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="42271-103">Tracing in ASP.NET Web API 2</span></span>

<span data-ttu-id="42271-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="42271-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="42271-105">Lorsque vous essayez de déboguer une application basée sur le web, rien ne vaut pour un bon jeu de journaux de suivi.</span><span class="sxs-lookup"><span data-stu-id="42271-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="42271-106">Ce didacticiel montre comment activer le suivi dans l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="42271-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="42271-107">Vous pouvez utiliser cette fonctionnalité pour effectuer le suivi de ce que fait l’infrastructure API Web avant et après que qu’il appelle votre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="42271-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="42271-108">Vous pouvez également l’utiliser pour effectuer le suivi de votre propre code.</span><span class="sxs-lookup"><span data-stu-id="42271-108">You can also use it to trace your own code.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="42271-109">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="42271-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="42271-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (fonctionne également avec Visual Studio 2015)</span><span class="sxs-lookup"><span data-stu-id="42271-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="42271-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="42271-111">Web API 2</span></span>
> - [<span data-ttu-id="42271-112">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="42271-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="42271-113">Activer System.Diagnostics Tracing in Web API</span><span class="sxs-lookup"><span data-stu-id="42271-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="42271-114">Tout d’abord, nous allons créer un nouveau projet d’Application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="42271-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="42271-115">Dans Visual Studio, à partir de la **fichier** menu, sélectionnez **New** > **projet**.</span><span class="sxs-lookup"><span data-stu-id="42271-115">In Visual Studio, from the **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="42271-116">Sous **modèles**, **Web**, sélectionnez **Application Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="42271-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="42271-117">Choisissez le modèle de projet API Web.</span><span class="sxs-lookup"><span data-stu-id="42271-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="42271-118">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis **Console de gestion de Package**.</span><span class="sxs-lookup"><span data-stu-id="42271-118">From the **Tools** menu, select **NuGet Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="42271-119">Dans la fenêtre de Console du Gestionnaire de Package, tapez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="42271-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="42271-120">La première commande installe le dernier package de suivi d’API Web.</span><span class="sxs-lookup"><span data-stu-id="42271-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="42271-121">Il met également à jour les principaux packages d’API Web.</span><span class="sxs-lookup"><span data-stu-id="42271-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="42271-122">La deuxième commande met à jour le package WebApi.WebHost vers la dernière version.</span><span class="sxs-lookup"><span data-stu-id="42271-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="42271-123">Si vous souhaitez cibler une version spécifique de l’API Web, utilisez - indicateur de Version lorsque vous installez le package de suivi.</span><span class="sxs-lookup"><span data-stu-id="42271-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>

<span data-ttu-id="42271-124">Ouvrez le fichier WebApiConfig.cs dans l’application\_dossier de démarrage.</span><span class="sxs-lookup"><span data-stu-id="42271-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="42271-125">Ajoutez le code suivant à la **inscrire** (méthode).</span><span class="sxs-lookup"><span data-stu-id="42271-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="42271-126">Ce code ajoute la [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) classe au pipeline API Web.</span><span class="sxs-lookup"><span data-stu-id="42271-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="42271-127">Le **SystemDiagnosticsTraceWriter** classe écrit des traces à [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="42271-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="42271-128">Pour voir les traces, exécutez l’application dans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="42271-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="42271-129">Dans le navigateur, accédez à `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="42271-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="42271-130">Les instructions de trace sont écrits dans la fenêtre Sortie dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="42271-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="42271-131">(À partir de la **vue** menu, sélectionnez **sortie**).</span><span class="sxs-lookup"><span data-stu-id="42271-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="42271-132">Étant donné que **SystemDiagnosticsTraceWriter** écrit des traces à **System.Diagnostics.Trace**, vous pouvez inscrire des écouteurs de suivi supplémentaires ; par exemple, pour écrire des traces dans un fichier journal.</span><span class="sxs-lookup"><span data-stu-id="42271-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="42271-133">Pour plus d’informations sur les enregistreurs de trace, consultez le [écouteurs de suivi](https://msdn.microsoft.com/library/4y5y10s7.aspx) sujet sur MSDN.</span><span class="sxs-lookup"><span data-stu-id="42271-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="42271-134">Configuration SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="42271-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="42271-135">Le code suivant montre comment configurer le writer de suivi.</span><span class="sxs-lookup"><span data-stu-id="42271-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="42271-136">Il existe deux paramètres que vous pouvez contrôler :</span><span class="sxs-lookup"><span data-stu-id="42271-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="42271-137">IsVerbose : Si la valeur est false, chaque trace contient un minimum d’informations.</span><span class="sxs-lookup"><span data-stu-id="42271-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="42271-138">Si la valeur est true, les suivis inclure plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="42271-138">If true, traces include more information.</span></span>
- <span data-ttu-id="42271-139">MinimumLevel : Définit le niveau de suivi minimale.</span><span class="sxs-lookup"><span data-stu-id="42271-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="42271-140">Les niveaux de suivi, dans l’ordre, sont Debug, Infos, avertissement, erreur et irrécupérable.</span><span class="sxs-lookup"><span data-stu-id="42271-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="42271-141">Ajout des Traces à votre Application API Web</span><span class="sxs-lookup"><span data-stu-id="42271-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="42271-142">Ajout d’un writer de suivi vous donne un accès immédiat aux traces créées par le pipeline API Web.</span><span class="sxs-lookup"><span data-stu-id="42271-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="42271-143">Vous pouvez également utiliser l’enregistreur de trace pour effectuer le suivi de votre propre code :</span><span class="sxs-lookup"><span data-stu-id="42271-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="42271-144">Pour obtenir le writer de suivi, appelez **HttpConfiguration.Services.GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="42271-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="42271-145">À partir d’un contrôleur, cette méthode est accessible via la **ApiController.Configuration** propriété.</span><span class="sxs-lookup"><span data-stu-id="42271-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="42271-146">Pour écrire une trace, vous pouvez appeler la **ITraceWriter.Trace** (méthode) directement, mais la [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) classe définit certaines méthodes d’extension qui sont plus convivial.</span><span class="sxs-lookup"><span data-stu-id="42271-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="42271-147">Par exemple, le **Info** méthode illustrée ci-dessus crée une trace avec niveau de trace **Info**.</span><span class="sxs-lookup"><span data-stu-id="42271-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="42271-148">Infrastructure de traçage d’API Web</span><span class="sxs-lookup"><span data-stu-id="42271-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="42271-149">Cette section décrit comment écrire un writer de suivi personnalisé pour l’API Web.</span><span class="sxs-lookup"><span data-stu-id="42271-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="42271-150">Le package Microsoft.AspNet.WebApi.Tracing est basé sur une infrastructure de suivi plus général dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="42271-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="42271-151">Au lieu d’utiliser Microsoft.AspNet.WebApi.Tracing, vous pouvez également incorporer dans une autre bibliothèque de journalisation/suivi, tel que [NLog](http://nlog-project.org/) ou [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="42271-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/logging library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="42271-152">Pour collecter des traces, vous devez implémenter le **ITraceWriter** interface.</span><span class="sxs-lookup"><span data-stu-id="42271-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="42271-153">Voici un exemple simple :</span><span class="sxs-lookup"><span data-stu-id="42271-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="42271-154">Le **ITraceWriter.Trace** méthode crée une trace.</span><span class="sxs-lookup"><span data-stu-id="42271-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="42271-155">L’appelant spécifie un niveau de la catégorie et de la trace.</span><span class="sxs-lookup"><span data-stu-id="42271-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="42271-156">La catégorie peut être n’importe quelle chaîne définie par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="42271-156">The category can be any user-defined string.</span></span> <span data-ttu-id="42271-157">Votre implémentation de **Trace** doit effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="42271-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="42271-158">Créer un nouveau **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="42271-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="42271-159">L’initialiser avec la demande, la catégorie et le niveau de trace, comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="42271-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="42271-160">Ces valeurs sont fournies par l’appelant.</span><span class="sxs-lookup"><span data-stu-id="42271-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="42271-161">Appeler le *traceAction* déléguer.</span><span class="sxs-lookup"><span data-stu-id="42271-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="42271-162">À l’intérieur de ce délégué, l’appelant est censé remplir le reste de la **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="42271-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="42271-163">Écrire le **TraceRecord**, à l’aide de toute technique de journalisation que vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="42271-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="42271-164">L’exemple présenté ici appelle simplement **System.Diagnostics.Trace**.</span><span class="sxs-lookup"><span data-stu-id="42271-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="42271-165">Définition de l’enregistreur de Trace</span><span class="sxs-lookup"><span data-stu-id="42271-165">Setting the Trace Writer</span></span>

<span data-ttu-id="42271-166">Pour activer le suivi, vous devez configurer des API Web à utiliser votre **ITraceWriter** implémentation.</span><span class="sxs-lookup"><span data-stu-id="42271-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="42271-167">Cela via le **HttpConfiguration** de l’objet, comme indiqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="42271-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="42271-168">Writer de suivi qu’une seule peut être active.</span><span class="sxs-lookup"><span data-stu-id="42271-168">Only one trace writer can be active.</span></span> <span data-ttu-id="42271-169">Par défaut, les API Web définit un &quot;nulle&quot; suivi qui ne fait rien.</span><span class="sxs-lookup"><span data-stu-id="42271-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="42271-170">(Le &quot;nulle&quot; suivi existe afin que le code de traçage n’a pas vérifier si le writer de suivi est **null** avant d’écrire une trace.)</span><span class="sxs-lookup"><span data-stu-id="42271-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="42271-171">Comment Web API suivi Works</span><span class="sxs-lookup"><span data-stu-id="42271-171">How Web API Tracing Works</span></span>

<span data-ttu-id="42271-172">Le suivi dans l’API Web utilise un *façade* modèle : Lorsque le traçage est activé, les API Web encapsule les différentes parties du pipeline de requête avec les classes qui effectuent des appels de trace.</span><span class="sxs-lookup"><span data-stu-id="42271-172">Tracing in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="42271-173">Par exemple, lorsque vous sélectionnez un contrôleur, le pipeline utilise le **IHttpControllerSelector** interface.</span><span class="sxs-lookup"><span data-stu-id="42271-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="42271-174">Avec le suivi activé, le pipeline insère une classe qui implémente **IHttpControllerSelector** mais via les appels à l’implémentation réelle :</span><span class="sxs-lookup"><span data-stu-id="42271-174">With tracing enabled, the pipeline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Suivi de l’API Web utilise le modèle de façade.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="42271-176">Avantages de cette conception :</span><span class="sxs-lookup"><span data-stu-id="42271-176">The benefits of this design include:</span></span>

- <span data-ttu-id="42271-177">Si vous n’ajoutez pas d’un writer de suivi, les composants de suivi ne sont pas instanciés et n’ont aucun impact sur les performances.</span><span class="sxs-lookup"><span data-stu-id="42271-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="42271-178">Si vous remplacez des services par défaut comme **IHttpControllerSelector** avec votre propre implémentation personnalisée, le traçage ne concerne pas, car le suivi est effectué par l’objet de wrapper.</span><span class="sxs-lookup"><span data-stu-id="42271-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="42271-179">Vous pouvez également remplacer l’infrastructure de suivi API Web entière avec votre propre infrastructure personnalisé, en remplaçant la valeur par défaut **ITraceManager** service :</span><span class="sxs-lookup"><span data-stu-id="42271-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="42271-180">Implémentez **ITraceManager.Initialize** pour initialiser votre système de suivi.</span><span class="sxs-lookup"><span data-stu-id="42271-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="42271-181">N’oubliez pas que cela remplace le *entière* framework de trace, y compris tout le code de suivi qui est intégré à l’API Web.</span><span class="sxs-lookup"><span data-stu-id="42271-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
