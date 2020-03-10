---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Activation des demandes Cross-Origin dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Montre comment prendre en charge le partage des ressources Cross-Origin (CORS) dans API Web ASP.NET.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d3016d98fa6c3a55359c6dab0737407b29925f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555705"
---
# <a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="5c3d1-103">Activer les demandes Cross-Origin dans API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="5c3d1-103">Enable cross-origin requests in ASP.NET Web API 2</span></span>

<span data-ttu-id="5c3d1-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5c3d1-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="5c3d1-105">La sécurité des navigateurs empêche une page web d’adresser des demandes AJAX à un autre domaine.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="5c3d1-106">Cette restriction est appelée *stratégie de même origine* et empêche un site malveillant de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="5c3d1-107">Toutefois, vous souhaitez parfois laisser d’autres sites appeler votre API web.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-107">However, sometimes you might want to let other sites call your web API.</span></span>
>
> <span data-ttu-id="5c3d1-108">Le [partage des ressources Cross-Origin](http://www.w3.org/TR/cors/) (cors) est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="5c3d1-109">À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes cross-origin lors du refus d’autres.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="5c3d1-110">CORS est plus sûr et plus flexible que les techniques antérieures telles que [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="5c3d1-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="5c3d1-111">Ce didacticiel montre comment activer CORS dans votre application API Web.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
>
> ## <a name="software-used-in-the-tutorial"></a><span data-ttu-id="5c3d1-112">Logiciel utilisé dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="5c3d1-112">Software used in the tutorial</span></span>
>
> - [<span data-ttu-id="5c3d1-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c3d1-113">Visual Studio</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="5c3d1-114">API Web 2,2</span><span class="sxs-lookup"><span data-stu-id="5c3d1-114">Web API 2.2</span></span>

## <a name="introduction"></a><span data-ttu-id="5c3d1-115">Introduction</span><span class="sxs-lookup"><span data-stu-id="5c3d1-115">Introduction</span></span>

<span data-ttu-id="5c3d1-116">Ce didacticiel illustre la prise en charge de CORS dans API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="5c3d1-117">Nous allons commencer par créer deux projets ASP.NET : l’un nommé « WebClient », qui héberge un contrôleur d’API Web et l’autre appelé « WebClient », qui appelle WebService.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="5c3d1-118">Étant donné que les deux applications sont hébergées dans des domaines différents, une requête AJAX de WebClient à WebService est une demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="5c3d1-119">Que veut dire « même origine » ?</span><span class="sxs-lookup"><span data-stu-id="5c3d1-119">What is "same origin"?</span></span>

<span data-ttu-id="5c3d1-120">Deux URL ont la même origine si elles ont des schémas, des hôtes et des ports identiques.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="5c3d1-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="5c3d1-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="5c3d1-122">Ces deux URL ont le même origine :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="5c3d1-123">Ces URL ont des origines différentes aux deux précédentes :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="5c3d1-124">`http://example.net`-domaine différent</span><span class="sxs-lookup"><span data-stu-id="5c3d1-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="5c3d1-125">`http://example.com:9000/foo.html` : port différent</span><span class="sxs-lookup"><span data-stu-id="5c3d1-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="5c3d1-126">`https://example.com/foo.html` : schéma différent</span><span class="sxs-lookup"><span data-stu-id="5c3d1-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="5c3d1-127">`http://www.example.com/foo.html`-sous-domaine différent</span><span class="sxs-lookup"><span data-stu-id="5c3d1-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="5c3d1-128">Internet Explorer ne prend pas en compte le port lors de la comparaison des origines.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-128">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="create-the-webservice-project"></a><span data-ttu-id="5c3d1-129">Créer le projet WebService</span><span class="sxs-lookup"><span data-stu-id="5c3d1-129">Create the WebService project</span></span>

> [!NOTE]
> <span data-ttu-id="5c3d1-130">Cette section suppose que vous savez déjà comment créer des projets d’API Web.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="5c3d1-131">Si ce n’est pas le cas, consultez [prise en main avec API Web ASP.net](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="5c3d1-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>

1. <span data-ttu-id="5c3d1-132">Démarrez Visual Studio et créez un projet d' **application Web ASP.net (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="5c3d1-132">Start Visual Studio and create a new **ASP.NET Web Application (.NET Framework)** project.</span></span>
2. <span data-ttu-id="5c3d1-133">Dans la boîte de dialogue **nouvelle application Web ASP.net** , sélectionnez le modèle de projet **vide** .</span><span class="sxs-lookup"><span data-stu-id="5c3d1-133">In the **New ASP.NET Web Application** dialog box, select the **Empty** project template.</span></span> <span data-ttu-id="5c3d1-134">Sous **Ajouter des dossiers et des références principales pour**, activez la case à cocher **API Web** .</span><span class="sxs-lookup"><span data-stu-id="5c3d1-134">Under **Add folders and core references for**, select the **Web API** checkbox.</span></span>

   ![Boîte de dialogue Nouveau projet ASP.NET dans Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. <span data-ttu-id="5c3d1-136">Ajoutez un contrôleur d’API Web nommé `TestController` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-136">Add a Web API controller named `TestController` with the following code:</span></span>

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. <span data-ttu-id="5c3d1-137">Vous pouvez exécuter l’application localement ou la déployer sur Azure.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-137">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="5c3d1-138">(Pour les captures d’écran de ce didacticiel, l’application se déploie sur Azure App Service Web Apps.) Pour vérifier que l’API Web fonctionne, accédez à `http://hostname/api/test/`, où *hostname* est le domaine dans lequel vous avez déployé l’application.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-138">(For the screenshots in this tutorial, the app deploys to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="5c3d1-139">Vous devez voir le texte de la réponse, &quot;obtenir : message de test&quot;.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-139">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

   ![Navigateur Web indiquant le message de test](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a><span data-ttu-id="5c3d1-141">Créer le projet WebClient</span><span class="sxs-lookup"><span data-stu-id="5c3d1-141">Create the WebClient project</span></span>

1. <span data-ttu-id="5c3d1-142">Créez un autre projet d' **application Web ASP.net (.NET Framework)** et sélectionnez le modèle de projet **MVC** .</span><span class="sxs-lookup"><span data-stu-id="5c3d1-142">Create another **ASP.NET Web Application (.NET Framework)** project and select the **MVC** project template.</span></span> <span data-ttu-id="5c3d1-143">Si vous le souhaitez, sélectionnez **modifier l’authentification** > **aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="5c3d1-144">Vous n’avez pas besoin d’authentification pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-144">You don't need authentication for this tutorial.</span></span>

   ![Modèle MVC dans la boîte de dialogue Nouveau projet ASP.NET dans Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. <span data-ttu-id="5c3d1-146">Dans **Explorateur de solutions**, ouvrez le fichier *views/orig/index. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-146">In **Solution Explorer**, open the file *Views/Home/Index.cshtml*.</span></span> <span data-ttu-id="5c3d1-147">Remplacez le code de ce fichier par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-147">Replace the code in this file with the following:</span></span>

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   <span data-ttu-id="5c3d1-148">Pour la variable *ServiceUrl* , utilisez l’URI de l’application WebService.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-148">For the *serviceUrl* variable, use the URI of the WebService app.</span></span>

3. <span data-ttu-id="5c3d1-149">Exécutez l’application WebClient localement ou publiez-la sur un autre site Web.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-149">Run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="5c3d1-150">Quand vous cliquez sur le bouton « essayer », une requête AJAX est soumise à l’application service Web à l’aide de la méthode HTTP indiquée dans la zone déroulante (obtenir, poster ou PUT).</span><span class="sxs-lookup"><span data-stu-id="5c3d1-150">When you click the "Try It" button, an AJAX request is submitted to the WebService app using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="5c3d1-151">Cela vous permet d’examiner différentes demandes Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-151">This lets you examine different cross-origin requests.</span></span> <span data-ttu-id="5c3d1-152">Actuellement, l’application WebService ne prend pas en charge CORS. par conséquent, si vous cliquez sur le bouton, vous obtenez une erreur.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-152">Currently, the WebService app does not support CORS, so if you click the button you'll get an error.</span></span>

![Erreur « essayer » dans le navigateur](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="5c3d1-154">Si vous regardez le trafic HTTP dans un outil tel que [Fiddler](https://www.telerik.com/fiddler), vous verrez que le navigateur envoie la requête d’extraction et que la demande est réussie, mais l’appel Ajax retourne une erreur.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-154">If you watch the HTTP traffic in a tool like [Fiddler](https://www.telerik.com/fiddler), you'll see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="5c3d1-155">Il est important de comprendre que la stratégie de même origine n’empêche pas le navigateur d' *Envoyer* la demande.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-155">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="5c3d1-156">Au lieu de cela, il empêche l’application de voir la *réponse*.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-156">Instead, it prevents the application from seeing the *response*.</span></span>

![Débogueur Web Fiddler avec les requêtes Web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a><span data-ttu-id="5c3d1-158">Activez CORS</span><span class="sxs-lookup"><span data-stu-id="5c3d1-158">Enable CORS</span></span>

<span data-ttu-id="5c3d1-159">Activons à présent CORS dans l’application WebService.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-159">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="5c3d1-160">Tout d’abord, ajoutez le package NuGet CORS.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-160">First, add the CORS NuGet package.</span></span> <span data-ttu-id="5c3d1-161">Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-161">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="5c3d1-162">Dans la fenêtre de la console du gestionnaire de package, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-162">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="5c3d1-163">Cette commande installe le package le plus récent et met à jour toutes les dépendances, y compris les bibliothèques principales de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-163">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="5c3d1-164">Utilisez l’indicateur `-Version` pour cibler une version spécifique.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-164">Use the `-Version` flag to target a specific version.</span></span> <span data-ttu-id="5c3d1-165">Le package CORS requiert l’API Web 2,0 ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-165">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="5c3d1-166">Ouvrez le fichier d' *application\_Start/WebApiConfig. cs*.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-166">Open the file *App\_Start/WebApiConfig.cs*.</span></span> <span data-ttu-id="5c3d1-167">Ajoutez le code suivant à la méthode **WebApiConfig. Register** :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-167">Add the following code to the **WebApiConfig.Register** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="5c3d1-168">Ensuite, ajoutez l’attribut **[EnableCors]** à la classe `TestController` :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-168">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="5c3d1-169">Pour le paramètre *Origins* , utilisez l’URI où vous avez déployé l’application WebClient.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-169">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="5c3d1-170">Cela permet d’effectuer des requêtes Cross-Origin à partir de WebClient, tout en désautorisant toutes les autres requêtes inter-domaines.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-170">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="5c3d1-171">Plus tard, je décrirai les paramètres pour **[EnableCors]** plus en détail.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-171">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="5c3d1-172">N’incluez pas de barre oblique à la fin de l’URL des *origines* .</span><span class="sxs-lookup"><span data-stu-id="5c3d1-172">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="5c3d1-173">Redéployez l’application WebService mise à jour.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-173">Redeploy the updated WebService application.</span></span> <span data-ttu-id="5c3d1-174">Vous n’avez pas besoin de mettre à jour WebClient.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-174">You don't need to update WebClient.</span></span> <span data-ttu-id="5c3d1-175">La requête AJAX de WebClient doit maintenant s’effectuer correctement.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-175">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="5c3d1-176">Les méthodes d’extraction, de placement et de publication sont toutes autorisées.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-176">The GET, PUT, and POST methods are all allowed.</span></span>

![Navigateur Web indiquant que le message de test a réussi](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a><span data-ttu-id="5c3d1-178">Fonctionnement de CORS</span><span class="sxs-lookup"><span data-stu-id="5c3d1-178">How CORS Works</span></span>

<span data-ttu-id="5c3d1-179">Cette section décrit ce qui se produit dans une demande CORS, au niveau des messages HTTP.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-179">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="5c3d1-180">Il est important de comprendre le fonctionnement de CORS, afin que vous puissiez configurer l’attribut **[EnableCors]** correctement et résoudre les problèmes si les choses ne fonctionnent pas comme prévu.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-180">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="5c3d1-181">La spécification CORS introduit plusieurs nouveaux en-têtes HTTP qui permettent les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-181">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="5c3d1-182">Si un navigateur prend en charge CORS, il définit automatiquement ces en-têtes pour les demandes Cross-Origin. vous n’avez rien à faire de spécial dans votre code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-182">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="5c3d1-183">Voici un exemple de demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-183">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="5c3d1-184">L’en-tête « Origin » indique le domaine du site qui effectue la demande.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-184">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="5c3d1-185">Si le serveur autorise la demande, il définit l’en-tête Access-Control-allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-185">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="5c3d1-186">La valeur de cet en-tête correspond à l’en-tête d’origine ou à la valeur de caractère générique «\*», ce qui signifie que toute origine est autorisée.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-186">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="5c3d1-187">Si la réponse n’inclut pas l’en-tête Access-Control-allow-Origin, la requête AJAX échoue.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-187">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="5c3d1-188">Plus précisément, le navigateur n’autorise pas la demande.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-188">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="5c3d1-189">Même si le serveur retourne une réponse correcte, le navigateur ne met pas la réponse à la disposition de l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-189">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="5c3d1-190">**Demandes préliminaires**</span><span class="sxs-lookup"><span data-stu-id="5c3d1-190">**Preflight Requests**</span></span>

<span data-ttu-id="5c3d1-191">Pour certaines demandes CORS, le navigateur envoie une demande supplémentaire, appelée « demande préliminaire », avant d’envoyer la demande réelle pour la ressource.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-191">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="5c3d1-192">Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-192">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="5c3d1-193">La méthode de demande est obtenir, tête ou publication, *et*</span><span class="sxs-lookup"><span data-stu-id="5c3d1-193">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="5c3d1-194">L’application ne définit pas d’en-têtes de requête autres que accepter, Accept-Language, Content-Language, Content-type ou Last-Event-ID, *et*</span><span class="sxs-lookup"><span data-stu-id="5c3d1-194">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="5c3d1-195">L’en-tête `Content-Type` (si défini) est une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-195">The Content-Type header (if set) is one of the following:</span></span>

    - <span data-ttu-id="5c3d1-196">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="5c3d1-196">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="5c3d1-197">multipart/formulaire-données</span><span class="sxs-lookup"><span data-stu-id="5c3d1-197">multipart/form-data</span></span>
    - <span data-ttu-id="5c3d1-198">texte/brut</span><span class="sxs-lookup"><span data-stu-id="5c3d1-198">text/plain</span></span>

<span data-ttu-id="5c3d1-199">La règle relative aux en-têtes de demande s’applique aux en-têtes définis par l’application en appelant **setRequestHeader** sur l’objet **XMLHttpRequest** .</span><span class="sxs-lookup"><span data-stu-id="5c3d1-199">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="5c3d1-200">(La spécification CORS appelle ces « en-têtes de demande d’auteur ».) La règle ne s’applique pas aux en-têtes que le *navigateur* peut définir, tels que user-agent, Host ou Content-Length.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-200">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="5c3d1-201">Voici un exemple de demande préliminaire :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-201">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="5c3d1-202">La requête de pré-vol utilise la méthode HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-202">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="5c3d1-203">Il comprend deux en-têtes spéciaux :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-203">It includes two special headers:</span></span>

- <span data-ttu-id="5c3d1-204">Access-Control-Request-Method : méthode HTTP qui sera utilisée pour la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-204">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="5c3d1-205">Access-Control-request-headers : liste des en-têtes de requête définis par l' *application* sur la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-205">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="5c3d1-206">(Là encore, cela n’inclut pas les en-têtes définis par le navigateur.)</span><span class="sxs-lookup"><span data-stu-id="5c3d1-206">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="5c3d1-207">Voici un exemple de réponse, en supposant que le serveur autorise la requête :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-207">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="5c3d1-208">La réponse inclut un en-tête `Access-Control-Allow-Methods` qui répertorie les méthodes autorisées et éventuellement un en-tête `Access-Control-Allow-Headers`, qui répertorie les en-têtes autorisés.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-208">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="5c3d1-209">Si la demande préliminaire réussit, le navigateur envoie la demande réelle, comme décrit précédemment.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-209">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<span data-ttu-id="5c3d1-210">Les outils couramment utilisés pour tester des points de terminaison avec des demandes d’OPTIONS préliminaires (par exemple, [Fiddler](https://www.telerik.com/fiddler) et [poster](https://www.getpostman.com/)) n’envoient pas les en-têtes d’options requis par défaut.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-210">Tools commonly used to test endpoints with preflight OPTIONS requests (for example, [Fiddler](https://www.telerik.com/fiddler) and [Postman](https://www.getpostman.com/)) don't send the required OPTIONS headers by default.</span></span> <span data-ttu-id="5c3d1-211">Vérifiez que les en-têtes `Access-Control-Request-Method` et `Access-Control-Request-Headers` sont envoyés avec la demande et que les en-têtes d’OPTIONS atteignent l’application via IIS.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-211">Confirm that the `Access-Control-Request-Method` and `Access-Control-Request-Headers` headers are sent with the request and that OPTIONS headers reach the app through IIS.</span></span>

<span data-ttu-id="5c3d1-212">Pour configurer IIS afin de permettre à une application ASP.NET de recevoir et de gérer les demandes d’OPTION, ajoutez la configuration suivante au fichier *Web. config* de l’application dans la section `<system.webServer><handlers>` :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-212">To configure IIS to allow an ASP.NET app to receive and handle OPTION requests, add the following configuration to the app's *web.config* file in the `<system.webServer><handlers>` section:</span></span>

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

<span data-ttu-id="5c3d1-213">La suppression de `OPTIONSVerbHandler` empêche IIS de gérer les demandes d’OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-213">The removal of `OPTIONSVerbHandler` prevents IIS from handling OPTIONS requests.</span></span> <span data-ttu-id="5c3d1-214">Le remplacement de `ExtensionlessUrlHandler-Integrated-4.0` permet aux demandes d’OPTIONS d’atteindre l’application, car l’inscription du module par défaut autorise uniquement les demandes d’extraction, de début, de publication et de débogage avec des URL sans extension.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-214">The replacement of `ExtensionlessUrlHandler-Integrated-4.0` allows OPTIONS requests to reach the app because the default module registration only allows GET, HEAD, POST, and DEBUG requests with extensionless URLs.</span></span>

## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="5c3d1-215">Règles d’étendue pour [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="5c3d1-215">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="5c3d1-216">Vous pouvez activer CORS par action, par contrôleur ou globalement pour tous les contrôleurs d’API Web dans votre application.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-216">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="5c3d1-217">**Par action**</span><span class="sxs-lookup"><span data-stu-id="5c3d1-217">**Per Action**</span></span>

<span data-ttu-id="5c3d1-218">Pour activer CORS pour une seule action, définissez l’attribut **[EnableCors]** sur la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-218">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="5c3d1-219">L’exemple suivant active CORS pour la méthode `GetItem` uniquement.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-219">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="5c3d1-220">**Par contrôleur**</span><span class="sxs-lookup"><span data-stu-id="5c3d1-220">**Per Controller**</span></span>

<span data-ttu-id="5c3d1-221">Si vous définissez **[EnableCors]** sur la classe Controller, elle s’applique à toutes les actions sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-221">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="5c3d1-222">Pour désactiver CORS pour une action, ajoutez l’attribut **[DisableCors]** à l’action.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-222">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="5c3d1-223">L’exemple suivant active CORS pour chaque méthode, à l’exception de `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-223">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="5c3d1-224">**Globalement**</span><span class="sxs-lookup"><span data-stu-id="5c3d1-224">**Globally**</span></span>

<span data-ttu-id="5c3d1-225">Pour activer CORS pour tous les contrôleurs d’API Web dans votre application, transmettez une instance **EnableCorsAttribute** à la méthode **EnableCors** :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-225">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="5c3d1-226">Si vous définissez l’attribut sur plusieurs étendues, l’ordre de priorité est le suivant :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-226">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="5c3d1-227">Action</span><span class="sxs-lookup"><span data-stu-id="5c3d1-227">Action</span></span>
2. <span data-ttu-id="5c3d1-228">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="5c3d1-228">Controller</span></span>
3. <span data-ttu-id="5c3d1-229">Global</span><span class="sxs-lookup"><span data-stu-id="5c3d1-229">Global</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="5c3d1-230">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="5c3d1-230">Set the allowed origins</span></span>

<span data-ttu-id="5c3d1-231">Le paramètre *Origins* de l’attribut **[EnableCors]** spécifie les origines autorisées à accéder à la ressource.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-231">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="5c3d1-232">La valeur est une liste séparée par des virgules des origines autorisées.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-232">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="5c3d1-233">Vous pouvez également utiliser la valeur de caractère générique «\*» pour autoriser les demandes émanant d’origines.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-233">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="5c3d1-234">Réfléchissez bien avant d’autoriser des demandes à partir de n’importe quelle origine.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-234">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="5c3d1-235">Cela signifie que tout site Web peut effectuer des appels AJAX à votre API Web.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-235">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="5c3d1-236">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="5c3d1-236">Set the allowed HTTP methods</span></span>

<span data-ttu-id="5c3d1-237">Le paramètre *Methods* de l’attribut **[EnableCors]** spécifie les méthodes http autorisées à accéder à la ressource.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-237">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="5c3d1-238">Pour autoriser toutes les méthodes, utilisez la valeur de caractère générique «\*».</span><span class="sxs-lookup"><span data-stu-id="5c3d1-238">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="5c3d1-239">L’exemple suivant autorise uniquement les demandes d’extraction et de publication.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-239">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="5c3d1-240">Définir les en-têtes de demande autorisés</span><span class="sxs-lookup"><span data-stu-id="5c3d1-240">Set the allowed request headers</span></span>

<span data-ttu-id="5c3d1-241">Cet article a décrit plus haut comment une demande préliminaire peut inclure un en-tête Access-Control-request-headers, qui répertorie les en-têtes HTTP définis par l’application (appelé « en-têtes de demande d’auteur »).</span><span class="sxs-lookup"><span data-stu-id="5c3d1-241">This article described earlier how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="5c3d1-242">Le paramètre *en-têtes* de l’attribut **[EnableCors]** spécifie les en-têtes de demande d’auteur autorisés.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-242">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="5c3d1-243">Pour autoriser tous les en-têtes, définissez les *en-têtes* sur «\*».</span><span class="sxs-lookup"><span data-stu-id="5c3d1-243">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="5c3d1-244">Pour obtenir les en-têtes spécifiques à la liste verte, définissez les *en-têtes* sur une liste séparée par des virgules des en-têtes autorisés :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-244">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="5c3d1-245">Toutefois, les navigateurs ne sont pas entièrement cohérents dans la façon dont ils définissent les en-têtes Access-Control-Request-Header.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-245">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="5c3d1-246">Par exemple, chrome contient actuellement « Origin ».</span><span class="sxs-lookup"><span data-stu-id="5c3d1-246">For example, Chrome currently includes "origin".</span></span> <span data-ttu-id="5c3d1-247">FireFox n’inclut pas les en-têtes standard tels que « accepter », même lorsque l’application les définit dans un script.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-247">FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="5c3d1-248">Si vous définissez *les en-têtes* sur une valeur autre que «\*», vous devez inclure au moins « Accept », « Content-type » et « Origin », ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-248">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="5c3d1-249">Définir les en-têtes de réponse autorisés</span><span class="sxs-lookup"><span data-stu-id="5c3d1-249">Set the allowed response headers</span></span>

<span data-ttu-id="5c3d1-250">Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-250">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="5c3d1-251">Les en-têtes de réponse qui sont disponibles par défaut sont :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-251">The response headers that are available by default are:</span></span>

- <span data-ttu-id="5c3d1-252">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="5c3d1-252">Cache-Control</span></span>
- <span data-ttu-id="5c3d1-253">Content-Language</span><span class="sxs-lookup"><span data-stu-id="5c3d1-253">Content-Language</span></span>
- <span data-ttu-id="5c3d1-254">Content-Type</span><span class="sxs-lookup"><span data-stu-id="5c3d1-254">Content-Type</span></span>
- <span data-ttu-id="5c3d1-255">Expires</span><span class="sxs-lookup"><span data-stu-id="5c3d1-255">Expires</span></span>
- <span data-ttu-id="5c3d1-256">Dernière modification</span><span class="sxs-lookup"><span data-stu-id="5c3d1-256">Last-Modified</span></span>
- <span data-ttu-id="5c3d1-257">Bali</span><span class="sxs-lookup"><span data-stu-id="5c3d1-257">Pragma</span></span>

<span data-ttu-id="5c3d1-258">La spécification CORS appelle ces [en-têtes de réponse simples](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="5c3d1-258">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="5c3d1-259">Pour mettre d’autres en-têtes à la disposition de l’application, définissez le paramètre *exposedHeaders* de **[EnableCors]** .</span><span class="sxs-lookup"><span data-stu-id="5c3d1-259">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="5c3d1-260">Dans l’exemple suivant, la méthode `Get` du contrôleur définit un en-tête personnalisé nommé « X-Custom-Header ».</span><span class="sxs-lookup"><span data-stu-id="5c3d1-260">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="5c3d1-261">Par défaut, le navigateur n’expose pas cet en-tête dans une demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-261">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="5c3d1-262">Pour rendre l’en-tête disponible, incluez « X-Custom-header » dans *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-262">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a><span data-ttu-id="5c3d1-263">Transmettre des informations d’identification dans les demandes Cross-Origin</span><span class="sxs-lookup"><span data-stu-id="5c3d1-263">Pass credentials in cross-origin requests</span></span>

<span data-ttu-id="5c3d1-264">Les informations d’identification nécessitent un traitement particulier dans une demande CORS.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-264">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="5c3d1-265">Par défaut, le navigateur n’envoie pas d’informations d’identification avec une demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-265">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="5c3d1-266">Les informations d’identification incluent les cookies, ainsi que des schémas d’authentification HTTP.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-266">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="5c3d1-267">Pour envoyer des informations d’identification avec une demande Cross-Origin, le client doit définir **XMLHttpRequest. withCredentials** sur true.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-267">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="5c3d1-268">Utilisation directe de **XMLHttpRequest** :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-268">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="5c3d1-269">Dans jQuery :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-269">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="5c3d1-270">En outre, le serveur doit autoriser les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-270">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="5c3d1-271">Pour autoriser les informations d’identification Cross-Origin dans l’API Web, définissez la propriété **SupportsCredentials** sur true dans l’attribut **[EnableCors]** :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-271">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="5c3d1-272">Si cette propriété a la valeur true, la réponse HTTP inclut un en-tête Access-Control-allow-Credential.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-272">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="5c3d1-273">Cet en-tête indique au navigateur que le serveur autorise les informations d’identification pour une demande Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-273">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="5c3d1-274">Si le navigateur envoie des informations d’identification, mais que la réponse n’inclut pas d’en-tête Access-Control-allow-Credential valide, le navigateur n’expose pas la réponse à l’application et la requête AJAX échoue.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-274">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="5c3d1-275">Veillez à affecter à **SupportsCredentials** la valeur true, car cela signifie qu’un site Web situé dans un autre domaine peut envoyer les informations d’identification d’un utilisateur connecté à votre API Web pour le compte de l’utilisateur, sans que l’utilisateur ne soit conscient.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-275">Be careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="5c3d1-276">La spécification CORS indique également que la définition d' *origines* pour &quot;\*&quot; n’est pas valide si **SupportsCredentials** a la valeur true.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-276">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

## <a name="custom-cors-policy-providers"></a><span data-ttu-id="5c3d1-277">Fournisseurs de stratégie CORS personnalisés</span><span class="sxs-lookup"><span data-stu-id="5c3d1-277">Custom CORS policy providers</span></span>

<span data-ttu-id="5c3d1-278">L’attribut **[EnableCors]** implémente l’interface **ICorsPolicyProvider** .</span><span class="sxs-lookup"><span data-stu-id="5c3d1-278">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="5c3d1-279">Vous pouvez fournir votre propre implémentation en créant une classe qui dérive de **attribute** et implémente **ICorsPolicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-279">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsPolicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="5c3d1-280">Vous pouvez maintenant appliquer l’attribut à tous les emplacements que vous placez dans **[EnableCors]** .</span><span class="sxs-lookup"><span data-stu-id="5c3d1-280">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="5c3d1-281">Par exemple, un fournisseur de stratégie CORS personnalisé peut lire les paramètres à partir d’un fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-281">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="5c3d1-282">En guise d’alternative à l’utilisation d’attributs, vous pouvez inscrire un objet **ICorsPolicyProviderFactory** qui crée des objets **ICorsPolicyProvider** .</span><span class="sxs-lookup"><span data-stu-id="5c3d1-282">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="5c3d1-283">Pour définir le **ICorsPolicyProviderFactory**, appelez la méthode d’extension **SetCorsPolicyProviderFactory** au démarrage, comme suit :</span><span class="sxs-lookup"><span data-stu-id="5c3d1-283">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a><span data-ttu-id="5c3d1-284">Prise en charge des navigateurs</span><span class="sxs-lookup"><span data-stu-id="5c3d1-284">Browser support</span></span>

<span data-ttu-id="5c3d1-285">Le package CORS de l’API Web est une technologie côté serveur.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-285">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="5c3d1-286">Le navigateur de l’utilisateur doit également prendre en charge CORS.</span><span class="sxs-lookup"><span data-stu-id="5c3d1-286">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="5c3d1-287">Heureusement, les versions actuelles de tous les principaux navigateurs incluent la [prise en charge de cors](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="5c3d1-287">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>
