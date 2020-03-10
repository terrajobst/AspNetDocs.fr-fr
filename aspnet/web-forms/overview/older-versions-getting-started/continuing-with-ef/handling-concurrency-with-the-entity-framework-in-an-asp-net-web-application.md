---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Gestion de l’accès concurrentiel avec la Entity Framework 4,0 dans une application Web ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le Prise en main avec la série de didacticiels Entity Framework 4,0. ...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 3df5f7d9c8fb22e1ea34fe16560bdb9a1309bb56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632586"
---
# <a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Gestion de l’accès concurrentiel avec la Entity Framework 4,0 dans une application Web ASP.NET 4

par [Tom Dykstra](https://github.com/tdykstra)

> Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le [prise en main avec la](https://asp.net/entity-framework/tutorials#Getting%20Started) série de didacticiels Entity Framework 4,0. Si vous n’avez pas terminé les didacticiels précédents, comme point de départ pour ce didacticiel, vous pouvez [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous auriez créée. Vous pouvez également [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créée par la série de didacticiels complète. Si vous avez des questions sur les didacticiels, vous pouvez les poster sur le [Forum de Entity Framework ASP.net](https://forums.asp.net/1227.aspx).

Dans le didacticiel précédent, vous avez appris à trier et filtrer les données à l’aide du contrôle `ObjectDataSource` et des Entity Framework. Ce didacticiel présente les options de gestion de l’accès concurrentiel dans une application Web ASP.NET qui utilise le Entity Framework. Vous allez créer une page Web dédiée à la mise à jour des attributions de bureaux des enseignants. Vous allez gérer les problèmes d’accès concurrentiel dans cette page et dans la page services que vous avez créée précédemment.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Conflits d’accès concurrentiel

Un conflit d’accès concurrentiel se produit lorsqu’un utilisateur modifie un enregistrement et qu’un autre utilisateur modifie le même enregistrement avant que la modification du premier utilisateur soit écrite dans la base de données. Si vous ne configurez pas les Entity Framework pour détecter de tels conflits, la personne qui met à jour la base de données remplace le dernier remplacement des modifications de l’autre utilisateur. Dans de nombreuses applications, ce risque est acceptable et vous n’avez pas besoin de configurer l’application pour gérer les éventuels conflits d’accès concurrentiel. (S’il n’y a que peu d’utilisateurs, ou peu de mises à jour, ou si n’est pas vraiment critique si certaines modifications sont remplacées, le coût de la programmation de l’accès concurrentiel peut être le même que celui de l’avantage.) Si vous n’avez pas besoin de vous soucier des conflits d’accès concurrentiel, vous pouvez ignorer ce didacticiel. les deux autres didacticiels de la série ne dépendent pas de ce que vous créez dans celui-ci.

### <a name="pessimistic-concurrency-locking"></a>Accès concurrentiel pessimiste (verrouillage)

Si votre application doit éviter la perte accidentelle de données dans des scénarios d’accès concurrentiel, une manière de le faire consiste à utiliser des verrous de base de données. C’est ce que l’on appelle l' *accès concurrentiel pessimiste*. Par exemple, avant de lire une ligne d’une base de données, vous demandez un verrou pour lecture seule ou pour accès avec mise à jour. Si vous verrouillez une ligne pour accès avec mise à jour, aucun autre utilisateur n’est autorisé à verrouiller la ligne pour lecture seule ou pour accès avec mise à jour, car ils obtiendraient ainsi une copie de données qui sont en cours de modification. Si vous verrouillez une ligne pour accès en lecture seule, d’autres utilisateurs peuvent également la verrouiller pour accès en lecture seule, mais pas pour accès avec mise à jour.

La gestion des verrous présente quelques inconvénients. Elle peut être complexe à programmer. Elle nécessite des ressources de gestion de base de données importantes et peut entraîner des problèmes de performances au fur et à mesure que le nombre d’utilisateurs d’une application augmente (autrement dit, il n’est pas adapté). Pour ces raisons, certains systèmes de gestion de base de données ne prennent pas en charge l’accès concurrentiel pessimiste. Le Entity Framework ne fournit aucune prise en charge intégrée pour celui-ci, et ce didacticiel ne vous montre pas comment l’implémenter.

### <a name="optimistic-concurrency"></a>Accès concurrentiel optimiste

L’alternative à l’accès concurrentiel pessimiste est l' *accès concurrentiel optimiste*. L’accès concurrentiel optimiste signifie autoriser la survenance des conflits d’accès concurrentiel, puis de réagir correctement quand ils surviennent. Par exemple, John exécute la page *Department. aspx* , clique sur le lien **Edit** pour le département de l’historique et réduit le montant du **budget** de $1 000 000,00 à $125 000,00. (John administre un service en concurrence et souhaite libérer de l’argent pour son propre service.)

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Pour que John clique sur **mettre à jour**, Jane exécute la même page, clique sur le lien **modifier** pour le département historique, puis remplace le champ **Date de début** 1/10/2011 par 1/1/1999. (Jane administre le département de l’historique et veut lui apporter plus d’ancienneté.)

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John clique d’abord sur **Update** , puis Jane clique sur **Update**. Le navigateur de Jane répertorie maintenant le montant du **budget** comme $1 000 000,00, mais cela est incorrect, car le montant a été modifié par John à $125 000,00.

Voici quelques-unes des actions que vous pouvez effectuer dans ce scénario :

- Vous pouvez effectuer le suivi des propriétés modifiées par un utilisateur et mettre à jour seulement les colonnes correspondantes dans la base de données. Dans l’exemple de scénario, aucune donnée ne serait perdue, car des propriétés différentes ont été mises à jour par chacun des deux utilisateurs. La prochaine fois qu’un utilisateur parcourt le département de l’historique, il verra 1/1/1999 et $125 000,00. 

    Il s’agit du comportement par défaut de la Entity Framework, qui peut réduire considérablement le nombre de conflits susceptibles de provoquer une perte de données. Toutefois, ce comportement n’évite pas la perte de données si des modifications concurrentes sont apportées à la même propriété d’une entité. En outre, ce comportement n’est pas toujours possible. Lorsque vous mappez des procédures stockées à un type d’entité, toutes les propriétés d’une entité sont mises à jour lorsque des modifications sont apportées à l’entité dans la base de données.
- Vous pouvez laisser les modifications de Jane remplacer les modifications de John. Lorsque Jane clique sur **Update**, le montant du **Budget** revient à $1 000 000,00. Ceci s’appelle un scénario *Priorité au client* ou *Priorité au dernier entré* (Last in Wins). (Les valeurs du client sont prioritaires par rapport à ce qui se trouve dans le magasin de données.)
- Vous pouvez empêcher la mise à jour de la modification de Jane dans la base de données. En règle générale, vous affichez un message d’erreur, affichez l’état actuel des données et autorisez-le à entrer à nouveau ses modifications s’il souhaite toujours les effectuer. Vous pouvez poursuivre l’automatisation du processus en enregistrant son entrée et en lui donnant la possibilité de le réappliquer sans avoir à le réentrer. Il s’agit alors d’un scénario *Priorité au magasin*. (Les valeurs du magasin de données sont prioritaires par rapport à celles soumises par le client.)

### <a name="detecting-concurrency-conflicts"></a>Détection des conflits d’accès concurrentiel

Dans le Entity Framework, vous pouvez résoudre les conflits en gérant `OptimisticConcurrencyException` exceptions levées par l’Entity Framework. Pour savoir quand lever ces exceptions, Entity Framework doit être en mesure de détecter les conflits. Par conséquent, vous devez configurer de façon appropriée la base de données et le modèle de données. Voici quelques options pour l’activation de la détection des conflits :

- Dans la base de données, incluez une colonne de table qui peut être utilisée pour déterminer quand une ligne a été modifiée. Vous pouvez ensuite configurer la Entity Framework pour inclure cette colonne dans la clause `Where` des commandes `Update` ou `Delete` SQL.

    C’est l’objectif de la colonne `Timestamp` dans la table `OfficeAssignment`.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Le type de données de la colonne `Timestamp` est également appelé `Timestamp`. Toutefois, la colonne ne contient pas réellement une valeur de date ou d’heure. Au lieu de cela, la valeur est un nombre séquentiel qui est incrémenté chaque fois que la ligne est mise à jour. Dans une commande `Update` ou `Delete`, la clause `Where` contient la valeur `Timestamp` d’origine. Si la ligne en cours de mise à jour a été modifiée par un autre utilisateur, la valeur de `Timestamp` est différente de la valeur d’origine, donc la clause `Where` ne retourne aucune ligne à mettre à jour. Lorsque le Entity Framework détecte qu’aucune ligne n’a été mise à jour par la commande `Update` ou `Delete` en cours (autrement dit, lorsque le nombre de lignes affectées est égal à zéro), il interprète cela comme un conflit d’accès concurrentiel.
- Configurez la Entity Framework pour inclure les valeurs d’origine de chaque colonne de la table dans la clause `Where` des commandes `Update` et `Delete`.

    Comme dans la première option, si quelque chose dans la ligne a changé depuis la première lecture de la ligne, la clause `Where` ne retourne pas de ligne à mettre à jour, que le Entity Framework interprète comme un conflit d’accès concurrentiel. Cette méthode est aussi efficace que l’utilisation d’un champ `Timestamp`, mais peut être inefficace. Pour les tables de base de données qui contiennent de nombreuses colonnes, cela peut générer des clauses de `Where` très volumineuses et, dans une application Web, il peut être nécessaire de conserver de grandes quantités d’État. La conservation d’un grand nombre d’États peut affecter les performances de l’application, car elle nécessite des ressources serveur (par exemple, un état de session) ou doit être incluse dans la page Web elle-même (par exemple, l’état d’affichage).

Dans ce didacticiel, vous allez ajouter la gestion des erreurs pour les conflits d’accès concurrentiel optimiste pour une entité qui n’a pas de propriété de suivi (l’entité `Department`) et pour une entité qui a une propriété de suivi (l’entité `OfficeAssignment`).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Gestion de l’accès concurrentiel optimiste sans propriété de suivi

Pour implémenter l’accès concurrentiel optimiste pour l’entité `Department`, qui n’a pas de propriété Tracking (`Timestamp`), vous allez effectuer les tâches suivantes :

- Modifiez le modèle de données pour activer le suivi d’accès concurrentiel pour les entités `Department`.
- Dans la classe `SchoolRepository`, gérez les exceptions d’accès concurrentiel dans la méthode `SaveChanges`.
- Dans la page *Departments. aspx* , gérez les exceptions d’accès concurrentiel en affichant un message indiquant à l’utilisateur que les tentatives de modification ont échoué. L’utilisateur peut alors voir les valeurs actuelles et réessayer les modifications si elles sont toujours nécessaires.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Activation du suivi des accès concurrentiels dans le modèle de données

Dans Visual Studio, ouvrez l’application Web Contoso University avec laquelle vous travailliez dans le didacticiel précédent de cette série.

Ouvrez *SchoolModel. edmx*et dans le concepteur de modèle de données, cliquez avec le bouton droit sur la propriété `Name` dans l’entité `Department`, puis cliquez sur **Propriétés**. Dans la fenêtre **Propriétés** , affectez à la propriété `ConcurrencyMode` la valeur `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Procédez de même pour les autres propriétés scalaires non-clé primaire (`Budget`, `StartDate`et `Administrator`). (Vous ne pouvez pas le faire pour les propriétés de navigation.) Cela spécifie que chaque fois que l’Entity Framework génère une `Update` ou `Delete` commande SQL pour mettre à jour l’entité `Department` dans la base de données, ces colonnes (avec les valeurs d’origine) doivent être incluses dans la clause `Where`. Si aucune ligne n’est trouvée lors de l’exécution de la commande `Update` ou `Delete`, le Entity Framework lèvera une exception d’accès concurrentiel optimiste.

Enregistrez et fermez le modèle de données.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Gestion des exceptions d’accès concurrentiel dans la couche DAL

Ouvrez *SchoolRepository.cs* et ajoutez l’instruction `using` suivante pour l’espace de noms `System.Data` :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Ajoutez la nouvelle méthode `SaveChanges` suivante, qui gère les exceptions d’accès concurrentiel optimiste :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Si une erreur d’accès concurrentiel se produit lorsque cette méthode est appelée, les valeurs de propriété de l’entité en mémoire sont remplacées par les valeurs actuellement présentes dans la base de données. L’exception d’accès concurrentiel est levée de nouveau de sorte que la page Web puisse la gérer.

Dans les méthodes `DeleteDepartment` et `UpdateDepartment`, remplacez l’appel existant à `context.SaveChanges()` par un appel à `SaveChanges()` afin d’appeler la nouvelle méthode.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Gestion des exceptions d’accès concurrentiel dans la couche de présentation

Ouvrez *Departments. aspx* et ajoutez un attribut `OnDeleted="DepartmentsObjectDataSource_Deleted"` au contrôle `DepartmentsObjectDataSource`. La balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Dans le contrôle `DepartmentsGridView`, spécifiez toutes les colonnes de table dans l’attribut `DataKeyNames`, comme indiqué dans l’exemple suivant. Notez que cela permet de créer des champs d’état d’affichage très volumineux, ce qui est l’une des raisons pour lesquelles l’utilisation d’un champ de suivi est généralement la meilleure façon de suivre les conflits d’accès concurrentiel.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Ouvrez *Departments.aspx.cs* et ajoutez l’instruction `using` suivante pour l’espace de noms `System.Data` :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Ajoutez la nouvelle méthode suivante, que vous appelez à partir de la `Updated` du contrôle de source de données et `Deleted` gestionnaires d’événements pour gérer les exceptions d’accès concurrentiel :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Ce code vérifie le type d’exception et, s’il s’agit d’une exception d’accès concurrentiel, le code crée dynamiquement un contrôle `CustomValidator` qui, à son tour, affiche un message dans le contrôle `ValidationSummary`.

Appelez la nouvelle méthode à partir du gestionnaire d’événements `Updated` que vous avez ajouté précédemment. En outre, créez un gestionnaire d’événements de `Deleted` qui appelle la même méthode (mais ne fait rien d’autre) :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Test de l’accès concurrentiel optimiste dans la page services

Exécutez la page *Departments. aspx* .

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Cliquez sur **modifier** dans une ligne et modifiez la valeur dans la colonne **budget** . (N’oubliez pas que vous ne pouvez modifier que les enregistrements que vous avez créés pour ce didacticiel, car les enregistrements de base de données `School` existants contiennent des données non valides. L’enregistrement du Département économie est un moyen sûr d’expérimenter.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Ouvrez une nouvelle fenêtre de navigateur et réexécutez la page (copiez l’URL de la zone d’adresse de la première fenêtre du navigateur vers la deuxième fenêtre du navigateur).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Cliquez sur **modifier** sur la même ligne que vous avez modifiée précédemment et remplacez la valeur de **budget** par une valeur différente.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Dans la deuxième fenêtre du navigateur, cliquez sur **mettre à jour**. Le montant du **budget** est correctement modifié en cette nouvelle valeur.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Dans la première fenêtre du navigateur, cliquez sur **mettre à jour**. La mise à jour échoue. Le montant du **budget** est réaffiché à l’aide de la valeur que vous avez définie dans la deuxième fenêtre de navigateur et un message d’erreur s’affiche.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Gestion de l’accès concurrentiel optimiste à l’aide d’une propriété Tracking

Pour gérer l’accès concurrentiel optimiste pour une entité qui a une propriété de suivi, vous devez effectuer les tâches suivantes :

- Ajoutez des procédures stockées au modèle de données pour gérer les entités `OfficeAssignment`. (Les propriétés de suivi et les procédures stockées n’ont pas besoin d’être utilisées ensemble ; elles sont simplement regroupées ici à titre d’illustration.)
- Ajoutez des méthodes CRUD à la couche DAL et à la couche BLL pour `OfficeAssignment` entités, y compris du code pour gérer les exceptions d’accès concurrentiel optimiste dans la couche DAL.
- Créer une page Web d’affectations de bureaux.
- Test de l’accès concurrentiel optimiste dans la nouvelle page Web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Ajout de procédures stockées OfficeAssignment au modèle de données

Ouvrez le fichier *SchoolModel. edmx* dans le générateur de modèles, cliquez avec le bouton droit sur l’aire de conception, puis cliquez sur **mettre à jour le modèle à partir de la base de données**. Dans l’onglet **Ajouter** de la boîte **de dialogue choisir vos objets de base de données** , développez **procédures stockées** , puis sélectionnez les trois `OfficeAssignment` procédures stockées (voir la capture d’écran suivante), puis cliquez sur **Terminer**. (Ces procédures stockées se trouvaient déjà dans la base de données lorsque vous l’avez téléchargée ou créée à l’aide d’un script.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Cliquez avec le bouton droit sur l’entité `OfficeAssignment`, puis sélectionnez **mappage de procédure stockée**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Définissez les fonctions **Insert**, **Update**et **Delete** pour utiliser leurs procédures stockées correspondantes. Pour le paramètre `OrigTimestamp` de la fonction `Update`, affectez à la **propriété** la valeur `Timestamp` et sélectionnez l’option **utiliser la valeur d’origine** .

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Lorsque le Entity Framework appelle la procédure stockée `UpdateOfficeAssignment`, il passe la valeur d’origine de la colonne `Timestamp` dans le paramètre `OrigTimestamp`. La procédure stockée utilise ce paramètre dans sa clause `Where` :

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

La procédure stockée sélectionne également la nouvelle valeur de la colonne `Timestamp` après la mise à jour afin que l’Entity Framework puisse conserver la synchronisation de l’entité `OfficeAssignment` en mémoire avec la ligne de base de données correspondante.

(Notez que la procédure stockée de suppression d’une affectation Office n’a pas de paramètre `OrigTimestamp`. Pour cette raison, le Entity Framework ne peut pas vérifier qu’une entité est inchangée avant de la supprimer.)

Enregistrez et fermez le modèle de données.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Ajout de méthodes OfficeAssignment à la couche DAL

Ouvrez *ISchoolRepository.cs* et ajoutez les méthodes CRUD suivantes pour le jeu d’entités `OfficeAssignment` :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Ajoutez les nouvelles méthodes suivantes à *SchoolRepository.cs*. Dans la méthode `UpdateOfficeAssignment`, vous appelez la méthode `SaveChanges` locale au lieu de `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Dans le projet de test, ouvrez *MockSchoolRepository.cs* et ajoutez-y la collection `OfficeAssignment` suivante et les méthodes CRUD. (Le référentiel factice doit implémenter l’interface du référentiel, sinon la solution ne sera pas compilée.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Ajout de méthodes OfficeAssignment à la couche BLL

Dans le projet principal, ouvrez *SchoolBL.cs* et ajoutez-lui les méthodes CRUD suivantes pour le jeu d’entités `OfficeAssignment` :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Création d’une page Web OfficeAssignments

Créez une nouvelle page Web qui utilise la page maître *site. Master* et nommez-la *OfficeAssignments. aspx*. Ajoutez le balisage suivant au contrôle `Content` nommé `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Notez que dans l’attribut `DataKeyNames`, le balisage spécifie la propriété `Timestamp` ainsi que la clé d’enregistrement (`InstructorID`). Si vous spécifiez des propriétés dans l’attribut `DataKeyNames`, le contrôle les enregistre dans l’état du contrôle (ce qui est semblable à l’état d’affichage) afin que les valeurs d’origine soient disponibles pendant le traitement de la publication (postback).

Si vous n’avez pas enregistré la valeur `Timestamp`, la Entity Framework ne l’aurait pas pour la clause `Where` de la commande SQL `Update`. Par conséquent, rien n’est trouvé à mettre à jour. Par conséquent, le Entity Framework lèvera une exception d’accès concurrentiel optimiste chaque fois qu’une entité `OfficeAssignment` est mise à jour.

Ouvrez *OfficeAssignments.aspx.cs* et ajoutez l’instruction `using` suivante pour la couche d’accès aux données :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Ajoutez la méthode `Page_Init` suivante, qui active les fonctionnalités de Dynamic Data. Ajoutez également le gestionnaire suivant pour l’événement `Updated` du contrôle `ObjectDataSource` afin de vérifier les erreurs d’accès concurrentiel :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Test de l’accès concurrentiel optimiste dans la page OfficeAssignments

Exécutez la page *OfficeAssignments. aspx* .

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Cliquez sur **modifier** dans une ligne et modifiez la valeur dans la colonne **emplacement** .

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Ouvrez une nouvelle fenêtre de navigateur et réexécutez la page (copiez l’URL de la première fenêtre du navigateur vers la deuxième fenêtre du navigateur).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Cliquez sur **modifier** dans la ligne que vous avez modifiée précédemment et remplacez la valeur d' **emplacement** par une valeur différente.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Dans la deuxième fenêtre du navigateur, cliquez sur **mettre à jour**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Basculez vers la première fenêtre de navigateur, puis cliquez sur **mettre à jour**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Vous voyez un message d’erreur et la valeur d' **emplacement** a été mise à jour pour afficher la valeur que vous avez remplacée par dans la deuxième fenêtre de navigateur.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Gestion de l’accès concurrentiel avec le contrôle EntityDataSource

Le contrôle `EntityDataSource` comprend une logique intégrée qui reconnaît les paramètres de concurrence dans le modèle de données et gère les opérations de mise à jour et de suppression en conséquence. Toutefois, comme avec toutes les exceptions, vous devez gérer les exceptions de `OptimisticConcurrencyException` vous-même pour fournir un message d’erreur convivial.

Ensuite, vous allez configurer la page *courses. aspx* (qui utilise un contrôle `EntityDataSource`) pour autoriser les opérations de mise à jour et de suppression et afficher un message d’erreur si un conflit d’accès concurrentiel se produit. L’entité `Course` n’a pas de colonne de suivi d’accès concurrentiel. vous allez donc utiliser la même méthode que celle que vous avez utilisée avec l’entité `Department` : suivre les valeurs de toutes les propriétés non-clés.

Ouvrez le fichier *SchoolModel. edmx* . Pour les propriétés non-clés de l’entité `Course` (`Title`, `Credits`et `DepartmentID`), affectez à la propriété **mode d’accès concurrentiel** la valeur `Fixed`. Ensuite, enregistrez et fermez le modèle de données.

Ouvrez la page *courses. aspx* et apportez les modifications suivantes :

- Dans le contrôle `CoursesEntityDataSource`, ajoutez des attributs `EnableUpdate="true"` et `EnableDelete="true"`. La balise d’ouverture pour ce contrôle ressemble maintenant à l’exemple suivant :

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- Dans le contrôle `CoursesGridView`, remplacez la valeur de l’attribut `DataKeyNames` par `"CourseID,Title,Credits,DepartmentID"`. Ajoutez ensuite un élément `CommandField` à l’élément `Columns` qui affiche les boutons **modifier** et **supprimer** (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). Le contrôle `GridView` ressemble maintenant à l’exemple suivant :

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Exécutez la page et créez une situation de conflit comme vous l’avez fait précédemment dans la page services. Exécutez la page dans deux fenêtres de navigateur, cliquez sur **modifier** sur la même ligne dans chaque fenêtre et apportez une autre modification dans chacune d’elles. Cliquez sur **mettre à jour** dans une fenêtre, puis cliquez sur **mettre à jour** dans l’autre fenêtre. Lorsque vous cliquez sur **mettre à jour** la deuxième fois, vous voyez la page d’erreur qui résulte d’une exception d’accès concurrentiel non gérée.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Vous gérez cette erreur de manière très similaire à la façon dont vous l’avez gérée pour le contrôle de `ObjectDataSource`. Ouvrez la page *courses. aspx* et, dans le contrôle `CoursesEntityDataSource`, spécifiez des gestionnaires pour les événements `Deleted` et `Updated`. La balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant :

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Avant le contrôle `CoursesGridView`, ajoutez le contrôle `ValidationSummary` suivant :

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

Dans *courses.aspx.cs*, ajoutez une instruction `using` pour l’espace de noms `System.Data`, ajoutez une méthode qui vérifie les exceptions d’accès concurrentiel et ajoutez des gestionnaires pour les gestionnaires `Updated` et `Deleted` du contrôle `EntityDataSource`. Le code ressemble à ce qui suit :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

La seule différence entre ce code et ce que vous avez fait pour le contrôle de `ObjectDataSource` est que dans ce cas, l’exception d’accès concurrentiel est dans la propriété `Exception` de l’objet d’arguments d’événement plutôt que dans la propriété `InnerException` de cette exception.

Exécutez la page et réessayez de créer un conflit d’accès concurrentiel. Cette fois, un message d’erreur s’affiche :

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Ceci termine l’introduction à la gestion des conflits d’accès concurrentiel. Le didacticiel suivant fournit des conseils sur la façon d’améliorer les performances dans une application Web qui utilise le Entity Framework.

> [!div class="step-by-step"]
> [Précédent](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Suivant](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
