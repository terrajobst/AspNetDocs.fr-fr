---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Ajout d’une colonne au modèle | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Créer une application Web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 1cf092c3db3959d6f47006f1be2ba82833c5dc06
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543581"
---
# <a name="adding-a-column-to-the-model"></a><span data-ttu-id="08e52-104">Ajout d’une colonne au modèle</span><span class="sxs-lookup"><span data-stu-id="08e52-104">Adding a Column to the Model</span></span>

<span data-ttu-id="08e52-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="08e52-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="08e52-106">Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="08e52-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="08e52-107">Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="08e52-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="08e52-108">Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="08e52-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="08e52-109">Dans cette section, nous allons découvrir comment nous pouvons apporter des modifications au schéma de notre base de données et gérer les modifications au sein de notre application.</span><span class="sxs-lookup"><span data-stu-id="08e52-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="08e52-110">Nous allons ajouter une colonne « Rating » à la table Movie.</span><span class="sxs-lookup"><span data-stu-id="08e52-110">Let's add a "Rating" Column to the Movie table.</span></span> <span data-ttu-id="08e52-111">Revenez à l’IDE, puis cliquez sur le explorateur de base de données.</span><span class="sxs-lookup"><span data-stu-id="08e52-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="08e52-112">Cliquez avec le bouton droit sur la table Movie et sélectionnez Ouvrir la définition de table.</span><span class="sxs-lookup"><span data-stu-id="08e52-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="08e52-113">Ajoutez une colonne « Rating » comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="08e52-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="08e52-114">Étant donné que nous n’avons pas de classification, la colonne peut accepter les valeurs NULL.</span><span class="sxs-lookup"><span data-stu-id="08e52-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="08e52-115">Cliquez sur Enregistrer.</span><span class="sxs-lookup"><span data-stu-id="08e52-115">Click Save.</span></span>

<span data-ttu-id="08e52-116">[Tableau des films de modification ![](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="08e52-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="08e52-117">Ensuite, revenez au Explorateur de solutions et ouvrez le fichier movies. edmx (qui se trouve dans le dossier \Models).</span><span class="sxs-lookup"><span data-stu-id="08e52-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="08e52-118">Cliquez avec le bouton droit sur l’aire de conception (zone blanche) et sélectionnez mettre à jour le modèle à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="08e52-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="08e52-119">[Films ![-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="08e52-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="08e52-120">Cette opération lance l' « Assistant Mise à jour ».</span><span class="sxs-lookup"><span data-stu-id="08e52-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="08e52-121">Cliquez sur l’onglet actualiser dans celui-ci, puis sur Terminer.</span><span class="sxs-lookup"><span data-stu-id="08e52-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="08e52-122">Notre classe Movie Model sera ensuite mise à jour avec la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="08e52-122">Our Movie model class will then be updated with the new column.</span></span>

![Assistant Mise à jour (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="08e52-124">Après avoir cliqué sur terminer, vous pouvez voir que la nouvelle colonne évaluation a été ajoutée à l’entité film dans notre modèle.</span><span class="sxs-lookup"><span data-stu-id="08e52-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="08e52-125">[Entité film ![](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="08e52-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="08e52-126">Nous avons ajouté une colonne dans le modèle de base de données, mais les vues ne le savent pas.</span><span class="sxs-lookup"><span data-stu-id="08e52-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="08e52-127">Mettre à jour les vues avec des modifications de modèle</span><span class="sxs-lookup"><span data-stu-id="08e52-127">Update Views with Model Changes</span></span>

<span data-ttu-id="08e52-128">Il existe plusieurs façons de mettre à jour nos modèles de vue pour refléter la nouvelle colonne d’évaluation.</span><span class="sxs-lookup"><span data-stu-id="08e52-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="08e52-129">Étant donné que nous avons créé ces vues en les générant via la boîte de dialogue Ajouter une vue, nous pourrions les supprimer et les recréer.</span><span class="sxs-lookup"><span data-stu-id="08e52-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="08e52-130">Toutefois, en général, les utilisateurs auront déjà apporté des modifications à leurs modèles de vue à partir de la génération générée automatiquement et voudront ajouter ou supprimer des champs manuellement, comme nous l’avons fait avec le champ ID pour Create.</span><span class="sxs-lookup"><span data-stu-id="08e52-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="08e52-131">Ouvrez le modèle \Views\Movies\Index.aspx et ajoutez un &lt;&gt;évaluation&lt;/th&gt; à l’en-tête de la table Movie.</span><span class="sxs-lookup"><span data-stu-id="08e52-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="08e52-132">J’ai ajouté la mine après le genre.</span><span class="sxs-lookup"><span data-stu-id="08e52-132">I added mine after Genre.</span></span> <span data-ttu-id="08e52-133">Ensuite, dans la même position de colonne mais en bas, ajoutez une ligne pour sortir notre nouvelle évaluation.</span><span class="sxs-lookup"><span data-stu-id="08e52-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="08e52-134">Notre dernier modèle index. aspx ressemble à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="08e52-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="08e52-135">Nous allons ensuite ouvrir le modèle \Views\Movies\Create.aspx et ajouter une étiquette et une zone de texte pour notre nouvelle propriété d’évaluation :</span><span class="sxs-lookup"><span data-stu-id="08e52-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="08e52-136">Notre dernier modèle Create. aspx ressemblera à ce qui suit, et nous allons modifier le titre de votre navigateur et le nom secondaire &lt;H2&gt; titre comme « créer un film », alors que nous sommes ici !</span><span class="sxs-lookup"><span data-stu-id="08e52-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="08e52-137">Exécutez votre application. à présent, vous disposez d’un nouveau champ dans la base de données qui a été ajouté à la page créer.</span><span class="sxs-lookup"><span data-stu-id="08e52-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="08e52-138">Ajoutez un nouveau film-cette fois avec une évaluation, puis cliquez sur créer.</span><span class="sxs-lookup"><span data-stu-id="08e52-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="08e52-139">[![créer un film-Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="08e52-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="08e52-140">Une fois que vous avez cliqué sur créer, vous êtes dirigé vers la page d’index dans laquelle le nouveau film est listé avec la nouvelle colonne d’évaluation de la base de données.</span><span class="sxs-lookup"><span data-stu-id="08e52-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="08e52-141">[Liste des films ![-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="08e52-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="08e52-142">Ce didacticiel de base vous a fait commencer à créer des contrôleurs, à les associer à des vues et à transmettre des données codées en dur.</span><span class="sxs-lookup"><span data-stu-id="08e52-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="08e52-143">Ensuite, nous avons créé et conçu une base de données et nous y avons mis des données.</span><span class="sxs-lookup"><span data-stu-id="08e52-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="08e52-144">Nous avons extrait les données de la base de données et les avons affichées dans une table HTML.</span><span class="sxs-lookup"><span data-stu-id="08e52-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="08e52-145">Ensuite, nous avons ajouté un formulaire créer qui permet à l’utilisateur d’ajouter des données à la base de données à partir de l’application Web.</span><span class="sxs-lookup"><span data-stu-id="08e52-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="08e52-146">Nous avons ajouté une validation, puis effectué la validation à l’aide de JavaScript côté client.</span><span class="sxs-lookup"><span data-stu-id="08e52-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="08e52-147">Enfin, nous avons modifié la base de données pour inclure une nouvelle colonne de données, puis mis à jour nos deux pages pour créer et afficher ces nouvelles données.</span><span class="sxs-lookup"><span data-stu-id="08e52-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="08e52-148">Je vous encourage à présent à passer à notre didacticiel de niveau intermédiaire «[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)», ainsi qu’aux nombreuses vidéos et ressources de [https://asp.net/mvc](https://asp.net/mvc) pour en savoir plus sur ASP.NET MVC !</span><span class="sxs-lookup"><span data-stu-id="08e52-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="08e52-149">Bonne lecture !</span><span class="sxs-lookup"><span data-stu-id="08e52-149">Enjoy!</span></span>

- <span data-ttu-id="08e52-150">Scott Hanselman- [http://hanselman.com](http://hanselman.com) et [@shanselman](http://twitter.com/shanselman) sur Twitter.</span><span class="sxs-lookup"><span data-stu-id="08e52-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="08e52-151">Précédent</span><span class="sxs-lookup"><span data-stu-id="08e52-151">Previous</span></span>](getting-started-with-mvc-part7.md)
