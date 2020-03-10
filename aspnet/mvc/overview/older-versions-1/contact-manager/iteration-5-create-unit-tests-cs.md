---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
title: '#5 d’itération – créer des testsC#unitaires () | Microsoft Docs'
author: microsoft
description: Dans la cinquième itération, nous rendons notre application plus facile à gérer et à modifier en ajoutant des tests unitaires. Nous imitons nos classes de modèle de données et créons des tests unitaires pour o...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 28ad8f80-b8a5-444e-b478-8b15a846060c
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
msc.type: authoredcontent
ms.openlocfilehash: 32e81cce34a0e0b1f6b01934334e1b66dce89651
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544302"
---
# <a name="iteration-5--create-unit-tests-c"></a>#5 d’itération – créer des testsC#unitaires ()

par [Microsoft](https://github.com/microsoft)

[Télécharger le code](iteration-5-create-unit-tests-cs/_static/contactmanager_5_cs1.zip)

> Dans la cinquième itération, nous rendons notre application plus facile à gérer et à modifier en ajoutant des tests unitaires. Nous imitons nos classes de modèle de données et créons des tests unitaires pour nos contrôleurs et la logique de validation.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Création d’une application MVC ASP.NET de gestionC#des contacts ()

Dans cette série de didacticiels, nous créons une application de gestion de contacts entière du début à la fin. L’application Gestionnaire de contacts vous permet de stocker les informations de contact, les noms de téléphone et les adresses de messagerie, pour obtenir la liste des personnes.

Nous générons l’application sur plusieurs itérations. À chaque itération, nous améliorons progressivement l’application. L’objectif de cette approche à plusieurs itérations est de vous permettre de comprendre la raison de chaque modification.

- #1 d’itération : créez l’application. Dans la première itération, nous créons le gestionnaire de contacts de la manière la plus simple possible. Nous ajoutons la prise en charge des opérations de base de données de base : créer, lire, mettre à jour et supprimer (CRUD).

- Itération #2 : rendez l’application agréable. Dans cette itération, nous améliorons l’apparence de l’application en modifiant la page maître par défaut de la vue MVC ASP.NET et la feuille de style en cascade.

- Itération #3 : ajouter une validation de formulaire. Dans la troisième itération, nous ajoutons la validation de base du formulaire. Nous empêchons les utilisateurs de soumettre un formulaire sans remplir les champs de formulaire requis. Nous validons également les adresses de messagerie et les numéros de téléphone.

- Itération #4 : rendez l’application faiblement couplée. Dans cette quatrième itération, nous tirant parti de plusieurs modèles de conception de logiciels pour faciliter la gestion et la modification de l’application de gestionnaire de contacts. Par exemple, nous refactorisons notre application pour utiliser le modèle de référentiel et le modèle d’injection de dépendances.

- #5 d’itération-créer des tests unitaires. Dans la cinquième itération, nous rendons notre application plus facile à gérer et à modifier en ajoutant des tests unitaires. Nous imitons nos classes de modèle de données et créons des tests unitaires pour nos contrôleurs et la logique de validation.

- Itération #6-Utilisez le développement piloté par les tests. Dans cette sixième itération, nous ajoutons de nouvelles fonctionnalités à notre application en écrivant d’abord des tests unitaires et en écrivant du code sur les tests unitaires. Dans cette itération, nous ajoutons des groupes de contacts.

- #7 d’itération-ajoutez des fonctionnalités AJAX. Dans la septième itération, nous améliorons la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.

## <a name="this-iteration"></a>Cette itération

Dans l’itération précédente de l’application du gestionnaire de contacts, nous avons refactorisé l’application pour qu’elle soit plus faiblement couplée. Nous avons séparé l’application en différentes couches de contrôleur, de service et de référentiel. Chaque couche interagit avec la couche sous-jacente via des interfaces.

Nous avons refactorisé l’application pour rendre l’application plus facile à gérer et à modifier. Par exemple, si nous avons besoin d’utiliser une nouvelle technologie d’accès aux données, nous pouvons simplement modifier la couche de référentiel sans toucher la couche de contrôleur ou de service. En rendant le gestionnaire de contacts faiblement couplé, nous avons rendu l’application plus résiliente à la modification.

Mais, que se passe-t-il quand nous devons ajouter une nouvelle fonctionnalité à l’application du gestionnaire de contacts ? Ou bien, que se passe-t-il quand nous corrigeons un bogue ? Une vérité, mais bien éprouvée, d’écriture de code est que chaque fois que vous touchez du code, vous créez le risque d’introduire de nouveaux bogues.

Par exemple, un jour précis, votre responsable peut vous demander d’ajouter une nouvelle fonctionnalité au gestionnaire de contacts. Elle souhaite que vous ajoutiez la prise en charge des groupes de contacts. Elle souhaite que les utilisateurs puissent organiser leurs contacts en groupes tels que Friends, Business, etc.

Pour implémenter cette nouvelle fonctionnalité, vous devez modifier les trois couches de l’application du gestionnaire de contacts. Vous devez ajouter de nouvelles fonctionnalités aux contrôleurs, à la couche de service et au référentiel. Dès que vous commencez à modifier le code, vous risquez de perturber les fonctionnalités qui fonctionnaient avant.

La refactorisation de notre application en couches distinctes, comme nous l’avons fait dans l’itération précédente, était une bonne chose. C’est une bonne chose, car cela nous permet d’apporter des modifications à des couches entières sans toucher au reste de l’application. Toutefois, si vous souhaitez faciliter la gestion et la modification du code dans une couche, vous devez créer des tests unitaires pour le code.

Vous utilisez un test unitaire pour tester une unité individuelle de code. Ces unités de code sont plus petites que les couches d’application entières. En général, vous utilisez un test unitaire pour vérifier si une méthode particulière de votre code se comporte comme prévu. Par exemple, vous créez un test unitaire pour la méthode CreateContact () exposée par la classe ContactManagerService.

Les tests unitaires d’une application fonctionnent comme un filet de sécurité. Chaque fois que vous modifiez du code dans une application, vous pouvez exécuter un ensemble de tests unitaires pour vérifier si la modification rompt les fonctionnalités existantes. Les tests unitaires permettent de modifier votre code en toute sécurité. Les tests unitaires rendent l’ensemble du code de votre application plus résilient pour la modification.

Dans cette itération, nous ajoutons des tests unitaires à notre application de gestion des contacts. Ainsi, dans l’itération suivante, nous pouvons ajouter des groupes de contacts à notre application sans vous soucier de la rupture des fonctionnalités existantes.

> [!NOTE] 
> 
> Il existe une variété de frameworks de tests unitaires, notamment NUnit, xUnit.net et MbUnit. Dans ce didacticiel, nous utilisons le Framework de tests unitaires inclus dans Visual Studio. Toutefois, vous pouvez tout aussi facilement utiliser l’un de ces autres frameworks.

## <a name="what-gets-tested"></a>Ce qui est testé

Dans le monde parfait, tout votre code serait couvert par les tests unitaires. Dans le monde parfait, vous auriez le filet de sécurité parfait. Vous pouvez modifier n’importe quelle ligne de code dans votre application et le savoir instantanément, en exécutant vos tests unitaires, si la modification a enfreint les fonctionnalités existantes.

Toutefois, nous ne vivons pas dans un monde parfait. Dans la pratique, lors de l’écriture de tests unitaires, vous vous concentrez sur l’écriture de tests pour votre logique métier (par exemple, la logique de validation). En particulier, vous *n’écrivez pas* de tests unitaires pour votre logique d’accès aux données ou votre logique d’affichage.

Pour être utile, les tests unitaires doivent s’exécuter très rapidement. Vous pouvez facilement accumuler des centaines (voire des milliers) de tests unitaires pour une application. Si l’exécution des tests unitaires prend beaucoup de temps, vous ne devez pas les exécuter. En d’autres termes, les tests unitaires de longue durée ne sont pas utilisables à des fins de codage quotidien.

Pour cette raison, vous n’écrivez généralement pas de tests unitaires pour le code qui interagit avec une base de données. L’exécution de centaines de tests unitaires sur une base de données active serait trop lente. Au lieu de cela, vous simulez votre base de données et écrivez du code qui interagit avec la base de données fictive (nous aborderons la simulation d’une base de données ci-dessous).

Pour une raison similaire, vous n’écrivez généralement pas de tests unitaires pour les vues. Pour tester une vue, vous devez créer un serveur Web. Étant donné que la mise en place d’un serveur Web est un processus relativement lent, il n’est pas recommandé de créer des tests unitaires pour vos vues.

Si votre vue contient une logique complexe, vous devez envisager de déplacer la logique dans les méthodes d’assistance. Vous pouvez écrire des tests unitaires pour les méthodes d’assistance qui s’exécutent sans tourner un serveur Web.

> [!NOTE] 
> 
> L’écriture de tests pour la logique d’accès aux données ou la logique de vue n’est pas une bonne idée lors de l’écriture de tests unitaires, ces tests peuvent être très utiles lors de la création de tests fonctionnels ou d’intégration.

> [!NOTE] 
> 
> ASP.NET MVC est le moteur d’affichage Web Forms. Tandis que le moteur d’affichage Web Forms dépend d’un serveur Web, d’autres moteurs d’affichage peuvent ne pas être.

## <a name="using-a-mock-object-framework"></a>Utilisation d’une infrastructure d’objets factices

Lors de la génération de tests unitaires, vous devez presque toujours tirer parti d’une infrastructure d’objets factices. Une infrastructure d’objets factices vous permet de créer des simulacres et des stubs pour les classes de votre application.

Par exemple, vous pouvez utiliser une infrastructure d’objets factices pour générer une version factice de votre classe de référentiel. De cette façon, vous pouvez utiliser la classe de référentiel factice au lieu de la classe de référentiel réelle dans vos tests unitaires. L’utilisation du référentiel fictif vous permet d’éviter l’exécution de code de base de données lors de l’exécution d’un test unitaire.

Visual Studio n’inclut pas d’infrastructure d’objets factices. Toutefois, il existe plusieurs infrastructures d’objets fictifs commerciales et open source disponibles pour le .NET Framework :

1. MOQ : ce Framework est disponible sous licence Open Source BSD. Vous pouvez télécharger MOQ à partir de [https://code.google.com/p/moq/](https://code.google.com/p/moq/).
2. Rhino Mocks : cette infrastructure est disponible sous licence Open Source BSD. Vous pouvez télécharger des simulacres Rhino à partir de [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx).
3. Isolateur Typemock : il s’agit d’un Framework commercial. Vous pouvez télécharger une version d’évaluation à partir de [http://www.typemock.com/](http://www.typemock.com/).

Dans ce didacticiel, j’ai décidé d’utiliser MOQ. Toutefois, vous pouvez tout aussi facilement utiliser les simulacres Rhino ou isolateur Typemock pour créer les objets factices pour l’application du gestionnaire de contacts.

Avant de pouvoir utiliser MOQ, vous devez effectuer les étapes suivantes :

1. .
2. Avant de décompresser le téléchargement, assurez-vous de cliquer avec le bouton droit sur le fichier et de cliquer sur le bouton **débloquer** (voir figure 1).
3. Décompressez le téléchargement.
4. Ajoutez une référence à l’assembly MOQ en cliquant avec le bouton droit sur le dossier références dans le projet ContactManager. tests et en sélectionnant **Ajouter une référence**. Sous l’onglet Parcourir, accédez au dossier dans lequel vous avez décompressé MOQ et sélectionnez l’assembly MOQ. dll. Cliquez sur le bouton **OK** .
5. Une fois ces étapes effectuées, votre dossier References doit ressembler à la figure 2.

[![de déblocage de MOQ](iteration-5-create-unit-tests-cs/_static/image1.jpg)](iteration-5-create-unit-tests-cs/_static/image1.png)

**Figure 01**: déblocage de MOQ ([cliquez pour afficher l’image en taille réelle](iteration-5-create-unit-tests-cs/_static/image2.png))

[![les références après l’ajout de MOQ](iteration-5-create-unit-tests-cs/_static/image2.jpg)](iteration-5-create-unit-tests-cs/_static/image3.png)

**Figure 02**: références après l’ajout de MOQ ([cliquez pour afficher l’image en taille réelle](iteration-5-create-unit-tests-cs/_static/image4.png))

## <a name="creating-unit-tests-for-the-service-layer"></a>Création de tests unitaires pour la couche de service

Commençons par créer un ensemble de tests unitaires pour notre couche de service d’application du gestionnaire de contacts. Nous allons utiliser ces tests pour vérifier notre logique de validation.

Créez un dossier nommé Models dans le projet ContactManager. tests. Ensuite, cliquez avec le bouton droit sur le dossier modèles, puis sélectionnez **Ajouter, nouveau test**. La boîte de dialogue **Ajouter un nouveau test** illustrée à la figure 3 s’affiche. Sélectionnez le modèle de **test unitaire** et nommez votre nouveau test ContactManagerServiceTest.cs. Cliquez sur le bouton **OK** pour ajouter votre nouveau test à votre projet de test.

> [!NOTE] 
> 
> En général, vous souhaitez que la structure de dossiers de votre projet de test corresponde à la structure de dossiers de votre projet MVC ASP.NET. Par exemple, vous placez des tests de contrôleur dans un dossier contrôleurs, des tests de modèle dans un dossier Models, et ainsi de suite.

[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-cs/_static/image3.jpg)](iteration-5-create-unit-tests-cs/_static/image5.png)

**Figure 03**: Models\ContactManagerServiceTest.cs ([cliquez pour afficher l’image en taille réelle](iteration-5-create-unit-tests-cs/_static/image6.png))

Au départ, nous souhaitons tester la méthode CreateContact () exposée par la classe ContactManagerService. Nous allons créer les cinq tests suivants :

- CreateContact ()-teste que CreateContact () retourne la valeur true lorsqu’un contact valide est passé à la méthode.
- CreateContactRequiredFirstName ()-vérifie qu’un message d’erreur est ajouté à l’état du modèle lorsqu’un contact avec un prénom manquant est passé à la méthode CreateContact ().
- CreateContactRequiredLastName ()-vérifie qu’un message d’erreur est ajouté à l’état du modèle lorsqu’un contact avec un nom manquant est passé à la méthode CreateContact ().
- CreateContactInvalidPhone ()-vérifie qu’un message d’erreur est ajouté à l’état du modèle lorsqu’un contact avec un numéro de téléphone non valide est passé à la méthode CreateContact ().
- CreateContactInvalidEmail ()-vérifie qu’un message d’erreur est ajouté à l’état du modèle lorsqu’un contact avec une adresse de messagerie non valide est passé à la méthode CreateContact ().

Le premier test vérifie qu’un contact valide ne génère pas d’erreur de validation. Les tests restants vérifient chacune des règles de validation.

Le code de ces tests est contenu dans la liste 1.

**Liste 1-Models\ContactManagerServiceTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample1.cs)]

Étant donné que nous utilisons la classe contact dans la liste 1, nous devons ajouter une référence à l’Entity Framework Microsoft à notre projet de test. Ajoutez une référence à l’assembly System. Data. Entity.

La liste 1 contient une méthode nommée Initialize () qui est décorée avec l’attribut [TestInitialize]. Cette méthode est appelée automatiquement avant l’exécution de chacun des tests unitaires (elle est appelée 5 fois juste avant chacun des tests unitaires). La méthode Initialize () crée un référentiel fictif avec la ligne de code suivante :

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample2.cs)]

Cette ligne de code utilise l’infrastructure MOQ pour générer un référentiel fictif à partir de l’interface IContactManagerRepository. Le référentiel fictif est utilisé à la place du EntityContactManagerRepository réel pour éviter d’accéder à la base de données lors de l’exécution de chaque test unitaire. Le référentiel fictif implémente les méthodes de l’interface IContactManagerRepository, mais les méthodes ne font rien.

> [!NOTE] 
> 
> Lors de l’utilisation de l’infrastructure MOQ, il existe une distinction entre \_mockRepository et \_mockRepository. Object. Le premier fait référence à la classe factice&lt;IContactManagerRepository&gt; qui contient des méthodes pour spécifier la façon dont le référentiel factice se comportera. Ce dernier fait référence au référentiel fictif réel qui implémente l’interface IContactManagerRepository.

Le référentiel fictif est utilisé dans la méthode Initialize () lors de la création d’une instance de la classe ContactManagerService. Tous les tests unitaires utilisent cette instance de la classe ContactManagerService.

La liste 1 contient cinq méthodes qui correspondent à chacun des tests unitaires. Chacune de ces méthodes est décorée avec l’attribut [TestMethod]. Lorsque vous exécutez les tests unitaires, toute méthode ayant cet attribut est appelée. En d’autres termes, toute méthode décorée avec l’attribut [TestMethod] est un test unitaire.

Le premier test unitaire, nommé CreateContact (), vérifie que l’appel de CreateContact () retourne la valeur true lorsqu’une instance valide de la classe contact est passée à la méthode. Le test crée une instance de la classe contact, appelle la méthode CreateContact () et vérifie que CreateContact () retourne la valeur true.

Les tests restants vérifient que lorsque la méthode CreateContact () est appelée avec un contact non valide, la méthode retourne false et le message d’erreur de validation attendu est ajouté à l’état du modèle. Par exemple, le test CreateContactRequiredFirstName () crée une instance de la classe contact avec une chaîne vide pour sa propriété FirstName. Ensuite, la méthode CreateContact () est appelée avec le contact non valide. Enfin, le test vérifie que CreateContact () retourne la valeur false et que l’état du modèle contient le message d’erreur de validation attendu « First Name is required ».

Vous pouvez exécuter les tests unitaires dans la liste 1 en sélectionnant l’option de menu **test, exécuter, tous les tests de la solution (Ctrl + R, A)** . Les résultats des tests s’affichent dans la fenêtre Résultats des tests (voir figure 4).

[![Résultats des tests](iteration-5-create-unit-tests-cs/_static/image4.jpg)](iteration-5-create-unit-tests-cs/_static/image7.png)

**Figure 04**: résultats des tests ([cliquez pour afficher l’image en taille réelle](iteration-5-create-unit-tests-cs/_static/image8.png))

## <a name="creating-unit-tests-for-controllers"></a>Création de tests unitaires pour les contrôleurs

L’application ASP. NETMVC contrôle le Flow de l’interaction de l’utilisateur. Lors du test d’un contrôleur, vous souhaitez tester si le contrôleur retourne le résultat de l’action appropriée et afficher les données. Vous pouvez également tester si un contrôleur interagit avec les classes de modèle de la manière attendue.

Par exemple, la liste 2 contient deux tests unitaires pour la méthode Contact Controller Create (). Le premier test unitaire vérifie que lorsqu’un contact valide est passé à la méthode Create (), la méthode Create () effectue une redirection vers l’action d’index. En d’autres termes, lorsqu’un contact valide lui est transmis, la méthode Create () doit retourner un RedirectToRouteResult qui représente l’action d’index.

Nous ne voulons pas tester la couche de service ContactManager lors du test de la couche de contrôleur. Par conséquent, nous imitons la couche de service avec le code suivant dans la méthode Initialize :

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample3.cs)]

Dans le test unitaire CreateValidContact (), nous imitons le comportement de l’appel de la méthode CreateContact () de la couche de service avec la ligne de code suivante :

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample4.cs)]

Cette ligne de code fait en sorte que le service ContactManager factice retourne la valeur true lorsque sa méthode CreateContact () est appelée. En simulant la couche de service, nous pouvons tester le comportement de notre contrôleur sans avoir à exécuter de code dans la couche de service.

Le deuxième test unitaire vérifie que l’action Create () retourne la vue Create quand un contact non valide est passé à la méthode. Nous faisons en sorte que la méthode CreateContact () de la couche de service retourne la valeur false avec la ligne de code suivante :

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample5.cs)]

Si la méthode Create () se comporte comme prévu, elle doit retourner la vue Create lorsque la couche de service renvoie la valeur false. De cette façon, le contrôleur peut afficher les messages d’erreur de validation dans la vue Create et l’utilisateur a la possibilité de corriger les propriétés de contact non valides.

Si vous envisagez de créer des tests unitaires pour vos contrôleurs, vous devez retourner des noms de vue explicites à partir des actions de votre contrôleur. Par exemple, ne retournez pas un affichage de la façon suivante :

retourne View ();

Au lieu de cela, retournez la vue comme suit :

Return View (« Create »);

Si vous n’êtes pas explicite lors du retour d’une vue, la propriété ViewResult. ViewName retourne une chaîne vide.

**Liste 2-Controllers\ContactControllerTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample6.cs)]

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons créé des tests unitaires pour notre application de gestionnaire de contacts. Nous pouvons exécuter ces tests unitaires à tout moment pour vérifier que notre application se comporte toujours comme prévu. Les tests unitaires jouent le rôle de filet de sécurité pour notre application, ce qui nous permet de modifier votre application à l’avenir en toute sécurité.

Nous avons créé deux jeux de tests unitaires. Tout d’abord, nous avons testé notre logique de validation en créant des tests unitaires pour notre couche de service. Nous avons ensuite testé notre logique de contrôle de Flow en créant des tests unitaires pour notre couche de contrôleur. Lors du test de notre couche de service, nous avons isolé nos tests de notre couche de service de notre couche de référentiels en imitant notre couche de référentiel. Lors du test de la couche de contrôleur, nous avons isolé nos tests pour notre couche de contrôleur en simulant la couche de service.

Dans l’itération suivante, nous modifions l’application du gestionnaire de contacts afin qu’elle prenne en charge les groupes de contacts. Nous allons ajouter cette nouvelle fonctionnalité à notre application à l’aide d’un processus de conception de logiciels appelé développement piloté par les tests.

> [!div class="step-by-step"]
> [Précédent](iteration-4-make-the-application-loosely-coupled-cs.md)
> [Suivant](iteration-6-use-test-driven-development-cs.md)
