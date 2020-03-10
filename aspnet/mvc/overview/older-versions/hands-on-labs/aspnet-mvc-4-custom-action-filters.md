---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtres d’action personnalisée ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC fournit des filtres d’action pour l’exécution de la logique de filtrage avant ou après l’appel d’une méthode d’action. Les filtres d’action sont des attributs personnalisés...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: eaeb32180f79fabf557cbc38ff067eb26b47fea7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579694"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Filtres d’actions personnalisés d’ASP.NET MVC 4

par l' [équipe Web camps](https://twitter.com/webcamps)

[Télécharger le kit de formation Web camps](https://aka.ms/webcamps-training-kit)

ASP.NET MVC fournit des filtres d’action pour l’exécution de la logique de filtrage avant ou après l’appel d’une méthode d’action. Les filtres d’action sont des attributs personnalisés qui fournissent des moyens déclaratifs pour ajouter un comportement de pré-action et après action aux méthodes d’action du contrôleur.

Dans ce laboratoire pratique, vous allez créer un attribut de filtre d’action personnalisé dans la solution MvcMusicStore pour intercepter les demandes du contrôleur et enregistrer l’activité d’un site dans une table de base de données. Vous pourrez ajouter votre filtre de journalisation par injection à n’importe quel contrôleur ou action. Enfin, vous verrez la vue de journal qui affiche la liste des visiteurs.

Ce laboratoire pratique suppose que vous avez une connaissance de base de **ASP.NET MVC**. Si vous n’avez pas encore utilisé **ASP.NET MVC** , nous vous recommandons de passer au-dessus d’un laboratoire pratique de **notions de base sur ASP.NET MVC 4** .

> [!NOTE]
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet propre à ce laboratoire est disponible dans les [filtres d’action personnalisés ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Créer un attribut de filtre d’action personnalisé pour étendre les fonctionnalités de filtrage
- Appliquer un attribut de filtre personnalisé par injection à un niveau spécifique
- Inscrire globalement des filtres d’action personnalisés

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

Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;l' [annexe C : utilisation d’extraits de Code](#AppendixC)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique est constitué des exercices suivants :

1. [Exercice 1 : journalisation des actions](#Exercise1)
2. [Exercice 2 : gestion de plusieurs filtres d’action](#Exercise2)

Durée estimée pour effectuer ce laboratoire : **30 minutes**.

> [!NOTE]
> Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices. Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.

<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Exercice 1 : journalisation des actions

Dans cet exercice, vous allez apprendre à créer un filtre de journal des actions personnalisées à l’aide de fournisseurs de filtres ASP.NET MVC 4. À cet effet, vous allez appliquer un filtre de journalisation au site MusicStore qui enregistre toutes les activités dans les contrôleurs sélectionnés.

Le filtre étend **ActionFilterAttributeClass** et remplace la méthode **OnActionExecuting** pour intercepter chaque demande, puis effectuer les actions de journalisation. Les informations de contexte relatives aux requêtes HTTP, à l’exécution des méthodes, aux résultats et aux paramètres seront fournies par la classe **ACTIONEXECUTINGCONTEXT** MVC ASP.NET **.**

> [!NOTE]
> ASP.NET MVC 4 possède également des fournisseurs de filtres par défaut que vous pouvez utiliser sans créer de filtre personnalisé. ASP.NET MVC 4 fournit les types de filtres suivants :
> 
> - Filtre **d’autorisation** , qui prend des décisions en matière de sécurité quant à l’exécution d’une méthode d’action, telle que l’exécution de l’authentification ou la validation des propriétés de la demande.
> - Filtre d' **action** , qui encapsule l’exécution de la méthode d’action. Ce filtre peut effectuer un traitement supplémentaire, tel que la fourniture de données supplémentaires à la méthode d’action, l’inspection de la valeur de retour ou l’annulation de l’exécution de la méthode d’action.
> - Filtre de **résultat** , qui encapsule l’exécution de l’objet ActionResult. Ce filtre peut effectuer un traitement supplémentaire du résultat, tel que la modification de la réponse HTTP.
> - Filtre d' **exception** , qui s’exécute si une exception non gérée est levée quelque part dans la méthode d’action, en commençant par les filtres d’autorisation et en terminant par l’exécution du résultat. Les filtres d'exception peuvent être utilisés pour des tâches telles que la journalisation ou l'affichage d'une page d'erreurs.
> 
> Pour plus d’informations sur les fournisseurs de filtres, visitez ce lien MSDN : ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).

<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>À propos de la fonctionnalité de journalisation des applications du Store musical MVC

Cette solution du store de musique contient une nouvelle table de modèle de données pour la journalisation du site, **ActionLog**, avec les champs suivants : nom du contrôleur qui a reçu une demande, appelée action, adresse IP du client et horodatage.

![Modèle de données. Table ActionLog.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modèle de données. Table ActionLog.")

*Modèle de données-table ActionLog*

La solution fournit une vue MVC ASP.NET pour le journal des actions, accessible à **MvcMusicStores/views/ActionLog**:

![Vue du journal des actions](aspnet-mvc-4-custom-action-filters/_static/image2.png "Vue du journal des actions")

*Vue du journal des actions*

Avec cette structure donnée, tout le travail est axé sur l’interruption de la demande du contrôleur et sur l’exécution de la journalisation à l’aide d’un filtrage personnalisé.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Tâche 1 : création d’un filtre personnalisé pour intercepter la demande d’un contrôleur

Au cours de cette tâche, vous allez créer une classe d’attributs de filtre personnalisée qui contiendra la logique de journalisation. Dans ce but, vous étendez la classe **ACTIONFILTERATTRIBUTE** MVC ASP.net et implémenterez l’interface **IActionFilter**.

> [!NOTE]
> **ActionFilterAttribute** est la classe de base pour tous les filtres d’attribut. Il fournit les méthodes suivantes pour exécuter une logique spécifique après et avant l’exécution de l’action du contrôleur :
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext) : juste avant l’appel de la méthode d’action.
> - **OnActionExecuted**(ActionExecutedContext filterContext) : après l’appel de la méthode d’action et avant l’exécution du résultat (avant le rendu de la vue).
> - **OnResultExecuting**(ResultExecutingContext filterContext) : juste avant l’exécution du résultat (avant le rendu de la vue).
> - **OnResultExecuted**(ResultExecutedContext filterContext) : après l’exécution du résultat (après le rendu de la vue).
> 
> En remplaçant chacune de ces méthodes dans une classe dérivée, vous pouvez exécuter votre propre code de filtrage.

1. Ouvrez le dossier **Begin** solution situé dans le dossier **\Source\Ex01-LoggingActions\Begin** .

   1. Vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
      > 
      > Pour plus d’informations, consultez cet article : [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Ajoutez une nouvelle C# classe dans le dossier **Filters** et nommez-la *CustomActionFilter.cs*. Ce dossier stocke tous les filtres personnalisés.
3. Ouvrez **CustomActionFilter.cs** et ajoutez une référence aux espaces de noms **System. Web. Mvc** et **MvcMusicStore. Models** :

    (Extrait de code- *ASP.NET MVC 4 filtres d’action personnalisés-EX1-CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Héritez de la classe **CustomActionFilter** de **ActionFilterAttribute** , puis faites en sorte que la classe **CustomActionFilter** implémente l’interface **IActionFilter** .

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Faites en sorte que la classe **CustomActionFilter** remplace la méthode **OnActionExecuting** et ajoute la logique nécessaire pour enregistrer l’exécution du filtre. Pour ce faire, ajoutez le code en surbrillance suivant dans la classe **CustomActionFilter** .

    (Extrait de code- *ASP.NET MVC 4 filtres d’action personnalisés-EX1-LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > La méthode **OnActionExecuting** utilise **Entity Framework** pour ajouter un nouveau registre ActionLog. Il crée et remplit une nouvelle instance d’entité avec les informations de contexte de **filterContext**.
    > 
    > Vous pouvez en savoir plus sur la classe **ControllerContext** sur [MSDN](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Tâche 2 : injection d’un intercepteur de code dans la classe de contrôleur du magasin

Au cours de cette tâche, vous allez ajouter le filtre personnalisé en l’injectant à toutes les classes de contrôleur et à toutes les actions du contrôleur qui seront journalisées. Dans le cadre de cet exercice, la classe de contrôleur de magasin aura un journal.

La méthode **OnActionExecuting** à partir du filtre personnalisé **ActionLogFilterAttribute** s’exécute lorsqu’un élément injecté est appelé.

Il est également possible d’intercepter une méthode de contrôleur spécifique.

1. Ouvrez **StoreController** sur **MvcMusicStore\Controllers** et ajoutez une référence à l’espace de noms **Filters** :

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Injectez le filtre personnalisé **CustomActionFilter** dans la classe **StoreController** en ajoutant l’attribut **[CustomActionFilter]** avant la déclaration de classe.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Lorsqu’un filtre est injecté dans une classe de contrôleur, toutes ses actions sont également injectées. Si vous souhaitez appliquer le filtre uniquement à un ensemble d’actions, vous devez injecter **[CustomActionFilter]** à chacun d’eux :
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tâche 3 : exécution de l’application

Dans cette tâche, vous allez tester le fonctionnement du filtre de journalisation. Vous allez démarrer l’application et visiter le Windows Store, puis vérifier les activités journalisées.

1. Appuyez sur **F5** pour exécuter l’application.
2. Accédez à **/ActionLog** pour afficher l’état initial de l’affichage des journaux :

    ![État du suivi des journaux avant l’activité de la page](aspnet-mvc-4-custom-action-filters/_static/image3.png "État du suivi des journaux avant l’activité de la page")

    *État du suivi des journaux avant l’activité de la page*

   > [!NOTE]
   > Par défaut, il affiche toujours un élément qui est généré lors de la récupération des genres existants du menu.
   > 
   > Pour des raisons de simplicité, nous supprimons la table **ActionLog** chaque fois que l’application s’exécute, de sorte qu’elle affiche uniquement les journaux de la vérification de chaque tâche particulière.
   > 
   > Vous devrez peut-être supprimer le code suivant de la **Session\_méthode Start** (dans la classe **global. asax** ), afin d’enregistrer un journal historique pour toutes les actions exécutées dans le contrôleur du magasin.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Cliquez sur l’un des **genres** dans le menu et effectuez des actions à cet emplacement, comme parcourir un album disponible.
4. Accédez à **/ActionLog** et, si le journal est vide, appuyez sur **F5** pour actualiser la page. Vérifiez que vos visites ont été suivies :

    ![Journal des actions avec activité journalisée](aspnet-mvc-4-custom-action-filters/_static/image4.png "Journal des actions avec activité journalisée")

    *Journal des actions avec activité journalisée*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Exercice 2 : gestion de plusieurs filtres d’action

Dans cet exercice, vous allez ajouter un deuxième filtre d’action personnalisé à la classe StoreController et définir l’ordre spécifique dans lequel les deux filtres seront exécutés. Ensuite, vous allez mettre à jour le code pour inscrire le filtre globalement.

Il existe différentes options à prendre en compte lors de la définition de l’ordre d’exécution des filtres. Par exemple, la propriété Order et l’étendue des filtres :

Vous pouvez définir une **étendue** pour chaque filtre. par exemple, vous pouvez définir l’étendue de tous les filtres d’action à exécuter dans l' **étendue du contrôleur**et tous les filtres d’autorisation à exécuter dans l' **étendue globale**. Les étendues ont un ordre d’exécution défini.

En outre, chaque filtre d’action a une propriété Order qui est utilisée pour déterminer l’ordre d’exécution dans l’étendue du filtre.

Pour plus d’informations sur l’ordre d’exécution des filtres d’action personnalisés, consultez cet article MSDN : ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Tâche 1 : création d’un filtre d’action personnalisé

Dans cette tâche, vous allez créer un filtre d’action personnalisé à injecter dans la classe StoreController, en apprenant à gérer l’ordre d’exécution des filtres.

1. Ouvrez le dossier **Begin** solution situé dans le dossier **\Source\Ex02-ManagingMultipleActionFilters\Begin** . Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

    1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
    2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
    3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

        > [!NOTE]
        > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
        > 
        > Pour plus d’informations, consultez cet article : [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Ajoutez une nouvelle C# classe dans le dossier **Filters** et nommez-la *MyNewCustomActionFilter.cs*
3. Ouvrez **MyNewCustomActionFilter.cs** et ajoutez une référence à **System. Web. Mvc** et à l’espace de noms **MvcMusicStore. Models** :

    (Extrait de code- *ASP.NET MVC 4 filtres d’action personnalisés-EX2-MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Remplacez la déclaration de classe par défaut par le code suivant.

    (Extrait de code- *ASP.NET MVC 4 filtres d’action personnalisés-EX2-MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Ce filtre d’action personnalisé est quasiment identique à celui que vous avez créé au cours de l’exercice précédent. La principale différence réside dans le fait qu’elle a le *&quot;enregistré par&quot;* attribut mis à jour avec ce nouveau nom de classe pour identifier le filtre qui a inscrit le journal.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Tâche 2 : injection d’un nouvel intercepteur de code dans la classe StoreController

Dans cette tâche, vous allez ajouter un nouveau filtre personnalisé à la classe StoreController et exécuter la solution pour vérifier la façon dont les deux filtres fonctionnent ensemble.

1. Ouvrez la classe **StoreController** située dans **MvcMusicStore\Controllers** et injectez le nouveau filtre personnalisé **MyNewCustomActionFilter** dans la classe **StoreController** comme indiqué dans le code suivant.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. À présent, exécutez l’application afin de voir comment ces deux filtres d’action personnalisés fonctionnent. Pour ce faire, appuyez sur **F5** et attendez que l’application démarre.
3. Accédez à **/ActionLog** pour afficher l’état initial de l’affichage des journaux.

    ![État du suivi des journaux avant l’activité de la page](aspnet-mvc-4-custom-action-filters/_static/image5.png "État du suivi des journaux avant l’activité de la page")

    *État du suivi des journaux avant l’activité de la page*
4. Cliquez sur l’un des **genres** dans le menu et effectuez des actions à cet emplacement, comme parcourir un album disponible.
5. Vérifiez que cette fois-ci ; vos visites ont été suivies deux fois : une fois pour chacun des filtres d’action personnalisés que vous avez ajoutés à la classe **StorageController** .

    ![Journal des actions avec activité journalisée](aspnet-mvc-4-custom-action-filters/_static/image6.png "Journal des actions avec activité journalisée")

    *Journal des actions avec activité journalisée*
6. Fermez le navigateur.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Tâche 3 : gestion de l’ordre des filtres

Dans cette tâche, vous allez apprendre à gérer l’ordre d’exécution des filtres à l’aide de la propriété Order.

1. Ouvrez la classe **StoreController** située dans **MvcMusicStore\Controllers** et spécifiez la propriété **Order** dans les deux filtres, comme indiqué ci-dessous.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. À présent, vérifiez la façon dont les filtres sont exécutés en fonction de la valeur de la propriété Order. Vous constaterez que le filtre avec la plus petite valeur d’ordre (**CustomActionFilter**) est le premier qui est exécuté. Appuyez sur **F5** et attendez que l’application démarre.
3. Accédez à **/ActionLog** pour afficher l’état initial de l’affichage des journaux.

    ![État du suivi des journaux avant l’activité de la page](aspnet-mvc-4-custom-action-filters/_static/image7.png "État du suivi des journaux avant l’activité de la page")

    *État du suivi des journaux avant l’activité de la page*
4. Cliquez sur l’un des **genres** dans le menu et effectuez des actions à cet emplacement, comme parcourir un album disponible.
5. Vérifiez que cette fois, vos visites ont été suivies triées par la valeur de commande des filtres : **CustomActionFilter** logs.

    ![Journal des actions avec activité journalisée](aspnet-mvc-4-custom-action-filters/_static/image8.png "Journal des actions avec activité journalisée")

    *Journal des actions avec activité journalisée*
6. À présent, vous allez mettre à jour la valeur de commande des filtres et vérifier comment l’ordre de journalisation change. Dans la classe **StoreController** , mettez à jour la valeur d’ordre des filtres comme indiqué ci-dessous.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Réexécutez l’application en appuyant sur **F5**.
8. Cliquez sur l’un des **genres** dans le menu et effectuez des actions à cet emplacement, comme parcourir un album disponible.
9. Vérifiez que cette fois, le filtre journaux créés par **MyNewCustomActionFilter** s’affiche en premier.

    ![Journal des actions avec activité journalisée](aspnet-mvc-4-custom-action-filters/_static/image9.png "Journal des actions avec activité journalisée")

    *Journal des actions avec activité journalisée*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Tâche 4 : enregistrement global des filtres

Dans cette tâche, vous allez mettre à jour la solution pour inscrire le nouveau filtre (**MyNewCustomActionFilter**) en tant que filtre global. En procédant ainsi, elle est déclenchée par toutes les actions effectuées dans l’application et non seulement dans les StoreController, comme dans la tâche précédente.

1. Dans la classe **StoreController** , supprimez l’attribut **[MyNewCustomActionFilter]** et la propriété Order de **[CustomActionFilter]** . Il doit se présenter comme suit :

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Ouvrez le fichier **global. asax** et recherchez l' **application\_méthode Start** . Notez que chaque fois que l’application démarre, il inscrit les filtres globaux en appelant la méthode **RegisterGlobalFilters** dans la classe **FilterConfig** .

    ![Inscription de filtres globaux dans global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Inscription de filtres globaux dans global. asax")

    *Inscription de filtres globaux dans global. asax*
3. Ouvrez le fichier **FilterConfig.cs** dans le dossier de démarrage de l' **application\_** .
4. Ajoutez une référence à à l’aide de System. Web. Mvc ; utilisation de MvcMusicStore. filters ; joint.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Mettez à jour la méthode **RegisterGlobalFilters** en ajoutant votre filtre personnalisé. Pour ce faire, ajoutez le code en surbrillance :

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Exécutez l’application en appuyant sur **F5**.
7. Cliquez sur l’un des **genres** dans le menu et effectuez des actions à cet emplacement, comme parcourir un album disponible.
8. Vérifiez que **[MyNewCustomActionFilter]** est également injecté dans HomeController et ActionLogController.

    ![Journal des actions avec activité journalisée](aspnet-mvc-4-custom-action-filters/_static/image11.png "Journal des actions avec activité journalisée")

    *Journal des actions avec activité globale journalisée*

> [!NOTE]
> En outre, vous pouvez déployer cette application sur les sites Web Windows Azure à l' [annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En effectuant ce laboratoire pratique, vous avez appris à étendre un filtre d’action pour exécuter des actions personnalisées. Vous avez également appris à injecter un filtre sur vos contrôleurs de page. Les concepts suivants ont été utilisés :

- Comment créer des filtres d’action personnalisés avec la classe ActionFilterAttribute MVC ASP.NET
- Comment injecter des filtres dans les contrôleurs MVC ASP.NET
- Comment gérer le classement des filtres à l’aide de la propriété Order
- Comment inscrire des filtres globalement

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe A : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.
2. Cliquez sur **Installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.
3. Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.

    ![Installer Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.

    ![Acceptation des termes du contrat de licence](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Acceptation des termes du contrat de licence*
5. Attendez que le processus de téléchargement et d’installation se termine.

    ![Progression de l'installation](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation terminée](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Installation terminée*
7. Cliquez sur **quitter** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .

    ![Vignette VS Express pour le Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *Vignette VS Express pour le Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

Cette annexe vous indique comment créer un nouveau site Web à partir de Windows Azure Portail de gestion et publier l’application que vous avez obtenue en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tâche 1 : création d’un nouveau site Web à partir du portail Windows Azure

1. Accédez au [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous à l’aide des informations d’identification Microsoft associées à votre abonnement.

    > [!NOTE]
    > Avec Windows Azure, vous pouvez héberger gratuitement 10 sites Web ASP.NET, puis mettre à l’échelle à mesure que votre trafic augmente. Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).

    ![Ouvrir une session sur Windows Portail Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Ouvrir une session sur Windows Portail Azure")

    *Connectez-vous à Windows Azure Portail de gestion*
2. Cliquez sur **nouveau** dans la barre de commandes.

    ![Création d’un nouveau site Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "Création d’un nouveau site Web")

    *Création d’un nouveau site Web*
3. Cliquez sur **compute** | **site Web**. Sélectionnez ensuite l’option **création rapide** . Fournissez une URL disponible pour le nouveau site Web, puis cliquez sur **créer un site Web**.

    > [!NOTE]
    > Un site Web Windows Azure est l’hôte d’une application Web s’exécutant dans le Cloud que vous pouvez contrôler et gérer. L’option création rapide vous permet de déployer une application Web terminée sur le site Web Windows Azure depuis l’extérieur du portail. Elle n’inclut pas les étapes de configuration d’une base de données.

    ![Création d’un nouveau site Web à l’aide de la création rapide](aspnet-mvc-4-custom-action-filters/_static/image19.png "Création d’un nouveau site Web à l’aide de la création rapide")

    *Création d’un nouveau site Web à l’aide de la création rapide*
4. Patientez jusqu’à la création du nouveau **site Web** .
5. Une fois le site Web créé, cliquez sur le lien sous la colonne **URL** . Vérifiez que le nouveau site Web fonctionne.

    ![Navigation vers le nouveau site Web](aspnet-mvc-4-custom-action-filters/_static/image20.png "Navigation vers le nouveau site Web")

    *Navigation vers le nouveau site Web*

    ![Site Web en cours d’exécution](aspnet-mvc-4-custom-action-filters/_static/image21.png "Site Web en cours d’exécution")

    *Site Web en cours d’exécution*
6. Revenez au portail et cliquez sur le nom du site Web dans la colonne **nom** pour afficher les pages de gestion.

    ![Ouverture des pages de gestion de site Web](aspnet-mvc-4-custom-action-filters/_static/image22.png "Ouverture des pages de gestion de site Web")

    *Ouverture des pages de gestion de site Web*
7. Dans la page **tableau de bord** , sous la section **Aperçu rapide** , cliquez sur le lien **Télécharger le profil de publication** .

    > [!NOTE]
    > Le *profil de publication* contient toutes les informations requises pour publier une application Web sur un site Web Windows Azure pour chaque méthode de publication activée. Le profil de publication contient les URL, les informations d'identification de l'utilisateur et les chaînes de base de données nécessaires pour la connexion et l'authentification auprès de chacun des points de terminaison pour lesquels une méthode de publication est activée. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour le web** et **Microsoft Visual Studio 2012** prennent en charge la lecture des profils de publication pour automatiser la configuration de ces programmes pour la publication d’applications Web sur les sites Web Windows Azure.

    ![Téléchargement du profil de publication du site Web](aspnet-mvc-4-custom-action-filters/_static/image23.png "Téléchargement du profil de publication du site Web")

    *Téléchargement du profil de publication du site Web*
8. Téléchargez le fichier de profil de publication dans un emplacement connu. Dans cet exercice, vous allez apprendre à utiliser ce fichier pour publier une application Web sur un site Web Windows Azure à partir de Visual Studio.

    ![Enregistrement du fichier de profil de publication](aspnet-mvc-4-custom-action-filters/_static/image24.png "Enregistrement du profil de publication")

    *Enregistrement du fichier de profil de publication*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tâche 2 : configuration du serveur de base de données

Si votre application utilise des bases de données SQL Server, vous devez créer un serveur SQL Database. Si vous souhaitez déployer une application simple qui n’utilise pas SQL Server vous pouvez ignorer cette tâche.

1. Vous aurez besoin d’un serveur de SQL Database pour stocker la base de données d’application. Vous pouvez afficher les serveurs SQL Database à partir de votre abonnement dans le portail de gestion Windows Azure, à partir des **bases de données SQL** | **serveurs** | **tableau de bord du serveur**. Si vous n’avez pas de serveur créé, vous pouvez en créer un à l’aide du bouton **Ajouter** de la barre de commandes. Notez le **nom et l’URL du serveur,** ainsi que le nom et le mot de passe de connexion de l’administrateur, car vous les utiliserez dans les tâches suivantes. Ne créez pas encore la base de données, car elle sera créée dans une étape ultérieure.

    ![Tableau de bord du serveur SQL Database](aspnet-mvc-4-custom-action-filters/_static/image25.png "Tableau de bord du serveur SQL Database")

    *Tableau de bord du serveur SQL Database*
2. Dans la tâche suivante, vous allez tester la connexion à la base de données à partir de Visual Studio. pour cette raison, vous devez inclure votre adresse IP locale dans la liste des **adresses IP autorisées**du serveur. Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de l' **adresse IP actuelle du client** et collez-la dans les zones de texte **adresse IP de début** et **adresse IP de fin** , puis cliquez sur le bouton ![ajouter-client-IP-adresse-OK-button](aspnet-mvc-4-custom-action-filters/_static/image26.png).

    ![Ajout de l’adresse IP du client](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Ajout de l’adresse IP du client*
3. Une fois l' **adresse IP du client** ajoutée à la liste des adresses IP autorisées, cliquez sur **Enregistrer** pour confirmer les modifications.

    ![Confirmer les modifications](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Confirmer les modifications*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tâche 3 : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

1. Revenez à la solution ASP.NET MVC 4. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet de site Web et sélectionnez **publier**.

    ![Publication de l’application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publication de l'application")

    *Publication du site Web*
2. Importez le profil de publication que vous avez enregistré dans la première tâche.

    ![Importation du profil de publication](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importation du profil de publication")

    *Importation du profil de publication*
3. Cliquez sur **valider la connexion**. Une fois la validation terminée, cliquez sur **suivant**.

    > [!NOTE]
    > La validation est terminée une fois que vous voyez une coche verte s’affiche en regard du bouton valider la connexion.

    ![Validation de la connexion](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validation de la connexion")

    *Validation de la connexion*
4. Dans la page **paramètres** , sous la section **bases de données** , cliquez sur le bouton en regard de la zone de texte de votre connexion à la base de données (par exemple, **DefaultConnection**).

    ![Configuration de Web Deploy](aspnet-mvc-4-custom-action-filters/_static/image32.png "Configuration de Web Deploy")

    *Configuration de Web Deploy*
5. Configurez la connexion de base de données comme suit :

   - Dans **nom du serveur** , tapez l’URL de votre serveur SQL Database à l’aide du préfixe *TCP :* .
   - Dans **nom d’utilisateur** , tapez le nom de connexion de l’administrateur du serveur.
   - Dans **mot de passe** , tapez le mot de passe de connexion de l’administrateur du serveur.
   - Tapez un nouveau nom de base de données.

     ![Configuration de la chaîne de connexion de destination](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuration de la chaîne de connexion de destination")

     *Configuration de la chaîne de connexion de destination*
6. Cliquez ensuite sur **OK**. Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.

    ![Création de la base de données](aspnet-mvc-4-custom-action-filters/_static/image34.png "Création de la chaîne de base de données")

    *Création de la base de données*
7. La chaîne de connexion que vous allez utiliser pour vous connecter à SQL Database dans Windows Azure est affichée dans la zone de texte connexion par défaut. Cliquez ensuite sur **Suivant**.

    ![Chaîne de connexion pointant vers SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Chaîne de connexion pointant vers SQL Database")

    *Chaîne de connexion pointant vers SQL Database*
8. Dans la page **Aperçu** , cliquez sur **publier**.

    ![Publication de l’application Web](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publication de l’application Web")

    *Publication de l’application Web*
9. Une fois le processus de publication terminé, votre navigateur par défaut ouvre le site Web publié.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Annexe C : utilisation d’extraits de code

Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main. Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.

![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-custom-action-filters/_static/image37.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")

*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***

1. Placez le curseur à l’endroit où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).
3. Regarder comme IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).
5. Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-custom-action-filters/_static/image38.png "Commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](aspnet-mvc-4-custom-action-filters/_static/image39.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*

![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](aspnet-mvc-4-custom-action-filters/_static/image40.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")

*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*

***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0. Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.

1. Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.
2. Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.

![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](aspnet-mvc-4-custom-action-filters/_static/image41.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")

*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*

![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](aspnet-mvc-4-custom-action-filters/_static/image42.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")

*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*
