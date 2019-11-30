---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Gestion de l’accès concurrentiel avec l’Entity Framework dans une application ASP.NET MVC (7 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 0383974baa16bb0d5fc588f9303290bdb0fd979c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595345"
---
# <a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Gestion de l’accès concurrentiel avec l’Entity Framework dans une application ASP.NET MVC (7 sur 10)

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et de Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels depuis le début ou [Télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et démarrer ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [Téléchargez le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire le problème. Vous pouvez généralement trouver la solution au problème en comparant votre code au code terminé. Pour obtenir des erreurs courantes et comment les résoudre, consultez [Erreurs et solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Dans les deux didacticiels précédents, vous avez travaillé avec des données associées. Ce didacticiel montre comment gérer l’accès concurrentiel. Vous allez créer des pages Web qui fonctionnent avec l’entité `Department`, et les pages qui modifient et suppriment les `Department` entités gèrent les erreurs d’accès concurrentiel. Les illustrations suivantes montrent les pages d’index et de suppression, y compris certains messages qui s’affichent si un conflit d’accès concurrentiel se produit.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Conflits d’accès concurrentiel

Un conflit d’accès concurrentiel se produit quand un utilisateur affiche les données d’une entité pour la modifier, puis qu’un autre utilisateur met à jour les données de la même entité avant que les modifications du premier utilisateur soient écrites dans la base de données. Si vous n’activez pas la détection de ces conflits, la personne qui met à jour la base de données en dernier remplace les modifications de l’autre utilisateur. Dans de nombreuses applications, ce risque est acceptable : s’il n’y a que quelques utilisateurs ou quelques mises à jour, ou s’il n’est pas réellement critique que des modifications soient remplacées, le coût de la programmation nécessaire à la gestion des accès concurrentiels peut être supérieur au bénéfice qu’elle apporte. Dans ce cas, vous ne devez pas configurer l’application pour gérer les conflits d’accès concurrentiel.

### <a name="pessimistic-concurrency-locking"></a>Accès concurrentiel pessimiste (verrouillage)

Si votre application doit éviter la perte accidentelle de données dans des scénarios d’accès concurrentiel, une manière de le faire consiste à utiliser des verrous de base de données. C’est ce que l’on appelle l' *accès concurrentiel pessimiste*. Par exemple, avant de lire une ligne d’une base de données, vous demandez un verrou pour lecture seule ou pour accès avec mise à jour. Si vous verrouillez une ligne pour accès avec mise à jour, aucun autre utilisateur n’est autorisé à verrouiller la ligne pour lecture seule ou pour accès avec mise à jour, car ils obtiendraient ainsi une copie de données qui sont en cours de modification. Si vous verrouillez une ligne pour accès en lecture seule, d’autres utilisateurs peuvent également la verrouiller pour accès en lecture seule, mais pas pour accès avec mise à jour.

La gestion des verrous présente des inconvénients. Elle peut être complexe à programmer. Elle nécessite des ressources de gestion de base de données importantes et peut entraîner des problèmes de performances au fur et à mesure que le nombre d’utilisateurs d’une application augmente (autrement dit, il n’est pas adapté). Pour ces raisons, certains systèmes de gestion de base de données ne prennent pas en charge l’accès concurrentiel pessimiste. Le Entity Framework ne fournit aucune prise en charge intégrée pour celui-ci, et ce didacticiel ne vous montre pas comment l’implémenter.

### <a name="optimistic-concurrency"></a>Accès concurrentiel optimiste

L’alternative à l’accès concurrentiel pessimiste est l' *accès concurrentiel optimiste*. L’accès concurrentiel optimiste signifie autoriser la survenance des conflits d’accès concurrentiel, puis de réagir correctement quand ils surviennent. Par exemple, John exécute la page de modification des départements, modifie le montant du **budget** pour le département anglais de $350 000,00 à $0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Pour que John clique sur **Enregistrer**, Jane exécute la même page et remplace le champ de **Date de début** 9/1/2007 par 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Jean clique tout d’abord sur **Enregistrer** et voit sa modification quand le navigateur revient à la page index, puis Jane clique sur **Enregistrer**. Ce qui se passe ensuite est déterminé par la façon dont vous gérez les conflits d’accès concurrentiel. Voici quelques-unes des options :

- Vous pouvez effectuer le suivi des propriétés modifiées par un utilisateur et mettre à jour seulement les colonnes correspondantes dans la base de données. Dans l’exemple de scénario, aucune donnée ne serait perdue, car des propriétés différentes ont été mises à jour par chacun des deux utilisateurs. La prochaine fois qu’un utilisateur parcourt le service en anglais, il voit les modifications de John et de Jane, à savoir une date de début de 8/8/2013 et un budget de zéro dollar.

    Cette méthode de mise à jour peut réduire le nombre de conflits qui peuvent entraîner des pertes de données, mais elle ne peut pas éviter la perte de données si des modifications concurrentes sont apportées à la même propriété d’une entité. Un tel fonctionnement d’Entity Framework dépend de la façon dont vous implémentez votre code de mise à jour. Il n’est pas souvent pratique dans une application web, car il peut nécessiter la gestion de grandes quantités d’états pour effectuer le suivi de toutes les valeurs de propriété d’origine d’une entité, ainsi que des nouvelles valeurs. La conservation d’un grand nombre d’États peut affecter les performances de l’application, car elle nécessite des ressources serveur ou elle doit être incluse dans la page Web elle-même (par exemple, dans les champs masqués).
- Vous pouvez laisser les modifications de Jane remplacer les modifications de John. La prochaine fois qu’un utilisateur parcourt le service en anglais, il voit 8/8/2013 et la valeur $350 000,00 restaurée. Ceci s’appelle un scénario *Priorité au client* ou *Priorité au dernier entré* (Last in Wins). (Les valeurs du client sont prioritaires par rapport à ce qui se trouve dans le magasin de données.) Comme indiqué dans la présentation de cette section, si vous n’effectuez aucun codage pour la gestion de l’accès concurrentiel, cela se produit automatiquement.
- Vous pouvez empêcher la mise à jour de la modification de Jane dans la base de données. En règle générale, vous affichez un message d’erreur, affichez l’état actuel des données et autorisez-le à réappliquer ses modifications s’il souhaite toujours les effectuer. Il s’agit alors d’un scénario *Priorité au magasin*. (Les valeurs du magasin de données ont priorité sur les valeurs soumises par le client.) Vous allez implémenter le scénario de stockage WINS dans ce didacticiel. Cette méthode garantit qu’aucune modification n’est remplacée sans qu’un utilisateur soit averti de ce qui se passe.

### <a name="detecting-concurrency-conflicts"></a>Détection des conflits d’accès concurrentiel

Vous pouvez résoudre les conflits en gérant les exceptions [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) levées par l’Entity Framework. Pour savoir quand lever ces exceptions, Entity Framework doit être en mesure de détecter les conflits. Par conséquent, vous devez configurer de façon appropriée la base de données et le modèle de données. Voici quelques options pour l’activation de la détection des conflits :

- Dans la table de base de données, incluez une colonne de suivi qui peut être utilisée pour déterminer quand une ligne a été modifiée. Vous pouvez ensuite configurer la Entity Framework pour inclure cette colonne dans la clause `Where` des commandes `Update` ou `Delete` SQL.

    Le type de données de la colonne de suivi est généralement [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). La valeur [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) est un nombre séquentiel qui est incrémenté chaque fois que la ligne est mise à jour. Dans une commande `Update` ou `Delete`, la clause `Where` contient la valeur d’origine de la colonne Tracking (la version de ligne d’origine). Si la ligne en cours de mise à jour a été modifiée par un autre utilisateur, la valeur de la colonne `rowversion` est différente de la valeur d’origine, de sorte que l’instruction `Update` ou `Delete` ne peut pas trouver la ligne à mettre à jour en raison de la clause `Where`. Lorsque le Entity Framework détecte qu’aucune ligne n’a été mise à jour par la commande `Update` ou `Delete` (autrement dit, lorsque le nombre de lignes affectées est égal à zéro), il interprète cela comme un conflit d’accès concurrentiel.
- Configurez la Entity Framework pour inclure les valeurs d’origine de chaque colonne de la table dans la clause `Where` des commandes `Update` et `Delete`.

    Comme dans la première option, si quelque chose dans la ligne a changé depuis la première lecture de la ligne, la clause `Where` ne retourne pas de ligne à mettre à jour, que le Entity Framework interprète comme un conflit d’accès concurrentiel. Pour les tables de base de données qui contiennent de nombreuses colonnes, cette approche peut générer des clauses de `Where` très volumineuses et peut nécessiter la conservation de grandes quantités d’État. Comme indiqué précédemment, la conservation d’un grand nombre d’États peut affecter les performances de l’application, car elle nécessite des ressources serveur ou doit être incluse dans la page Web elle-même. Par conséquent, cette approche n’est généralement pas recommandée, et il ne s’agit pas de la méthode utilisée dans ce didacticiel.

    Si vous ne souhaitez pas implémenter cette approche pour la concurrence, vous devez marquer toutes les propriétés de clé non primaire de l’entité pour lesquelles vous souhaitez effectuer le suivi de la concurrence en y ajoutant l’attribut [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) . Cette modification permet à l’Entity Framework d’inclure toutes les colonnes dans la clause SQL `WHERE` des instructions `UPDATE`.

Dans le reste de ce didacticiel, vous allez ajouter une propriété de suivi [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) à l’entité `Department`, créer un contrôleur et des vues, puis effectuer un test pour vérifier que tout fonctionne correctement.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Ajouter une propriété d’accès concurrentiel optimiste à l’entité Department

Dans *Models\Department.cs*, ajoutez une propriété de suivi nommée `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

L’attribut [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) spécifie que cette colonne sera incluse dans la clause `Where` de `Update` et `Delete` commandes envoyées à la base de données. L’attribut est appelé [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) , car les versions précédentes de SQL Server utilisé un type de données [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) SQL avant que le [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) SQL ne le remplace. Le type .net pour `rowversion` est un tableau d’octets. Si vous préférez utiliser l’API Fluent, vous pouvez utiliser la méthode [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) pour spécifier la propriété de suivi, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

En ajoutant une propriété, vous avez changé le modèle de base de données et vous devez donc effectuer une autre migration. Dans la console du Gestionnaire de package, entrez les commandes suivantes :

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Créer un contrôleur de service

Créez un contrôleur de `Department` et des vues de la même façon que vous avez effectué les autres contrôleurs, en utilisant les paramètres suivants :

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

Dans *Controllers\DepartmentController.cs*, ajoutez une instruction `using` :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Remplacez « LastName » par « FullName » dans ce fichier (quatre occurrences) afin que les listes déroulantes de l’administrateur de service contiennent le nom complet de l’Instructor plutôt que le nom de famille.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Remplacez le code existant pour la méthode `HttpPost` `Edit` par le code suivant :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

La vue stocke la valeur de `RowVersion` d’origine dans un champ masqué. Lorsque le Binder de modèle crée l’instance de `department`, cet objet aura la valeur de la propriété `RowVersion` d’origine et les nouvelles valeurs pour les autres propriétés, telles qu’elles sont entrées par l’utilisateur sur la page de modification. Ensuite, lorsque le Entity Framework crée une commande SQL `UPDATE`, cette commande inclut une clause `WHERE` qui recherche une ligne qui a la valeur `RowVersion` d’origine.

Si aucune ligne n’est affectée par la commande `UPDATE` (aucune ligne n’a la valeur de `RowVersion` d’origine), le Entity Framework lève une exception `DbUpdateConcurrencyException`, et le code dans le bloc `catch` obtient l’entité `Department` affectée à partir de l’objet exception. Cette entité a à la fois les valeurs lues à partir de la base de données et les nouvelles valeurs entrées par l’utilisateur :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Ensuite, le code ajoute un message d’erreur personnalisé pour chaque colonne dont les valeurs de base de données sont différentes de celles que l’utilisateur a entrées dans la page de modification :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Un message d’erreur plus long explique ce qui s’est passé et la marche à suivre :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Enfin, le code définit la valeur `RowVersion` de l’objet `Department` sur la nouvelle valeur extraite de la base de données. Cette nouvelle valeur de `RowVersion` est stockée dans le champ masqué quand la page Edit est réaffichée et, la prochaine fois que l’utilisateur clique sur **Save**, seules les erreurs d’accès concurrentiel qui se produisent depuis le réaffichage de la page Edit sont interceptées.

Dans *Views\Department\Edit.cshtml*, ajoutez un champ masqué pour enregistrer la valeur de la propriété `RowVersion`, immédiatement après le champ masqué pour la propriété `DepartmentID` :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

Dans *Views\Department\Index.cshtml*, remplacez le code existant par le code suivant pour déplacer les liens de ligne vers la gauche, puis modifiez le titre de la page et les en-têtes de colonne pour afficher `FullName` au lieu de `LastName` dans la colonne **administrateur** :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Test de la gestion de l’accès concurrentiel optimiste

Exécutez le site et cliquez sur **Departments**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Cliquez avec le bouton droit sur le lien hypertexte **modifier** pour Kim Abercrombie et sélectionnez **ouvrir dans un nouvel onglet,** puis cliquez sur le lien hypertexte **modifier** pour Kim Abercrombie. Les deux fenêtres affichent les mêmes informations.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Modifiez un champ dans la première fenêtre du navigateur et cliquez sur **Enregistrer**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Le navigateur affiche la page Index avec la valeur modifiée.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Modifiez le champ tout dans la deuxième fenêtre du navigateur, puis cliquez sur **Enregistrer**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Cliquez sur **Enregistrer** dans la deuxième fenêtre du navigateur. Vous voyez un message d’erreur :

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Cliquez à nouveau sur **Save**. La valeur que vous avez entrée dans le deuxième navigateur est enregistrée avec la valeur d’origine des données que vous modifiez dans le premier navigateur. Vous voyez les valeurs enregistrées quand la page Index apparaît.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Mise à jour de la page de suppression

Pour la page Delete, Entity Framework détecte les conflits d’accès concurrentiel provoqués par un autre utilisateur qui modifie le service de façon similaire. Lorsque la méthode `HttpGet` `Delete` affiche l’affichage confirmation, la vue comprend la valeur `RowVersion` d’origine dans un champ masqué. Cette valeur est ensuite disponible pour la méthode `HttpPost` `Delete` qui est appelée lorsque l’utilisateur confirme la suppression. Lorsque le Entity Framework crée la commande SQL `DELETE`, il comprend une clause `WHERE` avec la valeur `RowVersion` d’origine. Si la commande ne produit aucune ligne affectée (ce qui signifie que la ligne a été modifiée après l’affichage de la page de confirmation de la suppression), une exception d’accès concurrentiel est levée et la méthode `HttpGet Delete` est appelée avec un indicateur d’erreur défini sur `true` afin de réafficher la page de confirmation avec un message d’erreur. Il est également possible qu’aucune ligne n’ait été affectée, car la ligne a été supprimée par un autre utilisateur. dans ce cas, un autre message d’erreur s’affiche.

Dans *DepartmentController.cs*, remplacez la méthode `HttpGet` `Delete` par le code suivant :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

La méthode accepte un paramètre facultatif qui indique si la page est réaffichée après une erreur d’accès concurrentiel. Si cet indicateur est `true`, un message d’erreur est envoyé à la vue à l’aide d’une propriété `ViewBag`.

Remplacez le code dans la méthode `HttpPost` `Delete` (nommée `DeleteConfirmed`) par le code suivant :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Dans le code du modèle généré automatiquement que vous venez de remplacer, cette méthode n’acceptait qu’un seul ID d’enregistrement :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Vous avez modifié ce paramètre en une instance d’entité `Department` créée par le classeur de modèles. Cela vous donne accès à la valeur de la propriété `RowVersion` en plus de la clé d’enregistrement.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Vous avez également changé le nom de la méthode d’action de `DeleteConfirmed` en `Delete`. Le code de génération de modèles automatique nommé `HttpPost` méthode `Delete` `DeleteConfirmed` pour attribuer une signature unique à la méthode `HttpPost`. (Le CLR exige que les méthodes surchargées aient des paramètres de méthode différents.) Maintenant que les signatures sont uniques, vous pouvez respecter la convention MVC et utiliser le même nom pour les `HttpPost` et `HttpGet` les méthodes Delete.

Si une erreur d’accès concurrentiel est interceptée, le code réaffiche la page de confirmation de suppression et fournit un indicateur indiquant qu’elle doit afficher un message d’erreur d’accès concurrentiel.

Dans *Views\Department\Delete.cshtml*, remplacez le code généré automatiquement par le code suivant qui effectue certaines modifications de mise en forme et ajoute un champ de message d’erreur. Les modifications sont mises en surbrillance.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Ce code ajoute un message d’erreur entre les en-têtes `h2` et `h3` :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Il remplace `LastName` par `FullName` dans le champ `Administrator` :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Enfin, il ajoute des champs masqués pour les propriétés `DepartmentID` et `RowVersion` après l’instruction `Html.BeginForm` :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Exécutez la page d’index des services. Cliquez avec le bouton droit sur le lien hypertexte **supprimer** du département anglais et sélectionnez **ouvrir dans une nouvelle fenêtre,** puis dans la première fenêtre, cliquez sur le lien hypertexte **modifier** pour le département anglais.

Dans la première fenêtre, modifiez l’une des valeurs, puis cliquez sur **Enregistrer** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

La page d’index confirme la modification.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Dans la deuxième fenêtre, cliquez sur **supprimer**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Vous voyez le message d’erreur d’accès concurrentiel et les valeurs du département sont actualisées avec ce qui est actuellement dans la base de données.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Si vous recliquez sur **Delete**, vous êtes redirigé vers la page Index, qui montre que le département a été supprimé.

## <a name="summary"></a>Récapitulatif

Ceci termine l’introduction à la gestion des conflits d’accès concurrentiel. Pour plus d’informations sur les autres façons de gérer différents scénarios d’accès concurrentiel, consultez [modèles d’accès concurrentiel optimiste](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) et [utilisation des valeurs de propriété](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) sur le blog de l’équipe Entity Framework. Le didacticiel suivant montre comment implémenter l’héritage TPH (table par hiérarchie) pour les entités `Instructor` et `Student`.

Vous trouverez des liens vers d’autres ressources de Entity Framework dans le [plan de contenu d’accès aux données ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Suivant](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
