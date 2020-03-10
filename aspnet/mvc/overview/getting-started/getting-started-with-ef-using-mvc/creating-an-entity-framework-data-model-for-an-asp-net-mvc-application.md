---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 'Didacticiel : prise en main de Entity Framework 6 Code First à l’aide de MVC 5 | Microsoft Docs'
description: Dans cette série de didacticiels, vous allez apprendre à créer une application ASP.NET MVC 5 qui utilise Entity Framework 6 pour accéder aux données.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a00a5a3aa295c2584b90abbf931c258c9e1b58ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616129"
---
# <a name="tutorial-get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Didacticiel : prise en main de Entity Framework 6 Code First à l’aide de MVC 5

> [!NOTE]
> Pour les nouveaux développements, nous vous recommandons de [ASP.NET Core Razor pages](/aspnet/core/razor-pages) sur les contrôleurs et les vues MVC ASP.net. Pour obtenir une série de didacticiels similaire à celle-ci à l’aide de Razor Pages, consultez [Didacticiel : prise en main de Razor pages dans ASP.net Core](/aspnet/core/tutorials/razor-pages/razor-pages-start). Le nouveau didacticiel :
> * est plus facile à suivre ;
> * Fournit d’autres bonnes pratiques sur EF Core.
> * Utilise des requêtes plus efficaces.
> * Est plus à jour avec la dernière API.
> * couvre davantage de fonctionnalités ;
> * représente la meilleure approche pour le développement de nouvelles applications.

Dans cette série de didacticiels, vous allez apprendre à créer une application ASP.NET MVC 5 qui utilise Entity Framework 6 pour accéder aux données. Ce didacticiel utilise le flux de travail Code First. Pour plus d’informations sur le choix entre Code First, Database First et Model First, consultez [créer un modèle](/ef/ef6/modeling/).

Cette série de didacticiels explique comment créer l’exemple d’application Contoso University. L’exemple d’application est un simple site Web universitaire. Il vous permet d’afficher et de mettre à jour les informations relatives aux étudiants, aux cours et aux enseignants. Voici deux des écrans que vous créez :

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Modifier le Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créer une application Web MVC
> * Définir le style de site
> * Installer Entity Framework 6
> * Créer le modèle de données
> * Créer le contexte de base de données
> * Initialiser la base de données avec des données de test
> * Configurer EF 6 pour utiliser la base de données locale
> * Créer un contrôleur et des vues
> * Afficher la base de données

## <a name="prerequisites"></a>Conditions préalables requises

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="create-an-mvc-web-app"></a>Créer une application Web MVC

1. Ouvrez Visual Studio et créez un C# projet Web à l’aide du modèle d' **application web ASP.net (.NET Framework)** . Nommez le projet *ContosoUniversity* et sélectionnez **OK**.

   ![Boîte de dialogue Nouveau projet dans Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

1. Dans **nouvelle application Web ASP.net-ContosoUniversity**, sélectionnez **MVC**.

   ![Boîte de dialogue nouvelle application Web dans Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

    > [!NOTE]
    > Par défaut, l’option **d’authentification** est définie sur **aucune authentification**. Pour ce didacticiel, l’application Web ne requiert pas que les utilisateurs se connectent. En outre, il ne limite pas l’accès en fonction de la personne qui est connectée.

1. Sélectionnez **OK** pour créer le projet.

## <a name="set-up-the-site-style"></a>Définir le style de site

Quelques changements simples configureront le menu, la disposition et la page d’accueil du site.

1. Ouvrez *Views\Shared\\_Layout. cshtml*et apportez les modifications suivantes :

   - Remplacez chaque occurrence de « mon application ASP.NET » et « nom de l’application » par « Contoso University ».
   - Ajoutez des entrées de menu pour les élèves, les cours, les enseignants et les services, puis supprimez l’entrée de contact.

   Les modifications sont mises en surbrillance dans l’extrait de code suivant :

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. Dans *Views\Home\Index.cshtml*, remplacez le contenu du fichier par le code suivant pour remplacer le texte sur ASP.net et MVC par le texte de cette application :

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Appuyez sur CTRL + F5 pour exécuter le site Web. La page d’accueil s’affiche avec le menu principal.

## <a name="install-entity-framework-6"></a>Installer Entity Framework 6

1. Dans le menu **Outils** , choisissez **Gestionnaire de package NuGet**, puis **console du gestionnaire de package**.

2. Dans la fenêtre **Console du Gestionnaire de package** , entrez la commande suivante :

   ```text
   Install-Package EntityFramework
   ```

Cette étape est l’une des étapes que ce didacticiel vous propose manuellement, mais qui aurait pu être effectuée automatiquement par la fonctionnalité de génération de modèles automatique ASP.NET MVC. Vous les exécutez manuellement pour voir les étapes nécessaires à l’utilisation d’Entity Framework (EF). Vous utiliserez la génération de modèles automatique ultérieurement pour créer le contrôleur MVC et les vues. Une alternative consiste à laisser la structure installer automatiquement le package NuGet EF, créer la classe de contexte de base de données et créer la chaîne de connexion. Lorsque vous êtes prêt à effectuer cette opération, il vous suffit d’ignorer ces étapes et de générer un modèle de structure pour votre contrôleur MVC après avoir créé vos classes d’entité.

## <a name="create-the-data-model"></a>Créer le modèle de données

Ensuite, vous allez créer des classes d’entités pour l’application Contoso University. Vous allez commencer par les trois entités suivantes :

**Cours** <-> **inscription** <-> **étudiant**

| Entités | Relationship |
| -------- | ------------ |
| Cours d’inscription | Un-à-plusieurs |
| Étudiants à inscrire | Un-à-plusieurs |

Il existe une relation un-à-plusieurs entre les entités `Student` et `Enrollment`, et une relation un-à-plusieurs entre les entités `Course` et `Enrollment`. En d’autres termes, un étudiant peut être inscrit dans un nombre quelconque de cours et un cours peut avoir un nombre quelconque d’élèves inscrits.

Dans les sections suivantes, vous allez créer une classe pour chacune de ces entités.

> [!NOTE]
> Si vous essayez de compiler le projet avant de terminer la création de toutes ces classes d’entité, vous obtiendrez des erreurs de compilation.

### <a name="the-student-entity"></a>Entité Student

- Dans le dossier *Models* , créez un fichier de classe nommé *Student.cs* en cliquant avec le bouton droit sur le dossier dans **Explorateur de solutions** et en choisissant **Ajouter** > **classe**. Remplacez le code du modèle par le code suivant :

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

La propriété `ID` devient la colonne de clé primaire de la table de base de données qui correspond à cette classe. Par défaut, Entity Framework interprète une propriété nommée `ID` ou *classname* `ID` comme clé primaire.

La propriété `Enrollments` est une *propriété de navigation*. Les propriétés de navigation contiennent d’autres entités qui sont associées à cette entité. Dans ce cas, la propriété `Enrollments` d’une entité `Student` contiendra toutes les entités `Enrollment` associées à cette `Student` entité. En d’autres termes, si une ligne de `Student` donnée dans la base de données a deux lignes `Enrollment` associées (lignes qui contiennent la valeur de clé primaire de cet étudiant dans leur colonne de clé étrangère `StudentID`), cette propriété de navigation `Enrollments` de l’entité `Student` contient ces deux entités `Enrollment`.

Les propriétés de navigation sont généralement définies comme `virtual` afin qu’elles puissent tirer parti de certaines fonctionnalités Entity Framework telles que le *chargement différé*. (Le chargement différé sera expliqué ultérieurement, dans le didacticiel sur la [lecture des données associées](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) plus loin dans cette série.)

Si une propriété de navigation peut contenir plusieurs entités (comme dans des relations plusieurs à plusieurs ou un -à-plusieurs), son type doit être une liste dans laquelle les entrées peuvent être ajoutées, supprimées et mises à jour, telle que `ICollection`.

### <a name="the-enrollment-entity"></a>L’entité Enrollment

- Dans le dossier *Models*, créez *Enrollment.cs* et remplacez le code existant par le code suivant :

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

La propriété `EnrollmentID` est la clé primaire ; cette entité utilise le modèle de `ID` *className* au lieu de `ID` comme vous l’avez vu dans l’entité `Student`. En général, vous choisissez un modèle et l’utilisez dans tout votre modèle de données. Ici, la variante illustre que vous pouvez utiliser l’un ou l’autre modèle. Dans un didacticiel ultérieur, vous verrez comment l’utilisation de `ID` sans `classname` facilite l’implémentation de l’héritage dans le modèle de données.

La propriété `Grade` est une [énumération](/ef/ef6/modeling/code-first/data-types/enums). La présence du point d’interrogation après la déclaration de type `Grade` indique que la propriété `Grade`[accepte les valeurs Null](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Un niveau null est différent d’un niveau zéro (null signifie qu’un niveau n’est pas connu ou n’a pas encore été affecté).

La propriété `StudentID` est une clé étrangère et la propriété de navigation correspondante est `Student`. Une entité `Enrollment` est associée à une entité `Student`, donc la propriété peut contenir uniquement une entité `Student` unique (contrairement à la propriété de navigation `Student.Enrollments` que vous avez vue précédemment, qui peut contenir plusieurs entités `Enrollment`).

La propriété `CourseID` est une clé étrangère et la propriété de navigation correspondante est `Course`. Une entité `Enrollment` est associée à une entité `Course`.

Entity Framework interprète une propriété comme une propriété de clé étrangère si elle est nommée *&lt;nom de la propriété de navigation&gt;&lt;nom de la propriété de clé primaire&gt;* (par exemple, `StudentID` pour la propriété de navigation `Student`, car la clé primaire de l’entité `Student` est `ID`). Les propriétés de clé étrangère peuvent également être nommées de la même façon *&lt;nom de propriété de clé primaire&gt;* (par exemple, `CourseID` depuis la `CourseID`de la clé primaire de l’entité `Course`).

### <a name="the-course-entity"></a>Entité Course

- Dans le dossier *Models* , créez *course.cs*, en remplaçant le code du modèle par le code suivant :

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La propriété `Enrollments` est une propriété de navigation. Une entité `Course` peut être associée à un nombre quelconque d’entités `Enrollment`.

Vous en apprendrez davantage sur l’attribut <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute> dans un didacticiel ultérieur de cette série. En fait, cet attribut vous permet d’entrer la clé primaire pour le cours plutôt que de laisser la base de données la générer.

## <a name="create-the-database-context"></a>Créer le contexte de base de données

La classe principale qui coordonne Entity Framework fonctionnalité pour un modèle de données donné est la classe *de contexte de base de données* . Vous créez cette classe en dérivant de la classe [System. Data. Entity. DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx) . Dans votre code, vous spécifiez les entités qui sont incluses dans le modèle de données. Vous pouvez également personnaliser un certain comportement d’Entity Framework. Dans ce projet, la classe se nomme `SchoolContext`.

- Pour créer un dossier dans le projet ContosoUniversity, cliquez avec le bouton droit sur le projet dans **Explorateur de solutions** , cliquez sur **Ajouter**, puis sur **nouveau dossier**. Nommez le nouveau dossier *dal* (pour la couche d’accès aux données). Dans ce dossier, créez un fichier de classe nommé *SchoolContext.cs*et remplacez le code du modèle par le code suivant :

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>Spécifier les jeux d’entités

Ce code crée une propriété [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx) pour chaque jeu d’entités. Dans Entity Framework terminologie, un *jeu d’entités* correspond généralement à une table de base de données, et une *entité* correspond à une ligne de la table.

> [!NOTE]
>
> Vous pouvez omettre les instructions `DbSet<Enrollment>` et `DbSet<Course>` et cela fonctionnerait de la même façon. Entity Framework les inclurait implicitement parce que l’entité `Student` référence l’entité `Enrollment` et que l’entité `Enrollment` référence l’entité `Course`.

### <a name="specify-the-connection-string"></a>Spécifier la chaîne de connexion

Le nom de la chaîne de connexion (que vous ajouterez ultérieurement au fichier Web. config) est passé au constructeur.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Vous pouvez également transmettre la chaîne de connexion elle-même au lieu du nom d’une chaîne stockée dans le fichier Web. config. Pour plus d’informations sur les options permettant de spécifier la base de données à utiliser, consultez [chaînes de connexion et modèles](/ef/ef6/fundamentals/configuring/connection-strings).

Si vous ne spécifiez pas de chaîne de connexion ou le nom d’un explicitement, Entity Framework suppose que le nom de la chaîne de connexion est le même que le nom de la classe. Dans cet exemple, le nom de la chaîne de connexion par défaut serait `SchoolContext`, ce qui correspond à ce que vous spécifiez explicitement.

### <a name="specify-singular-table-names"></a>Spécifier des noms de tables singulières

L’instruction `modelBuilder.Conventions.Remove` dans la méthode [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) empêche la pluralisation des noms de table. Si vous ne l’avez pas fait, les tables générées dans la base de données seraient nommées `Students`, `Courses`et `Enrollments`. Au lieu de cela, les noms de table sont `Student`, `Course`et `Enrollment`. Les développeurs ne sont pas tous d’accord sur la nécessité d’utiliser des noms de table au pluriel. Ce didacticiel utilise la forme singulière, mais le point important est que vous pouvez sélectionner le formulaire que vous préférez en incluant ou en omettant cette ligne de code.

## <a name="initialize-db-with-test-data"></a>Initialiser la base de données avec des données de test

Entity Framework pouvez créer automatiquement (ou supprimer et recréer) une base de données lors de l’exécution de l’application. Vous pouvez spécifier que cette opération doit être effectuée chaque fois que votre application s’exécute ou uniquement lorsque le modèle n’est pas synchronisé avec la base de données existante. Vous pouvez également écrire une méthode `Seed` qui Entity Framework appelle automatiquement après la création de la base de données afin de la remplir avec des données de test.

Le comportement par défaut consiste à créer une base de données uniquement si elle n’existe pas (et à lever une exception si le modèle a changé et que la base de données existe déjà). Dans cette section, vous allez spécifier que la base de données doit être supprimée et recréée à chaque modification du modèle. La suppression de la base de données entraîne la perte de toutes vos données. C’est généralement le cas lors du développement, car la méthode `Seed` s’exécute lorsque la base de données est recréée et recrée vos données de test. En production, vous ne souhaitez généralement pas perdre toutes vos données chaque fois que vous devez modifier le schéma de base de données. Plus tard, vous verrez comment gérer les modifications de modèle à l’aide de Migrations Code First pour modifier le schéma de base de données au lieu de supprimer et de recréer la base de données.

1. Dans le dossier DAL, créez un nouveau fichier de classe nommé *SchoolInitializer.cs* et remplacez le code du modèle par le code suivant, ce qui entraîne la création d’une base de données lorsque cela est nécessaire et charge les données de test dans la nouvelle base de données.

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   La méthode `Seed` prend l’objet de contexte de base de données en tant que paramètre d’entrée, et le code de la méthode utilise cet objet pour ajouter de nouvelles entités à la base de données. Pour chaque type d’entité, le code crée une collection de nouvelles entités, les ajoute à la propriété `DbSet` appropriée, puis enregistre les modifications dans la base de données. Il n’est pas nécessaire d’appeler la méthode `SaveChanges` après chaque groupe d’entités, comme cela est fait ici, mais cela vous permet de localiser la source d’un problème si une exception se produit pendant que le code écrit dans la base de données.

2. Pour indiquer à Entity Framework d’utiliser votre classe d’initialiseur, ajoutez un élément à l’élément `entityFramework` dans le fichier *Web. config* de l’application (celui qui se trouve dans le dossier racine du projet), comme indiqué dans l’exemple suivant :

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   L' `context type` spécifie le nom complet de la classe de contexte et l’assembly dans lequel elle se trouve, et le `databaseinitializer type` spécifie le nom qualifié complet de la classe d’initialiseur et l’assembly dans lequel elle se trouve. (Lorsque vous ne voulez pas que EF utilise l’initialiseur, vous pouvez définir un attribut sur l’élément `context` : `disableDatabaseInitialization="true"`.) Pour plus d’informations, consultez [paramètres du fichier de configuration](/ef/ef6/fundamentals/configuring/config-file).

   Une alternative à la définition de l’initialiseur dans le fichier *Web. config* consiste à le faire dans le code en ajoutant une instruction `Database.SetInitializer` à la méthode `Application_Start` dans le fichier *global.asax.cs* . Pour plus d’informations, consultez [fonctionnement des initialiseurs de base de données dans Entity Framework code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

L’application est maintenant configurée de telle sorte que lorsque vous accédez à la base de données pour la première fois dans une exécution donnée de l’application, Entity Framework compare la base de données au modèle (vos classes `SchoolContext` et d’entité). En cas de différence, l’application supprime et recrée la base de données.

> [!NOTE]
> Lorsque vous déployez une application sur un serveur Web de production, vous devez supprimer ou désactiver le code qui supprime et recrée la base de données. Vous le ferez dans un didacticiel ultérieur de cette série.

## <a name="set-up-ef-6-to-use-localdb"></a>Configurer EF 6 pour utiliser la base de données locale

La base de données [locale est une](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) version allégée du moteur de base de données SQL Server Express. Il est facile à installer et à configurer, démarre à la demande et s’exécute en mode utilisateur. La base de données locale s’exécute dans un mode d’exécution spécial de SQL Server Express qui vous permet d’utiliser des bases de données en tant que fichiers *. mdf* . Vous pouvez placer des fichiers de base de données de base de données locale dans le dossier *application\_Data* d’un projet Web si vous souhaitez pouvoir copier la base de données avec le projet. La fonctionnalité d’instance utilisateur de SQL Server Express vous permet également d’utiliser des fichiers *. mdf* , mais la fonctionnalité d’instance utilisateur est déconseillée. par conséquent, la base de données locale est recommandée pour travailler avec des fichiers *. mdf* . La base de données locale est installée par défaut dans Visual Studio.

En règle générale, SQL Server Express n’est pas utilisé pour les applications Web de production. En particulier, la base de données locale n’est pas recommandée pour une utilisation en production avec une application Web, car elle n’est pas conçue pour fonctionner avec IIS.

- Dans ce didacticiel, vous allez utiliser la base de données locale. Ouvrez le fichier *Web. config* de l’application et ajoutez un `connectionStrings` élément qui précède l’élément `appSettings`, comme indiqué dans l’exemple suivant. (Veillez à mettre à jour le fichier *Web. config* dans le dossier racine du projet. Il y a également un fichier *Web. config* dans le sous-dossier *views* que vous n’avez pas besoin de mettre à jour.)

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

La chaîne de connexion que vous avez ajoutée spécifie que Entity Framework utilise une base de données de base de données locale nommée *ContosoUniversity1. mdf*. (La base de données n’existe pas encore, mais EF la crée.) Si vous souhaitez créer la base de données dans votre *application\_dossier Data* , vous pouvez ajouter `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` à la chaîne de connexion. Pour plus d’informations sur les chaînes de connexion, consultez [SQL Server des chaînes de connexion pour les applications Web ASP.net](/previous-versions/aspnet/jj653752(v=vs.110)).

Vous n’avez pas besoin d’une chaîne de connexion dans le fichier *Web. config* . Si vous ne fournissez pas de chaîne de connexion, Entity Framework utilise une chaîne de connexion par défaut basée sur votre classe de contexte. Pour plus d’informations, consultez [Code First à une nouvelle base de données](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-controller-and-views"></a>Créer un contrôleur et des vues

Vous allez maintenant créer une page Web pour afficher les données. Le processus de demande des données déclenche automatiquement la création de la base de données. Vous commencerez par créer un nouveau contrôleur. Mais avant cela, générez le projet pour rendre les classes de modèle et de contexte disponibles pour la génération de modèles automatique de contrôleur MVC.

1. Cliquez avec le bouton droit sur le dossier **Controllers** dans **Explorateur de solutions**, sélectionnez **Ajouter**, puis cliquez sur **nouvel élément**généré automatiquement.
2. Dans la boîte de dialogue **Ajouter une structure** , sélectionnez **contrôleur MVC 5 avec affichages, à l’aide de Entity Framework**, puis choisissez **Ajouter**.

     ![Boîte de dialogue Ajouter une structure dans Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. Dans la boîte de dialogue **Ajouter un contrôleur** , effectuez les sélections suivantes, puis choisissez **Ajouter**:

   - Classe de modèle : **Student (ContosoUniversity. Models)** . (Si vous ne voyez pas cette option dans la liste déroulante, générez le projet et réessayez.)
   - Classe de contexte de données : **SchoolContext (ContosoUniversity. dal)** .
   - Nom du contrôleur : **StudentController** (et non StudentsController).
   - Laissez les valeurs par défaut pour les autres champs.

     Lorsque vous cliquez sur **Ajouter**, le générateur de modèles crée un fichier *StudentController.cs* et un ensemble de vues (fichiers *. cshtml* ) qui fonctionnent avec le contrôleur. À l’avenir, lorsque vous créerez des projets qui utilisent Entity Framework, vous pouvez également tirer parti de certaines fonctionnalités supplémentaires de l’échafaudage : créer votre première classe de modèle, ne pas créer de chaîne de connexion, puis, dans la zone **Ajouter un contrôleur** , spécifier un **nouveau contexte de données** en sélectionnant le bouton **+** en regard de classe de **contexte de données**. L’échafaudage crée votre classe `DbContext` et votre chaîne de connexion, ainsi que le contrôleur et les vues.
4. Visual Studio ouvre le fichier *Controllers\StudentController.cs* . Vous voyez qu’une variable de classe a été créée pour instancier un objet de contexte de base de données :

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     La méthode d’action `Index` obtient une liste des étudiants du jeu d’entités *students* en lisant la propriété `Students` de l’instance de contexte de base de données :

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     La vue *Student\Index.cshtml* affiche cette liste dans une table :

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Appuyez sur Ctrl+F5 pour exécuter le projet. (Si vous recevez l’erreur « Impossible de créer un cliché instantané », fermez le navigateur, puis réessayez.)

     Cliquez sur l’onglet **students** pour afficher les données de test insérées par la méthode `Seed`. En fonction de la précision de la fenêtre de votre navigateur, vous verrez le lien de l’onglet Student dans la barre d’adresse principale ou vous devrez cliquer sur le coin supérieur droit pour afficher le lien.

     ![Bouton Menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

## <a name="view-the-database"></a>Afficher la base de données

Lorsque vous avez exécuté la page Students et que l’application a tenté d’accéder à la base de données, EF a découvert qu’il n’existait aucune base de données et n’en a créé une. EF a ensuite exécuté la méthode Seed pour remplir la base de données avec des données.

Vous pouvez utiliser **Explorateur de serveurs** ou **Explorateur d’objets SQL Server** (SSOX) pour afficher la base de données dans Visual Studio. Pour ce didacticiel, vous allez utiliser **Explorateur de serveurs**.

1. Fermez le navigateur.
2. Dans **Explorateur de serveurs**, développez **connexions de données** (vous devrez peut-être sélectionner d’abord le bouton Actualiser), développer le **contexte School (ContosoUniversity)** , puis développer les **tables** pour afficher les tables dans votre nouvelle base de données.

3. Cliquez avec le bouton droit sur la table **Student** et cliquez sur **afficher les données** de la table pour afficher les colonnes qui ont été créées et les lignes qui ont été insérées dans la table.

4. Fermez la connexion **Explorateur de serveurs** .

Les fichiers de base de données *ContosoUniversity1. mdf* et *. ldf* se trouvent dans le dossier *% UserProfile%* .

Étant donné que vous utilisez l’initialiseur `DropCreateDatabaseIfModelChanges`, vous pouvez maintenant apporter une modification à la classe `Student`, réexécuter l’application et la base de données sera automatiquement recréée pour correspondre à votre modification. Par exemple, si vous ajoutez une propriété `EmailAddress` à la classe `Student`, réexécutez la page Students, puis examinez à nouveau la table, vous verrez une nouvelle colonne `EmailAddress`.

## <a name="conventions"></a>Conventions

La quantité de code que vous devez écrire pour Entity Framework être en mesure de créer une base de données complète pour vous est minime en raison de *conventions*ou d’hypothèses que Entity Framework effectue. Certains d’entre eux ont déjà été notés ou ont été utilisés sans que vous en soyez conscient :

- Les formes plusieurs des noms de classes d’entité sont utilisées comme noms de tables.
- Les noms des propriétés d’entités sont utilisés comme noms de colonnes.
- Les propriétés d’entité nommées `ID` ou *classname* `ID` sont reconnues en tant que propriétés de clé primaire.
- Une propriété est interprétée comme une propriété de clé étrangère si elle est nommée *&lt;nom de la propriété de navigation&gt;&lt;nom de la propriété de clé primaire&gt;* (par exemple, `StudentID` pour la propriété de navigation `Student`, car la clé primaire de l’entité `Student` est `ID`). Les propriétés de clé étrangère peuvent également être nommées de la même façon &lt;nom de propriété de clé primaire&gt; (par exemple, `EnrollmentID` depuis la `EnrollmentID`de la clé primaire de l’entité `Enrollment`).

Vous avez vu que les conventions peuvent être remplacées. Par exemple, vous avez spécifié que les noms de tables ne doivent pas être plurielés, et vous verrez plus tard comment marquer explicitement une propriété en tant que propriété de clé étrangère.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Pour plus d’informations sur EF 6, consultez les articles suivants :

* [Accès aux données ASP.NET - Ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md)

* [Conventions de Code First](/ef/ef6/modeling/code-first/conventions/built-in)

* [Création d’un modèle de données plus complexe](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Création d’une application Web MVC
> * Définir le style de site
> * Installé le Entity Framework 6
> * Créer le modèle de données
> * Créer le contexte de base de données
> * Initialiser la base de données avec des données de test
> * Configurer EF 6 pour utiliser la base de données locale
> * Créer un contrôleur et des vues
> * Afficher la base de données

Passez à l’article suivant pour découvrir comment examiner et personnaliser le code de création, lecture, mise à jour, suppression (CRUD) dans vos contrôleurs et vues.
> [!div class="nextstepaction"]
> [Implémenter la fonctionnalité CRUD de base](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)