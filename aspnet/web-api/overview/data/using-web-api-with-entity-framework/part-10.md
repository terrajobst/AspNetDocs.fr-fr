---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publier l’application sur Azure Azure App Service | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: a9a7b74a07c71b47253c0af304c7a26ffa4eaae5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622387"
---
# <a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="8c007-102">Publier l’application sur Azure Azure App Service</span><span class="sxs-lookup"><span data-stu-id="8c007-102">Publish the App to Azure Azure App Service</span></span>

<span data-ttu-id="8c007-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8c007-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8c007-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="8c007-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="8c007-105">La dernière étape consiste à publier l’application sur Azure.</span><span class="sxs-lookup"><span data-stu-id="8c007-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="8c007-106">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="8c007-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="8c007-107">Cliquez sur **publier** pour appeler la boîte de dialogue **publier le site Web** .</span><span class="sxs-lookup"><span data-stu-id="8c007-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="8c007-108">Si vous avez activé l' **hôte dans le Cloud lors de** la création initiale du projet, la connexion et les paramètres sont déjà configurés.</span><span class="sxs-lookup"><span data-stu-id="8c007-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="8c007-109">Dans ce cas, cliquez simplement sur l’onglet **paramètres** et cochez &quot;exécuter migrations code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="8c007-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="8c007-110">(Si vous n’avez pas vérifié l' **hôte dans le Cloud** au début, suivez les étapes de la [section suivante](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="8c007-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="8c007-111">Pour déployer l’application, cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="8c007-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="8c007-112">Vous pouvez afficher la progression de la publication dans la fenêtre **activité de publication Web** .</span><span class="sxs-lookup"><span data-stu-id="8c007-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="8c007-113">(Dans le menu **affichage** , sélectionnez **autres fenêtres**, puis sélectionnez **activité de publication Web**.)</span><span class="sxs-lookup"><span data-stu-id="8c007-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="8c007-114">Une fois que Visual Studio a terminé le déploiement de l’application, le navigateur par défaut s’ouvre automatiquement à l’URL du site Web déployé, et l’application que vous avez créée s’exécute maintenant dans le Cloud.</span><span class="sxs-lookup"><span data-stu-id="8c007-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="8c007-115">L’URL dans la barre d’adresse du navigateur indique que le site est chargé à partir d’Internet.</span><span class="sxs-lookup"><span data-stu-id="8c007-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="8c007-116">Déploiement sur un nouveau site Web</span><span class="sxs-lookup"><span data-stu-id="8c007-116">Deploying to a New Website</span></span>

<span data-ttu-id="8c007-117">Si vous n’avez pas activé l' **ordinateur hôte dans le Cloud lors de** la création initiale du projet, vous pouvez configurer une nouvelle application Web maintenant.</span><span class="sxs-lookup"><span data-stu-id="8c007-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="8c007-118">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="8c007-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="8c007-119">Sélectionnez l’onglet **Profil** , puis cliquez sur **Microsoft Azure sites Web**.</span><span class="sxs-lookup"><span data-stu-id="8c007-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="8c007-120">Si vous n’êtes pas actuellement connecté à Azure, vous êtes invité à vous connecter.</span><span class="sxs-lookup"><span data-stu-id="8c007-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="8c007-121">Dans la boîte de dialogue **sites Web existants** , cliquez sur **nouveau**.</span><span class="sxs-lookup"><span data-stu-id="8c007-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="8c007-122">Entrez un nom de site.</span><span class="sxs-lookup"><span data-stu-id="8c007-122">Enter a site name.</span></span> <span data-ttu-id="8c007-123">Sélectionnez votre abonnement Azure et la région.</span><span class="sxs-lookup"><span data-stu-id="8c007-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="8c007-124">Sous **serveur de base de données**, sélectionnez **créer un nouveau serveur**ou sélectionnez un serveur existant.</span><span class="sxs-lookup"><span data-stu-id="8c007-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="8c007-125">Cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="8c007-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="8c007-126">Cliquez sur l’onglet **paramètres** et cochez &quot;exécuter migrations code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="8c007-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="8c007-127">Ils cliquent ensuite sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="8c007-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8c007-128">Précédent</span><span class="sxs-lookup"><span data-stu-id="8c007-128">Previous</span></span>](part-9.md)
