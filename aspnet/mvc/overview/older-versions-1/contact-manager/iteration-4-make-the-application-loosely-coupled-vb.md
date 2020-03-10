---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: '#4 d’itération : rendre l’application faiblement couplée (VB) | Microsoft Docs'
author: microsoft
description: Dans cette quatrième itération, nous tirant parti de plusieurs modèles de conception de logiciels pour faciliter la gestion et la modification de l’application de gestionnaire de contacts. Pré...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: 422c75406d9c08279d0c2224ee4b6db3a71eb1b3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582081"
---
# <a name="iteration-4--make-the-application-loosely-coupled-vb"></a>#4 d’itération : rendre l’application faiblement couplée (VB)

par [Microsoft](https://github.com/microsoft)

[Télécharger le code](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> Dans cette quatrième itération, nous tirant parti de plusieurs modèles de conception de logiciels pour faciliter la gestion et la modification de l’application de gestionnaire de contacts. Par exemple, nous refactorisons notre application pour utiliser le modèle de référentiel et le modèle d’injection de dépendances.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Création d’une application MVC ASP.NET de gestion des contacts (VB)

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

Dans cette quatrième itération de l’application du gestionnaire de contacts, nous refactorisons l’application de façon à ce que l’application soit plus faiblement couplée. Quand une application est faiblement couplée, vous pouvez modifier le code dans une partie de l’application sans avoir à modifier le code dans d’autres parties de l’application. Les applications faiblement couplées sont plus résistantes à la modification.

Actuellement, la logique d’accès aux données et de validation utilisée par l’application du gestionnaire de contacts est contenue dans les classes de contrôleur. C’est une mauvaise idée. Chaque fois que vous devez modifier une partie de votre application, vous risquez d’introduire des bogues dans une autre partie de votre application. Par exemple, si vous modifiez votre logique de validation, vous risquez d’introduire de nouveaux bogues dans votre logique d’accès aux données ou de contrôleur.

> [!NOTE] 
> 
> (SRP), une classe ne doit jamais avoir plus d’une raison de changer. La combinaison de la logique du contrôleur, de la validation et de la base de données est une violation massive du principe de responsabilité unique.

Il existe plusieurs raisons pour lesquelles vous devrez peut-être modifier votre application. Vous devrez peut-être ajouter une nouvelle fonctionnalité à votre application, vous devrez peut-être corriger un bogue dans votre application, ou vous devrez peut-être modifier le mode d’implémentation d’une fonctionnalité de votre application. Les applications sont rarement statiques. Ils ont tendance à croître et à muter au fil du temps.

Imaginez, par exemple, que vous décidez de modifier la façon dont vous implémentez votre couche d’accès aux données. Pour l’instant, l’application du gestionnaire de contacts utilise le Entity Framework Microsoft pour accéder à la base de données. Toutefois, vous pouvez décider de migrer vers une nouvelle ou une autre technologie d’accès aux données, comme ADO.NET Data Services ou NHibernate. Toutefois, étant donné que le code d’accès aux données n’est pas isolé de la validation et du code de contrôleur, il n’existe aucun moyen de modifier le code d’accès aux données dans votre application sans modifier le code qui n’est pas directement lié à l’accès aux données.

En revanche, lorsqu’une application est faiblement couplée, vous pouvez apporter des modifications à une partie d’une application sans toucher aux autres parties d’une application. Par exemple, vous pouvez changer les technologies d’accès aux données sans modifier la logique de validation ou de contrôleur.

Dans cette itération, nous utilisons plusieurs modèles de conception de logiciels qui nous permettent de refactoriser notre application de gestionnaire de contacts dans une application plus faiblement couplée. Une fois que nous avons terminé, le gestionnaire de contacts gagné t fait tout ce qu’il n’a pas fait auparavant. Toutefois, nous pourrons modifier l’application plus facilement à l’avenir.

> [!NOTE] 
> 
> La refactorisation est le processus de réécriture d’une application de sorte qu’elle ne perde aucune fonctionnalité existante.

## <a name="using-the-repository-software-design-pattern"></a>Utilisation du modèle de conception de logiciel de référentiel

Notre première modification consiste à tirer parti d’un modèle de conception de logiciels appelé modèle de référentiel. Nous allons utiliser le modèle de référentiel pour isoler notre code d’accès aux données du reste de notre application.

L’implémentation du modèle de référentiel nous oblige à effectuer les deux étapes suivantes :

1. Créer une interface
2. Créer une classe concrète qui implémente l’interface

Tout d’abord, nous devons créer une interface qui décrit toutes les méthodes d’accès aux données que nous devons effectuer. L’interface IContactManagerRepository est contenue dans la liste 1. Cette interface décrit cinq méthodes : CreateContact (), DeleteContact (), EditContact (), GetContact et ListContacts ().

**Liste 1-Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

Ensuite, nous devons créer une classe concrète qui implémente l’interface IContactManagerRepository. Étant donné que nous utilisons le Entity Framework Microsoft pour accéder à la base de données, nous allons créer une nouvelle classe nommée EntityContactManagerRepository. Cette classe est contenue dans la liste 2.

**Liste 2-Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

Notez que la classe EntityContactManagerRepository implémente l’interface IContactManagerRepository. La classe implémente les cinq méthodes décrites par cette interface.

Vous vous demandez peut-être pourquoi nous devons nous soucier d’une interface. Pourquoi est-ce que nous devons créer une interface et une classe qui l’implémente ?

À une exception près, le reste de notre application interagit avec l’interface et non avec la classe concrète. Au lieu d’appeler les méthodes exposées par la classe EntityContactManagerRepository, nous appelons les méthodes exposées par l’interface IContactManagerRepository.

De cette façon, nous pouvons implémenter l’interface avec une nouvelle classe sans avoir à modifier le reste de notre application. Par exemple, à une date ultérieure, nous pourrions vouloir implémenter une classe DataServicesContactManagerRepository qui implémente l’interface IContactManagerRepository. La classe DataServicesContactManagerRepository peut utiliser ADO.NET Data Services pour accéder à une base de données au lieu de la Entity Framework Microsoft.

Si notre code d’application est programmé sur l’interface IContactManagerRepository au lieu de la classe EntityContactManagerRepository concrète, nous pouvons basculer des classes concrètes sans modifier le reste de notre code. Par exemple, nous pouvons passer de la classe EntityContactManagerRepository à la classe DataServicesContactManagerRepository sans modifier notre logique d’accès aux données ou de validation.

La programmation sur des interfaces (abstractions) au lieu de classes concrètes rend notre application plus résiliente pour la modification.

> [!NOTE] 
> 
> Vous pouvez créer rapidement une interface à partir d’une classe concrète dans Visual Studio en sélectionnant l’option de menu Refactoriser, extraire l’interface. Par exemple, vous pouvez créer la classe EntityContactManagerRepository en premier, puis utiliser l’interface Extract pour générer automatiquement l’interface IContactManagerRepository.

## <a name="using-the-dependency-injection-software-design-pattern"></a>Utilisation du modèle de conception de logiciel d’injection de dépendances

Maintenant que nous avons migré notre code d’accès aux données vers une classe de référentiel distincte, nous devons modifier notre contrôleur de contact pour utiliser cette classe. Nous allons tirer parti d’un modèle de conception de logiciels appelé injection de dépendances pour utiliser la classe de référentiel dans notre contrôleur.

Le contrôleur de contact modifié est contenu dans la liste 3.

**Liste 3-Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

Notez que le contrôleur de contact dans la liste 3 a deux constructeurs. Le premier constructeur passe une instance concrète de l’interface IContactManagerRepository au second constructeur. La classe Contact Controller utilise l' *injection de dépendances de constructeur*.

L’utilisation de la classe EntityContactManagerRepository est celle qui est utilisée dans le premier constructeur. Le reste de la classe utilise l’interface IContactManagerRepository au lieu de la classe EntityContactManagerRepository concrète.

Cela facilite le basculement des implémentations de la classe IContactManagerRepository à l’avenir. Si vous souhaitez utiliser la classe DataServicesContactRepository au lieu de la classe EntityContactManagerRepository, il vous suffit de modifier le premier constructeur.

L’injection de dépendances de constructeur rend également la classe de contrôleur de contact très testable. Dans vos tests unitaires, vous pouvez instancier le contrôleur de contact en passant une implémentation factice de la classe IContactManagerRepository. Cette fonctionnalité de l’injection de dépendances est très importante pour nous lors de la prochaine itération lors de la génération de tests unitaires pour l’application du gestionnaire de contacts.

> [!NOTE] 
> 
> Si vous souhaitez découpler complètement la classe de contrôleur de contact d’une implémentation particulière de l’interface IContactManagerRepository, vous pouvez tirer parti d’une infrastructure qui prend en charge l’injection de dépendances, par exemple StructureMap ou Microsoft Entity Framework (MEF). En tirant parti d’une infrastructure d’injection de dépendances, vous n’avez jamais besoin de faire référence à une classe concrète dans votre code.

## <a name="creating-a-service-layer"></a>Création d’une couche de service

Vous avez peut-être remarqué que notre logique de validation est toujours combinée à la logique de votre contrôleur dans la classe de contrôleur modifiée dans le Listing 3. Pour la même raison qu’il est judicieux d’isoler notre logique d’accès aux données, il est judicieux d’isoler notre logique de validation.

Pour résoudre ce problème, nous pouvons créer une [couche de service](http://martinfowler.com/eaaCatalog/serviceLayer.html)distincte. La couche de service est une couche distincte que nous pouvons insérer entre nos classes de contrôleur et de référentiel. La couche de service contient notre logique métier, y compris l’ensemble de notre logique de validation.

Le ContactManagerService est contenu dans la liste 4. Il contient la logique de validation de la classe Contact Controller.

**Liste 4-Models\ContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

Notez que le constructeur pour ContactManagerService requiert un ValidationDictionary. La couche de service communique avec la couche de contrôleur via ce ValidationDictionary. Nous aborderons le ValidationDictionary en détail dans la section suivante, lorsque nous aborderons le modèle Decorator.

Notez, en outre, que le ContactManagerService implémente l’interface IContactManagerService. Vous devez toujours vous efforcer de programmer des interfaces plutôt que des classes concrètes. Les autres classes de l’application du gestionnaire de contacts n’interagissent pas directement avec la classe ContactManagerService. Au lieu de cela, à une exception près, le reste de l’application du gestionnaire de contacts est programmé en fonction de l’interface IContactManagerService.

L’interface IContactManagerService est contenue dans la liste 5.

**Liste 5-Models\IContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

La classe de contrôleur de contact modifiée est contenue dans la liste 6. Notez que le contrôleur de contact n’interagit plus avec le référentiel ContactManager. Au lieu de cela, le contrôleur de contact interagit avec le service ContactManager. Chaque couche est isolée autant que possible des autres couches.

**Liste 6-Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

Notre application n’exécute plus afoul du principe de responsabilité unique (SRP). Le contrôleur de contact de la liste 6 a été supprimé de toutes les responsabilités autres que le contrôle du déroulement de l’exécution de l’application. Toute la logique de validation a été supprimée du contrôleur de contact et a fait l’objet d’un push dans la couche de service. Toute la logique de la base de données a fait l’objet d’un push dans la couche de référentiel.

## <a name="using-the-decorator-pattern"></a>Utilisation du modèle Decorator

Nous voulons être en mesure de découpler complètement notre couche de service de notre couche de contrôleur. En principe, nous devrions pouvoir compiler notre couche de service dans un assembly distinct de notre couche de contrôleur sans avoir à ajouter une référence à notre application MVC.

Toutefois, notre couche de service doit être en mesure de renvoyer les messages d’erreur de validation à la couche de contrôleur. Comment pouvons-nous autoriser la couche de service à communiquer des messages d’erreur de validation sans coupler le contrôleur et la couche de service ? Nous pouvons tirer parti d’un modèle de conception de logiciels nommé « [Decorator](http://en.wikipedia.org/wiki/Decorator_pattern)».

Un contrôleur utilise un ModelStateDictionary nommé ModelState pour représenter les erreurs de validation. Par conséquent, vous pouvez être tenté de passer ModelState de la couche de contrôleur à la couche de service. Toutefois, l’utilisation de ModelState dans la couche de service rend votre couche de service dépendante d’une fonctionnalité de l’infrastructure MVC ASP.NET. Cela serait incorrect, car, un à un, vous souhaiterez peut-être utiliser la couche de service avec une application WPF au lieu d’une application MVC ASP.NET. Dans ce cas, vous ne souhaitez pas référencer l’infrastructure MVC ASP.NET pour utiliser la classe ModelStateDictionary.

Le modèle Decorator vous permet d’encapsuler une classe existante dans une nouvelle classe afin d’implémenter une interface. Notre projet de gestionnaire de contacts comprend la classe ModelStateWrapper contenue dans le Listing 7. La classe ModelStateWrapper implémente l’interface dans la liste 8.

**Liste 7-Models\Validation\ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**Liste 8-Models\Validation\IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

Si vous jetez un coup d’œil à la liste 5, vous verrez que la couche de service ContactManager utilise l’interface IValidationDictionary exclusivement. Le service ContactManager n’est pas dépendant de la classe ModelStateDictionary. Lorsque le contrôleur de contact crée le service ContactManager, le contrôleur encapsule son ModelState comme suit :

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous n’avons pas ajouté de nouvelles fonctionnalités à l’application du gestionnaire de contacts. L’objectif de cette itération était de refactoriser l’application du gestionnaire de contacts pour qu’elle soit plus facile à gérer et à modifier.

Tout d’abord, nous avons implémenté le modèle de conception de logiciel de référentiel. Nous avons migré tout le code d’accès aux données vers une classe de référentiel ContactManager distincte.

Nous avons également isolé notre logique de validation de notre logique de contrôleur. Nous avons créé une couche de service distincte contenant l’ensemble de notre code de validation. La couche de contrôleur interagit avec la couche de service, et la couche de service interagit avec la couche de référentiel.

Lorsque nous avons créé la couche de service, nous avons tiré parti du modèle Decorator pour isoler ModelState de notre couche de service. Dans notre couche de service, nous avons programmé avec l’interface IValidationDictionary au lieu de ModelState.

Enfin, nous avons tiré parti d’un modèle de conception de logiciels appelé modèle d’injection de dépendances. Ce modèle nous permet de programmer des interfaces (abstractions) au lieu de classes concrètes. L’implémentation du modèle de conception d’injection de dépendances rend également notre code plus testable. Dans l’itération suivante, nous ajoutons des tests unitaires à notre projet.

> [!div class="step-by-step"]
> [Précédent](iteration-3-add-form-validation-vb.md)
> [Suivant](iteration-5-create-unit-tests-vb.md)
