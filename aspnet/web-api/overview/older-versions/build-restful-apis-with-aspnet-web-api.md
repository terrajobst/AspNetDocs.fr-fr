---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Créer des API RESTful avec API Web ASP.NET ASP.NET 4. x
author: rick-anderson
description: 'Atelier pratique : utilisez l’API Web dans ASP.NET 4. x pour créer une API REST simple pour une application de gestionnaire de contacts.'
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 35b115d6b4f84084e78e429bbb4842670e57bba4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621813"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a>Créer des API RESTful avec API Web ASP.NET

par l' [équipe Web camps](https://twitter.com/webcamps)

> Atelier pratique : utilisez l’API Web dans ASP.NET 4. x pour créer une API REST simple pour une application de gestionnaire de contacts. Vous allez également créer un client pour consommer l’API.

Au cours des dernières années, il est devenu évident que le protocole HTTP ne sert pas seulement à traiter les pages HTML. Il s’agit également d’une plateforme puissante pour créer des API Web, à l’aide de quelques verbes (obtenir, poster, etc.), ainsi que de quelques concepts simples tels que les *URI* et *les en-têtes*. API Web ASP.NET est un ensemble de composants qui simplifient la programmation HTTP. Comme il repose sur le runtime ASP.NET MVC, l’API Web gère automatiquement les détails de transport de bas niveau de HTTP. En même temps, l’API Web expose naturellement le modèle de programmation HTTP. En fait, l’un des objectifs de l’API Web est de *ne pas* faire abstraction de la réalité du protocole http. Par conséquent, l’API Web est à la fois flexible et facile à étendre.  Le style architectural REST s’est avéré être un moyen efficace de tirer parti du protocole HTTP, bien qu’il ne s’agisse pas de la seule approche valide pour HTTP. Le gestionnaire de contacts exposera la RESTful pour répertorier, ajouter et supprimer des contacts, entre autres. 

Ce laboratoire requiert une compréhension de base du protocole HTTP, REST et suppose que vous avez une connaissance de base de HTML, JavaScript et jQuery.
> 
> > [!NOTE]
> > Le site Web ASP.NET dispose d’un domaine dédié à l’infrastructure API Web ASP.NET dans [https://asp.net/web-api](https://asp.net/web-api). Ce site continuera à fournir des informations, des exemples et des informations de dernière minute relatifs à l’API Web. il est donc important de le faire si vous souhaitez approfondir l’art de créer des API Web personnalisées accessibles à pratiquement n’importe quel Framework d’appareil ou de développement.
> > 
> > API Web ASP.NET, similaire à ASP.NET MVC 4, offre une grande flexibilité en termes de séparation de la couche de service des contrôleurs, ce qui vous permet d’utiliser assez facilement plusieurs infrastructures d’injection de dépendances disponibles. Il existe un bon exemple dans MSDN qui montre comment utiliser Ninject pour l’injection de dépendances dans un projet API Web ASP.NET que vous pouvez télécharger à partir d' [ici](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible sur [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Implémenter une API Web RESTful
- Appeler l’API à partir d’un client HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

Les éléments suivants sont requis pour effectuer ce laboratoire pratique :

- [Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe B](#AppendixB) pour obtenir des instructions sur la façon de l’installer).

<a id="Setup"></a>
### <a name="setup"></a>Installation

**Installation d’extraits de code**

Pour plus de commodité, la majeure partie du code que vous allez gérer dans le cadre de ce laboratoire est disponible sous forme d’extraits de code Visual Studio. Pour installer les extraits de code, exécutez le fichier **.\Source\Setup\CodeSnippets.vsi** .

Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;[annexe A : utilisation d’extraits de Code](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique comprend l’exercice suivant :

1. [Exercice 1 : créer une API Web en lecture seule](#Exercise1)
2. [Exercice 2 : créer une API Web en lecture/écriture](#Exercise2)
3. [Exercice 3 : utilisation de l’API Web à partir d’un client HTML](#Exercise3)

> [!NOTE]
> Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices. Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.

Durée estimée pour effectuer ce laboratoire : **60 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Exercice 1 : créer une API Web en lecture seule

Dans cet exercice, vous allez implémenter les méthodes d’extraction en lecture seule pour le gestionnaire de contacts.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Tâche 1 : création du projet d’API

Dans cette tâche, vous allez utiliser les nouveaux modèles de projet Web ASP.NET pour créer une application Web API Web.

1. Exécutez **Visual Studio 2012 Express pour le Web**. pour ce faire, accédez à **démarrer** , tapez **vs Express pour le Web** puis appuyez sur **entrée**.
2. Dans le menu **fichier** , sélectionnez **nouveau projet**. Sélectionner le **visuel C# |** Type de projet Web à partir de la vue d’arborescence de type de projet, puis sélectionnez le type de projet d' **Application Web ASP.NET MVC 4** . Définissez le **nom** du projet sur *ContactManager* et le **nom** de la solution à *commencer*, puis cliquez sur **OK**.

    ![Création d’un nouveau projet d’application Web ASP.NET MVC 4,0](build-restful-apis-with-aspnet-web-api/_static/image1.png "Création d’un nouveau projet d’application Web ASP.NET MVC 4,0")

    *Création d’un nouveau projet d’application Web ASP.NET MVC 4,0*
3. Dans la boîte de dialogue type de projet ASP.NET MVC 4, sélectionnez le type de projet **API Web** . Cliquez sur **OK**.

    ![Spécification du type de projet d’API Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "Spécification du type de projet d’API Web")

    *Spécification du type de projet d’API Web*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Tâche 2 : création des contrôleurs d’API du gestionnaire de contacts

Dans cette tâche, vous allez créer les classes de contrôleur dans lesquelles les méthodes d’API résideront.

1. Supprimez le fichier nommé **ValuesController.cs** dans le dossier **Controllers** du projet.
2. Cliquez avec le bouton droit sur le dossier **Controllers** dans le projet et sélectionnez **Ajouter | Dans le** menu contextuel.

    ![Ajout d’un nouveau contrôleur au projet](build-restful-apis-with-aspnet-web-api/_static/image3.png "Ajout d’un nouveau contrôleur au projet")

    *Ajout d’un nouveau contrôleur au projet*
3. Dans la boîte de dialogue **Ajouter un contrôleur** qui s’affiche, sélectionnez **contrôleur d’API vide** dans le menu modèle. Nommez la classe de contrôleur **ContactController**. Cliquez ensuite sur **Ajouter.**

    ![Utilisation de la boîte de dialogue Ajouter un contrôleur pour créer un nouveau contrôleur d’API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "Utilisation de la boîte de dialogue Ajouter un contrôleur pour créer un nouveau contrôleur d’API Web")

    *Utilisation de la boîte de dialogue Ajouter un contrôleur pour créer un nouveau contrôleur d’API Web*
4. Ajoutez le code suivant à **ContactController**.

    (Extrait de code- *API Web Lab-Ex01-obtient la méthode API*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Appuyez sur **F5** pour déboguer l’application. La page d’hébergement par défaut d’un projet d’API Web doit s’afficher.

    ![Page d’hébergement par défaut d’une application API Web ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "Page d’hébergement par défaut d’une application API Web ASP.NET")

    *Page d’hébergement par défaut d’une application API Web ASP.NET*
6. Dans la fenêtre Internet Explorer, appuyez sur la touche **F12** pour ouvrir la fenêtre **outils de développement** . Cliquez sur l’onglet **réseau** , puis sur le bouton **Démarrer la capture** pour commencer à capturer le trafic réseau dans la fenêtre.

    ![Ouverture de l’onglet réseau et lancement de la capture réseau](build-restful-apis-with-aspnet-web-api/_static/image6.png "Ouverture de l’onglet réseau et lancement de la capture réseau")

    *Ouverture de l’onglet réseau et lancement de la capture réseau*
7. Ajoutez l’URL dans la barre d’adresse du navigateur avec **/API/contact** et appuyez sur entrée. Les détails de la transmission s’affichent dans la fenêtre capture réseau. Notez que le type MIME de la réponse est **application/JSON**. Cela montre comment le format de sortie par défaut est JSON.

    ![Affichage de la sortie de la requête d’API Web dans la vue réseau](build-restful-apis-with-aspnet-web-api/_static/image7.png "Affichage de la sortie de la requête d’API Web dans la vue réseau")

    *Affichage de la sortie de la requête d’API Web dans la vue réseau*

    > [!NOTE]
    > À ce stade, le comportement par défaut d’Internet Explorer 10 consiste à demander si l’utilisateur souhaite enregistrer ou ouvrir le flux résultant de l’appel de l’API Web. La sortie est un fichier texte contenant le résultat JSON de l’appel de l’URL de l’API Web. N’annulez pas la boîte de dialogue afin de pouvoir regarder le contenu de la réponse via la fenêtre de l’outil développeurs.
8. Cliquez sur le bouton **accéder à la vue détaillée** pour afficher plus de détails sur la réponse de cet appel d’API.

    ![Basculer vers la vue détaillée](build-restful-apis-with-aspnet-web-api/_static/image8.png "Basculer vers le mode Détails")

    *Basculer vers la vue détaillée*
9. Cliquez sur l’onglet corps de la **réponse** pour afficher le texte de réponse JSON réel.

    ![Affichage du texte de sortie JSON dans le moniteur réseau](build-restful-apis-with-aspnet-web-api/_static/image9.png "Affichage du texte de sortie JSON dans le moniteur réseau")

    *Affichage du texte de sortie JSON dans le moniteur réseau*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Tâche 3 : création des modèles de contact et ajout du contrôleur de contact

Dans cette tâche, vous allez créer les classes de contrôleur dans lesquelles les méthodes d’API résideront.

1. Cliquez avec le bouton droit sur le dossier **modèles** et sélectionnez **Ajouter | Classe...** dans le menu contextuel.

    ![Ajout d’un nouveau modèle à l’application Web](build-restful-apis-with-aspnet-web-api/_static/image10.png "Ajout d’un nouveau modèle à l’application Web")

    *Ajout d’un nouveau modèle à l’application Web*
2. Dans la boîte de dialogue **Ajouter un nouvel élément** , nommez le nouveau fichier **contact.cs** , puis cliquez sur **Ajouter.**

    ![Création du fichier de classe contact](build-restful-apis-with-aspnet-web-api/_static/image11.png "Création du fichier de classe contact")

    *Création du fichier de classe contact*
3. Ajoutez le code en surbrillance suivant à la classe **contact** .

    (Extrait de code- *Lab API Web-Ex01-classe contact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. Dans la classe **ContactController** , sélectionnez la **chaîne** de mot dans la définition de méthode de la méthode d' **extraction** , puis tapez le mot *contact*. Une fois le mot tapé, un indicateur s’affiche au début du **contact**Word. Maintenez la touche **CTRL** enfoncée et appuyez sur la touche point (.) ou cliquez sur l’icône à l’aide de la souris pour ouvrir la boîte de dialogue d’assistance dans l’éditeur de code, afin de renseigner automatiquement la directive **using** pour l’espace de noms Models.

    ![Utilisation de l’assistance IntelliSense pour les déclarations d’espaces de noms](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Utilisation de l’assistance IntelliSense pour les déclarations d’espaces de noms*
5. Modifiez le code de la méthode d' **extraction** afin qu’il retourne un tableau d’instances de modèle de contact.

    (Extrait de code- *API Web-Lab-Ex01-retournant une liste de contacts*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Appuyez sur **F5** pour déboguer l’application Web dans le navigateur. Pour afficher les modifications apportées à la sortie de réponse de l’API, procédez comme suit.

   1. Une fois que le navigateur s’ouvre, appuyez sur **F12** si les outils de développement ne sont pas encore ouverts.
   2. Cliquez sur l’onglet **réseau** .
   3. Appuyez sur le bouton **Démarrer la capture** .
   4. Ajoutez le suffixe d’URL **/API/contact** à l’URL dans la barre d’adresses et appuyez sur la touche **entrée** .
   5. Appuyez sur le bouton **accéder à la vue détaillée** .
   6. Sélectionnez l’onglet corps de la **réponse** . Vous devez voir une chaîne JSON représentant la forme sérialisée d’un tableau d’instances de contact.

      ![Sortie sérialisée JSON d’un appel de méthode d’API Web complexe](build-restful-apis-with-aspnet-web-api/_static/image13.png "Sortie sérialisée JSON d’un appel de méthode d’API Web complexe")

      *Sortie sérialisée JSON d’un appel de méthode d’API Web complexe*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Tâche 4 : extraction des fonctionnalités dans une couche de service

Cette tâche montre comment extraire des fonctionnalités dans une couche de service pour permettre aux développeurs de séparer facilement leurs fonctionnalités de service de la couche de contrôleur, autorisant ainsi la réutilisation des services qui effectuent le travail.

1. Créez un nouveau dossier dans la racine de la solution et nommez-le **services**informatiques. Pour ce faire, cliquez avec le bouton droit sur le projet **ContactManager** , sélectionnez **Ajouter** | **nouveau dossier**, puis nommez-le *services*informatiques.

    ![Création du dossier services](build-restful-apis-with-aspnet-web-api/_static/image14.png "Création du dossier services")

    *Création du dossier services*
2. Cliquez avec le bouton droit sur le dossier **services** et sélectionnez **Ajouter | Classe...** dans le menu contextuel.

    ![Ajout d’une nouvelle classe au dossier services](build-restful-apis-with-aspnet-web-api/_static/image15.png "Ajout d’une nouvelle classe au dossier services")

    *Ajout d’une nouvelle classe au dossier services*
3. Quand la boîte de dialogue **Ajouter un nouvel élément** s’affiche, nommez la nouvelle classe **ContactRepository** , puis cliquez sur **Ajouter**.

    ![Création d’un fichier de classe pour contenir le code de la couche de service du dépôt de contacts](build-restful-apis-with-aspnet-web-api/_static/image16.png "Création d’un fichier de classe pour contenir le code de la couche de service du dépôt de contacts")

    *Création d’un fichier de classe pour contenir le code de la couche de service du dépôt de contacts*
4. Ajoutez une directive using au fichier **ContactRepository.cs** pour inclure l’espace de noms Models.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Ajoutez le code en surbrillance suivant au fichier **ContactRepository.cs** pour implémenter la méthode GetAllContacts.

    (Extrait de code- *Lab API Web-Ex01-dépôt du contact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Ouvrez le fichier **ContactController.cs** s’il n’est pas déjà ouvert.
7. Ajoutez l’instruction using suivante à la section de déclaration d’espace de noms du fichier.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Ajoutez le code en surbrillance suivant à la classe **ContactController.cs** pour ajouter un champ privé afin de représenter l’instance du référentiel, afin que les autres membres de la classe puissent utiliser l’implémentation du service.

    (Extrait de code- *Lab API Web-Ex01-Contact Controller*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Modifiez la méthode d' **extraction** afin qu’elle utilise le service de référentiel de contacts.

    (Extrait de code- *Lab API Web-Ex01-renvoi d’une liste de contacts via le référentiel*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Placez un point d’arrêt sur la définition de méthode d' **extraction** de **ContactController**.

   ![Ajout de points d’arrêt au contrôleur de contact](build-restful-apis-with-aspnet-web-api/_static/image17.png "Ajout de points d’arrêt au contrôleur de contact")

   *Ajout de points d’arrêt au contrôleur de contact*
11. Appuyez sur **F5** pour exécuter l’application.
12. Lorsque le navigateur s’ouvre, appuyez sur **F12** pour ouvrir les outils de développement.
13. Cliquez sur l’onglet **réseau** .
14. Cliquez sur le bouton **Démarrer la capture** .
15. Ajoutez l’URL dans la barre d’adresses avec le suffixe **/API/contact** et appuyez sur **entrée** pour charger le contrôleur d’API.
16. Visual Studio 2012 doit s’arrêter une fois que l’exécution de la méthode de **récupération** commence.

   ![Rupture dans la méthode d’extraction](build-restful-apis-with-aspnet-web-api/_static/image18.png "Rupture dans la méthode d’extraction")

   *Rupture dans la méthode d’extraction*
17. Appuyez sur **F5** pour continuer.
18. Revenez à Internet Explorer s’il n’est pas déjà actif. Notez la fenêtre de capture réseau.

    ![Vue réseau dans Internet Explorer présentant les résultats de l’appel de l’API Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "Vue réseau dans Internet Explorer présentant les résultats de l’appel de l’API Web")

    *Vue réseau dans Internet Explorer présentant les résultats de l’appel de l’API Web*
19. Cliquez sur le bouton **accéder à la vue détaillée** .
20. Cliquez sur l’onglet corps de la **réponse** . Notez la sortie JSON de l’appel d’API et la façon dont il représente les deux contacts récupérés par la couche de service.

    ![Affichage de la sortie JSON de l’API Web dans la fenêtre outils de développement](build-restful-apis-with-aspnet-web-api/_static/image20.png "Affichage de la sortie JSON de l’API Web dans la fenêtre outils de développement")

    *Affichage de la sortie JSON de l’API Web dans la fenêtre outils de développement*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Exercice 2 : créer une API Web en lecture/écriture

Dans cet exercice, vous allez implémenter des méthodes de publication et de placement pour que le gestionnaire de contacts les active avec les fonctionnalités d’édition de données.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Tâche 1 : ouverture du projet d’API Web

Dans cette tâche, vous allez préparer l’amélioration du projet d’API Web créé dans l’exercice 1 afin qu’il puisse accepter les entrées utilisateur.

1. Exécutez **Visual Studio 2012 Express pour le Web**. pour ce faire, accédez à **démarrer** , tapez **vs Express pour le Web** puis appuyez sur **entrée**.
2. Ouvrez le **début** de la solution situé dans **source/Ex02-ReadWriteWebAPI/Begin/** Folder. Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
3. Ouvrez le fichier **services/ContactRepository. cs** .

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Tâche 2 : ajout de fonctionnalités de persistance des données à l’implémentation du dépôt de contacts

Dans cette tâche, vous allez augmenter la classe ContactRepository du projet d’API Web créé dans l’exercice 1 afin qu’elle puisse conserver et accepter les entrées d’utilisateur et les nouvelles instances de contact.

1. Ajoutez la constante suivante à la classe **ContactRepository** pour représenter le nom du nom de clé de l’élément de cache du serveur Web plus loin dans cet exercice.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Ajoutez un constructeur au **ContactRepository** contenant le code suivant.

    (Extrait de code- *Lab API Web-Ex02-constructeur du dépôt de contacts*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Modifiez le code de la méthode **GetAllContacts** comme illustré ci-dessous.

    (Extrait de code- *API Web-Lab-Ex02-obtient tous les contacts*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Cet exemple est fourni à des fins de démonstration et utilise le cache du serveur Web comme support de stockage, afin que les valeurs soient disponibles simultanément pour plusieurs clients, au lieu d’utiliser un mécanisme de stockage de session ou une durée de vie de stockage des demandes. Vous pouvez utiliser Entity Framework, le stockage XML ou toute autre variété à la place du cache du serveur Web.
4. Implémentez une nouvelle méthode nommée **SaveContact** à la classe **ContactRepository** pour effectuer le travail d’enregistrement d’un contact. La méthode **SaveContact** doit accepter un seul paramètre de **contact** et retourner une valeur booléenne indiquant la réussite ou l’échec.

    (Extrait de code- *Lab API Web-Ex02-implémentation de la méthode SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Exercice 3 : utilisation de l’API Web à partir d’un client HTML

Dans cet exercice, vous allez créer un client HTML pour appeler l’API Web. Ce client facilite l’échange de données avec l’API Web à l’aide de JavaScript et affiche les résultats dans un navigateur Web à l’aide du balisage HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Tâche 1 : modification de la vue d’index pour fournir une interface utilisateur graphique pour l’affichage des contacts

Dans cette tâche, vous allez modifier la vue d’index par défaut de l’application Web pour qu’elle prenne en charge l’affichage de la liste des contacts existants dans un navigateur HTML.

1. Ouvrez **Visual Studio 2012 Express pour le Web** s’il n’est pas déjà ouvert.
2. Ouvrez le **début** de la solution situé dans **source/Ex03-ConsumingWebAPI/Begin/** Folder. Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
3. Ouvrez le fichier **index. cshtml** situé dans **views/** dossier de démarrage.
4. Remplacez le code HTML dans l’élément div par le **corps** de l’ID afin qu’il ressemble au code suivant.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Ajoutez le code JavaScript suivant au bas du fichier pour effectuer la requête HTTP à l’API Web.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Ouvrez le fichier **ContactController.cs** s’il n’est pas déjà ouvert.
7. Placez un point d’arrêt sur la méthode d' **extraction** de la classe **ContactController** .

    ![Placement d’un point d’arrêt sur la méthode d’extraction du contrôleur d’API](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placement d’un point d’arrêt sur la méthode d’extraction du contrôleur d’API")

    *Placement d’un point d’arrêt sur la méthode d’extraction du contrôleur d’API*
8. Appuyez sur **F5** pour exécuter le projet. Le navigateur chargera le document HTML.

    > [!NOTE]
    > Assurez-vous que vous accédez à l’URL racine de votre application.
9. Une fois la page chargée et le JavaScript exécuté, le point d’arrêt est atteint et l’exécution du code s’interrompt dans le contrôleur.

    ![Débogage dans les appels d’API Web à l’aide de VS Express pour le Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Débogage dans les appels d’API Web à l’aide de VS Express pour le Web")

    *Débogage dans l’appel d’API Web à l’aide de Visual Studio 2012 Express pour le Web*
10. Supprimez le point d’arrêt et appuyez sur **F5** ou sur le bouton **Continuer** de la barre d’outils de débogage pour continuer à charger la vue dans le navigateur. Une fois l’appel de l’API Web terminé, vous devez voir les contacts renvoyés à partir de l’appel d’API Web affiché en tant qu’éléments de liste dans le navigateur.

    ![Résultats de l’appel d’API affiché dans le navigateur en tant qu’éléments de liste](build-restful-apis-with-aspnet-web-api/_static/image23.png "Résultats de l’appel d’API affiché dans le navigateur en tant qu’éléments de liste")

    *Résultats de l’appel d’API affiché dans le navigateur en tant qu’éléments de liste*
11. Arrêter le débogage.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Tâche 2 : modification de la vue d’index pour fournir une interface utilisateur graphique pour la création de contacts

Dans cette tâche, vous allez continuer à modifier la vue index de l’application MVC. Un formulaire est ajouté à la page HTML qui capture l’entrée utilisateur et l’envoie à l’API Web pour créer un nouveau contact, et une nouvelle méthode de contrôleur d’API Web est créée pour collecter la date à partir de l’interface graphique.

1. Ouvrez le fichier **ContactController.cs** .
2. Ajoutez une nouvelle méthode à la classe de contrôleur nommée « **publication** », comme indiqué dans le code suivant.

    (Extrait de code- *API Web Lab-Ex03-méthode de publication*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Ouvrez le fichier **index. cshtml** dans Visual Studio s’il n’est pas déjà ouvert.
4. Ajoutez le code HTML ci-dessous au fichier juste après la liste non triée que vous avez ajoutée au cours de la tâche précédente.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. Dans l’élément script situé en bas du document, ajoutez le code en surbrillance suivant pour gérer les événements de clic de bouton, qui publient les données dans l’API Web à l’aide d’un appel HTTP postérieur.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. Dans **ContactController.cs**, placez un point d’arrêt sur la méthode de **publication** .
7. Appuyez sur **F5** pour exécuter l’application dans le navigateur.
8. Une fois la page chargée dans le navigateur, tapez un nouveau nom et un nouvel ID de contact, puis cliquez sur le bouton **Enregistrer** .

    ![Document HTML client chargé dans le navigateur](build-restful-apis-with-aspnet-web-api/_static/image24.png "Document HTML client chargé dans le navigateur")

    *Document HTML client chargé dans le navigateur*
9. Lorsque la fenêtre du débogueur s’arrête dans la méthode de **publication** , examinez les propriétés du paramètre de **contact** . Les valeurs doivent correspondre aux données que vous avez entrées dans le formulaire.

    ![Objet contact envoyé à l’API Web à partir du client](build-restful-apis-with-aspnet-web-api/_static/image25.png "Objet contact envoyé à l’API Web à partir du client")

    *Objet contact envoyé à l’API Web à partir du client*
10. Exécutez pas à pas la méthode dans le débogueur jusqu’à ce que la variable de **réponse** ait été créée. Une fois l’inspection effectuée dans la fenêtre **variables locales** du débogueur, vous verrez que toutes les propriétés ont été définies.

   ![La réponse qui suit la création dans le débogueur](build-restful-apis-with-aspnet-web-api/_static/image26.png "La réponse qui suit la création dans le débogueur")

   *La réponse qui suit la création dans le débogueur*
11. Si vous appuyez sur **F5** ou cliquez sur **Continuer** dans le débogueur, la demande se termine. Une fois que vous revenez dans le navigateur, le nouveau contact a été ajouté à la liste des contacts stockés par l’implémentation de **ContactRepository** .

   ![Le navigateur reflète la création réussie de la nouvelle instance de contact](build-restful-apis-with-aspnet-web-api/_static/image27.png "Le navigateur reflète la création réussie de la nouvelle instance de contact")

   *Le navigateur reflète la création réussie de la nouvelle instance de contact*

> [!NOTE]
> En outre, vous pouvez déployer cette application sur Azure à l' [aide de l’annexe C : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy](#AppendixC).

---

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

Ce laboratoire vous a présenté le nouvel API Web ASP.NET Framework et l’implémentation des API Web RESTful à l’aide de l’infrastructure. À partir de là, vous pouvez créer un référentiel qui facilite la persistance des données à l’aide de n’importe quel nombre de mécanismes et relier ce service au lieu du simple, fourni comme exemple dans ce laboratoire. L’API Web prend en charge un certain nombre de fonctionnalités supplémentaires, telles que l’activation de la communication à partir de clients non HTML écrits dans n’importe quel langage qui prend en charge les protocoles HTTP et JSON ou XML. La possibilité d’héberger une API Web en dehors d’une application Web classique est également possible, ainsi que la possibilité de créer vos propres formats de sérialisation.

Le site Web ASP.NET dispose d’un domaine dédié à l’infrastructure API Web ASP.NET dans [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api). Ce site continuera à fournir des informations, des exemples et des informations de dernière minute relatifs à l’API Web. il est donc important de le faire si vous souhaitez approfondir l’art de créer des API Web personnalisées accessibles à pratiquement n’importe quel Framework d’appareil ou de développement.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Annexe A : utilisation d’extraits de code

Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main. Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.

![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](build-restful-apis-with-aspnet-web-api/_static/image28.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")

*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)

1. Placez le curseur à l’endroit où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).
3. Regarder comme IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).
5. Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.

    ![Commencez à taper le nom de l’extrait de code](build-restful-apis-with-aspnet-web-api/_static/image29.png "Commencez à taper le nom de l’extrait de code")

    *Commencez à taper le nom de l’extrait de code*

    ![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](build-restful-apis-with-aspnet-web-api/_static/image30.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")

    *Appuyez sur Tab pour sélectionner l’extrait en surbrillance*

    ![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](build-restful-apis-with-aspnet-web-api/_static/image31.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")

    *Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)

1. Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.
2. Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.
3. Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.

    ![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](build-restful-apis-with-aspnet-web-api/_static/image32.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")

    *Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*

    ![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](build-restful-apis-with-aspnet-web-api/_static/image33.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")

    *Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Annexe B : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Azure</em>&quot;.
2. Cliquez sur **Installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.
3. Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.

    ![Installer Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.

    ![Acceptation des termes du contrat de licence](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Acceptation des termes du contrat de licence*
5. Attendez que le processus de téléchargement et d’installation se termine.

    ![Progression de l'installation](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation terminée](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Installation terminée*
7. Cliquez sur **quitter** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .

    ![Vignette VS Express pour le Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *Vignette VS Express pour le Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Annexe C : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

Cette annexe va vous montrer comment créer un nouveau site Web à partir du portail Azure et publier l’application que vous avez obtenue en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tâche 1 : création d’un nouveau site Web à partir du portail Azure

1. Accédez à la [portail de gestion Azure](https://manage.windowsazure.com/) et connectez-vous à l’aide des informations d’identification Microsoft associées à votre abonnement.

    > [!NOTE]
    > Avec Azure, vous pouvez héberger gratuitement 10 sites Web ASP.NET, puis mettre à l’échelle à mesure que votre trafic augmente. Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).

    ![Ouvrir une session sur Windows Portail Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Ouvrir une session sur Windows Portail Azure")

    *Se connecter au portail*
2. Cliquez sur **nouveau** dans la barre de commandes.

    ![Création d’un nouveau site Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "Création d’un nouveau site Web")

    *Création d’un nouveau site Web*
3. Cliquez sur **compute** | **site Web**. Sélectionnez ensuite l’option **création rapide** . Fournissez une URL disponible pour le nouveau site Web, puis cliquez sur **créer un site Web**.

    > [!NOTE]
    > Azure est l’hôte d’une application Web exécutée dans le Cloud que vous pouvez contrôler et gérer. L’option création rapide vous permet de déployer une application Web terminée sur Azure à partir de l’extérieur du portail. Elle n’inclut pas les étapes de configuration d’une base de données.

    ![Création d’un nouveau site Web à l’aide de la création rapide](build-restful-apis-with-aspnet-web-api/_static/image41.png "Création d’un nouveau site Web à l’aide de la création rapide")

    *Création d’un nouveau site Web à l’aide de la création rapide*
4. Patientez jusqu’à la création du nouveau **site Web** .
5. Une fois le site Web créé, cliquez sur le lien sous la colonne **URL** . Vérifiez que le nouveau site Web fonctionne.

    ![Navigation vers le nouveau site Web](build-restful-apis-with-aspnet-web-api/_static/image42.png "Navigation vers le nouveau site Web")

    *Navigation vers le nouveau site Web*

    ![Site Web en cours d’exécution](build-restful-apis-with-aspnet-web-api/_static/image43.png "Site Web en cours d’exécution")

    *Site Web en cours d’exécution*
6. Revenez au portail et cliquez sur le nom du site Web dans la colonne **nom** pour afficher les pages de gestion.

    ![Ouverture des pages de gestion de site Web](build-restful-apis-with-aspnet-web-api/_static/image44.png "Ouverture des pages de gestion de site Web")

    *Ouverture des pages de gestion de site Web*
7. Dans la page **tableau de bord** , sous la section **Aperçu rapide** , cliquez sur le lien **Télécharger le profil de publication** .

    > [!NOTE]
    > Le *profil de publication* contient toutes les informations requises pour publier une application Web sur Azure pour chaque méthode de publication activée. Le profil de publication contient les URL, les informations d'identification de l'utilisateur et les chaînes de base de données nécessaires pour la connexion et l'authentification auprès de chacun des points de terminaison pour lesquels une méthode de publication est activée. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour le web** et **Microsoft Visual Studio 2012** prennent en charge la lecture des profils de publication pour automatiser la configuration de ces programmes pour la publication d’applications Web sur Azure.

    ![Téléchargement du profil de publication du site Web](build-restful-apis-with-aspnet-web-api/_static/image45.png "Téléchargement du profil de publication du site Web")

    *Téléchargement du profil de publication du site Web*
8. Téléchargez le fichier de profil de publication dans un emplacement connu. Dans cet exercice, vous allez apprendre à utiliser ce fichier pour publier une application Web sur Azure à partir de Visual Studio.

    ![Enregistrement du fichier de profil de publication](build-restful-apis-with-aspnet-web-api/_static/image46.png "Enregistrement du profil de publication")

    *Enregistrement du fichier de profil de publication*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tâche 2 : configuration du serveur de base de données

Si votre application utilise des bases de données SQL Server, vous devez créer un serveur SQL Database. Si vous souhaitez déployer une application simple qui n’utilise pas SQL Server vous pouvez ignorer cette tâche.

1. Vous aurez besoin d’un serveur de SQL Database pour stocker la base de données d’application. Vous pouvez afficher les serveurs SQL Database à partir de votre abonnement dans le portail de gestion Azure, dans **bases de données SQL** | **serveurs** | **tableau de bord du serveur**. Si vous n’avez pas de serveur créé, vous pouvez en créer un à l’aide du bouton **Ajouter** de la barre de commandes. Notez le **nom et l’URL du serveur,** ainsi que le nom et le mot de passe de connexion de l’administrateur, car vous les utiliserez dans les tâches suivantes. Ne créez pas encore la base de données, car elle sera créée dans une étape ultérieure.

    ![Tableau de bord du serveur SQL Database](build-restful-apis-with-aspnet-web-api/_static/image47.png "Tableau de bord du serveur SQL Database")

    *Tableau de bord du serveur SQL Database*
2. Dans la tâche suivante, vous allez tester la connexion à la base de données à partir de Visual Studio. pour cette raison, vous devez inclure votre adresse IP locale dans la liste des **adresses IP autorisées**du serveur. Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de l' **adresse IP actuelle du client** et collez-la dans les zones de texte **adresse IP de début** et **adresse IP de fin** , puis cliquez sur le bouton ![ajouter-client-IP-adresse-OK-button](build-restful-apis-with-aspnet-web-api/_static/image48.png).

    ![Ajout de l’adresse IP du client](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Ajout de l’adresse IP du client*
3. Une fois l' **adresse IP du client** ajoutée à la liste des adresses IP autorisées, cliquez sur **Enregistrer** pour confirmer les modifications.

    ![Confirmer les modifications](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Confirmer les modifications*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tâche 3 : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

1. Revenez à la solution ASP.NET MVC 4. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet de site Web et sélectionnez **publier**.

    ![Publication de l’application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publication de l'application")

    *Publication du site Web*
2. Importez le profil de publication que vous avez enregistré dans la première tâche.

    ![Importation du profil de publication](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importation du profil de publication")

    *Importation du profil de publication*
3. Cliquez sur **valider la connexion**. Une fois la validation terminée, cliquez sur **suivant**.

    > [!NOTE]
    > La validation est terminée une fois que vous voyez une coche verte s’affiche en regard du bouton valider la connexion.

    ![Validation de la connexion](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validation de la connexion")

    *Validation de la connexion*
4. Dans la page **paramètres** , sous la section **bases de données** , cliquez sur le bouton en regard de la zone de texte de votre connexion à la base de données (par exemple, **DefaultConnection**).

    ![Configuration de Web Deploy](build-restful-apis-with-aspnet-web-api/_static/image54.png "Configuration de Web Deploy")

    *Configuration de Web Deploy*
5. Configurez la connexion de base de données comme suit :

   - Dans **nom du serveur** , tapez l’URL de votre serveur SQL Database à l’aide du préfixe *TCP :* .
   - Dans **nom d’utilisateur** , tapez le nom de connexion de l’administrateur du serveur.
   - Dans **mot de passe** , tapez le mot de passe de connexion de l’administrateur du serveur.
   - Tapez un nouveau nom de base de données, par exemple : *MVC4SampleDB*.

     ![Configuration de la chaîne de connexion de destination](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuration de la chaîne de connexion de destination")

     *Configuration de la chaîne de connexion de destination*
6. Cliquez ensuite sur **OK**. Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.

    ![Création de la base de données](build-restful-apis-with-aspnet-web-api/_static/image56.png "Création de la chaîne de base de données")

    *Création de la base de données*
7. La chaîne de connexion que vous allez utiliser pour vous connecter à SQL Database dans Windows Azure est affichée dans la zone de texte connexion par défaut. Cliquez ensuite sur **Suivant**.

    ![Chaîne de connexion pointant vers SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Chaîne de connexion pointant vers SQL Database")

    *Chaîne de connexion pointant vers SQL Database*
8. Dans la page **Aperçu** , cliquez sur **publier**.

    ![Publication de l’application Web](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publication de l’application Web")

    *Publication de l’application Web*
9. Une fois le processus de publication terminé, votre navigateur par défaut ouvre le site Web publié.

    ![Application publiée sur Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application publiée sur Windows Azure")

    *Application publiée sur Azure*
