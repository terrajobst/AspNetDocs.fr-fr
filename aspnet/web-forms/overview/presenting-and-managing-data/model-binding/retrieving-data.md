---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Récupération et affichage de données avec la liaison de modèle et les Web Forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 81cca22cb4752d071d2a68986ae9ac2bed737594
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640195"
---
# <a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Récupération et affichage de données avec la liaison de modèle et les Web Forms

> Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple que le traitement des objets de source de données (tels que ObjectDataSource ou SqlDataSource). Cette série commence par une documentation introductive et passe à des concepts plus avancés dans des didacticiels ultérieurs.
> 
> Le modèle de liaison de modèle fonctionne avec n’importe quelle technologie d’accès aux données. Dans ce didacticiel, vous allez utiliser Entity Framework, mais vous pouvez utiliser la technologie d’accès aux données qui vous est la plus familière. À partir d’un contrôle serveur lié aux données, tel qu’un contrôle GridView, ListView, DetailsView ou FormView, vous spécifiez les noms des méthodes à utiliser pour la sélection, la mise à jour, la suppression et la création de données. Dans ce didacticiel, vous allez spécifier une valeur pour SelectMethod. 
> 
> Dans cette méthode, vous fournissez la logique de récupération des données. Dans le didacticiel suivant, vous allez définir des valeurs pour UpdateMethod, DeleteMethod et InsertMethod.
>
> Vous pouvez [Télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou Visual Basic. Le code téléchargeable fonctionne avec Visual Studio 2012 et versions ultérieures. Elle utilise le modèle Visual Studio 2012, qui est légèrement différent du modèle Visual Studio 2017 présenté dans ce didacticiel.
> 
> Dans le didacticiel, vous exécutez l’application dans Visual Studio. Vous pouvez également déployer l’application sur un fournisseur d’hébergement et la rendre disponible sur Internet. Microsoft offre un hébergement Web gratuit pour un maximum de 10 sites Web dans un  
> [compte d’essai gratuit d’Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Pour plus d’informations sur le déploiement d’un projet Visual Studio Web sur Azure App Service Web Apps, consultez la série [déploiement web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) . Ce didacticiel montre également comment utiliser Migrations Entity Framework Code First pour déployer votre base de données SQL Server sur Azure SQL Database.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> - Microsoft Visual Studio 2017 ou Microsoft Visual Studio Community 2017
>   
> Ce didacticiel fonctionne également avec Visual Studio 2012 et Visual Studio 2013, mais il existe des différences dans l’interface utilisateur et le modèle de projet.

## <a name="what-youll-build"></a>Ce que vous allez générer

Dans ce didacticiel, vous allez :

* Créer des objets de données reflétant une université avec les étudiants inscrits dans des cours
* Créer des tables de base de données à partir des objets
* Remplir la base de données avec des données de test
* Afficher des données dans un formulaire Web

## <a name="create-the-project"></a>Créer le projet

1. Dans Visual Studio 2017, créez un projet d' **application Web ASP.net (.NET Framework)** nommé **ContosoUniversityModelBinding**.

   ![créer un projet](retrieving-data/_static/image19.png)

2. Sélectionnez **OK**. La boîte de dialogue permettant de sélectionner un modèle s’affiche.

   ![sélectionner Web Forms](retrieving-data/_static/image3.png)

3. Sélectionnez le modèle **Web Forms** . 

4. Si nécessaire, modifiez l’authentification en **comptes d’utilisateur individuels**. 

5. Sélectionnez **OK** pour créer le projet.

## <a name="modify-site-appearance"></a>Modifier l’apparence du site

   Apportez quelques modifications pour personnaliser l’apparence du site. 
   
   1. Ouvrez le fichier site. Master.
   
   2. Modifiez le titre pour afficher **Contoso University** et non **mon application ASP.net**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. Remplacez le texte d’en-tête du nom de l' **application** par **Contoso University**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. Modifiez les liens de l’en-tête de navigation vers le site approprié. 
   
      Supprimez les liens relatifs à à **propos** de et **Contactez** et, à la place, créez un lien vers une page **étudiants** , que vous allez créer.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. Enregistrez site. Master.

## <a name="add-a-web-form-to-display-student-data"></a>Ajouter un formulaire Web pour afficher les données des étudiants

   1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur votre projet, sélectionnez **Ajouter** , puis **nouvel élément**. 
   
   2. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez le **formulaire Web avec le modèle de page maître** et nommez-le Student **. aspx**.

      ![créer une page](retrieving-data/_static/image5.png)

   3. Sélectionnez **Ajouter** .
   
   4. Pour la page maître du formulaire Web, sélectionnez **site. Master**.
   
   5. Sélectionnez **OK**.

## <a name="add-the-data-model"></a>Ajouter le modèle de données

Dans le dossier **Models** , ajoutez une classe nommée **UniversityModels.cs**.

   1. Cliquez avec le bouton droit sur **modèles**, sélectionnez **Ajouter**, puis **nouvel élément**. La boîte de dialogue **Ajouter un nouvel élément** s’affiche.

   2. Dans le menu de navigation gauche, sélectionnez **code**, puis **classe**.

      ![créer une classe de modèle](retrieving-data/_static/image20.png)

   3. Nommez la classe **UniversityModels.cs** , puis sélectionnez **Ajouter**.

      Dans ce fichier, définissez les classes `SchoolContext`, `Student`, `Enrollment`et `Course` comme suit :

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      La classe `SchoolContext` dérive de `DbContext`, qui gère la connexion à la base de données et les modifications apportées aux données.

      Dans la classe `Student`, notez les attributs appliqués aux propriétés `FirstName`, `LastName`et `Year`. Ce didacticiel utilise ces attributs pour la validation des données. Pour simplifier le code, seules ces propriétés sont marquées avec des attributs de validation de données. Dans un projet réel, vous devez appliquer des attributs de validation à toutes les propriétés nécessitant une validation.

   4. Enregistrez UniversityModels.cs.

## <a name="set-up-the-database-based-on-classes"></a>Configurer la base de données en fonction des classes

Ce didacticiel utilise [migrations code First](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) pour créer des objets et des tables de base de données. Ces tables stockent des informations sur les étudiants et leurs cours.

   1. Cliquez sur **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.

   2. Dans la **console du gestionnaire de package**, exécutez la commande suivante :  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      Si la commande se termine correctement, un message indiquant que la migration a été activée s’affiche.

      ![activer les migrations](retrieving-data/_static/image8.png)

      Notez qu’un fichier nommé *Configuration.cs* a été créé. La classe `Configuration` a une méthode `Seed`, qui peut préremplir les tables de base de données avec des données de test.

## <a name="pre-populate-the-database"></a>Pré-remplir la base de données

   1. Ouvrez Configuration.cs.
   
   2. Ajoutez le code suivant à la méthode `Seed` . Ajoutez également une instruction `using` pour l’espace de noms `ContosoUniversityModelBinding. Models`.

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. Enregistrez Configuration.cs.

   4. Dans la console du gestionnaire de package, exécutez la commande **Add-migration initial**.

   5. Exécutez la commande **Update-Database**.

      Si vous recevez une exception lors de l’exécution de cette commande, les valeurs `StudentID` et `CourseID` peuvent être différentes des valeurs de méthode `Seed`. Ouvrez ces tables de base de données et recherchez les valeurs existantes pour `StudentID` et `CourseID`. Ajoutez ces valeurs au code pour l’amorçage de la table `Enrollments`.

## <a name="add-a-gridview-control"></a>Ajouter un contrôle GridView

Avec les données de base de données remplies, vous êtes maintenant prêt à récupérer ces données et à les afficher. 

1. Ouvrez students. aspx.

2. Recherchez l’espace réservé `MainContent`. Dans cet espace réservé, ajoutez un contrôle **GridView** qui comprend ce code.

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   Points à noter :
   * Notez la valeur définie pour la propriété `SelectMethod` dans l’élément GridView. Cette valeur spécifie la méthode utilisée pour récupérer les données GridView, que vous créez à l’étape suivante. 
   
   * La propriété `ItemType` est définie sur la classe `Student` créée précédemment. Ce paramètre vous permet de référencer des propriétés de classe dans le balisage. Par exemple, la classe `Student` a une collection nommée `Enrollments`. Vous pouvez utiliser `Item.Enrollments` pour récupérer cette collection, puis utiliser la [syntaxe LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) pour récupérer la somme des crédits inscrits de chaque étudiant.
   
3. Enregistrez students. aspx.

## <a name="add-code-to-retrieve-data"></a>Ajouter du code pour récupérer des données

   Dans le fichier code-behind studentss. aspx, ajoutez la méthode spécifiée pour la valeur `SelectMethod`. 
   
   1. Ouvrez Students.aspx.cs.
   
   2. Ajoutez des instructions `using` pour les espaces de noms `ContosoUniversityModelBinding. Models` et `System.Data.Entity`.

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. Ajoutez la méthode que vous avez spécifiée pour `SelectMethod`:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      La clause `Include` améliore les performances des requêtes, mais n’est pas obligatoire. Sans la clause `Include`, les données sont récupérées à l’aide du [*chargement différé*](https://en.wikipedia.org/wiki/Lazy_loading), ce qui implique l’envoi d’une requête distincte à la base de données chaque fois que des données associées sont récupérées. Avec la clause `Include`, les données sont récupérées à l’aide *du chargement hâtif*, ce qui signifie qu’une requête de base de données unique récupère toutes les données associées. Si les données associées ne sont pas utilisées, le chargement hâtif est moins efficace, car davantage de données sont récupérées. Toutefois, dans ce cas, le chargement hâtif offre des performances optimales, car les données associées sont affichées pour chaque enregistrement.

      Pour plus d’informations sur les considérations relatives aux performances lors du chargement de données associées, consultez la section **chargement différé, hâtif et explicite de données connexes** dans l’article [lecture des données associées avec l’Entity Framework dans une application MVC ASP.net](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .

      Par défaut, les données sont triées en fonction des valeurs de la propriété marquée comme clé. Vous pouvez ajouter une clause `OrderBy` pour spécifier une autre valeur de tri. Dans cet exemple, la propriété `StudentID` par défaut est utilisée pour le tri. Dans l’article [Trier, paginer et filtrer les données](sorting-paging-and-filtering-data.md) , l’utilisateur est autorisé à sélectionner une colonne pour le tri.
 
   4. Enregistrez Students.aspx.cs.

## <a name="run-your-application"></a>Exécuter votre application 

Exécutez votre application Web (**F5**) et accédez à la page des **étudiants** , qui affiche les informations suivantes :

   ![afficher les données](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Génération automatique de méthodes de liaison de modèle

Lorsque vous utilisez cette série de didacticiels, vous pouvez simplement copier le code du didacticiel vers votre projet. Toutefois, l’un des inconvénients de cette approche est que vous ne connaissez peut-être pas la fonctionnalité fournie par Visual Studio pour générer automatiquement du code pour les méthodes de liaison de modèle. Lorsque vous travaillez sur vos propres projets, la génération automatique de code peut vous faire gagner du temps et vous aider à mieux comprendre comment implémenter une opération. Cette section décrit la fonctionnalité de génération de code automatique. Cette section est uniquement à titre d’information et ne contient pas de code que vous devez implémenter dans votre projet. 

Lorsque vous définissez une valeur pour les propriétés `SelectMethod`, `UpdateMethod`, `InsertMethod`ou `DeleteMethod` dans le code de balisage, vous pouvez sélectionner l’option **créer une nouvelle méthode** .

![créer une méthode](retrieving-data/_static/image18.png)

Visual Studio crée non seulement une méthode dans le code-behind avec la signature appropriée, mais génère également le code d’implémentation pour effectuer l’opération. Si vous définissez d’abord la propriété `ItemType` avant d’utiliser la fonctionnalité de génération de code automatique, le code généré utilise ce type pour les opérations. Par exemple, lors de la définition de la propriété `UpdateMethod`, le code suivant est généré automatiquement :

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Là encore, ce code n’a pas besoin d’être ajouté à votre projet. Dans le didacticiel suivant, vous allez implémenter des méthodes de mise à jour, de suppression et d’ajout de nouvelles données.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez créé des classes de modèle de données et généré une base de données à partir de ces classes. Vous avez rempli les tables de base de données avec des données de test. Vous avez utilisé la liaison de modèle pour récupérer des données de la base de données, puis affiché les données dans un GridView.

Dans le [didacticiel](updating-deleting-and-creating-data.md) suivant de cette série, vous allez activer la mise à jour, la suppression et la création de données.

> [!div class="step-by-step"]
> [Next](updating-deleting-and-creating-data.md)
