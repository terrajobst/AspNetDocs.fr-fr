---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: Utilisation de TemplateFields dans le contrôle GridViewC#() | Microsoft Docs
author: rick-anderson
description: Pour garantir la flexibilité, le contrôle GridView offre le TemplateField, qui est rendu à l’aide d’un modèle. Un modèle peut inclure une combinaison de code HTML statique, de contrôles Web et de...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ec17a16d7bb487d1c5cacf2d5971bbeffc1ba031
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74628053"
---
# <a name="using-templatefields-in-the-gridview-control-c"></a>Utilisation de TemplateFields dans le contrôle GridView (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe) ou [Télécharger le PDF](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> Pour garantir la flexibilité, le contrôle GridView offre le TemplateField, qui est rendu à l’aide d’un modèle. Un modèle peut inclure une combinaison de code HTML statique, de contrôles Web et de syntaxe DataBinding. Dans ce didacticiel, nous allons examiner comment utiliser TemplateField pour obtenir un degré de personnalisation plus élevé avec le contrôle GridView.

## <a name="introduction"></a>Introduction

Le GridView est composé d’un ensemble de champs qui indiquent les propriétés de la `DataSource` doivent être incluses dans la sortie rendue, ainsi que la façon dont les données sont affichées. Le type de champ le plus simple est le BoundField, qui affiche une valeur de données sous forme de texte. Les autres types de champs affichent les données à l’aide d’autres éléments HTML. Le CheckBoxField, par exemple, est rendu sous la forme d’une case à cocher dont l’état activé dépend de la valeur d’un champ de données spécifié ; le ImageField restitue une image dont la source d’image est basée sur un champ de données spécifié. Les liens hypertexte et les boutons dont l’état dépend d’une valeur de champ de données sous-jacente peuvent être rendus à l’aide des types de champ HyperLinkField et ButtonField.

Tandis que les types de champs CheckBoxField, ImageField, HyperLinkField et ButtonField autorisent une autre vue des données, ils sont toujours assez limités en ce qui concerne la mise en forme. Un CheckBoxField peut uniquement afficher une seule case à cocher, tandis qu’un ImageField peut uniquement afficher une seule image. Que se passe-t-il si un champ particulier doit afficher du texte, une case à cocher *et* une image, tous basés sur des valeurs de champ de données différentes ? Ou que se passe-t-il si nous voulions afficher les données à l’aide d’un contrôle Web autre que la case à cocher, l’image, le lien hypertexte ou le bouton ? En outre, BoundField limite son affichage à un seul champ de données. Que se passe-t-il si nous voulions afficher deux valeurs de champs de données ou plus dans une seule colonne GridView ?

Pour prendre en charge ce niveau de flexibilité, le contrôle GridView offre le TemplateField, qui est rendu à l’aide d’un *modèle*. Un modèle peut inclure une combinaison de code HTML statique, de contrôles Web et de syntaxe DataBinding. En outre, le TemplateField possède un large éventail de modèles qui peuvent être utilisés pour personnaliser le rendu pour différentes situations. Par exemple, le `ItemTemplate` est utilisé par défaut pour restituer la cellule pour chaque ligne, mais le modèle `EditItemTemplate` peut être utilisé pour personnaliser l’interface lors de la modification de données.

Dans ce didacticiel, nous allons examiner comment utiliser TemplateField pour obtenir un degré de personnalisation plus élevé avec le contrôle GridView. Dans le [didacticiel précédent](custom-formatting-based-upon-data-cs.md) , nous avons vu comment personnaliser la mise en forme en fonction des données sous-jacentes à l’aide des gestionnaires d’événements `DataBound` et `RowDataBound`. Pour personnaliser la mise en forme en fonction des données sous-jacentes, vous pouvez également appeler des méthodes de mise en forme à partir d’un modèle. Nous examinerons également cette technique dans ce didacticiel.

Pour ce didacticiel, nous allons utiliser TemplateFields pour personnaliser l’apparence d’une liste d’employés. Plus précisément, nous allons répertorier tous les employés, mais ils affichent le prénom et le nom de l’employé dans une colonne, la date d’embauche dans un contrôle de calendrier et une colonne d’État qui indique le nombre de jours pendant lesquels ils ont été employés au sein de l’entreprise.

[![trois TemplateFields sont utilisés pour personnaliser l’affichage](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**Figure 1**: trois TemplateFields sont utilisés pour personnaliser l’affichage ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image3.png))

## <a name="step-1-binding-the-data-to-the-gridview"></a>Étape 1 : liaison des données au GridView

Dans les scénarios de création de rapports où vous devez utiliser TemplateFields pour personnaliser l’apparence, je trouve qu’il est plus facile de commencer par créer un contrôle GridView qui contient uniquement BoundFields en premier, puis d’ajouter de nouveaux TemplateFields ou de convertir le BoundFields existant en TemplateFields en fonction des besoins. Par conséquent, commençons ce didacticiel en ajoutant un GridView à la page par le biais du concepteur et en le liant à un ObjectDataSource qui retourne la liste des employés. Ces étapes vont créer un GridView avec BoundFields pour chacun des champs Employee.

Ouvrez la page `GridViewTemplateField.aspx` et faites glisser un contrôle GridView de la boîte à outils vers le concepteur. À partir de la balise active de GridView, choisissez d’ajouter un nouveau contrôle ObjectDataSource qui appelle la méthode `GetEmployees()` de la classe `EmployeesBLL`.

[![ajouter un nouveau contrôle ObjectDataSource qui appelle la méthode GetEmployees ()](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**Figure 2**: ajouter un nouveau contrôle ObjectDataSource qui appelle la méthode `GetEmployees()` ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image6.png))

La liaison du GridView de cette manière ajoute automatiquement un BoundField pour chacune des propriétés de l’employé : `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`et `Country`. Pour ce rapport, n’hésitez pas à afficher les propriétés `EmployeeID`, `ReportsTo`ou `Country`. Pour supprimer ces BoundFields, vous pouvez :

- Utilisez la boîte de dialogue champs. pour afficher cette boîte de dialogue, cliquez sur le lien modifier les colonnes à partir de la balise active de GridView. Ensuite, sélectionnez le BoundFields dans la liste inférieure gauche, puis cliquez sur le bouton X rouge pour supprimer le BoundField.
- Modifiez la syntaxe déclarative du contrôle GridView manuellement à partir de la vue source, supprimez l’élément `<asp:BoundField>` pour le BoundField que vous souhaitez supprimer.

Une fois que vous avez supprimé le `EmployeeID`, `ReportsTo`et `Country` BoundFields, le balisage de votre GridView doit ressembler à ceci :

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

Prenez un moment pour voir notre progression dans un navigateur. À ce stade, vous devriez voir une table avec un enregistrement pour chaque employé et quatre colonnes : une pour le nom de l’employé, une pour son prénom, une pour son titre et une pour la date d’embauche.

[![les champs LastName, FirstName, title et HireDate sont affichés pour chaque employé](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**Figure 3**: les champs `LastName`, `FirstName`, `Title`et `HireDate` s’affichent pour chaque employé ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image9.png))

## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Étape 2 : affichage du prénom et du nom dans une seule colonne

Actuellement, le prénom et le nom de chaque employé sont affichés dans une colonne distincte. Il peut être intéressant de les combiner en une seule colonne à la place. Pour ce faire, nous devons utiliser un TemplateField. Nous pouvons ajouter un nouveau TemplateField, y ajouter le balisage et la syntaxe DataBinding nécessaires, puis supprimer le `FirstName` et `LastName` BoundFields, ou nous pouvons convertir le `FirstName` BoundField en TemplateField, modifier le TemplateField pour inclure la valeur `LastName`, puis supprimer le `LastName` BoundField.

Les deux approches ont le même résultat, mais personnellement je souhaite convertir BoundFields en TemplateFields lorsque cela est possible, car la conversion ajoute automatiquement une `ItemTemplate` et `EditItemTemplate` avec les contrôles Web et la syntaxe DataBinding pour imiter l’apparence et les fonctionnalités de BoundField. L’avantage est que nous devrons travailler moins sur le TemplateField, car le processus de conversion aura effectué une partie du travail pour nous.

Pour convertir un BoundField existant en TemplateField, cliquez sur le lien modifier les colonnes à partir de la balise active du contrôle GridView, en acmenant à la boîte de dialogue champs. Sélectionnez le BoundField à convertir dans la liste dans l’angle inférieur gauche, puis cliquez sur le lien « convertir ce champ en TemplateField » dans le coin inférieur droit.

[![convertir un BoundField en TemplateField à partir de la boîte de dialogue champs](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**Figure 4**: convertir un BoundField en TemplateField à partir de la boîte de dialogue champs ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image12.png))

Poursuivez et convertissez le `FirstName` BoundField en TemplateField. Après cette modification, il n’y a aucune différence perceptive dans le concepteur. Cela est dû au fait que la conversion de BoundField en TemplateField crée un TemplateField qui maintient l’apparence du BoundField. Bien qu’il n’y ait aucune différence visuelle à ce stade dans le concepteur, ce processus de conversion a remplacé la syntaxe déclarative-`<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` de BoundField avec la syntaxe de TemplateField suivante :

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

Comme vous pouvez le voir, le TemplateField est constitué de deux modèles, une `ItemTemplate` qui a une étiquette dont la propriété `Text` est définie sur la valeur du champ de données `FirstName`, et d’un `EditItemTemplate` avec un contrôle TextBox dont la propriété `Text` a également la valeur du champ de données `FirstName`. La syntaxe DataBinding-`<%# Bind("fieldName") %>`-indique que le champ de données *`fieldName`* est lié à la propriété de contrôle Web spécifiée.

Pour ajouter la valeur du champ de données `LastName` à ce TemplateField, nous devons ajouter un autre contrôle Web Label dans le `ItemTemplate` et lier sa propriété `Text` à `LastName`. Cela peut être effectué manuellement ou par le biais du concepteur. Pour effectuer cette opération manuellement, ajoutez simplement la syntaxe déclarative appropriée au `ItemTemplate`:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

Pour l’ajouter par le biais du concepteur, cliquez sur le lien modifier les modèles à partir de la balise active de GridView. Cette opération affiche l’interface de modification de modèle de GridView. Dans, la balise active de cette interface est une liste des modèles dans le GridView. Étant donné que nous n’avons qu’un seul TemplateField à ce stade, les seuls modèles répertoriés dans la liste déroulante sont les modèles pour le `FirstName` TemplateField, ainsi que les `EmptyDataTemplate` et `PagerTemplate`. Le modèle `EmptyDataTemplate`, s’il est spécifié, est utilisé pour restituer la sortie de GridView s’il n’y a aucun résultat dans les données liées au GridView ; le `PagerTemplate`, s’il est spécifié, est utilisé pour restituer l’interface de pagination pour un GridView qui prend en charge la pagination.

[![les modèles du contrôle GridView peuvent être modifiés par le biais du concepteur](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**Figure 5**: les modèles du contrôle GridView peuvent être modifiés par le biais du concepteur ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image15.png))

Pour afficher également les `LastName` dans le `FirstName` TemplateField, faites glisser le contrôle Label de la boîte à outils vers le `ItemTemplate` de `FirstName` TemplateField dans l’interface de modification de modèle de GridView.

[![ajouter un contrôle Web Label au ItemTemplate du prénom du TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**Figure 6**: ajouter un contrôle Web Label au `FirstName` ItemTemplate du TemplateField ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image18.png))

À ce stade, le contrôle Web label ajouté à TemplateField a sa propriété `Text` définie sur « label ». Nous devons modifier cela afin que cette propriété soit liée à la valeur du champ de données `LastName` à la place. Pour ce faire, cliquez sur la balise active du contrôle Label et choisissez l’option modifier les DataBindings.

[![choisissez l’option modifier les DataBindings dans la balise active de l’étiquette](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**Figure 7**: choisissez l’option modifier les DataBindings dans la balise active de l’étiquette ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image21.png))

La boîte de dialogue DataBindings s’affiche. À partir de là, vous pouvez sélectionner la propriété pour participer à la liaison de données à partir de la liste à gauche et choisir le champ auquel lier les données dans la liste déroulante à droite. Choisissez la propriété `Text` à gauche et le champ `LastName` à droite, puis cliquez sur OK.

[![lier la propriété Text au champ de données LastName](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**Figure 8**: lier la propriété `Text` au champ de données `LastName` ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image24.png))

> [!NOTE]
> La boîte de dialogue DataBindings vous permet d’indiquer s’il faut effectuer une liaison de liaison bidirectionnelle. Si vous laissez cette option désactivée, la syntaxe de liaison de liaison `<%# Eval("LastName")%>` est utilisée à la place de `<%# Bind("LastName")%>`. Les deux approches conviennent parfaitement à ce didacticiel. La liaison de données bidirectionnelle devient importante lors de l’insertion et de la modification de données. Toutefois, pour afficher simplement des données, l’une ou l’autre approche fonctionne tout aussi bien. Nous discuterons de la liaison de données bidirectionnelle en détail dans les prochains didacticiels.

Prenez un moment pour afficher cette page par le biais d’un navigateur. Comme vous pouvez le voir, le GridView contient toujours quatre colonnes ; Toutefois, la colonne *`FirstName` répertorie à présent les valeurs* des champs de données `FirstName` et `LastName`.

[![les valeurs FirstName et LastName sont affichées dans une seule colonne](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**Figure 9**: les valeurs `FirstName` et `LastName` sont affichées dans une seule colonne ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image27.png))

Pour effectuer cette première étape, supprimez le `LastName` BoundField et renommez la propriété `HeaderText` de `FirstName` TemplateField en « Name ». Une fois ces modifications effectuées, le balisage déclaratif de GridView doit ressembler à ce qui suit :

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]

[![le prénom et le nom de chaque employé sont affichés dans une seule colonne](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**Figure 10**: le prénom et le nom de chaque employé sont affichés dans une colonne ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image30.png))

## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Étape 3 : utilisation du contrôle Calendar pour afficher le champ`HiredDate`

L’affichage d’une valeur de champ de données sous forme de texte dans un GridView est aussi simple que l’utilisation d’un BoundField. Pour certains scénarios, toutefois, les données sont mieux exprimées à l’aide d’un contrôle Web particulier plutôt que simplement du texte. Une telle personnalisation de l’affichage des données est possible avec TemplateFields. Par exemple, au lieu d’afficher la date d’embauche de l’employé sous forme de texte, nous pourrions afficher un calendrier (à l’aide [du contrôle calendrier](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) avec la date d’embauche mise en surbrillance.

Pour ce faire, commencez par convertir le `HiredDate` BoundField en TemplateField. Il vous suffit d’accéder à la balise active de GridView et de cliquer sur le lien modifier les colonnes pour afficher la boîte de dialogue champs. Sélectionnez l' `HiredDate` BoundField et cliquez sur « convertir ce champ en TemplateField ».

[![convertir HiredDate BoundField en TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**Figure 11**: convertir le `HiredDate` BoundField en TemplateField ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image33.png))

Comme nous l’avons vu à l’étape 2, cela remplacera le BoundField par un TemplateField qui contient une `ItemTemplate` et `EditItemTemplate` avec une étiquette et une zone de texte dont les propriétés `Text` sont liées à la valeur `HiredDate` à l’aide de la syntaxe DataBinding `<%# Bind("HiredDate")%>`.

Pour remplacer le texte par un contrôle de calendrier, modifiez le modèle en supprimant l’étiquette et en ajoutant un contrôle de calendrier. Dans le concepteur, sélectionnez Modifier les modèles à partir de la balise active de GridView et choisissez le `ItemTemplate` de `HireDate` TemplateField dans la liste déroulante. Ensuite, supprimez le contrôle Label et faites glisser un contrôle Calendar de la boîte à outils vers l’interface de modification de modèle.

[![ajouter un contrôle calendrier au ItemTemplate HireDate du TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**Figure 12**: ajouter un contrôle calendrier au `ItemTemplate` de `HireDate` TemplateField ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image36.png))

À ce stade, chaque ligne du GridView contient un contrôle calendrier dans son `HiredDate` TemplateField. Toutefois, la valeur réelle de `HiredDate` de l’employé n’est pas définie n’importe où dans le contrôle Calendar, ce qui amène chaque contrôle Calendar à indiquer par défaut le mois et la date actuels. Pour y remédier, nous devons affecter les `HiredDate` de chaque employé aux propriétés [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) et [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) du contrôle Calendar.

À partir de la balise active du contrôle Calendar, choisissez Modifier les DataBindings. Liez ensuite les propriétés `SelectedDate` et `VisibleDate` au champ de données `HiredDate`.

[![lier les propriétés SelectedDate et VisibleDate au champ de données HiredDate](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**Figure 13**: lier les propriétés `SelectedDate` et `VisibleDate` au champ de données `HiredDate` ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image39.png))

> [!NOTE]
> La date sélectionnée du contrôle Calendar ne doit pas nécessairement être visible. Par exemple, un calendrier peut avoir<sup>le 1er août</sup>, 1999 comme date sélectionnée, mais il doit indiquer le mois et l’année en cours. La date et la date visibles sélectionnées sont spécifiées par les propriétés `SelectedDate` et `VisibleDate` du contrôle Calendar. Étant donné que nous voulons sélectionner le `HiredDate` de l’employé et vous assurer qu’il est indiqué, nous devons lier ces deux propriétés au champ de données `HireDate`.

Lorsque vous affichez la page dans un navigateur, le calendrier affiche maintenant le mois de la date d’embauche de l’employé et sélectionne cette date particulière.

[![le HiredDate de l’employé est affiché dans le contrôle calendrier](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**Figure 14**: le `HiredDate` de l’employé est affiché dans le contrôle calendrier ([cliquez pour afficher l’image en plein écran](using-templatefields-in-the-gridview-control-cs/_static/image42.png))

> [!NOTE]
> Contrairement à tous les exemples que nous avons vus jusqu’à présent, pour ce didacticiel, nous n’avons *pas* défini `EnableViewState` propriété sur `false` pour ce GridView. La raison de cette décision est que le fait de cliquer sur les dates du contrôle calendrier entraîne une publication (postback), en définissant la date sélectionnée du calendrier sur la date que vous venez de cliquer. Toutefois, si l’état d’affichage de GridView est désactivé, sur chaque publication (postback), les données du GridView sont reliées à la source de données sous-jacente, ce qui a pour effet de *rétablir* la date sélectionnée du calendrier sur le `HireDate`de l’employé, remplaçant la date choisie par l’utilisateur.

Pour ce didacticiel, il s’agit d’une discussion discutable dans la mesure où l’utilisateur n’est pas en mesure de mettre à jour le `HireDate`de l’employé. Il serait probablement préférable de configurer le contrôle calendrier afin que ses dates ne soient pas sélectionnables. Quoi qu’il en soit, ce didacticiel montre que dans certains cas, l’état d’affichage doit être activé afin de fournir certaines fonctionnalités.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Étape 4 : indication du nombre de jours de travail de l’employé pour l’entreprise

Jusqu’à présent, nous avons vu deux applications de TemplateFields :

- Combinaison de plusieurs valeurs de champ de données en une seule colonne, et
- Expression d’une valeur de champ de données à l’aide d’un contrôle Web au lieu d’un texte

Une troisième utilisation de TemplateFields est l’affichage des métadonnées relatives aux données sous-jacentes du contrôle GridView. En plus d’afficher les dates d’embauche des employés, par exemple, nous pouvons souhaiter avoir une colonne qui indique le nombre total de jours pendant lesquels ils se trouvent sur le travail.

Une autre utilisation de TemplateFields se produit dans les scénarios où les données sous-jacentes doivent être affichées différemment dans le rapport de page Web que dans le format dans lequel elles sont stockées dans la base de données. Imaginez que la table `Employees` comportait un champ `Gender` qui stockait le caractère `M` ou `F` pour indiquer le sexe de l’employé. Lorsque vous affichez ces informations dans une page Web, nous pouvons souhaiter afficher le sexe « mâle » ou « féminin », par opposition à « M » ou « F ».

Ces deux scénarios peuvent être gérés en créant une *méthode de mise en forme* dans la classe code-behind de la page ASP.net (ou dans une bibliothèque de classes distincte, implémentée en tant que méthode `static`) qui est appelée à partir du modèle. Une telle méthode de mise en forme est appelée à partir du modèle à l’aide de la même syntaxe DataBinding vue précédemment. La méthode de mise en forme peut prendre n’importe quel nombre de paramètres, mais elle doit retourner une chaîne. Cette chaîne retournée est le code HTML injecté dans le modèle.

Pour illustrer ce concept, nous allons compléter notre didacticiel pour afficher une colonne qui indique le nombre total de jours pendant lesquels un employé a travaillé sur le travail. Cette méthode de mise en forme prend un objet `Northwind.EmployeesRow` et retourne le nombre de jours pendant lesquels l’employé a été employé comme chaîne. Cette méthode peut être ajoutée à la classe code-behind de la page ASP.NET, mais *doit* être marquée comme `protected` ou `public` afin d’être accessible à partir du modèle.

[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

Étant donné que le champ `HiredDate` peut contenir `NULL` valeurs de base de données, nous devons d’abord vérifier que la valeur n’est pas `NULL` avant de procéder au calcul. Si la valeur `HiredDate` est `NULL`, nous renvoyons simplement la chaîne « Unknown ». Si ce n’est pas `NULL`, nous calculons la différence entre l’heure actuelle et la valeur de `HiredDate` et retournent le nombre de jours.

Pour utiliser cette méthode, vous devez l’appeler à partir d’un TemplateField dans le GridView à l’aide de la syntaxe DataBinding. Commencez par ajouter un nouveau TemplateField au GridView en cliquant sur le lien modifier les colonnes dans la balise active de GridView et en ajoutant un nouveau TemplateField.

[![ajouter un nouveau TemplateField au contrôle GridView](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**Figure 15**: ajouter un nouveau TemplateField au contrôle GridView ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image45.png))

Affectez à la propriété de `HeaderText` de ce nouveau TemplateField la valeur « jours sur le travail » et la propriété de `HorizontalAlign` de son `ItemStyle`de la `Center`. Pour appeler la méthode `DisplayDaysOnJob` à partir du modèle, ajoutez un `ItemTemplate` et utilisez la syntaxe DataBinding suivante :

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem` retourne un objet `DataRowView` correspondant à l’enregistrement `DataSource` lié au `GridViewRow`. Sa propriété `Row` retourne la `Northwind.EmployeesRow`fortement typée, qui est transmise à la méthode `DisplayDaysOnJob`. Cette syntaxe DataBinding peut apparaître directement dans le `ItemTemplate` (comme indiqué dans la syntaxe déclarative ci-dessous) ou peut être assignée à la propriété `Text` d’un contrôle Web d’étiquette.

> [!NOTE]
> En guise d’alternative, au lieu de passer une instance `EmployeesRow`, nous pourrions simplement transmettre la valeur `HireDate` à l’aide de `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Toutefois, la méthode `Eval` retourne une `object`. nous devons donc modifier notre signature de méthode `DisplayDaysOnJob` pour accepter un paramètre d’entrée de type `object`à la place. Nous ne pouvons pas convertir en aveugle le `Eval("HireDate")` appel à un `DateTime`, car la colonne `HireDate` de la table `Employees` peut contenir des valeurs `NULL`. Par conséquent, nous devons accepter un `object` comme paramètre d’entrée pour la méthode `DisplayDaysOnJob`, vérifier s’il avait une valeur de `NULL` de base de données (qui peut être accomplie à l’aide de `Convert.IsDBNull(objectToCheck)`), puis continuer en conséquence.

En raison de ces subtilités, j’ai choisi de transmettre la totalité de l’instance de `EmployeesRow`. Dans le didacticiel suivant, nous verrons un exemple plus adapté à l’utilisation de la syntaxe `Eval("columnName")` pour passer un paramètre d’entrée dans une méthode de mise en forme.

L’exemple suivant illustre la syntaxe déclarative de notre GridView après l’ajout du TemplateField et la méthode `DisplayDaysOnJob` appelée à partir du `ItemTemplate`:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

La figure 16 illustre le didacticiel terminé, lorsqu’il est affiché dans un navigateur.

[![le nombre de jours d’affichage de l’employé sur le travail](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**Figure 16**: nombre de jours d’affichage de l’employé ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-cs/_static/image48.png))

## <a name="summary"></a>Récapitulatif

Le TemplateField dans le contrôle GridView offre une plus grande souplesse dans l’affichage des données que celles disponibles avec les autres contrôles de champ. Les TemplateFields sont idéaux pour les situations où :

- Plusieurs champs de données doivent être affichés dans une colonne GridView
- Les données sont mieux exprimées à l’aide d’un contrôle Web plutôt que du texte brut
- La sortie dépend des données sous-jacentes, telles que l’affichage des métadonnées ou le reformatage des données.

Outre la personnalisation de l’affichage des données, TemplateFields est également utilisé pour personnaliser les interfaces utilisateur utilisées pour la modification et l’insertion de données, comme nous le verrons dans les prochains didacticiels.

Les deux didacticiels suivants poursuivent l’exploration des modèles, en commençant par un examen de l’utilisation de TemplateFields dans un DetailsView. Nous allons ensuite passer au FormView, qui utilise des modèles plutôt que des champs pour fournir une plus grande flexibilité dans la disposition et la structure des données.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Dan Jagers. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](custom-formatting-based-upon-data-cs.md)
> [Suivant](using-templatefields-in-the-detailsview-control-cs.md)
