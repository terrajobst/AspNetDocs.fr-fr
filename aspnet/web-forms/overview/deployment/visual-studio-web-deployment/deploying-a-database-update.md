---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour de base de données | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 805eb84c24764cf921291f89054435601dbac48e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547739"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="f5659-103">Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour de base de données</span><span class="sxs-lookup"><span data-stu-id="f5659-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>

<span data-ttu-id="f5659-104">par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f5659-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="f5659-105">Télécharger le projet de démarrage</span><span class="sxs-lookup"><span data-stu-id="f5659-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="f5659-106">Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou de Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="f5659-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="f5659-107">Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f5659-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="f5659-108">Présentation</span><span class="sxs-lookup"><span data-stu-id="f5659-108">Overview</span></span>

<span data-ttu-id="f5659-109">Dans ce didacticiel, vous apportez une modification à la base de données et des modifications de code connexes, vous testez les modifications dans Visual Studio, puis vous déployez la mise à jour dans les environnements de test, intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="f5659-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="f5659-110">Le didacticiel explique d’abord comment mettre à jour une base de données gérée par Migrations Code First, puis comment mettre à jour une base de données à l’aide du fournisseur dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="f5659-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="f5659-111">Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f5659-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="f5659-112">Déployer une mise à jour de base de données à l’aide de Migrations Code First</span><span class="sxs-lookup"><span data-stu-id="f5659-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="f5659-113">Dans cette section, vous allez ajouter une colonne de date de naissance à la classe de base `Person` pour les entités `Student` et `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="f5659-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="f5659-114">Ensuite, vous mettez à jour la page qui affiche les données de l’instructeur afin qu’il affiche la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="f5659-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="f5659-115">Enfin, vous déployez les modifications apportées à test, intermédiaire et production.</span><span class="sxs-lookup"><span data-stu-id="f5659-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="f5659-116">Ajouter une colonne à une table dans la base de données d’application</span><span class="sxs-lookup"><span data-stu-id="f5659-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="f5659-117">Dans le projet *ContosoUniversity. dal* , ouvrez *Person.cs* et ajoutez la propriété suivante à la fin de la classe `Person` (il doit y avoir deux accolades fermantes après) :</span><span class="sxs-lookup"><span data-stu-id="f5659-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="f5659-118">Ensuite, mettez à jour la méthode `Seed` afin qu’elle fournisse une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="f5659-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="f5659-119">Ouvrez *Migrations\Configuration.cs* et remplacez le bloc de code qui commence `var instructors = new List<Instructor>` par le bloc de code suivant, qui comprend les informations de date de naissance :</span><span class="sxs-lookup"><span data-stu-id="f5659-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="f5659-120">Générez la solution, puis ouvrez la fenêtre de **console du gestionnaire de package** .</span><span class="sxs-lookup"><span data-stu-id="f5659-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="f5659-121">Assurez-vous que ContosoUniversity. DAL est toujours sélectionné comme **projet par défaut**.</span><span class="sxs-lookup"><span data-stu-id="f5659-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="f5659-122">Dans la fenêtre de la **console du gestionnaire de package** , sélectionnez **ContosoUniversity. dal** comme **projet par défaut**, puis entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f5659-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="f5659-123">Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMigration` classe, et dans la méthode `Up` vous pouvez voir le code qui crée la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="f5659-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="f5659-124">La méthode `Up` crée la colonne lorsque vous implémentez la modification, et la méthode `Down` supprime la colonne lorsque vous restaurez la modification.</span><span class="sxs-lookup"><span data-stu-id="f5659-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="f5659-126">Générez la solution, puis entrez la commande suivante dans la fenêtre de la **console du gestionnaire de package** (Assurez-vous que le projet CONTOSOUNIVERSITY. dal est toujours sélectionné) :</span><span class="sxs-lookup"><span data-stu-id="f5659-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="f5659-127">Le Entity Framework exécute la méthode `Up`, puis exécute la méthode `Seed`.</span><span class="sxs-lookup"><span data-stu-id="f5659-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="f5659-128">Afficher la nouvelle colonne dans la page des enseignants</span><span class="sxs-lookup"><span data-stu-id="f5659-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="f5659-129">Dans le projet ContosoUniversity, ouvrez *Instructors. aspx* et ajoutez un nouveau champ de modèle pour afficher la date de naissance.</span><span class="sxs-lookup"><span data-stu-id="f5659-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="f5659-130">Ajoutez-le entre ceux pour la date d’embauche et l’attribution de bureau :</span><span class="sxs-lookup"><span data-stu-id="f5659-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="f5659-131">(Si la mise en retrait du code est désynchronisée, vous pouvez appuyer sur CTRL-K, puis sur CTRL + D pour reformater automatiquement le fichier.)</span><span class="sxs-lookup"><span data-stu-id="f5659-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="f5659-132">Exécutez l’application et cliquez sur le lien **enseignants** .</span><span class="sxs-lookup"><span data-stu-id="f5659-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="f5659-133">Lorsque la page se charge, vous voyez qu’elle contient le champ nouvelle date de naissance.</span><span class="sxs-lookup"><span data-stu-id="f5659-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Page des enseignants avec DateNaissance](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="f5659-135">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="f5659-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="f5659-136">Déployer la mise à jour de la base de données</span><span class="sxs-lookup"><span data-stu-id="f5659-136">Deploy the database update</span></span>

1. <span data-ttu-id="f5659-137">Dans **Explorateur de solutions** sélectionnez le projet ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="f5659-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="f5659-138">Dans la barre d’outils **publication Web en un clic** , cliquez sur le profil de publication de **test** , puis sur **publier le site Web**.</span><span class="sxs-lookup"><span data-stu-id="f5659-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="f5659-139">(Si la barre d’outils est désactivée, sélectionnez le projet ContosoUniversity dans **Explorateur de solutions**.)</span><span class="sxs-lookup"><span data-stu-id="f5659-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="f5659-140">Visual Studio déploie l’application mise à jour et le navigateur s’ouvre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="f5659-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="f5659-141">Exécutez la page des **enseignants** pour vérifier que la mise à jour a été correctement déployée.</span><span class="sxs-lookup"><span data-stu-id="f5659-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="f5659-142">Lorsque l’application tente d’accéder à la base de données pour cette page, Code First met à jour le schéma de la base de données et exécute la méthode `Seed`.</span><span class="sxs-lookup"><span data-stu-id="f5659-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="f5659-143">Lorsque la page s’affiche, la colonne de **Date de naissance** attendue contient des dates.</span><span class="sxs-lookup"><span data-stu-id="f5659-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="f5659-144">Dans la barre d’outils **publication Web en un clic** , cliquez sur le profil de publication **intermédiaire** , puis sur **publier le site Web**.</span><span class="sxs-lookup"><span data-stu-id="f5659-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="f5659-145">Exécutez la page des **instructeurs** dans l’environnement intermédiaire pour vérifier que la mise à jour a été correctement déployée.</span><span class="sxs-lookup"><span data-stu-id="f5659-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="f5659-146">Dans la barre d’outils **publication Web en un clic** , cliquez sur le profil de publication de **production** , puis cliquez sur **publier le site Web**.</span><span class="sxs-lookup"><span data-stu-id="f5659-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="f5659-147">Exécutez la page des **enseignants** en production pour vérifier que la mise à jour a été correctement déployée.</span><span class="sxs-lookup"><span data-stu-id="f5659-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="f5659-148">Pour une mise à jour réelle de l’application de production qui comprend une modification de base de données, vous devez également mettre l’application hors connexion au cours du déploiement à l’aide de l’application *\_offline. htm*, comme vous l’avez vu dans le didacticiel précédent.</span><span class="sxs-lookup"><span data-stu-id="f5659-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="f5659-149">Déployer une mise à jour de base de données à l’aide du fournisseur dbDacFx</span><span class="sxs-lookup"><span data-stu-id="f5659-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="f5659-150">Dans cette section, vous allez ajouter une colonne *Commentaires* à la table *utilisateur* de la base de données d’appartenance et créer une page qui vous permet d’afficher et de modifier des commentaires pour chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f5659-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="f5659-151">Ensuite, vous déployez les modifications apportées à test, intermédiaire et production.</span><span class="sxs-lookup"><span data-stu-id="f5659-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="f5659-152">Ajouter une colonne à une table dans la base de données d’appartenance</span><span class="sxs-lookup"><span data-stu-id="f5659-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="f5659-153">Dans Visual Studio, ouvrez **Explorateur d’objets SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="f5659-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="f5659-154">Développez (base de données locale **) \v11.0**, développez **bases de données**, développez **ASPNET-ContosoUniversity** ( **et non ASPNET-ContosoUniversity-prod**), puis développez **tables**.</span><span class="sxs-lookup"><span data-stu-id="f5659-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="f5659-155">Si vous ne voyez pas la zone **(\v11.0)** sous le nœud **SQL Server** , cliquez avec le bouton droit sur le nœud **SQL Server** , puis cliquez sur **Ajouter un SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="f5659-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="f5659-156">Dans la boîte de dialogue **se connecter au serveur** , entrez (base de données locale *) \v11.0* comme **nom de serveur**, puis cliquez sur **se connecter**.</span><span class="sxs-lookup"><span data-stu-id="f5659-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="f5659-157">Si vous ne voyez pas **ASPNET-ContosoUniversity**, exécutez le projet et connectez-vous à l’aide des informations d’identification de l' *administrateur* (le mot de passe est *devpwd*), puis actualisez la fenêtre de **Explorateur d’objets SQL Server** .</span><span class="sxs-lookup"><span data-stu-id="f5659-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="f5659-158">Cliquez avec le bouton droit sur le tableau **utilisateurs** , puis cliquez sur **Concepteur de vues**.</span><span class="sxs-lookup"><span data-stu-id="f5659-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Concepteur de vues SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="f5659-160">Dans le concepteur, ajoutez une colonne de *Commentaires* et définissez-la comme *nvarchar (128)* et Nullable, puis cliquez sur **mettre à jour**.</span><span class="sxs-lookup"><span data-stu-id="f5659-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Ajout d’une colonne de commentaires](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="f5659-162">Dans la zone **aperçu des mises à jour de la base de données** , cliquez sur **mettre à jour la base de données**.</span><span class="sxs-lookup"><span data-stu-id="f5659-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Aperçu des mises à jour de la base de données](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="f5659-164">Créer une page pour afficher et modifier la nouvelle colonne</span><span class="sxs-lookup"><span data-stu-id="f5659-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="f5659-165">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier du **compte** dans le projet ContosoUniversity, cliquez sur **Ajouter**, puis sur **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="f5659-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="f5659-166">Créez un nouveau **formulaire Web à l’aide de la page maître** et nommez-le *UserInfo. aspx*.</span><span class="sxs-lookup"><span data-stu-id="f5659-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="f5659-167">Acceptez le fichier *site. Master* par défaut en tant que page maître.</span><span class="sxs-lookup"><span data-stu-id="f5659-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="f5659-168">Copiez le balisage suivant dans l’élément `MainContent` `Content` (le dernier des 3 éléments `Content`) :</span><span class="sxs-lookup"><span data-stu-id="f5659-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="f5659-169">Cliquez avec le bouton droit sur la page *UserInfo. aspx* , puis cliquez sur **afficher dans le navigateur**.</span><span class="sxs-lookup"><span data-stu-id="f5659-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="f5659-170">Connectez-vous avec vos informations d’identification de l’utilisateur *administrateur* (le mot de passe est *devpwd*) et ajoutez des commentaires à l’utilisateur pour vérifier que la page fonctionne correctement.</span><span class="sxs-lookup"><span data-stu-id="f5659-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Page UserInfo](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="f5659-172">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="f5659-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="f5659-173">Déployer la mise à jour de la base de données</span><span class="sxs-lookup"><span data-stu-id="f5659-173">Deploy the database update</span></span>

<span data-ttu-id="f5659-174">Pour procéder au déploiement à l’aide du fournisseur dbDacFx, il vous suffit de sélectionner l’option **mettre à jour la base de données** dans le profil de publication.</span><span class="sxs-lookup"><span data-stu-id="f5659-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="f5659-175">Toutefois, pour le déploiement initial lorsque vous avez utilisé cette option, vous avez également configuré des scripts SQL supplémentaires à exécuter : ceux-ci se trouvent toujours dans le profil et vous devrez les empêcher de s’exécuter à nouveau.</span><span class="sxs-lookup"><span data-stu-id="f5659-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="f5659-176">Ouvrez l’Assistant **publier le site Web** en cliquant avec le bouton droit sur le projet ContosoUniversity, puis en cliquant sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="f5659-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="f5659-177">Sélectionnez le profil **test** .</span><span class="sxs-lookup"><span data-stu-id="f5659-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="f5659-178">Cliquez sur l'onglet **Paramètres** .</span><span class="sxs-lookup"><span data-stu-id="f5659-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="f5659-179">Sous **DefaultConnection**, sélectionnez **mettre à jour la base de données**.</span><span class="sxs-lookup"><span data-stu-id="f5659-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="f5659-180">Désactivez les scripts supplémentaires que vous avez configurés pour s’exécuter pour le déploiement initial :</span><span class="sxs-lookup"><span data-stu-id="f5659-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="f5659-181">Cliquez sur **configurer les mises à jour de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="f5659-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="f5659-182">Dans la boîte de dialogue **configurer les mises à jour de la base de données** , désactivez les cases à cocher en regard de *Grant. SQL* et *ASPNET-Data-dev. SQL*.</span><span class="sxs-lookup"><span data-stu-id="f5659-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="f5659-183">Cliquez sur **Fermer**.</span><span class="sxs-lookup"><span data-stu-id="f5659-183">Click **Close**.</span></span>
6. <span data-ttu-id="f5659-184">Cliquez sur l'onglet **Aperçu** .</span><span class="sxs-lookup"><span data-stu-id="f5659-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="f5659-185">Sous **bases de données** et à droite de **DefaultConnection**, cliquez sur le lien **aperçu de la base de données** .</span><span class="sxs-lookup"><span data-stu-id="f5659-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Aperçu de la base de données](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="f5659-187">La fenêtre d’aperçu affiche le script qui sera exécuté dans la base de données de destination pour que le schéma de base de données corresponde au schéma de la base de données source.</span><span class="sxs-lookup"><span data-stu-id="f5659-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="f5659-188">Le script inclut une commande ALTER TABLE qui ajoute la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="f5659-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="f5659-189">Fermez la boîte **de dialogue Aperçu de la base de données** , puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="f5659-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="f5659-190">Visual Studio déploie l’application mise à jour et le navigateur s’ouvre sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="f5659-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="f5659-191">Exécutez la page UserInfo (ajouter *Account/UserInfo. aspx* à l’URL de la page d’hébergement) pour vérifier que la mise à jour a été correctement déployée.</span><span class="sxs-lookup"><span data-stu-id="f5659-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="f5659-192">Vous devez vous connecter en entrant *admin* et *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="f5659-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="f5659-193">Les données des tables ne sont pas déployées par défaut, et vous n’avez pas configuré de script de déploiement de données à exécuter. vous ne trouverez donc pas le commentaire que vous avez ajouté au développement.</span><span class="sxs-lookup"><span data-stu-id="f5659-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="f5659-194">Vous pouvez ajouter un nouveau commentaire maintenant dans l’environnement intermédiaire pour vérifier que la modification a été déployée dans la base de données et que la page fonctionne correctement.</span><span class="sxs-lookup"><span data-stu-id="f5659-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="f5659-195">Suivez la même procédure pour effectuer le déploiement dans un environnement intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="f5659-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="f5659-196">N’oubliez pas de désactiver les scripts supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="f5659-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="f5659-197">La seule différence par rapport au profil test est que vous ne désactivez qu’un seul script dans les profils intermédiaire et de production, car ils ont été configurés pour s’exécuter uniquement *ASPNET-prod-Data. SQL*.</span><span class="sxs-lookup"><span data-stu-id="f5659-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="f5659-198">Les informations d’identification pour la mise en lots et la production sont admin et prodpwd.</span><span class="sxs-lookup"><span data-stu-id="f5659-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="f5659-199">Pour une mise à jour réelle de l’application de production qui comprend une modification de base de données, vous devez également mettre l’application hors connexion pendant le déploiement en téléchargeant l’application *\_offline. htm* avant de la publier et de la supprimer, comme vous l’avez vu dans [le didacticiel précédent](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="f5659-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="f5659-200">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="f5659-200">Summary</span></span>

<span data-ttu-id="f5659-201">Vous avez maintenant déployé une mise à jour d’application qui comprenait une modification de base de données à l’aide des Migrations Code First et du fournisseur dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="f5659-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Page des enseignants avec DateNaissance](deploying-a-database-update/_static/image8.png)

![Page UserInfo](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="f5659-204">Le didacticiel suivant vous montre comment exécuter des déploiements à l’aide de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="f5659-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f5659-205">[Précédent](deploying-a-code-update.md)
> [Suivant](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="f5659-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
