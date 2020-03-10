---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Atelier pratique : créer une application à page unique (SPA) avec API Web ASP.NET et angulaire. js-ASP.NET 4. x'
author: rick-anderson
description: 'Code pas à pas : création d’une application à page unique (SPA) avec API Web ASP.NET et angulaire. js pour ASP.NET 4. x.'
ms.author: riande
ms.date: 09/30/2015
ms.custom: seoapril2019
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 86833a890da759e489dd11dc9afb128a9b7a75e3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557042"
---
# <a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Atelier pratique : créer une application à page unique (SPA) avec API Web ASP.NET et angulaire. js

par l' [équipe Web camps](https://twitter.com/webcamps)

[Télécharger le kit de formation Web camps](https://aka.ms/webcamps-training-kit)

Ce laboratoire pratique vous montre comment créer une application à page unique (SPA) avec API Web ASP.NET et angulaire. js pour ASP.NET 4. x.

Dans ce laboratoire, vous allez tirer parti de ces technologies pour implémenter un quiz de l’un des passionnés, un site Web de questionnaires basé sur le concept SPA. Vous allez tout d’abord implémenter la couche de service avec API Web ASP.NET pour exposer les points de terminaison requis afin de récupérer les questions de quiz et de stocker les réponses. Ensuite, vous allez créer une interface utilisateur riche et réactive à l’aide des effets de transformation AngularJS et CSS3.

Dans les applications Web traditionnelles, le client (navigateur) initie la communication avec le serveur en demandant une page. Le serveur traite ensuite la requête et envoie le code HTML de la page au client. Dans les interactions suivantes avec la page, par exemple, l’utilisateur accède à un lien ou envoie un formulaire avec des données : une nouvelle demande est envoyée au serveur et le workflow recommence : le serveur traite la demande et envoie une nouvelle page au navigateur en réponse à la nouvelle demande d’action. Ed par le client.
> 
> Dans les applications à page unique (SPAs), la page entière est chargée dans le navigateur après la demande initiale, mais les interactions suivantes sont effectuées via des demandes Ajax. Cela signifie que le navigateur doit mettre à jour uniquement la partie de la page qui a changé ; Il n’est pas nécessaire de recharger toute la page. L’approche SPA réduit le temps nécessaire à l’application pour répondre aux actions de l’utilisateur, ce qui entraîne une expérience plus fluide.
> 
> L’architecture d’un SPA implique certains défis qui ne sont pas présents dans les applications Web traditionnelles. Toutefois, les technologies émergentes telles que API Web ASP.NET, les infrastructures JavaScript comme AngularJS et les nouvelles fonctionnalités de stylisation fournies par CSS3 rendent très facile la conception et la création de SPAs.
> 
> 
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible sur [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

## <a name="overview"></a>Présentation

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Créer un service API Web ASP.NET pour envoyer et recevoir des données JSON
- Créer une interface utilisateur réactive à l’aide de AngularJS
- Améliorez l’utilisation de l’interface utilisateur avec les transformations CSS3

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

Les éléments suivants sont requis pour effectuer ce laboratoire pratique :

- [Visual Studio Express 2013 pour Web](https://www.microsoft.com/visualstudio/) ou version ultérieure

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

1. [Création d’une API Web](#Exercise1)
2. [Création d’une interface SPA](#Exercise2)

Durée estimée pour effectuer ce laboratoire : **60 minutes**

> [!NOTE]
> Lorsque vous démarrez Visual Studio pour la première fois, vous devez sélectionner l’une des collections de paramètres prédéfinies. Chaque collection prédéfinie est conçue pour correspondre à un style de développement particulier et détermine les dispositions de fenêtres, le comportement de l’éditeur, les extraits de code IntelliSense et les options de boîte de dialogue. Les procédures de ce laboratoire décrivent les actions nécessaires pour accomplir une tâche donnée dans Visual Studio lors de l’utilisation de la collection de **paramètres de développement généraux** . Si vous choisissez une autre collection de paramètres pour votre environnement de développement, il peut y avoir des différences entre les étapes que vous devez prendre en compte.

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Exercice 1 : création d’une API Web

La couche de service est l’une des principales parties d’un SPA. Il est responsable du traitement des appels AJAX envoyés par l’interface utilisateur et du retour des données en réponse à cet appel. Les données récupérées doivent être présentées dans un format lisible par machine afin d’être analysées et consommées par le client.

L’infrastructure de l’API Web fait partie de la pile ASP.NET et est conçue pour faciliter l’implémentation des services HTTP, en envoyant en général et en recevant des données au format JSON ou XML via une API RESTful. Dans cet exercice, vous allez créer le site Web pour héberger l’application de quiz de l’ordinateur, puis implémenter le service principal pour exposer et conserver les données de quiz à l’aide de API Web ASP.NET.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Tâche 1 : création du projet initial pour le quiz d’un passionné

Dans cette tâche, vous allez commencer à créer un projet MVC ASP.NET avec prise en charge de API Web ASP.NET basé sur le type de projet **ASP.net** fourni avec Visual Studio. **Un ASP.net** unifie toutes les technologies ASP.net et vous donne la possibilité de les mélanger et de les faire correspondre selon vos besoins. Vous allez ensuite ajouter les classes de modèle du Entity Framework et l’initialiseur de base de données pour insérer les questions du quiz.

1. Ouvrez **Visual Studio Express 2013 pour le Web** et sélectionnez **fichier | Nouveau projet...** pour démarrer une nouvelle solution.

    ![Création d’un nouveau projet](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "Création d'un projet")

    *Création d’un nouveau projet*
2. Dans la boîte de dialogue **nouveau projet** , sélectionnez **application Web ASP.net** sous **le C# visuel | Onglet Web** . Assurez-vous que **.NET Framework 4,5** est sélectionné, nommez-le *GeekQuiz*, choisissez un **emplacement** , puis cliquez sur **OK**.

    ![Création d’un projet d’application Web ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "Création d’un projet d’application Web ASP.NET")

    *Création d’un projet d’application Web ASP.NET*
3. Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le modèle **MVC** et sélectionnez l’option **API Web** . En outre, assurez-vous que l’option **d’authentification** est définie sur **comptes d’utilisateur individuels**. Cliquez sur **OK** pour continuer.

    ![Création d’un nouveau projet avec le modèle MVC, y compris les composants d’API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Création d’un nouveau projet avec le modèle MVC, y compris les composants d’API Web*
4. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier **modèles** du projet **GeekQuiz** et sélectionnez **Ajouter | Élément existant...**

    ![Ajout d’un élément existant](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "Ajout d’un élément existant")

    *Ajout d’un élément existant*
5. Dans la boîte de dialogue **Ajouter un élément existant** , accédez au dossier **source/actifs/Models** , puis sélectionnez tous les fichiers. Cliquez sur **Ajouter**.

    ![Ajout des ressources du modèle](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "Ajout des ressources du modèle")

    *Ajout des ressources du modèle*

    > [!NOTE]
    > En ajoutant ces fichiers, vous ajoutez le modèle de données, le contexte de base de données de l’Entity Framework et l’initialiseur de base de données pour l’application de quiz de l’inverseur.
    > 
    > **Entity Framework (EF)** est un mappeur objet-relationnel (ORM) qui vous permet de créer des applications d’accès aux données en programmant avec un modèle d’application conceptuel au lieu de programmer directement à l’aide d’un schéma de stockage relationnel. Vous pouvez en savoir plus sur Entity Framework [ici](../../../entity-framework.md).
    > 
    > Vous trouverez ci-dessous une description des classes que vous venez d’ajouter :
    > 
    > - **TriviaOption :** représente une option unique associée à une question de quiz
    > - **TriviaQuestion :** représente une question de quiz et expose les options associées via la propriété **options**
    > - **TriviaAnswer :** représente l’option sélectionnée par l’utilisateur en réponse à une question de quiz
    > - **TriviaContext :** représente le contexte de la base de données du Entity Framework de l’application du quiz du passionné. Cette classe dérive de **DContext** et expose les propriétés **DbSet** qui représentent des collections des entités décrites ci-dessus.
    > - **TriviaDatabaseInitializer :** implémentation de l’initialiseur de Entity Framework pour la classe **TriviaContext** qui hérite de **CreateDatabaseIfNotExists**. Le comportement par défaut de cette classe consiste à créer la base de données uniquement si elle n’existe pas, en insérant les entités spécifiées dans la méthode **Seed** .
6. Ouvrez le fichier **global.asax.cs** et ajoutez l’instruction using suivante.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Ajoutez le code suivant au début de l' **Application\_méthode Start** pour définir le **TriviaDatabaseInitializer** en tant qu’initialiseur de base de données.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Modifiez le contrôleur d' **hébergement** pour limiter l’accès aux utilisateurs authentifiés. Pour ce faire, ouvrez le fichier **HomeController.cs** dans le dossier **Controllers** et ajoutez l’attribut **Authorize** à la définition de la classe **HomeController** .

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > Le filtre d' **autorisation** vérifie si l’utilisateur est authentifié. Si l’utilisateur n’est pas authentifié, il retourne le code d’état HTTP 401 (non autorisé) sans appeler l’action. Vous pouvez appliquer le filtre globalement, au niveau du contrôleur ou au niveau des actions individuelles.
9. Vous allez maintenant personnaliser la disposition des pages Web et la personnalisation. Pour ce faire, ouvrez le fichier **\_Layout. cshtml** à l’intérieur des **vues | Dossier partagé** et mettez à jour le contenu de l’élément de **&gt;de titre&lt;** en remplaçant *mon application ASP.net* par un quiz d’un *passionné*.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. Dans le même fichier, mettez à jour la barre de navigation en supprimant les liens *à propos* de et *contact* et en renommant le lien *démarrage* en *lecture*. En outre, renommez le lien du nom de l' *application* en quiz de l’un des *passionnés*. Le code HTML de la barre de navigation doit ressembler au code suivant.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Mettez à jour le pied de page de la page de disposition en remplaçant *mon Application ASP.net* par le quiz de l' *fou*. Pour ce faire, remplacez le contenu de l’élément de **&gt;de pied de page&lt;** par le code en surbrillance suivant.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Tâche 2 : création de l’API Web TriviaController

Au cours de la tâche précédente, vous avez créé la structure initiale de l’application Web « quiz ». Vous allez maintenant créer un service d’API Web simple qui interagit avec le modèle de données de quiz et expose les actions suivantes :

- **Obtenir/API/Trivia**: récupère la question suivante à partir de la liste des quiz pour obtenir une réponse de l’utilisateur authentifié.
- **Poster/API/Trivia**: stocke la réponse du quiz spécifiée par l’utilisateur authentifié.

Vous allez utiliser les outils de génération de modèles automatique ASP.NET fournis par Visual Studio pour créer la ligne de base de la classe du contrôleur d’API Web.

1. Ouvrez le fichier **WebApiConfig.cs** dans le dossier de démarrage de l' **application\_** . Ce fichier définit la configuration du service API Web, comme le mappage des itinéraires aux actions du contrôleur d’API Web.
2. Ajoutez l’instruction using suivante au début du fichier.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Ajoutez le code en surbrillance suivant à la méthode **Register** pour configurer globalement le formateur pour les données JSON récupérées par les méthodes d’action de l’API Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > Le **CamelCasePropertyNamesContractResolver** convertit automatiquement les noms de propriété en casse *mixte* , qui est la Convention générale pour les noms de propriété en JavaScript.
4. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier **Controllers** du projet **GeekQuiz** et sélectionnez **Ajouter | Nouvel élément de génération de modèles automatique.** ..

    ![Création d’un élément de génération de modèles automatique](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "Création d’un élément de génération de modèles automatique")

    *Création d’un élément de génération de modèles automatique*
5. Dans la boîte de dialogue **Ajouter une structure** , assurez-vous que le nœud **commun** est sélectionné dans le volet gauche. Sélectionnez ensuite le **Contrôleur Web API 2-vide** dans le volet central, puis cliquez sur **Ajouter**.

    ![Sélection du modèle vide du contrôleur API Web 2](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "Sélection du modèle vide du contrôleur API Web 2")

    *Sélection du modèle vide du contrôleur API Web 2*

    > [!NOTE]
    > La génération de **modèles automatique ASP.net** est une infrastructure de génération de code pour les applications Web ASP.net. Visual Studio 2013 comprend des générateurs de code préinstallés pour les projets MVC et API Web. Vous devez utiliser la génération de modèles automatique dans votre projet lorsque vous souhaitez ajouter rapidement du code qui interagit avec les modèles de données afin de réduire la durée nécessaire au développement d’opérations de données standard.
    > 
    > Le processus de génération de modèles automatique garantit également que toutes les dépendances requises sont installées dans le projet. Par exemple, si vous démarrez avec un projet ASP.NET vide, puis utilisez la génération de modèles automatique pour ajouter un contrôleur d’API Web, les références et packages NuGet de l’API Web requis sont ajoutés automatiquement à votre projet.
6. Dans la boîte de dialogue **Ajouter un contrôleur** , tapez *TriviaController* dans la zone de texte **nom du contrôleur** , puis cliquez sur **Ajouter**.

    ![Ajout du contrôleur de anecdotes](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "Ajout du contrôleur de anecdotes")

    *Ajout du contrôleur de anecdotes*
7. Le fichier **TriviaController.cs** est ensuite ajouté au dossier **Controllers** du projet **GeekQuiz** , contenant une classe **TriviaController** vide. Ajoutez les instructions using suivantes au début du fichier.

    (Extrait de code- *AspNetWebApiSpa-EX1-TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Ajoutez le code suivant au début de la classe **TriviaController** pour définir, initialiser et supprimer l’instance **TriviaContext** dans le contrôleur.

    (Extrait de code- *AspNetWebApiSpa-EX1-TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > La méthode **dispose** de **TriviaController** appelle la méthode **dispose** de l’instance **TriviaContext** , ce qui garantit que toutes les ressources utilisées par l’objet Context sont libérées lorsque l’instance **TriviaContext** est supprimée ou récupérée par le garbage collector. Cela comprend la fermeture de toutes les connexions de base de données ouvertes par Entity Framework.
9. Ajoutez la méthode d’assistance suivante à la fin de la classe **TriviaController** . Cette méthode récupère dans la base de données la question de quiz suivante à laquelle l’utilisateur spécifié doit répondre.

    (Extrait de code- *AspNetWebApiSpa-EX1-TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Ajoutez la méthode d’action d' **extraction** suivante à la classe **TriviaController** . Cette méthode d’action appelle la méthode d’assistance **NextQuestionAsync** définie à l’étape précédente pour récupérer la question suivante pour l’utilisateur authentifié.

    (Extrait de code- *AspNetWebApiSpa-EX1-TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Ajoutez la méthode d’assistance suivante à la fin de la classe **TriviaController** . Cette méthode stocke la réponse spécifiée dans la base de données et retourne une valeur booléenne indiquant si la réponse est correcte.

    (Extrait de code- *AspNetWebApiSpa-EX1-TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Ajoutez la méthode d’action de **publication** suivante à la classe **TriviaController** . Cette méthode d’action associe la réponse à l’utilisateur authentifié et appelle la méthode d’assistance **StoreAsync** . Ensuite, il envoie une réponse avec la valeur booléenne retournée par la méthode d’assistance.

    (Extrait de code- *AspNetWebApiSpa-EX1-TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Modifiez le contrôleur de l’API Web pour limiter l’accès aux utilisateurs authentifiés en ajoutant l’attribut **Authorize** à la définition de la classe **TriviaController** .

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tâche 3 : exécution de la solution

Dans cette tâche, vous allez vérifier que le service d’API Web que vous avez créé dans la tâche précédente fonctionne comme prévu. Vous allez utiliser la Outils de développement Internet Explorer **F12** pour capturer le trafic réseau et examiner la réponse complète du service API Web.

> [!NOTE]
> Assurez-vous qu' **Internet Explorer** est sélectionné dans le bouton **Démarrer** situé dans la barre d’outils de Visual Studio.
> 
> ![Option Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)

1. Appuyez sur **F5** pour exécuter la solution. La page **de connexion** doit s’afficher dans le navigateur.

    > [!NOTE]
    > Lorsque l’application démarre, l’itinéraire MVC par défaut est déclenché, ce qui est mappé par défaut à l’action d' **index** de la classe **HomeController** . Étant donné que **HomeController** est limité aux utilisateurs authentifiés (n’oubliez pas que vous avez décoré cette classe avec l’attribut **Authorize** dans l’exercice 1) et qu’aucun utilisateur n’a encore été authentifié, l’application redirige la demande d’origine vers la page de connexion.

    ![Exécution de la solution](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "Exécution de la solution")

    *Exécution de la solution*
2. Cliquez sur **inscrire** pour créer un nouvel utilisateur.

    ![Inscription d’un nouvel utilisateur](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "Inscription d’un nouvel utilisateur")

    *Inscription d’un nouvel utilisateur*
3. Dans la page **inscrire** , entrez un **nom d’utilisateur** et un **mot de passe**, puis cliquez sur **inscrire**.

    ![Page inscrire](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "Page inscrire")

    *Page inscrire*
4. L’application inscrit le nouveau compte et l’utilisateur est authentifié et redirigé vers la page d’hébergement.

    ![L’utilisateur est authentifié](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "Utilisateur authentifié")

    *L’utilisateur est authentifié*
5. Dans le navigateur, appuyez sur **F12** pour ouvrir le panneau **outils de développement** . Appuyez sur **CTRL + 4** ou cliquez sur l’icône **réseau** , puis cliquez sur le bouton fléché vert pour commencer à capturer le trafic réseau.

    ![Lancement de la capture réseau de l’API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "Lancement de la capture réseau de l’API Web")

    *Lancement de la capture réseau de l’API Web*
6. Ajoutez **API/anecdote** à l’URL dans la barre d’adresse du navigateur. Vous allez maintenant inspecter les détails de la réponse de la méthode d’action d' **extraction** dans **TriviaController**.

    ![Récupération des données de question suivante via l’API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "Récupération des données de question suivante via l’API Web")

    *Récupération des données de question suivante via l’API Web*

    > [!NOTE]
    > Une fois le téléchargement terminé, vous êtes invité à effectuer une action avec le fichier téléchargé. Laissez la boîte de dialogue ouverte pour être en mesure de voir le contenu de la réponse dans la fenêtre outils de développement.
7. Vous allez maintenant inspecter le corps de la réponse. Pour ce faire, cliquez sur l’onglet **Détails** , puis sur corps de la **réponse**. Vous pouvez vérifier que les données téléchargées sont un objet avec les **options** propriétés (qui est une liste d’objets **TriviaOption** ), l' **ID** et le **titre** correspondant à la classe **TriviaQuestion** .

    ![Affichage du corps de la réponse de l’API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "Affichage du corps de la réponse de l’API Web")

    *Affichage du corps de la réponse de l’API Web*
8. Revenez à Visual Studio et appuyez sur **Maj + F5** pour arrêter le débogage.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Exercice 2 : création de l’interface SPA

Dans cet exercice, vous allez commencer par créer la partie Web frontale du quiz de l’un des passionnés, en vous concentrant sur l’interaction de l’application à page unique à l’aide de **AngularJS**. Vous améliorerez ensuite l’expérience utilisateur avec CSS3 pour exécuter des animations riches et fournir un effet visuel du basculement de contexte lors de la transition d’une question à la suivante.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Tâche 1 : création de l’interface SPA à l’aide de AngularJS

Au cours de cette tâche, vous allez utiliser **AngularJS** pour implémenter le côté client de l’application quiz du test. **AngularJS** est une infrastructure JavaScript open source qui enrichit les applications basées sur un navigateur avec la fonctionnalité MVC ( *Model-View-Controller* ), facilitant ainsi le développement et le test.

Vous allez commencer par installer AngularJS à partir de la console du gestionnaire de package de Visual Studio. Ensuite, vous allez créer le contrôleur pour fournir le comportement de l’application de quiz de l’inverseur et la vue pour afficher les questions et réponses du quiz à l’aide du moteur de modèles AngularJS.

> [!NOTE]
> Pour plus d’informations sur AngularJS, reportez-vous à [[http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/).

1. Ouvrez **Visual Studio Express 2013 pour le Web** et ouvrez la solution **GeekQuiz. sln** située dans le dossier **source/EX2-CreatingASPAInterface/Begin** . Vous pouvez également continuer avec la solution que vous avez obtenue au cours de l’exercice précédent.
2. Ouvrez la **console du gestionnaire de package** dans **Outils** > **Gestionnaire de package NuGet**. Tapez la commande suivante pour installer le package NuGet **AngularJS. Core** .

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier **scripts** du projet **GeekQuiz** et sélectionnez **Ajouter | Nouveau dossier**. Nommez le dossier **application** , puis appuyez sur **entrée**.
4. Cliquez avec le bouton droit sur le dossier de l' **application** que vous venez de créer, puis sélectionnez **Ajouter | Fichier JavaScript**.

    ![Création d’un nouveau fichier JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Création d’un nouveau fichier JavaScript*
5. Dans la boîte de dialogue **spécifier le nom de l’élément** , tapez *quiz-Controller* dans la zone de texte nom de l' **élément** , puis cliquez sur **OK**.

    ![Attribution d’un nom au nouveau fichier JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Attribution d’un nom au nouveau fichier JavaScript*
6. Dans le fichier **quiz-Controller. js** , ajoutez le code suivant pour déclarer et initialiser le contrôleur **QuizCtrl** AngularJS.

    (Extrait de code- *AspNetWebApiSpa-EX2-AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > La fonction constructeur du contrôleur **QuizCtrl** attend un paramètre injectable nommé **$scope**. L’état initial de l’étendue doit être défini dans la fonction constructeur en joignant les propriétés à l’objet **$scope** . Les propriétés contiennent le **modèle de vue**et seront accessibles au modèle lors de l’inscription du contrôleur.
    > 
    > Le contrôleur **QuizCtrl** est défini à l’intérieur d’un module nommé **QuizApp**. Les modules sont des unités de travail qui vous permettent de décomposer votre application en composants distincts. Les principaux avantages de l’utilisation des modules sont que le code est plus facile à comprendre et facilite les tests unitaires, la réutilisation et la maintenabilité.
7. Vous allez maintenant ajouter un comportement à l’étendue pour réagir aux événements déclenchés à partir de la vue. Ajoutez le code suivant à la fin du contrôleur **QuizCtrl** pour définir la fonction **nextQuestion** dans l’objet **$scope** .

    (Extrait de code- *AspNetWebApiSpa-EX2-AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Cette fonction récupère la question suivante de l’API Web des **anecdotes** créée dans l’exercice précédent et joint les données de question à l’objet **$scope** .
8. Insérez le code suivant à la fin du contrôleur **QuizCtrl** pour définir la fonction **sendAnswer** dans l’objet **$scope** .

    (Extrait de code- *AspNetWebApiSpa-EX2-AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Cette fonction envoie la réponse sélectionnée par l’utilisateur à l’API Web des **anecdotes** et stocke le résultat, c.-à-d. Si la réponse est correcte ou non, dans l’objet **$scope** .
    > 
    > Les fonctions **nextQuestion** et **sendAnswer** ci-dessus utilisent l’objet AngularJS **$http** pour abstraire la communication avec l’API Web via l’objet JavaScript XMLHttpRequest du navigateur. AngularJS prend en charge un autre service qui offre un niveau d’abstraction plus élevé pour effectuer des opérations CRUD sur une ressource via des API RESTful. L’objet AngularJS **$Resource** a des méthodes d’action qui fournissent des comportements de haut niveau sans avoir besoin d’interagir avec l’objet **$http** . Envisagez d’utiliser l’objet **$Resource** dans les scénarios qui requièrent le modèle CRUD (informations de premier plan, consultez la [documentation $Resource](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. L’étape suivante consiste à créer le modèle AngularJS qui définit la vue du quiz. Pour ce faire, ouvrez le fichier **index. cshtml** à l’intérieur des **vues |** Et remplacez le contenu par le code suivant.

    (Extrait de code- *AspNetWebApiSpa-EX2-GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > Le modèle AngularJS est une spécification déclarative qui utilise les informations du modèle et le contrôleur pour transformer le balisage statique en vue dynamique que l’utilisateur voit dans le navigateur. Voici des exemples d’éléments AngularJS et d’attributs d’élément qui peuvent être utilisés dans un modèle :
    > 
    > - La directive **ng-App** indique à AngularJS l’élément DOM qui représente l’élément racine de l’application.
    > - La directive **ng-Controller** attache un contrôleur au DOM à l’endroit où la directive est déclarée.
    > - La notation d’accolade **{{}}** désigne les liaisons aux propriétés d’étendue définies dans le contrôleur.
    > - La directive **ng-Click** est utilisée pour appeler les fonctions définies dans l’étendue en réponse aux clics de l’utilisateur.
10. Ouvrez le fichier **site. CSS** dans le dossier **content** et ajoutez les styles en surbrillance suivants à la fin du fichier pour donner un aspect à la vue du quiz.

    (Extrait de code- *AspNetWebApiSpa-EX2-GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tâche 2 : exécution de la solution

Dans cette tâche, vous allez exécuter la solution à l’aide de la nouvelle interface utilisateur que vous avez créée avec AngularJS pour répondre à certaines questions du quiz.

1. Appuyez sur **F5** pour exécuter la solution.
2. Inscrire un nouveau compte d’utilisateur. Pour ce faire, suivez les étapes d’inscription décrites dans l’exercice 1, tâche 3.

    > [!NOTE]
    > Si vous utilisez la solution de l’exercice précédent, vous pouvez vous connecter avec le compte d’utilisateur que vous avez créé précédemment.
3. La page d' **hébergement** doit apparaître, affichant la première question du quiz. Répondez à la question en cliquant sur l’une des options. Cette opération déclenche la fonction **sendAnswer** définie précédemment, qui envoie l’option sélectionnée à l’API Web de la **anecdote** .

    ![Répondre à une question](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "Répondre à une question")

    *Répondre à une question*
4. Une fois que vous avez cliqué sur l’un des boutons, la réponse doit s’afficher. Cliquez sur **question suivante** pour afficher la question suivante. Cette opération déclenche la fonction **nextQuestion** définie dans le contrôleur.

    ![Demande de la question suivante](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "Demande de la question suivante")

    *Demande de la question suivante*
5. La question suivante doit s’afficher. Continuez à répondre aux questions autant de fois que vous le souhaitez. Après avoir complété toutes les questions, vous devez revenir à la première question.

    ![Autre question](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "Autre question")

    *Question suivante*
6. Revenez à Visual Studio et appuyez sur **Maj + F5** pour arrêter le débogage.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Tâche 3 : création d’une animation de retournement à l’aide de CSS3

Au cours de cette tâche, vous allez utiliser les propriétés CSS3 pour exécuter des animations riches en ajoutant un effet de retournement lorsqu’une question est traitée et que la question suivante est récupérée.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier de **contenu** du projet **GeekQuiz** et sélectionnez **Ajouter | Élément existant...**

    ![Ajout d’un élément existant au dossier Content](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Ajout d’un élément existant au dossier Content")

    *Ajout d’un élément existant au dossier Content*
2. Dans la boîte de dialogue **Ajouter un élément existant** , accédez au dossier **source/Assets** et sélectionnez **Flip. CSS**. Cliquez sur **Ajouter**.

    ![Ajout du fichier Flip. CSS à partir des ressources](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "Ajout du fichier Flip. CSS à partir des ressources")

    *Ajout du fichier Flip. CSS à partir des ressources*
3. Ouvrez le fichier **Flip. CSS** que vous venez d’ajouter et examinez son contenu.
4. Localisez le commentaire de **transformation de retournement** . Les styles situés en dessous de ce commentaire utilisent les transformations de **perspective** et de **rotation** CSS pour générer une &quot;&quot; effet de retournement de carte.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Localisez le **volet de masquage de l’arrière-plan lors du basculement** . Le style ci-dessous permet de masquer le côté arrière des visages lorsqu’ils sont en face de la visionneuse en affectant à la propriété CSS de **visibilité de face** arrière la valeur *Hidden*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Ouvrez le fichier **BundleConfig.cs** dans le dossier de démarrage de l' **application\_** et ajoutez la référence au fichier **Flip. CSS** dans le groupe de styles **&quot;~/content/CSS&quot;**

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Appuyez sur **F5** pour exécuter la solution et connectez-vous avec vos informations d’identification.
8. Répondez à une question en cliquant sur l’une des options. Notez l’effet de retournement lors de la transition entre les vues.

    ![Réponse à une question à l’aide de l’effet Flip](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "Réponse à une question à l’aide de l’effet Flip")

    *Réponse à une question à l’aide de l’effet Flip*
9. Cliquez sur **question suivante** pour récupérer la question suivante. L’effet de retournement doit s’afficher à nouveau.

    ![Récupération de la question suivante avec l’effet Flip](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "Récupération de la question suivante avec l’effet Flip")

    *Récupération de la question suivante avec l’effet Flip*

---

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En effectuant ce laboratoire pratique, vous avez appris à :

- Créer un contrôleur de API Web ASP.NET à l’aide de la génération de modèles automatique ASP.NET
- Implémenter une action de l’API Web pour récupérer la question suivante du quiz
- Implémenter une action de publication d’API Web pour stocker les réponses au questionnaire
- Installer AngularJS à partir de la console du gestionnaire de package Visual Studio
- Implémenter des contrôleurs et des modèles AngularJS
- Utiliser des transitions CSS3 pour effectuer des effets d’animation
