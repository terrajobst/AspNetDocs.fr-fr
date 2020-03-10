---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: 'Didacticiel : créer un modèle de données plus complexe pour une application ASP.NET MVC'
author: tdykstra
description: Dans ce didacticiel, vous allez ajouter des entités et des relations, et vous personnaliserez le modèle de données en spécifiant des règles de mise en forme, de validation et de mappage de base de données.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 933354b09eb4674e6f523f8a65816410521f026f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583327"
---
# <a name="tutorial-create-a-more-complex-data-model-for-an-aspnet-mvc-app"></a>Didacticiel : créer un modèle de données plus complexe pour une application ASP.NET MVC

Dans les didacticiels précédents, vous avez travaillé avec un modèle de données simple composé de trois entités. Dans ce didacticiel, vous ajoutez des entités et des relations, et vous personnalisez le modèle de données en spécifiant des règles de mise en forme, de validation et de mappage de base de données. Cet article montre deux façons de personnaliser le modèle de données : en ajoutant des attributs aux classes d’entité et en ajoutant du code à la classe de contexte de base de données.

Lorsque vous aurez terminé, les classes d’entité composeront le modèle de données complet indiqué dans l’illustration suivante :

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Personnaliser le modèle de données
> * Mettre à jour l’entité Student
> * Créer une entité Instructor
> * Créer une entité OfficeAssignment
> * Modifier l’entité course
> * Créer l’entité Department
> * Modifier l’entité Enrollment
> * Ajouter du code au contexte de base de données
> * Remplir la base de données avec des données de test
> * Ajouter une migration
> * Mettre à jour la base de données

## <a name="prerequisites"></a>Conditions préalables requises

* [Migrations et déploiements de Code First](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-the-data-model"></a>Personnaliser le modèle de données

Dans cette section, vous allez apprendre à personnaliser le modèle de données en utilisant des attributs qui spécifient des règles de mise en forme, de validation et de mappage de base de données. Ensuite, dans plusieurs des sections suivantes, vous allez créer le modèle de données `School` complet en ajoutant des attributs aux classes que vous avez déjà créées et en créant de nouvelles classes pour les types d’entités restants dans le modèle.

### <a name="the-datatype-attribute"></a>Attribut DataType

Pour les dates d’inscription des étudiants, toutes les pages web affichent l’heure avec la date, alors que seule la date vous intéresse dans ce champ. Vous pouvez avoir recours aux attributs d’annotation de données pour apporter une modification au code, permettant de corriger le format d’affichage dans chaque vue qui affiche ces données. Pour voir un exemple de la procédure à suivre, vous allez ajouter un attribut à la propriété `EnrollmentDate` dans la classe `Student`.

Dans *Models\Student.cs*, ajoutez une instruction `using` pour l’espace de noms `System.ComponentModel.DataAnnotations` et ajoutez des attributs `DataType` et `DisplayFormat` à la propriété `EnrollmentDate`, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

L’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) est utilisé pour spécifier un type de données qui est plus spécifique que le type intrinsèque de la base de données. Dans le cas présent, nous voulons uniquement effectuer le suivi de la date, pas de la date et de l’heure. L' [énumération DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fournit de nombreux types de données, tels que *date, Time, PhoneNumber, Currency, EmailAddress* et bien plus encore. L’attribut `DataType` peut également permettre à l’application de fournir automatiquement des fonctionnalités propres au type. Par exemple, un lien `mailto:` peut être créé pour [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)et un sélecteur de date peut être fourni pour [DataType. date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) dans les navigateurs qui prennent en charge [HTML5](http://html5.org/). Les attributs [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) émettent des attributs HTML 5 [Data-](http://ejohn.org/blog/html-5-data-attributes/) (prononced *Data Dash*) que les navigateurs HTML 5 peuvent comprendre. Les attributs de [type de données](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) ne fournissent aucune validation.

`DataType.Date` ne spécifie pas le format de la date qui s’affiche. Par défaut, le champ de données est affiché conformément aux formats par défaut basés sur le [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)du serveur.

L’attribut `DisplayFormat` est utilisé pour spécifier explicitement le format de date :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]

Le paramètre `ApplyFormatInEditMode` spécifie que la mise en forme spécifiée doit également être appliquée lorsque la valeur est affichée dans une zone de texte pour modification. (Vous ne voulez peut-être pas que pour certains champs, par exemple, pour les valeurs monétaires, vous ne voudrez peut-être pas que le symbole monétaire dans la zone de texte soit modifié.)

Vous pouvez utiliser l’attribut [displayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) seul, mais il est généralement judicieux d’utiliser également l’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) . L’attribut `DataType` transmet la *sémantique* des données au lieu de les afficher sur un écran, et offre les avantages suivants, que vous ne pouvez pas obtenir avec `DisplayFormat`:

- Le navigateur peut activer des fonctionnalités HTML5 (par exemple pour afficher un contrôle de calendrier, le symbole monétaire correspondant aux paramètres régionaux, des liens de messagerie, une certaine validation des entrées côté client, etc.).
- Par défaut, le navigateur restitue les données à l’aide du format approprié en fonction de vos [paramètres régionaux](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- L’attribut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) peut permettre à MVC de choisir le modèle de champ approprié pour le rendu des données (le [displayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) utilise le modèle de chaîne). Pour plus d’informations, consultez [modèles ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)de Brad Wilson. (Bien qu’il soit écrit pour MVC 2, cet article s’applique toujours à la version actuelle de ASP.NET MVC.)

Si vous utilisez l’attribut `DataType` avec un champ date, vous devez spécifier l’attribut `DisplayFormat` également afin de garantir que le champ s’affiche correctement dans les navigateurs Chrome. Pour plus d’informations, consultez [ce thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Pour plus d’informations sur la façon de gérer d’autres formats de date dans MVC, accédez à [MVC 5 Introduction : examen des méthodes de modification et](../introduction/examining-the-edit-methods-and-edit-view.md) de la vue de modification et recherche dans la page pour &quot;l’internationalisation&quot;.

Réexécutez la page d’index des étudiants et notez que les heures ne sont plus affichées pour les dates d’inscription. Il en va de même pour toute vue qui utilise le modèle de `Student`.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

Vous pouvez également spécifier les règles de validation de données et les messages d’erreur de validation à l’aide d’attributs. L' [attribut StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) définit la longueur maximale dans la base de données et assure la validation côté client et côté serveur pour ASP.NET MVC. Vous pouvez également spécifier la longueur de chaîne minimale dans cet attribut, mais la valeur minimale n’a aucun impact sur le schéma de base de données.

Supposons que vous voulez garantir que les utilisateurs n’entrent pas plus de 50 caractères pour un nom. Pour ajouter cette limitation, ajoutez des attributs [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) aux propriétés `LastName` et `FirstMidName`, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

L’attribut [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) n’empêche pas un utilisateur d’entrer un espace blanc pour un nom. Vous pouvez utiliser l’attribut [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) pour appliquer des restrictions à l’entrée. Par exemple, le code suivant exige que le premier caractère soit en majuscule et que les autres caractères soient alphabétiques :

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

L’attribut [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) offre des fonctionnalités similaires à l’attribut [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) , mais ne fournit pas de validation côté client.

Exécutez l’application et cliquez sur l’onglet **students** . Vous recevez l’erreur suivante :

*Le modèle sauvegardant le contexte’SchoolContext’a changé depuis la création de la base de données. Envisagez d’utiliser Migrations Code First pour mettre à jour la base de données ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Le modèle de base de données a changé d’une manière qui nécessite une modification du schéma de la base de données, et Entity Framework détecté cela. Vous allez utiliser des migrations pour mettre à jour le schéma sans perdre les données que vous avez ajoutées à la base de données à l’aide de l’interface utilisateur. Si vous avez modifié les données créées par la méthode `Seed`, celles-ci seront rétablies à leur état d’origine en raison de la méthode [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) que vous utilisez dans la méthode `Seed`. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) est équivalent à une opération « upsert » de la terminologie de base de données.)

Dans la console du Gestionnaire de package, entrez les commandes suivantes :

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

La commande `add-migration` crée un fichier nommé *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*. Ce fichier contient du code dans la méthode `Up` qui met à jour la base de données pour qu’elle corresponde au modèle de données actuel. La commande `update-database` a exécuté ce code.

L’horodateur ajouté au nom de fichier de migrations est utilisé par Entity Framework pour ordonner les migrations. Vous pouvez créer plusieurs migrations avant d’exécuter la commande `update-database`, puis toutes les migrations sont appliquées dans l’ordre dans lequel elles ont été créées.

Exécutez la page **créer** et entrez un nom de plus de 50 caractères. Lorsque vous cliquez sur **créer**, la validation côté client affiche un message d’erreur : *le champ nom doit être une chaîne d’une longueur maximale de 50.*

### <a name="the-column-attribute"></a>Attribut de colonne

Vous pouvez également utiliser des attributs pour contrôler la façon dont les classes et les propriétés sont mappées à la base de données. Supposons que vous aviez utilisé le nom `FirstMidName` pour le champ de prénom, car le champ peut également contenir un deuxième prénom. Mais vous souhaitez que la colonne de base de données soit nommée `FirstName`, car les utilisateurs qui écriront des requêtes ad-hoc par rapport à la base de données sont habitués à ce nom. Pour effectuer ce mappage, vous pouvez utiliser l’attribut `Column`.

L’attribut `Column` spécifie que lorsque la base de données sera créée, la colonne de la table `Student` qui est mappée sur la propriété `FirstMidName` sera nommée `FirstName`. En d’autres termes, lorsque votre code fait référence à `Student.FirstMidName`, les données proviennent de la colonne `FirstName` de la table `Student` ou y sont mises à jour. Si vous ne spécifiez pas de noms de colonnes, ils reçoivent le même nom que le nom de la propriété.

Dans le fichier *Student.cs* , ajoutez une instruction `using` pour [System. ComponentModel. DataAnnotations. Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) et ajoutez l’attribut de nom de colonne à la propriété `FirstMidName`, comme indiqué dans le code en surbrillance suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

L’ajout de l' [attribut de colonne](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) modifie le modèle qui sauvegarde le SchoolContext, de sorte qu’il ne correspond pas à la base de données. Entrez les commandes suivantes dans le PMC pour créer une autre migration :

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

Dans **Explorateur de serveurs**, ouvrez le concepteur de tables *Student* en double-cliquant sur la table *Student* .

L’illustration suivante montre le nom de la colonne d’origine tel qu’il était avant que vous ayez appliqué les deux premières migrations. En plus du changement de nom de colonne de `FirstMidName` à `FirstName`, les deux colonnes de nom ont changé de `MAX` longueur à 50 caractères.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Vous pouvez également apporter des modifications de mappage de base de données à l’aide de l' [API Fluent](https://msdn.microsoft.com/data/jj591617), comme vous le verrez plus loin dans ce didacticiel.

> [!NOTE]
> Si vous essayez de compiler avant d’avoir fini de créer toutes les classes d’entité dans les sections suivantes, vous pouvez obtenir des erreurs de compilation.

## <a name="update-student-entity"></a>Mettre à jour l’entité Student

Dans *Models\Student.cs*, remplacez le code que vous avez ajouté précédemment par le code suivant. Les modifications apparaissent en surbrillance.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>Attribut requis

L' [attribut required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) fait des propriétés de nom les champs obligatoires. Le `Required attribute` n’est pas nécessaire pour les types valeur tels que DateTime, int, double et float. Il n’est pas possible d’assigner une valeur null aux types valeur. ils sont donc traités par nature comme des champs obligatoires. 

L'attribut `Required` doit être utilisé avec `MinimumLength` pour appliquer `MinimumLength`.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

`MinimumLength` et `Required` autorisent un espace blanc pour satisfaire la validation. Utilisez l’attribut `RegularExpression` pour une contrôle complet sur la chaîne.

### <a name="the-display-attribute"></a>Attribut d’affichage

L’attribut `Display` spécifie que la légende pour les zones de texte doit être « First Name », « Last Name », « Full Name » et « Enrollment Date », au lieu du nom de propriété dans chaque instance (qui n’a pas d’espace pour séparer les mots).

### <a name="the-fullname-calculated-property"></a>La propriété calculée FullName

`FullName` est une propriété calculée qui retourne une valeur créée par concaténation de deux autres propriétés. Par conséquent, il n’a qu’un accesseur `get` et aucune colonne `FullName` n’est générée dans la base de données.

## <a name="create-instructor-entity"></a>Créer une entité Instructor

Créez *Models\Instructor.cs*, en remplaçant le code du modèle par le code suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Notez que plusieurs propriétés sont identiques dans les entités `Student` et `Instructor`. Dans le didacticiel [Implémentation de l’héritage](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) plus loin dans cette série, vous allez refactoriser ce code pour éliminer la redondance.

Vous pouvez placer plusieurs attributs sur une seule ligne, ce qui vous permet également d’écrire la classe Instructor comme suit :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Les cours et les propriétés de navigation de l’OfficeAssignment

Les propriétés `Courses` et `OfficeAssignment` sont des propriétés de navigation. Comme expliqué précédemment, elles sont généralement définies comme [virtuelles](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) afin de pouvoir tirer parti d’une fonctionnalité de Entity Framework appelée [chargement différé](https://msdn.microsoft.com/magazine/hh205756.aspx). En outre, si une propriété de navigation peut contenir plusieurs entités, son type doit implémenter l’interface [ICollection&lt;t&gt;](https://msdn.microsoft.com/library/92t2ye13.aspx) . Par exemple, [IList&lt;t&gt;](https://msdn.microsoft.com/library/5y536ey6.aspx) qualifie, mais pas [IEnumerable&lt;t&gt;](https://msdn.microsoft.com/library/9eekhta0.aspx) , car `IEnumerable<T>` n’implémente pas [Add](https://msdn.microsoft.com/library/63ywd54z.aspx).

Un formateur peut enseigner un nombre quelconque de cours, de sorte que `Courses` est défini comme une collection d’entités `Course`.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Nos règles d’entreprise état d’un formateur ne peut avoir qu’un seul Office, donc `OfficeAssignment` est défini comme une entité `OfficeAssignment` unique (qui peut être `null` si aucun bureau n’est affecté).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-officeassignment-entity"></a>Créer une entité OfficeAssignment

Créez *Models\OfficeAssignment.cs* avec le code suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Générez le projet, qui enregistre vos modifications et vérifie que vous n’avez effectué aucune erreur de copie et de collage que le compilateur peut intercepter.

### <a name="the-key-attribute"></a>Attribut de clé

Il existe une relation un-à-zéro-ou-un entre les `Instructor` et les entités `OfficeAssignment`. Une attribution de bureau n’existe qu’en relation avec l’instructeur auquel elle est affectée. par conséquent, sa clé primaire est également sa clé étrangère pour l’entité `Instructor`. Toutefois, le Entity Framework ne peut pas reconnaître automatiquement `InstructorID` comme clé primaire de cette entité, car son nom ne suit pas la Convention d’affectation de noms `ID` ou *classname* `ID`. Par conséquent, l’attribut `Key` est utilisé pour l’identifier comme clé :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Vous pouvez également utiliser l’attribut `Key` si l’entité a sa propre clé primaire, mais que vous souhaitez attribuer à la propriété un nom différent de `classnameID` ou `ID`. Par défaut, EF traite la clé comme n’étant pas générée par la base de données, car la colonne est destinée à une relation d’identification.

### <a name="the-foreignkey-attribute"></a>Attribut ForeignKey

Lorsqu’il existe une relation un-à-zéro-ou-un ou une relation un-à-un entre deux entités (par exemple entre des `OfficeAssignment` et des `Instructor`), EF ne peut pas déterminer quelle terminaison de la relation est le principal et la terminaison qui en dépend. Les relations un-à-un ont une propriété de navigation de référence dans chaque classe à l’autre classe. L' [attribut ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) peut être appliqué à la classe dépendante pour établir la relation. Si vous omettez l' [attribut ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), vous recevez l’erreur suivante lorsque vous essayez de créer la migration :

*Impossible de déterminer la terminaison principale d’une association entre les types’ContosoUniversity. Models. OfficeAssignment’et’ContosoUniversity. Models. Instructor'. La terminaison principale de cette association doit être configurée explicitement à l’aide de l’API Fluent de la relation ou des annotations de données.*

Plus loin dans ce didacticiel, vous verrez comment configurer cette relation avec l’API Fluent.

### <a name="the-instructor-navigation-property"></a>Propriété de navigation de l’instructeur

L’entité `Instructor` a une propriété de navigation Nullable `OfficeAssignment` (car un formateur peut ne pas avoir d’affectation de bureau), et l’entité `OfficeAssignment` a une propriété de navigation `Instructor` non Nullable (car une attribution de bureau ne peut pas exister sans formateur--`InstructorID` n’accepte pas les valeurs null). Lorsqu’une entité de `Instructor` a une entité `OfficeAssignment` associée, chaque entité aura une référence à l’autre dans sa propriété de navigation.

Vous pouvez placer un attribut `[Required]` sur la propriété de navigation de l’instructeur pour indiquer qu’il doit y avoir un formateur associé, mais ce n’est pas le cas, car la clé étrangère InstructorID (qui est également la clé de cette table) n’autorise pas les valeurs NULL.

## <a name="modify-the-course-entity"></a>Modifier l’entité course

Dans *Models\Course.cs*, remplacez le code que vous avez ajouté précédemment par le code suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

L’entité course a une propriété de clé étrangère `DepartmentID` qui pointe vers l’entité `Department` associée et a une propriété de navigation `Department`. Entity Framework ne vous demande pas d’ajouter une propriété de clé étrangère à votre modèle de données lorsque vous avez une propriété de navigation pour une entité associée. EF crée automatiquement des clés étrangères dans la base de données partout où elles sont nécessaires. Mais le fait d’avoir la clé étrangère dans le modèle de données peut rendre les mises à jour plus simples et plus efficaces. Par exemple, lorsque vous extrayez une entité de cours à modifier, l’entité `Department` est null si vous ne la chargez pas. par conséquent, lorsque vous mettez à jour l’entité course, vous devez d’abord extraire l’entité `Department`. Lorsque la propriété de clé étrangère `DepartmentID` est incluse dans le modèle de données, vous n’avez pas besoin de récupérer l’entité `Department` avant d’effectuer la mise à jour.

### <a name="the-databasegenerated-attribute"></a>Attribut DatabaseGenerated

L' [attribut DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) avec le paramètre [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) sur la propriété `CourseID` spécifie que les valeurs de clé primaire sont fournies par l’utilisateur au lieu d’être générées par la base de données.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Par défaut, le Entity Framework suppose que les valeurs de clé primaire sont générées par la base de données. C’est ce que vous souhaitez dans la plupart des scénarios. Toutefois, pour les entités `Course`, vous utiliserez un numéro de cours spécifié par l’utilisateur, tel qu’une série 1000 pour un service, une série 2000 pour un autre service, et ainsi de suite.

### <a name="foreign-key-and-navigation-properties"></a>Clé étrangère et propriétés de navigation

Les propriétés de clé étrangère et les propriétés de navigation de l’entité `Course` reflètent les relations suivantes :

- Un cours est affecté à un seul département, donc il existe une clé étrangère `DepartmentID` et une propriété de navigation `Department` pour les raisons mentionnées ci-dessus.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Un cours pouvant avoir un nombre quelconque d’étudiants inscrits, la propriété de navigation `Enrollments` est une collection :

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Un cours pouvant être animé par plusieurs formateurs, la propriété de navigation `Instructors` est une collection :

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Créer l’entité Department

Créez *Models\Department.cs* avec le code suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Attribut de colonne

Précédemment, vous avez utilisé l' [attribut de colonne](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) pour modifier le mappage de nom de colonne. Dans le code de l’entité `Department`, l’attribut `Column` est utilisé pour modifier le mappage de type de données SQL afin que la colonne soit définie à l’aide du type de SQL Server [Money](https://msdn.microsoft.com/library/ms179882.aspx) dans la base de données :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Le mappage de colonnes n’est généralement pas obligatoire, car le Entity Framework choisit généralement le type de données SQL Server approprié en fonction du type CLR que vous définissez pour la propriété. Le type CLR `decimal` est mappé à un type SQL Server `decimal`. Mais dans ce cas, vous savez que la colonne contient des montants en devise et que le type de données [Money](https://msdn.microsoft.com/library/ms179882.aspx) est plus approprié. Pour plus d’informations sur les types de données CLR et leur correspondance avec les types de données SQL Server, consultez [SqlClient pour Entity FrameworkTypes](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Clé étrangère et propriétés de navigation

Les propriétés de clé étrangère et de navigation reflètent les relations suivantes :

- Un département peut ou non avoir un administrateur, et un administrateur est toujours un formateur. Par conséquent, la propriété `InstructorID` est incluse en tant que clé étrangère à l’entité `Instructor` et un point d’interrogation est ajouté après la désignation du type de `int` pour marquer la propriété comme Nullable. La propriété de navigation est nommée `Administrator` mais contient une entité `Instructor` :

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Un service peut avoir de nombreux cours, de sorte qu’il existe une `Courses` propriété de navigation :

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Par convention, Entity Framework permet la suppression en cascade pour les clés étrangères non nullables et pour les relations plusieurs à plusieurs. Cela peut entraîner des règles de suppression en cascade circulaires, qui provoqueront une exception lorsque vous essaierez d’ajouter une migration. Par exemple, si vous n’avez pas défini la propriété `Department.InstructorID` comme Nullable, vous obtenez le message d’exception suivant : « la relation référentielle entraînera une référence cyclique qui n’est pas autorisée ». Si vos règles d’entreprise requises `InstructorID` propriété pour ne pas accepter les valeurs NULL, vous devez utiliser l’instruction d’API Fluent suivante pour désactiver la suppression en cascade sur la relation :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modify-the-enrollment-entity"></a>Modifier l’entité Enrollment

 Dans *Models\Enrollment.cs*, remplacez le code que vous avez ajouté précédemment par le code suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Clé étrangère et propriétés de navigation

Les propriétés de clé étrangère et de navigation reflètent les relations suivantes :

- Un enregistrement d’inscription est utilisé pour un cours unique, si bien qu’il existe une propriété de clé étrangère `CourseID` et une propriété de navigation `Course` :

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Un enregistrement d’inscription est utilisé pour un étudiant unique, si bien qu’il existe une propriété de clé étrangère `StudentID` et une propriété de navigation `Student` :

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Relations plusieurs-à-plusieurs

Il existe une relation plusieurs-à-plusieurs entre les entités `Student` et `Course`, et l’entité `Enrollment` fonctionne comme une table de jointure plusieurs-à-plusieurs *avec une charge utile* dans la base de données. Cela signifie que la table `Enrollment` contient des données supplémentaires en plus des clés étrangères pour les tables jointes (dans ce cas, une clé primaire et une propriété `Grade`).

L’illustration suivante montre à quoi ressemblent ces relations dans un diagramme d’entité. (Ce diagramme a été généré à l’aide de l' [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); la création du diagramme ne fait pas partie du didacticiel, il est simplement utilisé ici comme illustration).

![Course_many étudiant-à-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Chaque ligne de relation a 1 à une extrémité et un astérisque (\*) à l’autre, ce qui indique une relation un-à-plusieurs.

Si la table `Enrollment` n’inclut pas d’informations sur les niveaux, elle ne doit contenir que les deux clés étrangères `CourseID` et `StudentID`. Dans ce cas, elle correspond à une table de jointure plusieurs-à-plusieurs *sans charge utile* (ou une *table de jointure pure*) dans la base de données, et il n’est pas nécessaire de créer une classe de modèle pour celle-ci. Les entités `Instructor` et `Course` ont ce type de relation plusieurs-à-plusieurs, et comme vous pouvez le voir, il n’y a aucune classe d’entité entre elles :

![Course_many Instructor-à-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Toutefois, une table de jointure est requise dans la base de données, comme indiqué dans le schéma de base de données suivant :

![Course_many Instructor-à-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

La Entity Framework crée automatiquement la table `CourseInstructor` et vous la Lisez et la mettez à jour indirectement en lisant et en mettant à jour les propriétés de navigation `Instructor.Courses` et `Course.Instructors`.

## <a name="entity-relationship-diagram"></a>Diagramme des relations d’entités

L’illustration suivante montre le diagramme que les outils Entity Framework Power Tools créent pour le modèle School complet.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

Outre les lignes de relation plusieurs-à-plusieurs (\* à \*) et les lignes de relation un-à-plusieurs (1 à \*), vous pouvez voir ici la ligne de relation un-à-zéro-ou-un (1 à 0.. 1) entre les entités `Instructor` et `OfficeAssignment` et la ligne de relation zéro-ou-un-à-plusieurs (0.. 1 à \*) entre les entités Instructor et Department.

## <a name="add-code-to-database-context"></a>Ajouter du code au contexte de base de données

Ensuite, vous allez ajouter les nouvelles entités à la classe `SchoolContext` et personnaliser une partie du mappage à l’aide d’appels d' [API Fluent](https://msdn.microsoft.com/data/jj591617) . L’API est « Fluent », car elle est souvent utilisée en regroupant une série d’appels de méthode en une seule instruction, comme dans l’exemple suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

Dans ce didacticiel, vous allez utiliser l’API Fluent uniquement pour le mappage de base de données que vous ne pouvez pas faire avec les attributs. Toutefois, vous pouvez également utiliser l’API Fluent pour spécifier la majorité des règles de mise en forme, de validation et de mappage que vous pouvez spécifier à l’aide d’attributs. Certains attributs, tels que `MinimumLength`, ne peuvent pas être appliqués avec l’API Fluent. Comme mentionné précédemment, `MinimumLength` ne modifie pas le schéma, il applique uniquement une règle de validation côté client et côté serveur.

Certains développeurs préfèrent utiliser exclusivement l’API Fluent afin de conserver des classes d’entité « propres ». Vous pouvez combiner les attributs et l’API Fluent si vous le voulez, et il existe quelques personnalisations qui peuvent être effectuées uniquement à l’aide de l’API Fluent, mais en général la pratique recommandée consiste à choisir l’une de ces deux approches et à l’utiliser constamment, autant que possible.

Pour ajouter les nouvelles entités au modèle de données et effectuer le mappage de base de données que vous n’avez pas effectué à l’aide d’attributs, remplacez le code dans *DAL\SchoolContext.cs* par le code suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

La nouvelle instruction de la méthode [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) configure la table de jointure plusieurs-à-plusieurs :

- Pour la relation plusieurs-à-plusieurs entre les entités `Instructor` et `Course`, le code spécifie les noms de table et de colonne pour la table de jointure. Code First pouvez configurer la relation plusieurs-à-plusieurs pour vous sans ce code, mais si vous ne l’appelez pas, vous obtiendrez des noms par défaut tels que `InstructorInstructorID` pour la colonne `InstructorID`.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Le code suivant fournit un exemple d’utilisation de l’API Fluent au lieu des attributs pour spécifier la relation entre les entités `Instructor` et `OfficeAssignment` :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Pour plus d’informations sur les instructions de l’API Fluent en arrière-plan, consultez le billet de blog de l' [API Fluent](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) .

## <a name="seed-database-with-test-data"></a>Remplir la base de données avec des données de test

Remplacez le code du fichier *Migrations\Configuration.cs* par le code suivant afin de fournir les données de départ pour les nouvelles entités que vous avez créées.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Comme vous l’avez vu dans le premier didacticiel, la majeure partie de ce code met simplement à jour ou crée de nouveaux objets d’entité et charge des exemples de données dans les propriétés en fonction des besoins pour les tests. Toutefois, Notez que l’entité `Course`, qui a une relation plusieurs-à-plusieurs avec l’entité `Instructor`, est gérée :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Lorsque vous créez un objet `Course`, vous initialisez la propriété de navigation `Instructors` en tant que collection vide à l’aide du `Instructors = new List<Instructor>()`de code. Cela permet d’ajouter `Instructor` entités associées à ce `Course` à l’aide de la méthode `Instructors.Add`. Si vous n’avez pas créé de liste vide, vous ne seriez pas en mesure d’ajouter ces relations, car la propriété `Instructors` est null et n’a pas de méthode `Add`. Vous pouvez également ajouter l’initialisation de la liste au constructeur.

## <a name="add-a-migration"></a>Ajouter une migration

À partir du PMC, entrez la commande `add-migration` (ne faites pas encore la commande `update-database`) :

`add-Migration ComplexDataModel`

Si vous tentiez d’exécuter la commande `update-database` à ce stade (ne le faites pas encore), vous obtiendriez l’erreur suivante :

*L’instruction ALTER TABLE est en conflit avec la contrainte de clé étrangère «FK\_dbo. Cours\_dbo. Service\_DepartmentID». Le conflit s’est produit dans la base de données « ContosoUniversity », table «dbo. Department», colonne « DepartmentID ».*

Parfois, lorsque vous exécutez des migrations avec des données existantes, vous devez insérer des données stub dans la base de données pour répondre aux contraintes de clé étrangère, et c’est ce que vous devez faire maintenant. Le code généré dans la méthode ComplexDataModel `Up` ajoute une clé étrangère non Nullable `DepartmentID` à la table `Course`. Étant donné qu’il y a déjà des lignes dans la table `Course` lors de l’exécution du code, l’opération `AddColumn` échoue, car SQL Server ne sait pas quelle valeur placer dans la colonne qui ne peut pas être null. Par conséquent, vous devez modifier le code pour donner une valeur par défaut à la nouvelle colonne et créer un service stub nommé « temp » pour agir en tant que service par défaut. Par conséquent, les lignes de `Course` existantes seront toutes associées au département « Temp » après l’exécution de la méthode `Up`. Vous pouvez les associer aux services appropriés dans la méthode `Seed`.

Modifiez le &lt;*horodateur&gt;\_fichier ComplexDataModel.cs* , commentez la ligne de code qui ajoute la colonne DepartmentID à la table course, puis ajoutez le code en surbrillance suivant (la ligne commentée est également mise en surbrillance) :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Lorsque la méthode `Seed` s’exécute, elle insère des lignes dans la table `Department` et associe les lignes `Course` existantes à ces nouvelles `Department` lignes. Si vous n’avez pas ajouté de cours dans l’interface utilisateur, vous n’avez plus besoin du service « TEMP » ou de la valeur par défaut de la colonne `Course.DepartmentID`. Pour permettre à une personne d’avoir ajouté des cours à l’aide de l’application, vous devez également mettre à jour le code de la méthode `Seed` pour vous assurer que toutes les lignes `Course` (pas seulement celles insérées par des exécutions antérieures de la méthode `Seed`) ont des valeurs `DepartmentID` valides avant de supprimer la valeur par défaut de la colonne et de supprimer le service « Temp ».

## <a name="update-the-database"></a>Mettre à jour la base de données

Une fois que vous avez fini de modifier le &lt;*horodateur&gt;\_fichier ComplexDataModel.cs* , entrez la commande `update-database` dans le PMC pour exécuter la migration.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> Il est possible d’établir d’autres erreurs lors de la migration des données et d’apporter des modifications au schéma. Si vous obtenez des erreurs de migration que vous ne pouvez pas résoudre, vous pouvez changer le nom de la base de données dans la chaîne de connexion ou supprimer la base de données. L’approche la plus simple consiste à renommer la base de données dans le fichier *Web. config* . L’exemple suivant montre le nom modifié en CU\_test :
>
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
>
> Avec une nouvelle base de données, il n’y a pas de données à migrer et la commande `update-database` est bien plus susceptible de s’exécuter sans erreur. Pour obtenir des instructions sur la suppression de la base de données, consultez Suppression d' [une base de données de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
>
> En cas d’échec, vous pouvez essayer de réinitialiser la base de données en entrant la commande suivante dans l’élément PMC :
>
> `update-database -TargetMigration:0`

Ouvrez la base de données dans **Explorateur de serveurs** comme vous l’avez fait précédemment, puis développez le nœud **tables** pour voir que toutes les tables ont été créées. (Si vous avez encore **Explorateur de serveurs** ouvrir à partir de l’heure précédente, cliquez sur le bouton **Actualiser** .)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Vous n’avez pas créé de classe de modèle pour la table `CourseInstructor`. Comme expliqué précédemment, il s’agit d’une table de jointure pour la relation plusieurs-à-plusieurs entre les entités `Instructor` et `Course`.

Cliquez avec le bouton droit sur la table `CourseInstructor` et sélectionnez **afficher les données** de la table pour vérifier qu’elle contient des données à la suite des entités `Instructor` que vous avez ajoutées à la propriété de navigation `Course.Instructors`.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Vous trouverez des liens vers d’autres ressources de Entity Framework dans l' [ASP.net accès aux données-ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Personnaliser le modèle de données
> * Entité Student mise à jour
> * Créer une entité Instructor
> * Créer une entité OfficeAssignment
> * Modification de l’entité course
> * Création de l’entité Department
> * Modification de l’entité d’inscription
> * Ajout de code dans le contexte de base de données
> * Remplir la base de données avec des données de test
> * Ajouter une migration
> * Mettre à jour la base de données

Passez à l’article suivant pour découvrir comment lire et afficher les données associées que le Entity Framework charge dans les propriétés de navigation.

> [!div class="nextstepaction"]
> [Lire les données associées](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
