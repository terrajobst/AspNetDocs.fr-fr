---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-part 2 | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications de Web Forms ASP.NET à l’aide de l’Entity Framework. L’exemple d’application est...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: bd6a2e29e6f0df04e39be29160e2e08cc99c4706
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566009"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-partie 2

par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET Web Forms à l’aide de la Entity Framework 4,0 et de Visual Studio 2010. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="the-entitydatasource-control"></a>Contrôle EntityDataSource

Dans le didacticiel précédent, vous avez créé un site Web, une base de données et un modèle de données. Dans ce didacticiel, vous travaillez avec le contrôle `EntityDataSource` fourni par ASP.NET pour faciliter l’utilisation d’un modèle de données Entity Framework. Vous allez créer un contrôle de `GridView` pour l’affichage et la modification des données des étudiants, un contrôle `DetailsView` pour l’ajout de nouveaux élèves et un contrôle `DropDownList` pour la sélection d’un service (que vous utiliserez ultérieurement pour afficher les cours associés).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Notez que dans cette application, vous n’ajouterez pas de validation d’entrée aux pages qui mettent à jour la base de données, et une partie de la gestion des erreurs n’est pas aussi robuste que nécessaire dans une application de production. Cela permet de conserver le didacticiel sur le Entity Framework et de le maintenir trop long. Pour plus d’informations sur la façon d’ajouter ces fonctionnalités à votre application, consultez Validation de l' [entrée d’utilisateur dans pages Web ASP.net](https://msdn.microsoft.com/library/7kh55542.aspx) et [gestion des erreurs dans les Pages et applications ASP.net](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Ajout et configuration du contrôle EntityDataSource

Vous commencerez par configurer un contrôle de `EntityDataSource` pour lire les entités `Person` à partir du jeu d’entités `People`.

Assurez-vous que Visual Studio est ouvert et que vous utilisez le projet que vous avez créé dans la partie 1. Si vous n’avez pas généré le projet depuis que vous avez créé le modèle de données ou depuis la dernière modification apportée à celui-ci, générez le projet maintenant. Les modifications apportées au modèle de données ne sont pas mises à la disposition du concepteur tant que le projet n’est pas généré.

Créez une nouvelle page Web à l’aide du **formulaire Web à l’aide** du modèle de page maître et nommez-la Student *. aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Spécifiez *site. Master* comme page maître. Toutes les pages que vous créez pour ces didacticiels utilisent cette page maître.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

En mode **source** , ajoutez une `h2` en-tête au contrôle `Content` nommé `Content2`, comme indiqué dans l’exemple suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

À partir de l’onglet **données** de la **boîte à outils**, faites glisser un contrôle `EntityDataSource` sur la page, déposez-le sous l’en-tête et remplacez l’ID par `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Basculez en mode **conception** , cliquez sur la balise active du contrôle de source de données, puis cliquez sur **configurer la source de données** pour lancer l’Assistant Configuration de la source de **données** .

[![image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

Dans l’étape **configurer** l’Assistant ObjectContext, sélectionnez **SchoolEntities** comme valeur pour **connexion nommée**et sélectionnez **SchoolEntities** comme valeur **DefaultContainerName** . Cliquez ensuite sur **Suivant**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Remarque : Si vous recevez la boîte de dialogue suivante à ce stade, vous devez générer le projet avant de continuer.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

Dans l’étape **configurer la sélection des données** , sélectionnez **personnes** comme valeur pour **EntitySetName**. Sous **Sélectionner**, assurez-vous que la case à cocher **Sélectionner une** ll est activée. Sélectionnez ensuite les options permettant d’activer la mise à jour et la suppression. Lorsque vous avez terminé, cliquez sur **Terminer**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Configuration des règles de base de données pour autoriser la suppression

Vous allez créer une page qui permet aux utilisateurs de supprimer les élèves de la table `Person`, qui a trois relations avec d’autres tables (`Course`, `StudentGrade`et `OfficeAssignment`). Par défaut, la base de données empêche la suppression d’une ligne dans `Person` s’il existe des lignes connexes dans l’une des autres tables. Vous pouvez supprimer manuellement les lignes associées en premier, ou vous pouvez configurer la base de données pour les supprimer automatiquement lorsque vous supprimez une ligne de `Person`. Pour les dossiers des élèves de ce didacticiel, vous allez configurer la base de données de façon à supprimer automatiquement les données associées. Étant donné que les élèves peuvent avoir des lignes associées uniquement dans la table `StudentGrade`, vous ne devez configurer qu’une seule des trois relations.

Si vous utilisez le fichier *School. mdf* que vous avez téléchargé à partir du projet qui accompagne ce didacticiel, vous pouvez ignorer cette section, car ces modifications de configuration ont déjà été effectuées. Si vous avez créé la base de données en exécutant un script, configurez la base de données en effectuant les procédures suivantes.

Dans **Explorateur de serveurs**, ouvrez le schéma de base de données que vous avez créé dans la partie 1. Cliquez avec le bouton droit sur la relation entre `Person` et `StudentGrade` (la ligne entre les tables), puis sélectionnez **Propriétés**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

Dans la fenêtre **Propriétés** , développez **spécification Insert et Update** , puis affectez à la propriété **DeleteRule** la valeur **cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Enregistrez et fermez le diagramme. Si vous êtes invité à indiquer si vous souhaitez mettre à jour la base de données, cliquez sur **Oui**.

Pour vous assurer que le modèle conserve les entités en mémoire synchronisées avec ce que fait la base de données, vous devez définir les règles correspondantes dans le modèle de données. Ouvrez *SchoolModel. edmx*, cliquez avec le bouton droit sur la ligne d’association entre `Person` et `StudentGrade`, puis sélectionnez **Propriétés**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

Dans la fenêtre **Propriétés** , affectez à **End1 OnDelete** la valeur **cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Enregistrez et fermez le fichier *SchoolModel. edmx* , puis régénérez le projet.

En général, lorsque la base de données change, vous avez plusieurs possibilités pour synchroniser le modèle :

- Pour certains types de modifications (telles que l’ajout ou l’actualisation des tables, des vues ou des procédures stockées), cliquez avec le bouton droit dans le concepteur et sélectionnez **mettre à jour le modèle à partir de la base de données** pour que le concepteur effectue automatiquement les modifications.
- Régénérez le modèle de données.
- Effectuez des mises à jour manuelles comme celle-ci.

Dans ce cas, vous pouvez avoir régénéré le modèle ou actualisé les tables affectées par la modification de la relation, mais vous devez alors réactiver la modification du nom de champ (de `FirstName` à `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Utilisation d’un contrôle GridView pour lire et mettre à jour des entités

Dans cette section, vous allez utiliser un contrôle de `GridView` pour afficher, mettre à jour ou supprimer des élèves.

Ouvrez ou basculez vers *students. aspx* et basculez en mode **Design** . À partir de l’onglet **données** de la **boîte à outils**, faites glisser un contrôle `GridView` à droite du contrôle `EntityDataSource`, nommez-le `StudentsGridView`, cliquez sur la balise active, puis sélectionnez **StudentsEntityDataSource** comme source de données.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Cliquez sur **Actualiser le schéma** (cliquez sur **Oui** si vous êtes invité à confirmer), puis cliquez sur **activer la pagination**, sur Activer le **Tri**, sur **activer la modification**et sur **activer la suppression**.

Cliquez sur **modifier les colonnes**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

Dans la zone **champs sélectionnés** , supprimez **PersonID**, **LastName**et **HireDate**. En général, vous n’affichez pas de clé d’enregistrement pour les utilisateurs, la date d’embauche n’est pas pertinente pour les élèves et vous placez les deux parties du nom dans un champ, de sorte que vous n’avez besoin que d’un des champs nom.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Sélectionnez le champ **FirstMidName** , puis cliquez sur **convertir ce champ en TemplateField**.

Procédez de la même façon pour **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Cliquez sur **OK** , puis basculez en mode **source** . Les modifications restantes seront plus faciles à effectuer directement dans le balisage. Le balisage de contrôle `GridView` se présente maintenant comme dans l’exemple suivant.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

La première colonne après le champ de commande est un champ de modèle qui affiche actuellement le prénom. Modifiez le balisage de ce champ de modèle pour qu’il ressemble à l’exemple suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

En mode d’affichage, deux contrôles de `Label` affichent le prénom et le nom. En mode édition, deux zones de texte sont fournies pour vous permettre de modifier le prénom et le nom. Comme avec les contrôles `Label` en mode d’affichage, vous utilisez des expressions `Bind` et `Eval` exactement comme vous le feriez avec les contrôles de source de données ASP.NET qui se connectent directement aux bases de données. La seule différence est que vous spécifiez des propriétés d’entité au lieu de colonnes de base de données.

La dernière colonne est un champ de modèle qui affiche la date d’inscription. Modifiez le balisage de ce champ pour qu’il ressemble à l’exemple suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

En mode affichage et édition, la chaîne de format « {0, d} » entraîne l’affichage de la date au format « date abrégée ». (Votre ordinateur peut être configuré pour afficher ce format différemment des images d’écran présentées dans ce didacticiel.)

Notez que dans chacun de ces champs de modèle, le concepteur a utilisé une expression `Bind` par défaut, mais vous l’avez changée en expression `Eval` dans les éléments `ItemTemplate`. L’expression `Bind` rend les données disponibles dans les propriétés de contrôle `GridView` au cas où vous devriez accéder aux données dans le code. Dans cette page, vous n’avez pas besoin d’accéder à ces données dans le code, vous pouvez donc utiliser `Eval`, ce qui est plus efficace. Pour plus d’informations, consultez [récupération de vos données des contrôles de données](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Révision du balisage de contrôle EntityDataSource pour améliorer les performances

Dans le balisage du contrôle `EntityDataSource`, supprimez les attributs `ConnectionString` et `DefaultContainerName` et remplacez-les par un attribut `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Il s’agit d’une modification que vous devez effectuer chaque fois que vous créez un contrôle `EntityDataSource`, sauf si vous devez utiliser une connexion différente de celle qui est codée en dur dans la classe de contexte de l’objet. L’utilisation de l’attribut `ContextTypeName` offre les avantages suivants :

- Performances améliorées. Lorsque le contrôle de `EntityDataSource` Initialise le modèle de données à l’aide des attributs `ConnectionString` et `DefaultContainerName`, il effectue un travail supplémentaire pour charger les métadonnées à chaque requête. Cela n’est pas nécessaire si vous spécifiez l’attribut `ContextTypeName`.
- Le chargement différé est activé par défaut dans les classes de contexte d’objet générées (telles que `SchoolEntities` dans ce didacticiel) dans Entity Framework 4,0. Cela signifie que les propriétés de navigation sont chargées automatiquement avec les données associées lorsque vous en avez besoin. Le chargement différé est expliqué plus en détail plus loin dans ce didacticiel.
- Toutes les personnalisations que vous avez appliquées à la classe de contexte de l’objet (dans ce cas, la classe `SchoolEntities`) sont disponibles pour les contrôles qui utilisent le contrôle `EntityDataSource`. La personnalisation de la classe de contexte de l’objet est une rubrique avancée qui n’est pas traitée dans cette série de didacticiels. Pour plus d’informations, consultez [extension des types générés par Entity Framework](https://msdn.microsoft.com/library/dd456844.aspx).

La balise est maintenant similaire à l’exemple suivant (l’ordre des propriétés peut être différent) :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

L’attribut `EnableFlattening` fait référence à une fonctionnalité qui était nécessaire dans les versions antérieures du Entity Framework, car les colonnes clés étrangères n’étaient pas exposées comme propriétés d’entité. La version actuelle permet d’utiliser des *associations de clé étrangère*, ce qui signifie que les propriétés de clé étrangère sont exposées pour toutes les associations sauf plusieurs-à-plusieurs. Si vos entités ont des propriétés de clé étrangère et aucun [type complexe](https://msdn.microsoft.com/library/bb738472.aspx), vous pouvez conserver cet attribut défini sur `False`. Ne supprimez pas l’attribut du balisage, car la valeur par défaut est `True`. Pour plus d’informations, consultez [aplatissement des objets (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Exécutez la page pour afficher la liste des étudiants et des employés (vous allez filtrer uniquement les élèves dans le didacticiel suivant). Les nom et prénom sont affichés ensemble.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Pour trier l’affichage, cliquez sur un nom de colonne.

Cliquez sur **modifier** dans une ligne. Les zones de texte s’affichent pour vous permettre de modifier le prénom et le nom.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

Le bouton **supprimer** fonctionne également. Cliquez sur supprimer pour une ligne qui a une date d’inscription et la ligne disparaît. (Les lignes sans date d’inscription représentent des instructeurs et vous pouvez obtenir une erreur d’intégrité référentielle. Dans le didacticiel suivant, vous allez filtrer cette liste pour inclure uniquement les élèves.)

## <a name="displaying-data-from-a-navigation-property"></a>Affichage de données à partir d’une propriété de navigation

Supposons à présent que vous souhaitiez connaître le nombre de cours inscrits dans chaque étudiant. Le Entity Framework fournit ces informations dans la propriété de navigation `StudentGrades` de l’entité `Person`. Étant donné que la conception de la base de données n’autorise pas l’inscription d’un étudiant dans un cours sans avoir de catégorie affectée, vous pouvez supposer qu’une ligne dans la ligne de table `StudentGrade` associée à un cours est la même que celle inscrite dans le cours. (La propriété de navigation `Courses` est uniquement pour les formateurs.)

Lorsque vous utilisez l’attribut `ContextTypeName` du contrôle `EntityDataSource`, le Entity Framework récupère automatiquement les informations d’une propriété de navigation lorsque vous accédez à cette propriété. C’est ce que l’on appelle le *chargement différé*. Toutefois, cela peut être inefficace, car cela entraîne un appel séparé à la base de données chaque fois que des informations supplémentaires sont nécessaires. Si vous avez besoin de données de la propriété de navigation pour chaque entité retournée par le contrôle `EntityDataSource`, il est plus efficace d’extraire les données associées avec l’entité elle-même dans un seul appel à la base de données. C’est ce que l’on appelle le *chargement hâtif*et vous spécifiez un chargement hâtif pour une propriété de navigation en définissant la propriété `Include` du contrôle `EntityDataSource`.

Dans *students. aspx*, vous souhaitez afficher le nombre de cours pour chaque étudiant. par conséquent, le chargement hâtif est le meilleur choix. Si vous affichiez tous les élèves, mais que vous affichez le nombre de cours uniquement pour certains d’entre eux (ce qui nécessiterait l’écriture de code en plus du balisage), le chargement différé peut être un meilleur choix.

Ouvrez ou basculez vers *students. aspx*, basculez en mode **conception** , sélectionnez `StudentsEntityDataSource`, puis dans la fenêtre **Propriétés** , affectez à la propriété **inclure** la valeur **StudentGrades**. (Si vous souhaitez obtenir plusieurs propriétés de navigation, vous pouvez spécifier leurs noms séparés par des virgules, par exemple **StudentGrades, courses**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Basculez en mode **source** . Dans le contrôle `StudentsGridView`, après le dernier élément `asp:TemplateField`, ajoutez le nouveau champ de modèle suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

Dans l’expression `Eval`, vous pouvez référencer la propriété de navigation `StudentGrades`. Étant donné que cette propriété contient une collection, elle a une propriété `Count` que vous pouvez utiliser pour afficher le nombre de cours dans lesquels l’étudiant est inscrit. Dans un didacticiel ultérieur, vous verrez comment afficher des données de propriétés de navigation qui contiennent des entités uniques au lieu de collections. (Notez que vous ne pouvez pas utiliser `BoundField` éléments pour afficher des données à partir des propriétés de navigation.)

Exécutez la page pour voir le nombre de cours inscrits dans chaque étudiant.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Utilisation d’un contrôle DetailsView pour insérer des entités

L’étape suivante consiste à créer une page qui possède un contrôle `DetailsView` qui vous permet d’ajouter de nouveaux élèves. Fermez le navigateur, puis créez une nouvelle page Web à l’aide de la page maître *site. Master* . Nommez la page *StudentsAdd. aspx*, puis basculez en mode **source** .

Ajoutez le balisage suivant pour remplacer le balisage existant pour le contrôle `Content` nommé `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Ce balisage crée un contrôle `EntityDataSource` similaire à celui que vous avez créé dans *students. aspx*, sauf qu’il permet l’insertion. Comme avec le contrôle `GridView`, les champs liés du contrôle `DetailsView` sont codés exactement comme s’il s’agissait d’un contrôle de données qui se connecte directement à une base de données, sauf qu’ils font référence à des propriétés d’entité. Dans ce cas, le contrôle de `DetailsView` est utilisé uniquement pour l’insertion de lignes. vous avez donc défini le mode par défaut sur `Insert`.

Exécutez la page et ajoutez un nouvel étudiant.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Rien ne se produit après l’insertion d’un nouvel étudiant, mais si vous exécutez maintenant *students. aspx*, vous verrez les nouvelles informations sur les étudiants.

## <a name="displaying-data-in-a-drop-down-list"></a>Affichage des données dans une liste déroulante

Dans les étapes suivantes, vous allez lier un contrôle `DropDownList` à un jeu d’entités à l’aide d’un contrôle `EntityDataSource`. Dans cette partie du didacticiel, vous ne faites pas grand-chose dans cette liste. Dans les parties suivantes, toutefois, vous allez utiliser la liste pour permettre aux utilisateurs de sélectionner un service afin d’afficher les cours associés au service.

Créez une nouvelle page Web nommée *courses. aspx*. En mode **source** , ajoutez un en-tête au contrôle `Content` nommé `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

En mode **Design** , ajoutez un contrôle `EntityDataSource` à la page comme vous l’avez fait précédemment, à l’exception de ce nom d’heure `DepartmentsEntityDataSource`. Sélectionnez **Departments** comme valeur **EntitySetName** , puis sélectionnez uniquement les propriétés **DepartmentID** et **Name** .

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

À partir de l’onglet **standard** de la **boîte à outils**, faites glisser un contrôle `DropDownList` sur la page, nommez-le `DepartmentsDropDownList`, cliquez sur la balise active, puis sélectionnez **choisir la source de données** pour démarrer l' **Assistant Configuration**de source de données.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

Dans l’étape **choisir une source de données** , sélectionnez **DepartmentsEntityDataSource** comme source de données, cliquez sur **Actualiser le schéma**, puis sélectionnez **nom** comme champ de données à afficher et **DepartmentID** comme champ de données de la valeur. Cliquez sur **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

La méthode que vous utilisez pour lier le contrôle à l’aide de l’Entity Framework est identique à celle des autres contrôles de source de données ASP.NET, à ceci près que vous spécifiez des entités et des propriétés d’entité.

Basculez en mode **source** et ajoutez « sélectionner un département : » immédiatement avant le contrôle `DropDownList`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

En guise de rappel, modifiez la balise du contrôle `EntityDataSource` à ce stade en remplaçant les attributs `ConnectionString` et `DefaultContainerName` par un attribut `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Il est souvent préférable d’attendre une fois que vous avez créé le contrôle lié aux données qui est lié au contrôle de source de données avant de modifier le balisage de contrôle `EntityDataSource`, car une fois que vous avez apporté la modification, le concepteur ne vous fournit pas une option **Actualiser le schéma** dans le contrôle lié aux données.

Exécutez la page et sélectionnez un service dans la liste déroulante.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Cela termine l’introduction à l’utilisation du contrôle `EntityDataSource`. L’utilisation de ce contrôle n’est généralement pas différente de celle des autres contrôles de source de données ASP.NET, à ceci près que vous référencez des entités et des propriétés au lieu de tables et de colonnes. La seule exception est lorsque vous souhaitez accéder aux propriétés de navigation. Dans le didacticiel suivant, vous verrez que la syntaxe que vous utilisez avec `EntityDataSource` contrôle peut également différer des autres contrôles de source de données lorsque vous filtrez, regroupez et commandez des données.

> [!div class="step-by-step"]
> [Précédent](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Suivant](the-entity-framework-and-aspnet-getting-started-part-3.md)
