---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Créer une base de données | Microsoft Docs
author: microsoft
description: L’étape 2 montre les étapes permettant de créer la base de données contenant toutes les données de dîner et RSVP pour notre application NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b0aa7c8cdf741f44e09ed18e2b2f73fe6bf786ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581003"
---
# <a name="create-a-database"></a><span data-ttu-id="fcc4f-103">Créer une base de données</span><span class="sxs-lookup"><span data-stu-id="fcc4f-103">Create a Database</span></span>

<span data-ttu-id="fcc4f-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="fcc4f-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="fcc4f-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="fcc4f-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="fcc4f-106">Il s’agit de l’étape 2 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="fcc4f-107">L’étape 2 montre les étapes permettant de créer la base de données contenant toutes les données de dîner et RSVP pour notre application NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="fcc4f-108">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="fcc4f-109">NerdDinner étape 2 : création de la base de données</span><span class="sxs-lookup"><span data-stu-id="fcc4f-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="fcc4f-110">Nous utiliserons une base de données pour stocker toutes les données de dîner et RSVP pour notre application NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="fcc4f-111">Les étapes ci-dessous illustrent la création de la base de données à l’aide de l’édition gratuite SQL Server Express (que vous pouvez facilement installer à l’aide de V2 du [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="fcc4f-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="fcc4f-112">Tout le code que nous allons écrire fonctionne avec SQL Server Express et la SQL Server complète.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="fcc4f-113">Création d’une nouvelle base de données SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="fcc4f-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="fcc4f-114">Nous allons commencer par cliquer avec le bouton droit sur notre projet Web, puis sélectionner la commande de menu **Ajouter-&gt;nouvel élément** :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="fcc4f-115">Cela permet d’afficher la boîte de dialogue « Ajouter un nouvel élément » de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="fcc4f-116">Nous allons filtrer par la catégorie « données » et sélectionner le modèle d’élément « base de données SQL Server » :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="fcc4f-117">Nous allons nommer la base de données SQL Server Express que nous voulons créer « NerdDinner. mdf » et cliquer sur OK.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="fcc4f-118">Visual Studio vous demandera ensuite si nous souhaitons ajouter ce fichier à notre répertoire de données \App\_(qui est un répertoire déjà configuré avec des ACL de sécurité en lecture et en écriture) :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="fcc4f-119">Nous cliquons sur « Oui » et la nouvelle base de données sera créée et ajoutée à notre Explorateur de solutions :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="fcc4f-120">Création de tables au sein de notre base de données</span><span class="sxs-lookup"><span data-stu-id="fcc4f-120">Creating Tables within our Database</span></span>

<span data-ttu-id="fcc4f-121">Nous avons maintenant une nouvelle base de données vide.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-121">We now have a new empty database.</span></span> <span data-ttu-id="fcc4f-122">Nous allons y ajouter des tables.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-122">Let's add some tables to it.</span></span>

<span data-ttu-id="fcc4f-123">Pour ce faire, nous allons accéder à la fenêtre « Explorateur de serveurs » de l’onglet dans Visual Studio, qui nous permet de gérer les serveurs et les bases de données.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="fcc4f-124">SQL Server Express bases de données stockées dans le dossier \App\_Data de notre application s’affichent automatiquement dans le Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="fcc4f-125">Vous pouvez également utiliser l’icône « se connecter à la base de données » en haut de la fenêtre « Explorateur de serveurs » pour ajouter d’autres bases de données SQL Server (à la fois locales et distantes) à la liste :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="fcc4f-126">Nous allons ajouter deux tables à notre base de données NerdDinner : une pour stocker nos dîners, et l’autre pour suivre les acceptations des RSVP.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="fcc4f-127">Nous pouvons créer des tables en cliquant avec le bouton droit sur le dossier « tables » dans notre base de données et en choisissant la commande de menu « Ajouter une nouvelle table » :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="fcc4f-128">Cela ouvre un concepteur de tables qui nous permet de configurer le schéma de notre table.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="fcc4f-129">Pour notre table « dîners », nous ajouterons 10 colonnes de données :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="fcc4f-130">Nous voulons que la colonne « DinnerID » soit une clé primaire unique pour la table.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="fcc4f-131">Pour ce faire, vous pouvez cliquer avec le bouton droit sur la colonne « DinnerID » et choisir l’élément de menu « définir la clé primaire » :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="fcc4f-132">En plus de faire de DinnerID une clé primaire, nous voulons également la configurer en tant que colonne « identity » dont la valeur est incrémentée automatiquement à mesure que de nouvelles lignes de données sont ajoutées à la table (ce qui signifie que la première ligne de dîner insérée aura un DinnerID de 1, la deuxième ligne insérée aura un DinnerID de 2, etc.).</span><span class="sxs-lookup"><span data-stu-id="fcc4f-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="fcc4f-133">Pour ce faire, vous pouvez sélectionner la colonne « DinnerID », puis utiliser l’éditeur de propriétés de colonne pour définir la propriété « (is Identity) » sur la colonne sur « Oui ».</span><span class="sxs-lookup"><span data-stu-id="fcc4f-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="fcc4f-134">Nous utiliserons les valeurs par défaut de l’identité standard (à partir de 1 et incrément 1 sur chaque nouvelle ligne de dîner) :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="fcc4f-135">Nous allons ensuite enregistrer notre table en tapant Ctrl-S ou à l’aide de la commande **de menu fichier-&gt;enregistrer** .</span><span class="sxs-lookup"><span data-stu-id="fcc4f-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="fcc4f-136">Cela nous invite à nommer la table.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-136">This will prompt us to name the table.</span></span> <span data-ttu-id="fcc4f-137">Nous allons le nommer « dîners » :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="fcc4f-138">Notre nouveau tableau des dîners s’affiche alors dans la base de données de l’Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="fcc4f-139">Nous répéterons ensuite les étapes ci-dessus et créons une table « RSVP ».</span><span class="sxs-lookup"><span data-stu-id="fcc4f-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="fcc4f-140">Cette table avec 3 colonnes.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-140">This table with have 3 columns.</span></span> <span data-ttu-id="fcc4f-141">Nous allons configurer la colonne RsvpID comme clé primaire et en faire une colonne d’identité :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="fcc4f-142">Nous allons l’enregistrer et lui attribuer le nom « RSVP ».</span><span class="sxs-lookup"><span data-stu-id="fcc4f-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="fcc4f-143">Configuration d’une relation de clé étrangère entre des tables</span><span class="sxs-lookup"><span data-stu-id="fcc4f-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="fcc4f-144">Nous avons maintenant deux tables au sein de notre base de données.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-144">We now have two tables within our database.</span></span> <span data-ttu-id="fcc4f-145">Notre dernière étape de conception de schéma consiste à configurer une relation « un-à-plusieurs » entre ces deux tables, afin que nous puissions associer chaque ligne de dîner à zéro, une ou plusieurs lignes RSVP qui s’y appliquent.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="fcc4f-146">Pour ce faire, nous allons configurer la colonne « DinnerID » de la table RSVP de façon à ce qu’elle ait une relation de clé étrangère avec la colonne « DinnerID » de la table « dîners ».</span><span class="sxs-lookup"><span data-stu-id="fcc4f-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="fcc4f-147">Pour ce faire, nous allons ouvrir la table RSVP dans le concepteur de tables en double-cliquant dessus dans l’Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="fcc4f-148">Nous allons ensuite sélectionner la colonne « DinnerID » dans celle-ci, cliquer avec le bouton droit et sélectionner « relations... ». commande de menu contextuel :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationships…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="fcc4f-149">Cela affiche une boîte de dialogue que nous pouvons utiliser pour configurer les relations entre les tables :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="fcc4f-150">Nous allons cliquer sur le bouton « Ajouter » pour ajouter une nouvelle relation à la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="fcc4f-151">Une fois qu’une relation a été ajoutée, nous allons développer le nœud de vue d’arborescence « tables et spécification de colonne » dans la grille des propriétés, à droite de la boîte de dialogue, puis cliquer sur le bouton « ... » à droite du bouton :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="fcc4f-152">En cliquant sur le bouton « ... » affichera une autre boîte de dialogue qui nous permet de spécifier les tables et les colonnes impliquées dans la relation, et de nous permettre de nommer la relation.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="fcc4f-153">Nous allons modifier la table de clé primaire en « dîners », puis sélectionner la colonne « DinnerID » dans la table des dîners comme clé primaire.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="fcc4f-154">Notre table RSVP est la table de clé étrangère et le RSVP. La colonne DinnerID sera associée en tant que clé étrangère :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="fcc4f-155">À présent, chaque ligne de la table RSVP sera associée à une ligne de la table dinner.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="fcc4f-156">SQL Server conserve l’intégrité référentielle pour nous, et nous empêche d’ajouter une nouvelle ligne RSVP si elle ne pointe pas vers une ligne de dîner valide.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="fcc4f-157">Cela nous empêchera également de supprimer une ligne dîner si des lignes RSVP y font référence.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="fcc4f-158">Ajout de données à nos tables</span><span class="sxs-lookup"><span data-stu-id="fcc4f-158">Adding Data to our Tables</span></span>

<span data-ttu-id="fcc4f-159">Commençons par ajouter des exemples de données à notre table dîners.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="fcc4f-160">Nous pouvons ajouter des données à une table en cliquant dessus avec le bouton droit dans le Explorateur de serveurs et en choisissant la commande « Afficher les données de la table » :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="fcc4f-161">Nous allons ajouter quelques lignes de données de dîner que nous pouvons utiliser ultérieurement lorsque nous commencerons à implémenter l’application :</span><span class="sxs-lookup"><span data-stu-id="fcc4f-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="fcc4f-162">Étape suivante</span><span class="sxs-lookup"><span data-stu-id="fcc4f-162">Next Step</span></span>

<span data-ttu-id="fcc4f-163">Nous avons terminé la création de notre base de données.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-163">We've finished creating our database.</span></span> <span data-ttu-id="fcc4f-164">Créons maintenant des classes de modèle que nous pouvons utiliser pour interroger et mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="fcc4f-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fcc4f-165">[Précédent](create-a-new-aspnet-mvc-project.md)
> [Suivant](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="fcc4f-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
