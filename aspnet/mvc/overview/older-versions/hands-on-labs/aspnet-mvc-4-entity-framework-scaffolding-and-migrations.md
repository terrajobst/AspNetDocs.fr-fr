---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework la génération de modèles automatique et les migrations | Microsoft Docs
author: rick-anderson
description: Si vous êtes familiarisé avec les méthodes du contrôleur ASP.NET MVC 4, ou si vous avez terminé les &quot;helpers, les formulaires et la validation&quot; laboratoire pratique, vous devez être conscient...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 2b26224390af70e19ca0593abe93a6867140f8ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598902"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Génération de modèles automatique et migrations d’ASP.NET MVC 4 Entity Framework

par l' [équipe Web camps](https://twitter.com/webcamps)

[Télécharger le kit de formation Web camps](https://aka.ms/webcamps-training-kit)

Si vous êtes familiarisé avec les méthodes du contrôleur ASP.NET MVC 4 ou si vous avez terminé les &quot;helpers, les formulaires et la validation&quot; laboratoire pratique, vous devez savoir que la logique de création, de mise à jour, de liste et de suppression d’une entité de données est répétée dans l’application. Pour ne pas mentionner que, si votre modèle a plusieurs classes à manipuler, vous devrez probablement consacrer beaucoup de temps à écrire les méthodes de publication et d’action pour chaque opération d’entité, ainsi que chacune des vues.

Dans ce laboratoire, vous allez apprendre à utiliser la génération de modèles automatique ASP.NET MVC 4 pour générer automatiquement la ligne de base du CRUD de votre application (créer, lire, mettre à jour et supprimer). À partir d’une classe de modèle simple et, sans écrire une seule ligne de code, vous allez créer un contrôleur qui contiendra toutes les opérations CRUD, ainsi que toutes les vues nécessaires. Après avoir généré et exécuté la solution simple, la base de données d’application est générée, ainsi que la logique MVC et les vues pour la manipulation des données.

En outre, vous allez apprendre combien il est facile d’utiliser Entity Framework migrations pour effectuer des mises à jour de modèle dans toute votre application. Entity Framework migrations vous permet de modifier votre base de données une fois que le modèle a été modifié avec des étapes simples. Avec tout cela à l’esprit, vous serez en mesure de créer et de gérer des applications Web plus efficacement, en tirant parti des fonctionnalités les plus récentes de ASP.NET MVC 4.

> [!NOTE]
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet propre à ce laboratoire est disponible dans [ASP.NET MVC 4 Entity Framework la génération de modèles automatique et les migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Utilisez la génération de modèles automatique ASP.NET pour les opérations CRUD dans les contrôleurs.
- Modifiez le modèle de base de données à l’aide de Entity Framework migrations.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

Pour effectuer ce laboratoire, vous devez disposer des éléments suivants :

- [Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe A](#AppendixA) pour obtenir des instructions sur son installation).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Installation

**Installation d’extraits de code**

Pour plus de commodité, la majeure partie du code que vous allez gérer dans le cadre de ce laboratoire est disponible sous forme d’extraits de code Visual Studio. Pour installer les extraits de code, exécutez le fichier **.\Source\Setup\CodeSnippets.vsi** .

Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;l' [annexe B : utilisation d’extraits de Code](#AppendixB)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

L’exercice suivant constitue ce laboratoire pratique :

1. [Utilisation de la génération de modèles automatique ASP.NET MVC 4 avec Entity Framework migrations](#Exercise1)

> [!NOTE]
> Cet exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir à l’issue de l’exercice. Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire dans le cadre de l’exercice.

Durée estimée pour effectuer ce laboratoire : **30 minutes**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Exercice 1 : utilisation de la génération de modèles automatique ASP.NET MVC 4 avec Entity Framework migrations

La génération de modèles automatique ASP.NET MVC offre un moyen rapide de générer les opérations CRUD de manière standardisée, créant ainsi la logique nécessaire qui permet à votre application d’interagir avec la couche de base de données.

Dans cet exercice, vous allez apprendre à utiliser la génération de modèles automatique ASP.NET MVC 4 avec code First pour créer les méthodes CRUD. Vous allez ensuite apprendre à mettre à jour votre modèle en appliquant les modifications apportées à la base de données à l’aide de Entity Framework migrations.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Tâche 1 : création d’un projet ASP.NET MVC 4 à l’aide de la génération de modèles automatique

1. S’il n’est pas déjà ouvert, démarrez **Visual Studio 2012**.
2. Sélectionner un **fichier | Nouveau projet**. Dans la boîte de dialogue Nouveau projet, sous le **visuel C# | Section Web** , sélectionnez **Application Web ASP.NET MVC 4**. Nommez le projet en **MVC4andEFMigrations** et définissez l’emplacement sur le dossier **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** de ce Lab. Définissez le **nom** de la solution sur **Démarrer** et assurez-vous que l’option **créer le répertoire pour la solution** est cochée. Cliquez sur **OK**.

    ![Boîte de dialogue Nouveau projet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "Boîte de dialogue Nouveau projet ASP.NET MVC 4")

    *Boîte de dialogue Nouveau projet ASP.NET MVC 4*
3. Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez le modèle **application Internet** et assurez-vous que **Razor** est le **moteur d’affichage**sélectionné. Cliquez sur **OK** pour créer le projet.

    ![Nouvelle application Internet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "Nouvelle application Internet ASP.NET MVC 4")

    *Nouvelle application Internet ASP.NET MVC 4*
4. Dans le Explorateur de solutions, cliquez avec le bouton droit sur **modèles** , puis sélectionnez **Ajouter | Classe** pour créer un utilisateur de classe simple (POCO). Nommez-le **Person** et cliquez sur **OK**.
5. Ouvrez la classe Person et insérez les propriétés suivantes.

    (Extrait de code- *ASP.NET MVC 4 et migrations de Entity Framework-propriétés de la personne EX1*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Cliquez sur **générer | Générez la solution** pour enregistrer les modifications et générer le projet.

    ![Génération de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Génération de l’application")

    *Génération de l’application*
7. Dans le Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers et sélectionnez **Ajouter | Contrôleur**.
8. Nommez le contrôleur *PersonController* et complétez les **options de génération de modèles** automatique avec les valeurs suivantes.

   1. Dans la liste déroulante **modèle** , sélectionnez le **contrôleur MVC avec des actions et des affichages en lecture/écriture, à l’aide de l’option Entity Framework** .
   2. Dans la liste déroulante **classe de modèle** , sélectionnez la classe **Person** .
   3. Dans la liste **classe du contexte de données** , sélectionnez **&lt;nouveau contexte de données...&gt;** . Choisissez un nom et cliquez sur **OK**.
   4. Dans la liste déroulante **affichages** , assurez-vous que l’option **Razor** est sélectionnée.

      ![Ajout du contrôleur person avec la génération de modèles automatique](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Ajout du contrôleur person avec la génération de modèles automatique")

      *Ajout du contrôleur person avec la génération de modèles automatique*
9. Cliquez sur **Ajouter** pour créer le contrôleur pour la personne avec génération de modèles automatique. Vous avez maintenant généré les actions du contrôleur ainsi que les vues.

    ![Après avoir créé le contrôleur person avec la génération de modèles automatique](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Après avoir créé le contrôleur person avec la génération de modèles automatique")

    *Après avoir créé le contrôleur person avec la génération de modèles automatique*
10. Ouvrez la classe **PersonController** . Notez que les méthodes d’action CRUD complètes ont été générées automatiquement.

   ![À l’intérieur du contrôleur person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "À l’intérieur du contrôleur person")

   *À l’intérieur du contrôleur person*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Tâche 2 : exécution de l’application

À ce stade, la base de données n’est pas encore créée. Dans cette tâche, vous allez exécuter l’application pour la première fois et tester les opérations CRUD. La base de données sera créée à la volée avec Code First.

1. Appuyez sur **F5** pour exécuter l’application.
2. Dans le navigateur, ajoutez **/Person** à l’URL pour ouvrir la page Person.

    ![Première exécution de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Première exécution de l’application")

    *Application : première exécution*
3. Vous allez maintenant explorer les pages Person et tester les opérations CRUD.

    1. Cliquez sur **créer** pour ajouter une nouvelle personne. Entrez un prénom et un nom, puis cliquez sur **créer**.

        ![Ajout d’une nouvelle personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Ajout d’une nouvelle personne")

        *Ajout d’une nouvelle personne*
    2. Dans la liste des personnes, vous pouvez supprimer, modifier ou ajouter des éléments.

        ![Liste des personnes](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "Liste des personnes")

        *Liste des personnes*
    3. Cliquez sur **Détails** pour ouvrir les détails de la personne.

        ![Détails de la personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Détails de la personne")

        *Détails de la personne*
4. Fermez le navigateur et revenez à Visual Studio. Notez que vous avez créé l’ensemble de la CRUD pour l’entité Person dans votre application, du modèle aux vues, sans avoir à écrire une seule ligne de code !

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Tâche 3 : mise à jour de la base de données à l’aide de Entity Framework migrations

Au cours de cette tâche, vous allez mettre à jour la base de données à l’aide de Entity Framework migrations. Vous allez découvrir combien il est facile de modifier le modèle et de refléter les modifications apportées à vos bases de données à l’aide de la fonctionnalité de migration Entity Framework.

1. Ouvrez la console du gestionnaire de package. Cliquez sur **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.
2. Dans la Console du Gestionnaire de Package, entrez la commande suivante :

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Activation des migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Activation des migrations")

    *Activation des migrations*

    La commande Enable-migration crée le dossier **migrations** , qui contient un script pour initialiser la base de données.

    ![Dossier migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Dossier migrations")

    *Dossier migrations*
3. Ouvrez le fichier **Configuration.cs** dans le dossier migrations. Recherchez le constructeur de classe et remplacez la valeur **AutomaticMigrationsEnabled** par *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Ouvrez la classe Person et ajoutez un attribut pour le deuxième prénom de la personne. Avec ce nouvel attribut, vous modifiez le modèle.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Sélectionner une **Build | Générez la solution** dans le menu pour générer l’application.

    ![Génération de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Génération de l’application")

    *Génération de l’application*
6. Dans la Console du Gestionnaire de Package, entrez la commande suivante :

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Cette commande recherche les modifications dans les objets de données, puis ajoute les commandes nécessaires pour modifier la base de données en conséquence.

    ![Ajout d’un deuxième prénom](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Ajout d’un deuxième prénom")

    *Ajout d’un deuxième prénom*
7. Facultatif Vous pouvez exécuter la commande suivante pour générer un script SQL avec la mise à jour différentielle. Cela vous permet de mettre à jour la base de données manuellement (dans ce cas, il n’est pas nécessaire) ou d’appliquer les modifications dans d’autres bases de données :

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Génération d’un script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Génération d’un script SQL")

    *Génération d’un script SQL*

    ![Mise à jour des scripts SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Mise à jour des scripts SQL")

    *Mise à jour des scripts SQL*
8. Dans la console du gestionnaire de package, entrez la commande suivante pour mettre à jour la base de données :

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Mise à jour de la base de données](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Mise à jour de la base de données")

    *Mise à jour de la base de données*

    Cette opération ajoute la colonne **MiddleName** dans la table **People** pour correspondre à la définition actuelle de la classe **Person** .
9. Une fois la base de données mise à jour, cliquez avec le bouton droit sur le dossier du contrôleur et sélectionnez **Ajouter | Pour rajouter le contrôleur de** personne (avec les mêmes valeurs). Cela permet de mettre à jour les méthodes et les vues existantes qui ajoutent le nouvel attribut.

    ![Ajout d’une mise à jour du contrôleur](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Ajout d’une mise à jour du contrôleur")

    *Mise à jour du contrôleur*
10. Cliquez sur **Ajouter**. Ensuite, sélectionnez les valeurs **overwrite PersonController.cs** et remplacer les **affichages associés** , puis cliquez sur **OK**.

   ![Ajout d’un remplacement de contrôleur](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Mise à jour du contrôleur*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4-exécution de l’application

1. Appuyez sur **F5** pour exécuter l’application.
2. Ouvrez **/Person**. Notez que les données ont été conservées, tandis que la colonne de deuxième prénom a été ajoutée.

    ![Deuxième prénom ajouté](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Deuxième prénom ajouté")

    *Deuxième prénom ajouté*
3. Si vous cliquez sur **modifier**, vous pouvez ajouter un deuxième prénom à la personne en cours.

    ![Édition du deuxième prénom](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Édition du deuxième prénom")

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

Dans ce laboratoire pratique, vous avez appris des étapes simples pour créer des opérations CRUD avec la génération de modèles automatique ASP.NET MVC 4 à l’aide de n’importe quelle classe de modèle. Ensuite, vous avez appris à effectuer une mise à jour de bout en bout dans votre application, de la base de données aux vues, à l’aide de Entity Framework migrations.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe A : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.
2. Cliquez sur **Installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.
3. Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.

    ![Installer Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.

    ![Acceptation des termes du contrat de licence](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Acceptation des termes du contrat de licence*
5. Attendez que le processus de téléchargement et d’installation se termine.

    ![Progression de l'installation](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation terminée](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Installation terminée*
7. Cliquez sur **quitter** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .

    ![Vignette VS Express pour le Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *Vignette VS Express pour le Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Annexe B : utilisation d’extraits de code

Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main. Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.

![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")

*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***

1. Placez le curseur à l’endroit où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).
3. Regarder comme IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).
5. Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*

![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")

*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*

***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0. Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.

1. Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.
2. Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.

![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")

*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*

![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")

*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*
