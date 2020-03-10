---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Créer un nouveau projet MVC ASP.NET | Microsoft Docs
author: microsoft
description: L’étape 1 montre comment mettre en place la structure d’application NerdDinner de base.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 189ddc187fc83db14106b2da199ba12a70a32b45
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580933"
---
# <a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="405aa-103">Créer un projet ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="405aa-103">Create a New ASP.NET MVC Project</span></span>

<span data-ttu-id="405aa-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="405aa-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="405aa-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="405aa-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="405aa-106">Il s’agit de l’étape 1 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="405aa-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="405aa-107">L’étape 1 montre comment mettre en place la structure d’application NerdDinner de base.</span><span class="sxs-lookup"><span data-stu-id="405aa-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="405aa-108">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.</span><span class="sxs-lookup"><span data-stu-id="405aa-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="405aa-109">NerdDinner étape 1 : fichier-&gt;nouveau projet</span><span class="sxs-lookup"><span data-stu-id="405aa-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="405aa-110">Nous allons commencer notre application NerdDinner en sélectionnant l’élément de menu **fichier-&gt;nouveau projet** dans visual Studio 2008 ou la version gratuite de Visual Web developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="405aa-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="405aa-111">La boîte de dialogue « Nouveau projet » s’affiche.</span><span class="sxs-lookup"><span data-stu-id="405aa-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="405aa-112">Pour créer une application ASP.NET MVC, nous allons sélectionner le nœud « Web » sur le côté gauche de la boîte de dialogue, puis choisir le modèle de projet « application Web ASP.NET MVC » sur la droite :</span><span class="sxs-lookup"><span data-stu-id="405aa-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="405aa-113">*Important : Assurez-vous que vous avez téléchargé et installé ASP.NET MVC ; sinon, il n’apparaîtra pas dans la boîte de dialogue Nouveau projet. Vous pouvez utiliser v2 de l' [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) si vous ne l’avez pas encore installé (ASP.NET MVC est disponible dans la section «plateformes et runtimes de&gt;Web).*</span><span class="sxs-lookup"><span data-stu-id="405aa-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="405aa-114">Nous allons nommer le nouveau projet que nous allons créer « NerdDinner », puis cliquer sur le bouton « OK » pour le créer.</span><span class="sxs-lookup"><span data-stu-id="405aa-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="405aa-115">Lorsque nous cliquons sur « OK », Visual Studio affiche une boîte de dialogue supplémentaire qui nous invite à créer également un projet de test unitaire pour la nouvelle application.</span><span class="sxs-lookup"><span data-stu-id="405aa-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="405aa-116">Ce projet de test unitaire nous permet de créer des tests automatisés qui vérifient les fonctionnalités et le comportement de notre application (nous aborderons la procédure à suivre plus loin dans ce didacticiel).</span><span class="sxs-lookup"><span data-stu-id="405aa-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="405aa-117">La liste déroulante infrastructure de test dans la boîte de dialogue ci-dessus est remplie avec tous les modèles de projet de test unitaire ASP.NET MVC disponibles installés sur l’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="405aa-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="405aa-118">Les versions peuvent être téléchargées pour NUnit, MBUnit et XUnit.</span><span class="sxs-lookup"><span data-stu-id="405aa-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="405aa-119">L’infrastructure de test unitaire Visual Studio intégrée est également prise en charge.</span><span class="sxs-lookup"><span data-stu-id="405aa-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="405aa-120">*Remarque : l’infrastructure de tests unitaires Visual Studio est uniquement disponible avec Visual Studio 2008 Professional et versions ultérieures. Si vous utilisez VS 2008 Standard Edition ou Visual Web Developer 2008 Express, vous devez télécharger et installer les extensions NUnit, MBUnit ou XUnit pour ASP.NET MVC afin de pouvoir afficher cette boîte de dialogue. La boîte de dialogue ne s’affiche pas si aucun Framework de test n’est installé.*</span><span class="sxs-lookup"><span data-stu-id="405aa-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="405aa-121">Nous allons utiliser le nom par défaut « NerdDinner. tests » pour le projet de test que nous créons, et utiliser l’option d’infrastructure « test unitaire Visual Studio ».</span><span class="sxs-lookup"><span data-stu-id="405aa-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="405aa-122">Lorsque nous cliquons sur le bouton « OK », Visual Studio crée une solution pour nous avec deux projets en informatique : un pour notre application Web et un pour nos tests unitaires :</span><span class="sxs-lookup"><span data-stu-id="405aa-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="405aa-123">Examen de la structure de répertoires NerdDinner</span><span class="sxs-lookup"><span data-stu-id="405aa-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="405aa-124">Quand vous créez une application MVC ASP.NET avec Visual Studio, elle ajoute automatiquement un certain nombre de fichiers et de répertoires au projet :</span><span class="sxs-lookup"><span data-stu-id="405aa-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="405aa-125">Par défaut, les projets MVC ASP.NET ont six répertoires de niveau supérieur :</span><span class="sxs-lookup"><span data-stu-id="405aa-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="405aa-126">**Répertoire**</span><span class="sxs-lookup"><span data-stu-id="405aa-126">**Directory**</span></span> | <span data-ttu-id="405aa-127">**Fonction**</span><span class="sxs-lookup"><span data-stu-id="405aa-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="405aa-128">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="405aa-128">**/Controllers**</span></span> | <span data-ttu-id="405aa-129">Où vous placez les classes de contrôleur qui gèrent les demandes d’URL</span><span class="sxs-lookup"><span data-stu-id="405aa-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="405aa-130">**/Models**</span><span class="sxs-lookup"><span data-stu-id="405aa-130">**/Models**</span></span> | <span data-ttu-id="405aa-131">Où vous placez des classes qui représentent et manipulent des données</span><span class="sxs-lookup"><span data-stu-id="405aa-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="405aa-132">**/Views**</span><span class="sxs-lookup"><span data-stu-id="405aa-132">**/Views**</span></span> | <span data-ttu-id="405aa-133">Où vous placez les fichiers de modèle d’interface utilisateur chargés du rendu de la sortie</span><span class="sxs-lookup"><span data-stu-id="405aa-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="405aa-134">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="405aa-134">**/Scripts**</span></span> | <span data-ttu-id="405aa-135">Où placer les scripts et les fichiers de la bibliothèque JavaScript (. js)</span><span class="sxs-lookup"><span data-stu-id="405aa-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="405aa-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="405aa-136">**/Content**</span></span> | <span data-ttu-id="405aa-137">Où vous placez les fichiers CSS et image, ainsi que d’autres contenus non dynamiques/non-JavaScript</span><span class="sxs-lookup"><span data-stu-id="405aa-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="405aa-138">**/App\_données**</span><span class="sxs-lookup"><span data-stu-id="405aa-138">**/App\_Data**</span></span> | <span data-ttu-id="405aa-139">Où vous stockez les fichiers de données que vous souhaitez lire/écrire.</span><span class="sxs-lookup"><span data-stu-id="405aa-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="405aa-140">ASP.NET MVC ne nécessite pas cette structure.</span><span class="sxs-lookup"><span data-stu-id="405aa-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="405aa-141">En fait, les développeurs qui travaillent sur des applications volumineuses partitionnent généralement l’application entre plusieurs projets pour faciliter leur gestion (par exemple : les classes de modèle de données sont souvent placées dans un projet de bibliothèque de classes distinct de l’application Web).</span><span class="sxs-lookup"><span data-stu-id="405aa-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="405aa-142">Toutefois, la structure de projet par défaut fournit une bonne Convention de répertoire par défaut que nous pouvons utiliser pour assurer le bon fonctionnement de notre application.</span><span class="sxs-lookup"><span data-stu-id="405aa-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="405aa-143">Lorsque nous développons le répertoire/Controllers, vous constaterez que Visual Studio a ajouté deux classes de contrôleur – HomeController et AccountController – par défaut au projet :</span><span class="sxs-lookup"><span data-stu-id="405aa-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="405aa-144">Lorsque nous développons le répertoire/views, vous trouverez trois sous-répertoires (/Home,/Account et/Shared), ainsi que plusieurs fichiers de modèles qu’ils contiennent, qui ont également été ajoutés au projet par défaut :</span><span class="sxs-lookup"><span data-stu-id="405aa-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="405aa-145">Lorsque nous développons les répertoires/content et/scripts, nous trouvons un fichier site. CSS qui est utilisé pour appliquer un style à tous les codes HTML sur le site, ainsi que des bibliothèques JavaScript capables d’activer la prise en charge d’AJAX et de jQuery dans l’application :</span><span class="sxs-lookup"><span data-stu-id="405aa-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="405aa-146">Lorsque nous développons le projet NerdDinner. tests, vous trouverez deux classes qui contiennent des tests unitaires pour nos classes de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="405aa-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="405aa-147">Ces fichiers par défaut ajoutés par Visual Studio fournissent une structure de base pour une application opérationnelle, avec la page d’installation, à propos de la page, des pages de connexion de compte/de déconnexion/inscription et une page d’erreur non gérée (tout en étant connecté et prêt à l’emploi).</span><span class="sxs-lookup"><span data-stu-id="405aa-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="405aa-148">Exécution de l’application NerdDinner</span><span class="sxs-lookup"><span data-stu-id="405aa-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="405aa-149">Pour exécuter le projet, vous pouvez choisir les éléments de menu Déboguer **-&gt;démarrer le débogage** ou **Déboguer-&gt;exécuter sans débogage** :</span><span class="sxs-lookup"><span data-stu-id="405aa-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="405aa-150">Cette opération lance le serveur Web ASP.NET intégré qui est fourni avec Visual Studio et exécute notre application :</span><span class="sxs-lookup"><span data-stu-id="405aa-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="405aa-151">Vous trouverez ci-dessous la page d’hébergement de notre nouveau projet (URL : « / ») lors de son exécution :</span><span class="sxs-lookup"><span data-stu-id="405aa-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="405aa-152">En cliquant sur l’onglet « à propos de », vous affichez une page à propos de (URL : « /Home/About ») :</span><span class="sxs-lookup"><span data-stu-id="405aa-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="405aa-153">Cliquer sur le lien « connexion » en haut à droite nous amène à une page de connexion (URL : « /Account/LogOn »)</span><span class="sxs-lookup"><span data-stu-id="405aa-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="405aa-154">Si nous n’avons pas de compte de connexion, nous pouvons cliquer sur le lien register (URL : « /Account/Register ») pour en créer un :</span><span class="sxs-lookup"><span data-stu-id="405aa-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="405aa-155">Le code permettant d’implémenter la fonctionnalité de démarrage, d’informations et de déconnexion/inscription ci-dessus a été ajouté par défaut lors de la création de notre nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="405aa-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="405aa-156">Nous l’utiliserons comme point de départ de notre application.</span><span class="sxs-lookup"><span data-stu-id="405aa-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="405aa-157">Test de l’application NerdDinner</span><span class="sxs-lookup"><span data-stu-id="405aa-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="405aa-158">Si vous utilisez l’édition Professional ou une version ultérieure de Visual Studio 2008, nous pouvons utiliser la prise en charge intégrée de l’IDE de test unitaire dans Visual Studio pour tester le projet :</span><span class="sxs-lookup"><span data-stu-id="405aa-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="405aa-159">Le choix de l’une des options ci-dessus permet d’ouvrir le volet « Résultats des tests » dans l’IDE et de nous fournir un état de réussite/échec sur les 27 tests unitaires inclus dans le nouveau projet, qui couvrent les fonctionnalités intégrées :</span><span class="sxs-lookup"><span data-stu-id="405aa-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="405aa-160">Plus loin dans ce didacticiel, nous aborderons les tests automatisés et ajoutons des tests unitaires supplémentaires qui couvrent les fonctionnalités d’application que nous implémentons.</span><span class="sxs-lookup"><span data-stu-id="405aa-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="405aa-161">Étape suivante</span><span class="sxs-lookup"><span data-stu-id="405aa-161">Next Step</span></span>

<span data-ttu-id="405aa-162">Nous disposons désormais d’une structure d’application de base en place.</span><span class="sxs-lookup"><span data-stu-id="405aa-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="405aa-163">Créons maintenant [une base de données pour stocker les données](create-a-database.md)de l’application.</span><span class="sxs-lookup"><span data-stu-id="405aa-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="405aa-164">[Précédent](introducing-the-nerddinner-tutorial.md)
> [Suivant](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="405aa-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
