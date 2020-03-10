---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Atelier pratique : un ASP.NET : intégration de ASP.NET Web Forms, MVC et API Web | Microsoft Docs'
author: rick-anderson
description: ASP.NET est une infrastructure permettant de créer des sites Web, des applications et des services à l’aide de technologies spécialisées telles que MVC, l’API Web et d’autres. Avec l’extension ASP.NET h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 165d104b5d3ef3281af449cc8673ad96f531d628
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623199"
---
# <a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Atelier pratique : un ASP.NET : intégration de ASP.NET Web Forms, MVC et API Web

par l' [équipe Web camps](https://twitter.com/webcamps)

[Télécharger le kit de formation Web camps](https://aka.ms/webcamps-training-kit)

> ASP.NET est une infrastructure permettant de créer des sites Web, des applications et des services à l’aide de technologies spécialisées telles que MVC, l’API Web et d’autres. Avec l’ASP.NET d’expansion qui a été observé depuis sa création et la nécessité d’intégrer ces technologies, il y a eu des efforts récents pour travailler dans **un ASP.net**.
> 
> Visual Studio 2013 introduit un nouveau système de projet unifié qui vous permet de générer une application et d’utiliser toutes les technologies ASP.NET dans un seul projet. Grâce à cette fonctionnalité, vous n’avez plus besoin de choisir une technologie au début d’un projet et de l’utiliser, mais plutôt d’encourager l’utilisation de plusieurs infrastructures ASP.NET dans un même projet.
> 
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible sur [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

<a id="Overview"></a>
## <a name="overview"></a>Présentation

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Créer un site Web basé sur un type de projet **ASP.net**
- Utiliser différents frameworks **ASP.net** comme **MVC** et l' **API Web** dans le même projet
- Identifier les principaux composants d’une application **ASP.net**
- Tirez parti de l’infrastructure de **génération de modèles automatique ASP.net** pour créer automatiquement des contrôleurs et des vues afin d’effectuer des opérations CRUD basées sur vos classes de modèle
- Exposer le même ensemble d’informations dans des formats lisibles par l’ordinateur et par l’utilisateur à l’aide de l’outil approprié pour chaque travail

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

Les éléments suivants sont requis pour effectuer ce laboratoire pratique :

- [Visual Studio Express 2013 pour Web](https://www.microsoft.com/visualstudio/) ou version ultérieure
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Installation

Pour exécuter les exercices dans ce laboratoire pratique, vous devez d’abord configurer votre environnement.

1. Ouvrez l’Explorateur Windows et accédez au dossier **source** du laboratoire.
2. Cliquez avec le bouton droit sur **Setup. cmd** et sélectionnez **exécuter en tant qu’administrateur** pour lancer le processus d’installation qui va configurer votre environnement et installer les extraits de code Visual Studio pour ce Lab.
3. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, confirmez l’action pour continuer.

> [!NOTE]
> Vérifiez que vous avez vérifié toutes les dépendances de ce Lab avant d’exécuter le programme d’installation.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Utilisation des extraits de code

Tout au long du document Lab, vous serez invité à insérer des blocs de code. Pour plus de commodité, la majeure partie de ce code est fournie sous la forme d’extraits de Visual Studio Code, auxquels vous pouvez accéder à partir de Visual Studio 2013 pour éviter d’avoir à l’ajouter manuellement.

> [!NOTE]
> Chaque exercice est accompagné d’une solution de démarrage située dans le dossier **Begin** de l’exercice, qui vous permet de suivre chaque exercice indépendamment des autres. N’oubliez pas que les extraits de code ajoutés au cours d’un exercice sont absents de ces solutions de démarrage et peuvent ne pas fonctionner tant que vous n’avez pas terminé l’exercice. À l’intérieur du code source d’un exercice, vous trouverez également un dossier de **fin** contenant une solution Visual Studio avec le code qui résulte de l’exécution des étapes de l’exercice correspondant. Vous pouvez utiliser ces solutions comme conseils si vous avez besoin d’aide supplémentaire au fur et à mesure que vous travaillez dans ce laboratoire pratique.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique comprend les exercices suivants :

1. [Création d’un projet de Web Forms](#Exercise1)
2. [Création d’un contrôleur MVC à l’aide de la génération de modèles automatique](#Exercise2)
3. [Création d’un contrôleur d’API Web à l’aide de la génération de modèles automatique](#Exercise3)

Durée estimée pour effectuer ce laboratoire : **60 minutes**

> [!NOTE]
> Lorsque vous démarrez Visual Studio pour la première fois, vous devez sélectionner l’une des collections de paramètres prédéfinies. Chaque collection prédéfinie est conçue pour correspondre à un style de développement particulier et détermine les dispositions de fenêtres, le comportement de l’éditeur, les extraits de code IntelliSense et les options de boîte de dialogue. Les procédures de ce laboratoire décrivent les actions nécessaires pour accomplir une tâche donnée dans Visual Studio lors de l’utilisation de la collection de **paramètres de développement généraux** . Si vous choisissez une autre collection de paramètres pour votre environnement de développement, il peut y avoir des différences entre les étapes que vous devez prendre en compte.

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Exercice 1 : création d’un projet de Web Forms

Dans cet exercice, vous allez créer un nouveau site Web Forms dans Visual Studio 2013 à l’aide de l’expérience de projet unifiée **ASP.net** , ce qui vous permettra d’intégrer facilement des composants Web Forms, MVC et d’API Web dans la même application. Vous allez ensuite explorer la solution générée et identifier ses parties, puis le site Web en action.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Tâche 1 : création d’un nouveau site à l’aide de l’expérience ASP.NET

Dans cette tâche, vous allez commencer à créer un nouveau site Web dans Visual Studio en fonction du type de projet **ASP.net** . **Un ASP.net** unifie toutes les technologies ASP.net et vous donne la possibilité de les mélanger et de les faire correspondre selon vos besoins. Vous reconnaîtrez ensuite les différents composants de Web Forms, MVC et l’API Web qui résident côte à côte dans votre application.

1. Ouvrez **Visual Studio Express 2013 pour le Web** et sélectionnez **fichier | Nouveau projet...** pour démarrer une nouvelle solution.

    ![Création d'un projet](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Création d’un nouveau projet*
2. Dans la boîte de dialogue **nouveau projet** , sélectionnez **application Web ASP.net** sous **le C# visuel | Web** et assurez-vous que **.NET Framework 4,5** est sélectionné. Nommez le projet *MyHybridSite*, choisissez un **emplacement** , puis cliquez sur **OK**.

    ![Nouveau projet d’application Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Création d’un projet d’application Web ASP.NET*
3. Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le modèle **Web Forms** et sélectionnez les options **MVC** et **API Web** . En outre, assurez-vous que l’option **d’authentification** est définie sur **comptes d’utilisateur individuels**. Cliquez sur **OK** pour continuer.

    ![Création d’un projet avec le modèle Web Forms, y compris l’API Web et les composants MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Création d’un projet avec le modèle Web Forms, y compris l’API Web et les composants MVC*
4. Vous pouvez maintenant explorer la structure de la solution générée.

    ![Exploration de la solution générée](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Exploration de la solution générée*

    1. **Compte :** Ce dossier contient les pages Web Form à inscrire, vous connecter et gérer les comptes d’utilisateur de l’application. Ce dossier est ajouté lorsque l’option d’authentification **comptes d’utilisateur individuels** est sélectionnée lors de la configuration du modèle de projet Web Forms.
    2. **Modèles :** Ce dossier contiendra les classes qui représentent les données de votre application.
    3. **Contrôleurs** et **vues**: ces dossiers sont requis pour les composants **ASP.NET MVC** et **API Web ASP.net** . Vous allez explorer les technologies MVC et API Web dans les prochains exercices.
    4. Les fichiers **default. aspx**, **contact. aspx** et **about. aspx** sont des pages de formulaires Web prédéfinis que vous pouvez utiliser comme points de départ pour générer les pages spécifiques à votre application. La logique de programmation de ces fichiers réside dans un fichier distinct appelé &quot;code-behind&quot; fichier, qui a une extension &quot;. aspx. vb&quot; ou &quot;. aspx.cs&quot; (selon le langage utilisé). La logique code-behind s’exécute sur le serveur et génère dynamiquement la sortie HTML de votre page.
    5. Les pages **site. Master** et **site. mobile. Master** définissent l’apparence et le comportement standard de toutes les pages de l’application.
5. Double-cliquez sur le fichier **default. aspx** pour explorer le contenu de la page.

    ![Exploration de la page default. aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Exploration de la page default. aspx*

    > [!NOTE]
    > La directive **page** en haut du fichier définit les attributs de la page Web Forms. Par exemple, l’attribut **MasterPageFile** spécifie le chemin d’accès à la page maître. dans ce cas, la page *site. Master* et l’attribut **Inherits** définissent la classe code-behind pour que la page hérite. Cette classe se trouve dans le fichier déterminé par l’attribut **CodeBehind** .
    > 
    > Le contrôle **asp : content** contient le contenu réel de la page (texte, balisage et contrôles) et est mappé à un contrôle **asp : ContentPlaceHolder** sur la page maître. Dans ce cas, le contenu de la page est affiché dans le contrôle *Maincontent* défini dans la page *site. Master* .
6. Développez le dossier de démarrage de l' **application\_** et notez le fichier **WebApiConfig.cs** . Visual Studio incluait ce fichier dans la solution générée, car vous avez inclus l’API Web lors de la configuration de votre projet avec le modèle ASP.NET.
7. Ouvrez le fichier **WebApiConfig.cs** . Dans la classe *WebApiConfig* , vous trouverez la configuration associée à l’API Web, qui mappe les itinéraires http aux **contrôleurs d’API Web**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Ouvrez le fichier **RouteConfig.cs** . À l’intérieur de la méthode *RegisterRoutes* , vous trouverez la configuration associée à MVC, qui mappe les itinéraires http aux **contrôleurs MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tâche 2 : exécution de la solution

Dans cette tâche, vous allez exécuter la solution générée, explorer l’application et certaines de ses fonctionnalités, telles que la réécriture d’URL et l’authentification intégrée.

1. Pour exécuter la solution, appuyez sur **F5** ou cliquez sur le bouton **Démarrer** situé dans la barre d’outils. La page d’hébergement de l’application doit s’ouvrir dans le navigateur.

    ![Exécution de la solution](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Vérifiez que les pages de Web Forms sont appelées. Pour ce faire, ajoutez **/contact.aspx** à l’URL dans la barre d’adresses et appuyez sur **entrée**.

    ![URL conviviales](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *URL conviviales*

    > [!NOTE]
    > Comme vous pouvez le voir, l’URL devient **/contact**. À partir de **ASP.net 4**, les fonctionnalités de routage d’URL ont été ajoutées à Web Forms, ce qui vous permet d’écrire des url comme *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* au lieu de *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)* . Pour plus d’informations, consultez [routage d’URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Vous allez maintenant découvrir le workflow d’authentification intégré à l’application. Pour ce faire, cliquez sur **s’inscrire** dans le coin supérieur droit de la page.

    ![Inscription d’un nouvel utilisateur](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Inscription d’un nouvel utilisateur*
4. Dans la page **inscrire** , entrez un **nom d’utilisateur** et un **mot de passe**, puis cliquez sur **inscrire**.

    ![Page inscrire](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Page inscrire*
5. L’application inscrit le nouveau compte et l’utilisateur est authentifié.

    ![Utilisateur authentifié](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Utilisateur authentifié*
6. Revenez à Visual Studio et appuyez sur **Maj + F5** pour arrêter le débogage.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Exercice 2 : création d’un contrôleur MVC à l’aide de la génération de modèles automatique

Dans cet exercice, vous allez tirer parti de l’infrastructure de génération de modèles automatique ASP.NET fournie par Visual Studio pour créer un contrôleur ASP.NET MVC 5 avec des actions et des vues Razor pour effectuer des opérations CRUD, sans écrire une seule ligne de code. Le processus de génération de modèles automatique utilise Entity Framework Code First pour générer le contexte de données et le schéma de base de données dans la base de données SQL.

**À propos de Entity Framework Code First**

Entity Framework (EF) est un mappeur objet-relationnel (ORM) qui vous permet de créer des applications d’accès aux données en programmant avec un modèle d’application conceptuel au lieu de programmer directement à l’aide d’un schéma de stockage relationnel.

Le flux de travail de modélisation Entity Framework Code First vous permet d’utiliser vos propres classes de domaine pour représenter le modèle sur lequel EF s’appuie lors de l’exécution d’une requête, d’un suivi des modifications et de fonctions de mise à jour. En utilisant le flux de travail de développement Code First, vous n’avez pas besoin de commencer votre application en créant une base de données ou en spécifiant un schéma. Au lieu de cela, vous pouvez écrire des classes .NET standard qui définissent les objets de modèle de domaine les plus appropriés pour votre application, et Entity Framework créera la base de données pour vous.

> [!NOTE]
> Vous pouvez en savoir plus sur Entity Framework [ici](../../../entity-framework.md).

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Tâche 1 : création d’un modèle

Vous allez maintenant définir une classe **Person** , qui sera le modèle utilisé par le processus de génération de modèles automatique pour créer le contrôleur MVC et les vues. Vous allez commencer par créer une classe de modèle **Person** , et les opérations CRUD dans le contrôleur seront créées automatiquement à l’aide des fonctionnalités de génération de modèles automatique.

1. Ouvrez **Visual Studio Express 2013 pour le Web** et la solution **MyHybridSite. sln** située dans le dossier **source/EX2-MvcScaffolding/Begin** . Vous pouvez également continuer avec la solution que vous avez obtenue au cours de l’exercice précédent.
2. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier **modèles** du projet **MyHybridSite** et sélectionnez **Ajouter | Classe...** .

    ![Ajout de la classe de modèle person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Ajout de la classe de modèle person*
3. Dans la boîte de dialogue **Ajouter un nouvel élément** , nommez le fichier *Person.cs* , puis cliquez sur **Ajouter**.

    ![Création de la classe de modèle person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Création de la classe de modèle person*
4. Remplacez le contenu du fichier **Person.cs** par le code suivant. Appuyez sur **CTRL + S** pour enregistrer les modifications.

    (Extrait de code- *BringingTogetherOneAspNet-EX2-PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **MyHybridSite** et sélectionnez **générer**, ou appuyez sur **Ctrl + Maj + B** pour générer le projet.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Tâche 2 : création d’un contrôleur MVC

Maintenant que le modèle **Person** est créé, vous allez utiliser la génération de modèles automatique ASP.NET MVC avec Entity Framework pour créer les actions et les vues du contrôleur CRUD pour **Person**.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier **Controllers** du projet **MyHybridSite** et sélectionnez **Ajouter | Nouvel élément de génération de modèles automatique.** ..

    ![Création d’un contrôleur de génération de modèles automatique](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Création d’un contrôleur de génération de modèles automatique*
2. Dans la boîte de dialogue **Ajouter une structure** , sélectionnez **contrôleur MVC 5 avec affichages, à l’aide de Entity Framework,** puis cliquez sur **Ajouter.**

    ![Sélection du contrôleur MVC 5 avec des vues et des Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Sélection du contrôleur MVC 5 avec des vues et des Entity Framework*
3. Définissez *MvcPersonController* comme **nom du contrôleur**, sélectionnez l’option utiliser les **actions du contrôleur Async** et sélectionnez **Person (MyHybridSite. Models)** comme **classe de modèle**.

    ![Ajout d’un contrôleur MVC avec la génération de modèles automatique](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Ajout d’un contrôleur MVC avec la génération de modèles automatique*
4. Sous **classe du contexte de données**, cliquez sur **nouveau contexte de données...** .

    ![Création d’un contexte de données](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Création d’un contexte de données*
5. Dans la boîte de dialogue **nouveau contexte de données** , nommez le nouveau contexte de données *PersonContext* , puis cliquez sur **Ajouter**.

    ![Création du nouveau PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Création du nouveau type de PersonContext*
6. Cliquez sur **Ajouter** pour créer le contrôleur pour la **personne** avec génération de modèles automatique. Visual Studio génère ensuite les actions du contrôleur, le contexte de données de la personne et les vues Razor.

    ![Après avoir créé le contrôleur MVC avec la génération de modèles automatique](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Après avoir créé le contrôleur MVC avec la génération de modèles automatique*
7. Ouvrez le fichier **MvcPersonController.cs** dans le dossier **Controllers** . Notez que les méthodes d’action CRUD ont été générées automatiquement.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > En activant la case à cocher **utiliser les actions du contrôleur Async** à partir des options de génération de modèles automatique des étapes précédentes, Visual Studio génère des méthodes d’action asynchrones pour toutes les actions qui impliquent l’accès au contexte de données Person. Il est recommandé d’utiliser des méthodes d’action asynchrones pour les requêtes à long terme et non liées à l’UC afin d’éviter que le serveur Web n’exécute le travail pendant le traitement de la requête.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tâche 3 : exécution de la solution

Dans cette tâche, vous allez réexécuter la solution pour vérifier que les vues de la **personne** fonctionnent comme prévu. Vous allez ajouter une nouvelle personne pour vérifier qu’elle est correctement enregistrée dans la base de données.

1. Appuyez sur **F5** pour exécuter la solution.
2. Accédez à **/MvcPerson**. La vue de génération de modèles automatique qui affiche la liste des personnes doit apparaître.
3. Cliquez sur **créer** pour ajouter une nouvelle personne.

    ![Navigation vers les vues MVC de génération de modèles automatique](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Navigation vers les vues MVC de génération de modèles automatique*
4. Dans la vue **créer** , fournissez un **nom** et une **ancienneté** pour la personne, puis cliquez sur **créer**.

    ![Ajout d’une nouvelle personne](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Ajout d’une nouvelle personne*
5. La nouvelle personne est ajoutée à la liste. Dans la liste d’éléments, cliquez sur **Détails** pour afficher la vue Détails de la personne. Ensuite, dans la vue **Détails** , cliquez sur **revenir à la liste** pour revenir à la vue liste.

    ![Vue Détails de la personne](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Vue Détails de la personne*
6. Cliquez sur le lien **supprimer** pour supprimer la personne. Dans la vue **supprimer** , cliquez sur **supprimer** pour confirmer l’opération.

    ![Suppression d’une personne](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Suppression d’une personne*
7. Revenez à Visual Studio et appuyez sur **Maj + F5** pour arrêter le débogage.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Exercice 3 : création d’un contrôleur d’API Web à l’aide de la génération de modèles automatique

L’infrastructure d’API Web fait partie de la pile ASP.NET et conçue pour faciliter l’implémentation des services HTTP, en envoyant et en recevant en général des données au format JSON ou XML via une API RESTful.

Dans cet exercice, vous allez utiliser à nouveau la génération de modèles automatique ASP.NET pour générer un contrôleur d’API Web. Vous allez utiliser les mêmes classes **Person** et **PersonContext** de l’exercice précédent pour fournir les mêmes données person au format JSON. Vous verrez comment vous pouvez exposer les mêmes ressources de différentes façons au sein de la même application ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Tâche 1 : création d’un contrôleur d’API Web

Au cours de cette tâche, vous allez créer un nouveau **contrôleur d’API Web** qui exposera les données Person dans un format de consommation d’ordinateur, tel que JSON.

1. S’il n’est pas déjà ouvert, ouvrez **Visual Studio Express 2013 pour le Web** et ouvrez la solution **MyHybridSite. sln** située dans le dossier **source/EX3-WebAPI/Begin** . Vous pouvez également continuer avec la solution que vous avez obtenue au cours de l’exercice précédent.

    > [!NOTE]
    > Si vous démarrez avec la solution Begin de l’exercice 3, appuyez sur **Ctrl + Maj + B** pour générer la solution.
2. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier **Controllers** du projet **MyHybridSite** et sélectionnez **Ajouter | Nouvel élément de génération de modèles automatique.** ..

    ![Création d’un contrôleur de génération de modèles automatique](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Création d’un contrôleur de génération de modèles automatique*
3. Dans la boîte de dialogue **Ajouter une structure** , sélectionnez **API Web** dans le volet gauche, puis **contrôleur d’API Web 2 avec actions, en utilisant Entity Framework** dans le volet central, puis cliquez sur **Ajouter.**

    ![Sélection du contrôleur d’API Web 2 avec des actions et des Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Sélection du contrôleur d’API Web 2 avec des actions et des Entity Framework")

    *Sélection du contrôleur d’API Web 2 avec des actions et des Entity Framework*
4. Définissez *ApiPersonController* comme **nom de contrôleur**, sélectionnez l’option utiliser les **actions du contrôleur Async** et sélectionnez **Person (MyHybridSite. Models)** et **PersonContext (MyHybridSite. Models)** comme classes de contexte de **modèle** et de **données** respectivement. Cliquez ensuite sur **Ajouter**.

    ![Ajout d’un contrôleur d’API Web à l’aide de la génération de modèles automatique](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Ajout d’un contrôleur d’API Web à l’aide de la génération de modèles automatique")

    *Ajout d’un contrôleur d’API Web à l’aide de la génération de modèles automatique*
5. Visual Studio génère ensuite la classe **ApiPersonController** avec les quatre actions CRUD pour travailler avec vos données.

    ![Après la création du contrôleur d’API Web à l’aide de la génération de modèles automatique](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "Après la création du contrôleur d’API Web à l’aide de la génération de modèles automatique")

    *Après la création du contrôleur d’API Web à l’aide de la génération de modèles automatique*
6. Ouvrez le fichier **ApiPersonController.cs** et examinez la méthode d’action *GetPeople* . Cette méthode interroge le champ DB du type **PersonContext** pour récupérer les données People.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. À présent, notez le commentaire au-dessus de la définition de la méthode. Il fournit l’URI qui expose cette action que vous allez utiliser dans la tâche suivante.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Par défaut, l’API Web est configurée pour intercepter les requêtes sur le chemin d’accès */API* afin d’éviter les collisions avec les contrôleurs Mvc. Si vous avez besoin de modifier ce paramètre, reportez-vous à [routage dans API Web ASP.net](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tâche 2 : exécution de la solution

Dans cette tâche, vous allez utiliser les **outils de développement F12** d’Internet Explorer pour inspecter la réponse complète du contrôleur d’API Web. Vous verrez comment vous pouvez capturer le trafic réseau pour obtenir plus d’informations sur vos données d’application.

> [!NOTE]
> Assurez-vous qu' **Internet Explorer** est sélectionné dans le bouton **Démarrer** situé dans la barre d’outils de Visual Studio.
> 
> ![Option Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> Les **outils de développement F12** disposent d’un vaste ensemble de fonctionnalités qui ne sont pas traitées dans ce laboratoire pratique. Si vous souhaitez en savoir plus à ce sujet, reportez-vous à [la rubrique utilisation des outils de développement F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).

1. Appuyez sur **F5** pour exécuter la solution.

    > [!NOTE]
    > Pour pouvoir suivre cette tâche correctement, votre application doit avoir des données. Si votre base de données est vide, vous pouvez revenir à la tâche 3 de l’exercice 2 et suivre les étapes de création d’une nouvelle personne à l’aide des vues MVC.
2. Dans le navigateur, appuyez sur **F12** pour ouvrir le panneau **outils de développement** . Appuyez sur **CTRL** + **4** ou cliquez sur l’icône **réseau** , puis cliquez sur le bouton fléché vert pour commencer à capturer le trafic réseau.

    ![Lancement de la capture réseau de l’API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Lancement de la capture réseau de l’API Web")

    *Lancement de la capture réseau de l’API Web*
3. Ajoutez **API/ApiPerson** à l’URL dans la barre d’adresse du navigateur. Vous allez maintenant examiner les détails de la réponse à partir du **ApiPersonController**.

    ![Récupération de données Person via l’API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Récupération de données Person via l’API Web")

    *Récupération de données Person via l’API Web*

    > [!NOTE]
    > Une fois le téléchargement terminé, vous êtes invité à effectuer une action avec le fichier téléchargé. Laissez la boîte de dialogue ouverte pour être en mesure de voir le contenu de la réponse dans la fenêtre outils de développement.
4. Vous allez maintenant inspecter le corps de la réponse. Pour ce faire, cliquez sur l’onglet **Détails** , puis sur corps de la **réponse**. Vous pouvez vérifier que les données téléchargées sont une liste d’objets avec l' **ID**des propriétés, le **nom** et l' **âge** correspondant à la classe **Person** .

    ![Affichage du corps de la réponse de l’API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Affichage du corps de la réponse de l’API Web")

    *Affichage du corps de la réponse de l’API Web*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Tâche 3 : ajout des pages d’aide de l’API Web

Lorsque vous créez une API Web, il est utile de créer une page d’aide afin que d’autres développeurs sachent comment appeler votre API. Vous pouvez créer et mettre à jour les pages de documentation manuellement, mais il est préférable de les générer automatiquement pour éviter d’avoir à effectuer des tâches de maintenance. Dans cette tâche, vous allez utiliser un package NuGet pour générer automatiquement les pages d’aide de l’API Web dans la solution.

1. Dans le menu **Outils** de Visual Studio, sélectionnez **Gestionnaire de package NuGet**, puis cliquez sur **console du gestionnaire de package**.
2. Dans la fenêtre **console du gestionnaire de package** , exécutez la commande suivante :

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > Le package **Microsoft. Aspnet. WebApi. HelpPage** installe les assemblys nécessaires et ajoute des vues MVC pour les pages d’aide sous le dossier **Areas/HelpPage** .

    ![Zone HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "Zone HelpPage")

    *Zone HelpPage*
3. Par défaut, les pages d’aide contiennent des chaînes d’espace réservé pour la documentation. Vous pouvez utiliser des commentaires de documentation XML pour créer la documentation. Pour activer cette fonctionnalité, ouvrez le fichier **HelpPageConfig.cs** situé dans le dossier **Areas/HelpPage/App\_Start** et supprimez les marques de commentaire de la ligne suivante :

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **MyHybridSite**, sélectionnez **Propriétés** , puis cliquez sur l’onglet **générer** .

    ![Onglet générer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Section Build")

    *Onglet générer*
5. Sous **sortie**, sélectionnez **fichier de documentation XML**. Dans la zone d’édition, tapez **application\_Data/XmlDocument. xml**.

    ![Section sortie dans l’onglet générer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Section sortie dans l’onglet générer")

    *Section sortie dans l’onglet générer*
6. Appuyez sur **CTRL** + **S** pour enregistrer les modifications.
7. Ouvrez le fichier **ApiPersonController.cs** à partir du dossier **Controllers** .
8. Entrez une nouvelle ligne entre la signature de la méthode *GetPeople* et le commentaire *//obtenir l’API/ApiPerson* , puis tapez trois barres obliques.

    > [!NOTE]
    > Visual Studio insère automatiquement les éléments XML qui définissent la documentation de la méthode.
9. Ajoutez un texte Résumé et la valeur de retour pour la méthode *GetPeople* . Elle doit ressembler à ce qui suit.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Appuyez sur **F5** pour exécuter la solution.
11. Ajoutez **/Help** à l’URL dans la barre d’adresses pour accéder à la page d’aide.

    ![API Web ASP.NET la page d’aide](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "API Web ASP.NET la page d’aide")

    *API Web ASP.NET la page d’aide*

    > [!NOTE]
    > Le contenu principal de la page est un tableau d’API, regroupées par contrôleur. Les entrées de table sont générées de manière dynamique, à l’aide de l’interface **IApiExplorer** . Si vous ajoutez ou mettez à jour un contrôleur d’API, la table est automatiquement mise à jour la prochaine fois que vous générez l’application.
    > 
    > La colonne **API** répertorie la méthode http et l’URI relatif. La colonne **Description** contient des informations qui ont été extraites de la documentation de la méthode.
12. Notez que la description que vous avez ajoutée au-dessus de la définition de méthode s’affiche dans la colonne Description.

    ![Description de la méthode API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "Description de la méthode API")

    *Description de la méthode API*
13. Cliquez sur l’une des méthodes de l’API pour accéder à une page contenant des informations plus détaillées, y compris des exemples de corps de réponse.

    ![Page d’informations détaillées](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Page d’informations détaillées")

    *Page d’informations détaillées*

---

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En effectuant ce laboratoire pratique, vous avez appris à :

- Créer une application Web à l’aide de l’expérience ASP.NET dans Visual Studio 2013
- Intégrer plusieurs technologies ASP.NET dans un seul projet
- Générer des contrôleurs et des vues MVC à partir de vos classes de modèle à l’aide de la structure ASP.NET
- Générez des contrôleurs d’API Web qui utilisent des fonctionnalités telles que la programmation asynchrone et l’accès aux données via Entity Framework
- Générer automatiquement les pages d’aide de l’API Web pour vos contrôleurs
