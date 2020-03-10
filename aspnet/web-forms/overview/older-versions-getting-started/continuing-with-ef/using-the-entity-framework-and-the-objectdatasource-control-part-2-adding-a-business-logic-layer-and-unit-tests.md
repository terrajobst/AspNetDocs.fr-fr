---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Utilisation de l’Entity Framework 4,0 et du contrôle ObjectDataSource, partie 2 : ajout d’une couche de logique métier et de tests unitaires | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le Prise en main avec la série de didacticiels Entity Framework 4,0. ...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 24344cc33d7c26d7c408db26c0530ef2c708a7d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546766"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Utilisation de l’Entity Framework 4,0 et du contrôle ObjectDataSource, partie 2 : ajout d’une couche de logique métier et de tests unitaires

par [Tom Dykstra](https://github.com/tdykstra)

> Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le [prise en main avec la](https://asp.net/entity-framework/tutorials#Getting%20Started) série de didacticiels Entity Framework 4,0. Si vous n’avez pas terminé les didacticiels précédents, comme point de départ pour ce didacticiel, vous pouvez [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous auriez créée. Vous pouvez également [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créée par la série de didacticiels complète. Si vous avez des questions sur les didacticiels, vous pouvez les poster sur le [Forum de Entity Framework ASP.net](https://forums.asp.net/1227.aspx).

Dans le didacticiel précédent, vous avez créé une application Web multiniveau à l’aide de la Entity Framework et du contrôle `ObjectDataSource`. Ce didacticiel montre comment ajouter une logique métier tout en conservant la couche logique métier (BLL) et la couche d’accès aux données (DAL) séparément, et indique comment créer des tests unitaires automatisés pour la couche BLL.

Dans ce didacticiel, vous allez effectuer les tâches suivantes :

- Créez une interface de référentiel qui déclare les méthodes d’accès aux données dont vous avez besoin.
- Implémentez l’interface de référentiel dans la classe de référentiel.
- Créez une classe de logique métier qui appelle la classe de référentiel pour exécuter des fonctions d’accès aux données.
- Connectez le contrôle `ObjectDataSource` à la classe de logique métier au lieu de la classe de référentiel.
- Créez un projet de test unitaire et une classe de référentiel qui utilise des collections en mémoire pour son magasin de données.
- Créez un test unitaire pour la logique métier que vous souhaitez ajouter à la classe de logique métier, puis exécutez le test et observez l’échec.
- Implémentez la logique métier dans la classe de logique métier, puis réexécutez le test unitaire et consultez-le Pass.

Vous utiliserez les pages *Departments. aspx* et *DepartmentsAdd. aspx* que vous avez créées dans le didacticiel précédent.

## <a name="creating-a-repository-interface"></a>Création d’une interface de référentiel

Vous commencerez par créer l’interface de référentiel.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

Dans le dossier *dal* , créez un nouveau fichier de classe, nommez-le *ISchoolRepository.cs*, puis remplacez le code existant par le code suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

L’interface définit une méthode pour chacune des méthodes CRUD (création, lecture, mise à jour, suppression) que vous avez créées dans la classe de référentiel.

Dans la classe `SchoolRepository` dans *SchoolRepository.cs*, indiquez que cette classe implémente l’interface `ISchoolRepository` :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Création d’une classe de logique métier

Ensuite, vous allez créer la classe de logique métier. Pour cela, vous pouvez ajouter la logique métier qui sera exécutée par le contrôle `ObjectDataSource`, même si vous ne le faites pas encore. Pour le moment, la nouvelle classe de logique métier effectuera uniquement les mêmes opérations CRUD que le référentiel.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Créez un nouveau dossier et nommez-le *BLL*. (Dans une application réelle, la couche logique métier est généralement implémentée en tant que bibliothèque de classes : un projet distinct, mais pour conserver ce didacticiel simple, les classes BLL sont conservées dans un dossier de projet.)

Dans le dossier *BLL* , créez un nouveau fichier de classe, nommez-le *SchoolBL.cs*, puis remplacez le code existant par le code suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Ce code crée les mêmes méthodes CRUD que celles que vous avez vues précédemment dans la classe de référentiel, mais au lieu d’accéder directement aux méthodes Entity Framework, il appelle les méthodes de la classe de référentiel.

La variable de classe qui contient une référence à la classe de référentiel est définie comme un type d’interface et le code qui instancie la classe de référentiel est contenu dans deux constructeurs. Le constructeur sans paramètre sera utilisé par le contrôle `ObjectDataSource`. Il crée une instance de la classe `SchoolRepository` que vous avez créée précédemment. L’autre constructeur autorise tout code qui instancie la classe de logique métier à passer dans tout objet qui implémente l’interface de référentiel.

Les méthodes CRUD qui appellent la classe de référentiel et les deux constructeurs permettent d’utiliser la classe de logique métier avec n’importe quel magasin de données principal que vous choisissez. La classe de logique métier n’a pas besoin d’être conscient de la façon dont la classe qu’elle appelle rend persistantes les données. (Cela est souvent appelé *ignorance de la persistance*.) Cela facilite les tests unitaires, car vous pouvez connecter la classe de logique métier à une implémentation de référentiel qui utilise des éléments aussi simples que les collections en mémoire `List` pour stocker des données.

> [!NOTE]
> Techniquement, les objets d’entité n’ignorent toujours pas la persistance, car ils sont instanciés à partir des classes qui héritent de la classe `EntityObject` de l’Entity Framework. Pour l’ignorance de la persistance complète, vous pouvez utiliser des *objets CLR ordinaires*, ou *poco*, à la place des objets qui héritent de la classe `EntityObject`. L’utilisation des POCO n’entre pas dans le cadre de ce didacticiel. Pour plus d’informations, consultez [testabilité et Entity Framework 4,0](https://msdn.microsoft.com/library/ff714955.aspx) sur le site Web MSDN.)

À présent, vous pouvez connecter les contrôles de `ObjectDataSource` à la classe de logique métier plutôt qu’au référentiel et vérifier que tout fonctionne comme avant.

Dans *Departments. aspx* et *DepartmentsAdd. aspx*, remplacez chaque occurrence de `TypeName="ContosoUniversity.DAL.SchoolRepository"` par `TypeName="ContosoUniversity.BLL.SchoolBL`». (Il y a quatre instances dans tout.)

Exécutez les pages *Departments. aspx* et *DepartmentsAdd. aspx* pour vérifier qu’elles fonctionnent toujours comme avant.

[![image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Création d’un projet de test unitaire et d’une implémentation de référentiel

Ajoutez un nouveau projet à la solution à l’aide du modèle de **projet de test** , puis nommez-le `ContosoUniversity.Tests`.

Dans le projet de test, ajoutez une référence à `System.Data.Entity` et ajoutez une référence de projet au projet `ContosoUniversity`.

Vous pouvez maintenant créer la classe de référentiel que vous allez utiliser avec les tests unitaires. Le magasin de données pour ce dépôt sera dans la classe.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

Dans le projet de test, créez un nouveau fichier de classe, nommez-le *MockSchoolRepository.cs*, puis remplacez le code existant par le code suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Cette classe de référentiel a les mêmes méthodes CRUD que celle qui accède directement au Entity Framework, mais elles fonctionnent avec `List` collections en mémoire et non avec une base de données. Il est ainsi plus facile pour une classe de test de configurer et de valider des tests unitaires pour la classe de logique métier.

## <a name="creating-unit-tests"></a>Création de tests unitaires

Le modèle de projet de **test** a créé une classe de test unitaire stub pour vous, et la tâche suivante consiste à modifier cette classe en y ajoutant des méthodes de test unitaire pour la logique métier que vous souhaitez ajouter à la classe de logique métier.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Chez Contoso University, tout formateur individuel ne peut être administrateur que d’un seul service et vous devez ajouter une logique métier pour appliquer cette règle. Vous allez commencer par ajouter des tests et exécuter les tests pour les voir échouer. Vous allez ensuite ajouter le code et réexécuter les tests, en attendant de les voir passer.

Ouvrez le fichier *UnitTest1.cs* et ajoutez `using` instructions pour la logique métier et les couches d’accès aux données que vous avez créées dans le projet ContosoUniversity :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Remplacez la méthode `TestMethod1` par les méthodes suivantes :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

La méthode `CreateSchoolBL` crée une instance de la classe de référentiel que vous avez créée pour le projet de test unitaire, qu’elle passe ensuite à une nouvelle instance de la classe logique métier. La méthode utilise ensuite la classe de logique métier pour insérer trois services que vous pouvez utiliser dans les méthodes de test.

Les méthodes de test vérifient que la classe de logique métier lève une exception si quelqu’un tente d’insérer un nouveau service avec le même administrateur qu’un service existant, ou si quelqu’un tente de mettre à jour l’administrateur d’un service en lui attribuant l’ID d’une personne qui est déjà l’administrateur d’un autre service.

Vous n’avez pas encore créé la classe d’exception, ce code ne sera donc pas compilé. Pour le faire, cliquez avec le bouton droit sur `DuplicateAdministratorException`, puis sélectionnez **générer**, puis **classe**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Cela crée une classe dans le projet de test que vous pouvez supprimer après avoir créé la classe d’exception dans le projet principal. et implémenté la logique métier.

Exécutez le projet de test. Comme prévu, les tests échouent.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Ajout d’une logique métier pour effectuer un test

Ensuite, vous allez implémenter la logique métier qui rend impossible de définir en tant qu’administrateur d’un service une personne qui est déjà administrateur d’un autre service. Vous allez lever une exception à partir de la couche de logique métier, puis l’intercepter dans la couche de présentation si un utilisateur modifie un service et clique sur **mettre à jour** après avoir sélectionné une personne qui est déjà administrateur. (Vous pouvez également supprimer des formateurs de la liste déroulante qui sont déjà administrateurs avant le rendu de la page, mais l’objectif ici est de travailler avec la couche logique métier.)

Commencez par créer la classe d’exception que vous allez lever lorsqu’un utilisateur tente de faire d’un formateur l’administrateur de plusieurs départements. Dans le projet principal, créez un nouveau fichier de classe dans le dossier *BLL* , nommez-le *DuplicateAdministratorException.cs*et remplacez le code existant par le code suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Maintenant, supprimez le fichier *DuplicateAdministratorException.cs* temporaire que vous avez créé précédemment dans le projet de test afin de pouvoir le compiler.

Dans le projet principal, ouvrez le fichier *SchoolBL.cs* et ajoutez la méthode suivante qui contient la logique de validation. (Le code fait référence à une méthode que vous allez créer ultérieurement.)

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Vous appellerez cette méthode lors de l’insertion ou de la mise à jour de `Department` entités afin de vérifier si un autre service a déjà le même administrateur.

Le code appelle une méthode pour rechercher dans la base de données une entité `Department` ayant la même valeur de propriété `Administrator` que l’entité insérée ou mise à jour. Si un est trouvé, le code lève une exception. Aucun contrôle de validation n’est nécessaire si l’entité qui est insérée ou mise à jour n’a pas de valeur `Administrator`, et si aucune exception n’est levée si la méthode est appelée pendant une mise à jour et que l’entité `Department` trouvée correspond à la `Department` entité en cours de mise à jour.

Appelez la nouvelle méthode à partir des méthodes `Insert` et `Update` :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

Dans *ISchoolRepository.cs*, ajoutez la déclaration suivante pour la nouvelle méthode d’accès aux données :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

Dans *SchoolRepository.cs*, ajoutez l’instruction `using` suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

Dans *SchoolRepository.cs*, ajoutez la nouvelle méthode d’accès aux données suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Ce code récupère `Department` entités qui ont un administrateur spécifié. Un seul service doit être trouvé (le cas échéant). Toutefois, étant donné qu’aucune contrainte n’est intégrée à la base de données, le type de retour est une collection au cas où plusieurs services seraient trouvés.

Par défaut, lorsque le contexte de l’objet récupère des entités de la base de données, il les conserve dans son gestionnaire d’état d’objet. Le paramètre `MergeOption.NoTracking` spécifie que ce suivi ne sera pas effectué pour cette requête. Cela est nécessaire, car la requête peut retourner l’entité exacte que vous essayez de mettre à jour, puis vous ne pouvez pas attacher cette entité. Par exemple, si vous modifiez le service d’historique dans la page *Departments. aspx* et que vous laissez l’administrateur inchangé, cette requête retourne le département de l’historique. Si `NoTracking` n’est pas défini, le contexte de l’objet a déjà l’entité de service d’historique dans son gestionnaire d’état d’objet. Ensuite, lorsque vous attachez l’entité de service d’historique qui est recréée à partir de l’état d’affichage, le contexte de l’objet lèvera une exception qui indique `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Au lieu de spécifier `MergeOption.NoTracking`, vous pouvez créer un nouveau contexte d’objet uniquement pour cette requête. Étant donné que le nouveau contexte d’objet a son propre gestionnaire d’état d’objet, il n’y aurait aucun conflit quand vous appelez la méthode `Attach`. Le nouveau contexte d’objet partageait les métadonnées et la connexion de base de données avec le contexte d’objet d’origine, de sorte que la baisse des performances de cette approche serait minime. Toutefois, l’approche présentée ici présente l’option `NoTracking`, que vous trouverez utile dans d’autres contextes. L’option `NoTracking` est décrite plus en détail dans un didacticiel ultérieur de cette série.)

Dans le projet de test, ajoutez la nouvelle méthode d’accès aux données à *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Ce code utilise LINQ pour effectuer la même sélection de données que celle utilisée par le dépôt de projet `ContosoUniversity` LINQ to Entities.

Réexécutez le projet de test. Cette fois-ci, les tests réussissent.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Gestion des exceptions ObjectDataSource

Dans le projet `ContosoUniversity`, exécutez la page *Departments. aspx* et essayez de modifier l’administrateur d’un service pour qu’une personne qui est déjà administrateur d’un autre service. (N’oubliez pas que vous ne pouvez modifier que les services que vous avez ajoutés au cours de ce didacticiel, car la base de données est préchargée avec des données non valides.) Vous recevez la page d’erreur de serveur suivante :

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Vous ne souhaitez pas que les utilisateurs voient ce type de page d’erreur. vous devez donc ajouter du code de gestion des erreurs. Ouvrez *Departments. aspx* et spécifiez un gestionnaire pour l’événement `OnUpdated` de l' `DepartmentsObjectDataSource`. La balise d’ouverture `ObjectDataSource` ressemble maintenant à l’exemple suivant.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

Dans *Departments.aspx.cs*, ajoutez l’instruction `using` suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Ajoutez le gestionnaire suivant pour l’événement `Updated` :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Si le contrôle de `ObjectDataSource` intercepte une exception lorsqu’il essaie d’effectuer la mise à jour, il passe l’exception dans l’argument d’événement (`e`) à ce gestionnaire. Le code dans le gestionnaire vérifie si l’exception est l’exception d’administrateur en double. Si c’est le cas, le code crée un contrôle de validateur qui contient un message d’erreur pour le contrôle `ValidationSummary` à afficher.

Exécutez la page et essayez à nouveau de faire de la part de l’administrateur de deux départements. Cette fois, le contrôle de `ValidationSummary` affiche un message d’erreur.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Apportez des modifications similaires à la page *DepartmentsAdd. aspx* . Dans *DepartmentsAdd. aspx*, spécifiez un gestionnaire pour l’événement `OnInserted` de l' `DepartmentsObjectDataSource`. Le balisage résultant ressemble à l’exemple suivant.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

Dans *DepartmentsAdd.aspx.cs*, ajoutez la même instruction `using` :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Ajoutez le gestionnaire d’événements suivant :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Vous pouvez maintenant tester la page *DepartmentsAdd.aspx.cs* pour vérifier qu’elle gère également correctement les tentatives de faire d’une personne l’administrateur de plusieurs départements.

Cela termine la présentation de l’implémentation du modèle de référentiel pour l’utilisation du contrôle `ObjectDataSource` avec l’Entity Framework. Pour plus d’informations sur le modèle de référentiel et la testabilité, consultez le livre blanc [testabilité de MSDN et Entity Framework 4,0](https://msdn.microsoft.com/library/ff714955.aspx).

Dans le didacticiel suivant, vous allez apprendre à ajouter des fonctionnalités de tri et de filtrage à l’application.

> [!div class="step-by-step"]
> [Précédent](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [Suivant](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
