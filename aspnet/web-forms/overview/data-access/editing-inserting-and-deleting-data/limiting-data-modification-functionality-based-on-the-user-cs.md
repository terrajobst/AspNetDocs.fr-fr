---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
title: Limitation des fonctionnalités de modification des données en fonctionC#de l’utilisateur () | Microsoft Docs
author: rick-anderson
description: Dans une application Web qui permet aux utilisateurs de modifier des données, différents comptes d’utilisateur peuvent avoir des privilèges d’édition de données différents. Dans ce didacticiel, nous allons examiner la manière dont t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2b251c82-77cf-4e36-baa9-b648eddaa394
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
msc.type: authoredcontent
ms.openlocfilehash: c3cacaddb7e9b493ba39718f41dcaab360d36fd9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78591650"
---
# <a name="limiting-data-modification-functionality-based-on-the-user-c"></a>Limitation des fonctionnalités de modification des données en fonction de l’utilisateur (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_CS.exe) ou [Télécharger le PDF](limiting-data-modification-functionality-based-on-the-user-cs/_static/datatutorial23cs1.pdf)

> Dans une application Web qui permet aux utilisateurs de modifier des données, différents comptes d’utilisateur peuvent avoir des privilèges d’édition de données différents. Dans ce didacticiel, nous allons examiner comment ajuster dynamiquement les fonctionnalités de modification des données en fonction de l’utilisateur visité.

## <a name="introduction"></a>Introduction

Plusieurs applications Web prennent en charge les comptes d’utilisateur et fournissent différentes options, rapports et fonctionnalités en fonction de l’utilisateur connecté. Par exemple, avec nos didacticiels, nous pouvons autoriser les utilisateurs des sociétés fournisseurs à se connecter au site et à mettre à jour des informations générales sur leurs produits (leur nom et leur quantité par unité), ainsi que des informations sur le fournisseur, telles que le nom de la société. adresse, informations sur la personne à contacter, etc. En outre, nous pouvons souhaiter inclure des comptes d’utilisateur pour les personnes de notre société afin qu’ils puissent se connecter et mettre à jour les informations sur les produits, comme les unités en stock, le niveau de réorganisation, etc. Notre application Web peut également autoriser les utilisateurs anonymes à visiter (les personnes qui n’ont pas ouvert de session), mais les limiterait à afficher simplement les données. Avec un tel système de compte d’utilisateur en place, nous souhaitons que les contrôles Web de données de nos pages ASP.NET offrent les fonctionnalités d’insertion, de modification et de suppression appropriées pour l’utilisateur actuellement connecté.

Dans ce didacticiel, nous allons examiner comment ajuster dynamiquement les fonctionnalités de modification des données en fonction de l’utilisateur visité. En particulier, nous allons créer une page qui affiche les informations sur les fournisseurs dans un DetailsView modifiable avec un GridView qui répertorie les produits fournis par le fournisseur. Si l’utilisateur visitant la page provient de notre entreprise, il peut : afficher les informations sur les fournisseurs. modifier leur adresse ; et de modifier les informations pour tous les produits fournis par le fournisseur. Toutefois, si l’utilisateur provient d’une société particulière, il peut uniquement afficher et modifier ses propres informations d’adresse et ne peut modifier que les produits qui n’ont pas été marqués comme abandonnés.

[![un utilisateur de notre société peut modifier les informations sur les fournisseurs](limiting-data-modification-functionality-based-on-the-user-cs/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image1.png)

**Figure 1**: un utilisateur de notre société peut modifier les informations sur les fournisseurs ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image3.png))

[![un utilisateur d’un fournisseur particulier peut uniquement afficher et modifier ses informations](limiting-data-modification-functionality-based-on-the-user-cs/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image4.png)

**Figure 2**: un utilisateur d’un fournisseur particulier peut uniquement afficher et modifier ses informations ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image6.png))

Commençons !

> [!NOTE]
> Le système d’appartenance ASP.NET 2,0 s fournit une plateforme standardisée et extensible pour la création, la gestion et la validation des comptes d’utilisateur. Étant donné qu’un examen du système d’appartenance n’entre pas dans le cadre de ces didacticiels, il est préférable de « simuler » l’appartenance à des visiteurs anonymes pour choisir s’ils proviennent d’un fournisseur particulier ou de notre entreprise. Pour plus d’informations sur l’adhésion, reportez-vous à la série d’articles examen de l' [appartenance, des rôles et des profils de ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) .

## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Étape 1 : permettre à l’utilisateur de spécifier ses droits d’accès

Dans une application Web réelle, les informations sur le compte des utilisateurs incluent la nécessité de travailler pour notre société ou pour un fournisseur particulier, et ces informations sont accessibles par programmation à partir de nos pages de ASP.NET une fois que l’utilisateur s’est connecté au site. Ces informations peuvent être capturées via le système de rôles ASP.NET 2,0 s, en tant qu’informations de compte au niveau de l’utilisateur via le système de profil ou via des moyens personnalisés.

Étant donné que l’objectif de ce didacticiel est de montrer comment ajuster les fonctionnalités de modification des données en fonction de l’utilisateur connecté et qu’il n’est pas destiné à présenter l’appartenance, les rôles et les systèmes de profil ASP.NET 2,0 s, nous allons utiliser un mécanisme très simple pour déterminer le fonctionnalités de l’utilisateur visitant la page : contrôle DropDownList à partir duquel l’utilisateur peut indiquer s’il doit être en mesure d’afficher et de modifier les informations sur les fournisseurs ou, le cas échéant, les informations spécifiques du fournisseur qu’ils peuvent afficher et modifier. Si l’utilisateur indique qu’il peut afficher et modifier toutes les informations sur le fournisseur (valeur par défaut), il peut parcourir tous les fournisseurs, modifier les informations sur l’adresse du fournisseur et modifier le nom et la quantité par unité pour tout produit fourni par le fournisseur sélectionné. Si l’utilisateur indique qu’il peut uniquement afficher et modifier un fournisseur particulier, il peut uniquement afficher les détails et les produits pour ce fournisseur et ne peut mettre à jour que les informations de nom et de quantité par unité pour les produits qui *ne sont pas* interrompus.

Dans le cadre de ce didacticiel, la première étape consiste à créer ce DropDownList et à le remplir avec les fournisseurs dans le système. Ouvrez la page `UserLevelAccess.aspx` dans le dossier `EditInsertDelete`, ajoutez un contrôle DropDownList dont la propriété `ID` est définie sur `Suppliers`et liez ce DropDownList à un nouvel ObjectDataSource nommé `AllSuppliersDataSource`.

[![créer un ObjectDataSource nommé AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-cs/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image7.png)

**Figure 3**: créer un ObjectDataSource nommé `AllSuppliersDataSource` ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image9.png))

Comme nous voulons que ce DropDownList comprenne tous les fournisseurs, configurez le ObjectDataSource pour appeler la méthode de `GetSuppliers()` de la classe `SuppliersBLL`. Assurez-vous également que la méthode d' `Update()` ObjectDataSource est mappée à la méthode de `SuppliersBLL` classe s `UpdateSupplierAddress`, car cet ObjectDataSource sera également utilisé par le contrôle DetailsView que nous ajouterons à l’étape 2.

Après avoir terminé l’Assistant ObjectDataSource, effectuez les étapes en configurant le `Suppliers` DropDownList de telle sorte qu’il affiche le champ de données `CompanyName` et utilise le champ de données `SupplierID` comme valeur pour chaque `ListItem`.

[![configurer la DropDownList des fournisseurs pour utiliser les champs de données CompanyName et RéfFournisseur](limiting-data-modification-functionality-based-on-the-user-cs/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image10.png)

**Figure 4**: configurer la `Suppliers` DropDownList pour utiliser les champs de données `CompanyName` et `SupplierID` ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image12.png))

À ce stade, le DropDownList répertorie les noms de société des fournisseurs dans la base de données. Toutefois, nous devons également inclure l’option « Afficher/modifier tous les fournisseurs » sur le DropDownList. Pour ce faire, affectez à la propriété `Suppliers` DropDownList s `AppendDataBoundItems` la valeur `true` puis ajoutez une `ListItem` dont la propriété `Text` est « afficher/modifier tous les fournisseurs » et dont la valeur est `-1`. Cela peut être ajouté directement par le biais du balisage déclaratif ou via le concepteur en accédant au Fenêtre Propriétés et en cliquant sur les ellipses dans la propriété DropDownList s `Items`.

> [!NOTE]
> Reportez-vous au didacticiel [*maître/détail avec un didacticiel DropDownList*](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) pour une discussion plus détaillée sur l’ajout d’un élément Sélectionner tout à un contrôle DropDownList lié aux données.

Une fois la propriété `AppendDataBoundItems` définie et le `ListItem` ajouté, le balisage déclaratif s doit ressembler à ceci :

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample1.aspx)]

La figure 5 illustre une capture d’écran de la progression actuelle, affichée dans un navigateur.

[![la DropDownList des fournisseurs contient un ListItem afficher tout, plus un pour chaque fournisseur](limiting-data-modification-functionality-based-on-the-user-cs/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image13.png)

**Figure 5**: le DropDownList `Suppliers` contient un `ListItem`afficher tout, plus un pour chaque fournisseur ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image15.png))

Étant donné que nous voulons mettre à jour l’interface utilisateur immédiatement après que l’utilisateur a modifié sa sélection, définissez la propriété `Suppliers` DropDownList s `AutoPostBack` sur `true`. À l’étape 2, nous allons créer un contrôle DetailsView qui affichera les informations du ou des fournisseurs en fonction de la sélection DropDownList. Ensuite, à l’étape 3, nous allons créer un gestionnaire d’événements pour cet événement `SelectedIndexChanged` DropDownList, dans lequel nous ajouterons du code qui lie les informations de fournisseur appropriées au DetailsView en fonction du fournisseur sélectionné.

## <a name="step-2-adding-a-detailsview-control"></a>Étape 2 : ajout d’un contrôle DetailsView

Nous utilisons un DetailsView pour afficher les informations sur les fournisseurs. Pour l’utilisateur qui peut afficher et modifier tous les fournisseurs, le contrôle DetailsView prend en charge la pagination, ce qui permet à l’utilisateur de parcourir les informations de fournisseur d’un enregistrement à la fois. Toutefois, si l’utilisateur travaille pour un fournisseur particulier, le contrôle DetailsView affiche uniquement les informations spécifiques de ce fournisseur et n’inclut pas d’interface de pagination. Dans les deux cas, le contrôle DetailsView doit autoriser l’utilisateur à modifier les champs adresse, ville et pays du fournisseur.

Ajoutez un DetailsView à la page sous la `Suppliers` DropDownList, affectez à sa propriété `ID` la valeur `SupplierDetails`et liez-le à la `AllSuppliersDataSource` ObjectDataSource créée à l’étape précédente. Cochez ensuite les cases activer la pagination et activer la modification à partir de la balise active DetailsView s.

> [!NOTE]
> Si vous ne voyez pas l’option Activer la modification dans la balise active de DetailsView s, elle est introuvable, car vous n’avez pas mappé la méthode ObjectDataSource s `Update()` à la méthode de la classe `SuppliersBLL` s `UpdateSupplierAddress`. Prenez un moment pour revenir en arrière et apporter cette modification de configuration, après quoi l’option Activer la modification doit apparaître dans la balise active de DetailsView s.

Étant donné que la méthode `SuppliersBLL` classe s `UpdateSupplierAddress` accepte uniquement quatre paramètres : `supplierID`, `address`, `city`et `country`-modifier le BoundFields DetailsView s afin que les BoundFields `CompanyName` et `Phone` soient en lecture seule. En outre, supprimez complètement le `SupplierID` BoundField. Enfin, la propriété `OldValuesParameterFormatString` de la `AllSuppliersDataSource` ObjectDataSource est actuellement définie sur `original_{0}`. Prenez un moment pour supprimer complètement ce paramètre de propriété de la syntaxe déclarative ou affectez-lui la valeur par défaut `{0}`.

Après avoir configuré les `SupplierDetails` DetailsView et `AllSuppliersDataSource` ObjectDataSource, nous aurons le balisage déclaratif suivant :

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample2.aspx)]

À ce stade, le contrôle DetailsView peut être paginé et les informations sur l’adresse du fournisseur sélectionné peuvent être mises à jour, quelle que soit la sélection effectuée dans la `Suppliers` DropDownList (voir figure 6).

[![les informations sur les fournisseurs peuvent être affichées et l’adresse mise à jour](limiting-data-modification-functionality-based-on-the-user-cs/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image16.png)

**Figure 6**: les informations de tous les fournisseurs peuvent être affichées et l’adresse mise à jour ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image18.png))

## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Étape 3 : afficher uniquement les informations sur le fournisseur sélectionné

Notre page affiche actuellement les informations pour tous les fournisseurs, qu’un fournisseur particulier ait été sélectionné à partir de la `Suppliers` DropDownList. Pour afficher uniquement les informations du fournisseur sélectionné, nous devons ajouter un autre ObjectDataSource à notre page, qui récupère des informations sur un fournisseur particulier.

Ajoutez un nouvel ObjectDataSource à la page, en lui attribuant un nom `SingleSupplierDataSource`. À partir de sa balise active, cliquez sur le lien configurer la source de données et faites en sorte qu’il utilise la méthode `SuppliersBLL` classe s `GetSupplierBySupplierID(supplierID)`. Comme pour le `AllSuppliersDataSource` ObjectDataSource, la méthode `SingleSupplierDataSource` ObjectDataSource s `Update()` mappée à la méthode `UpdateSupplierAddress` de la classe `SuppliersBLL` s.

[![configurer le ObjectDataSource SingleSupplierDataSource pour utiliser la méthode GetSupplierBySupplierID (RéfFournisseur)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image19.png)

**Figure 7**: configurer le `SingleSupplierDataSource` ObjectDataSource pour utiliser la méthode `GetSupplierBySupplierID(supplierID)` ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image21.png))

Ensuite, nous avons invité à spécifier la source du paramètre de la méthode `GetSupplierBySupplierID(supplierID)` `supplierID` paramètre d’entrée. Dans la mesure où nous souhaitons afficher les informations du fournisseur sélectionné à partir de la DropDownList, utilisez la propriété `Suppliers` DropDownList s `SelectedValue` comme source du paramètre.

[![utiliser la DropDownList Suppliers comme source de paramètres RéfFournisseur](limiting-data-modification-functionality-based-on-the-user-cs/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image22.png)

**Figure 8**: utiliser la `Suppliers` DropDownList comme source de paramètre `supplierID` ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image24.png))

Même avec ce second ObjectDataSource ajouté, le contrôle DetailsView est actuellement configuré pour toujours utiliser le `AllSuppliersDataSource` ObjectDataSource. Nous devons ajouter une logique pour ajuster la source de données utilisée par le DetailsView en fonction de l’élément `Suppliers` DropDownList sélectionné. Pour ce faire, créez un gestionnaire d’événements `SelectedIndexChanged` pour la DropDownList des fournisseurs. Vous pouvez le créer le plus facilement en double-cliquant sur le DropDownList dans le concepteur. Ce gestionnaire d’événements doit déterminer la source de données à utiliser et doit relier les données au contrôle DetailsView. Pour ce faire, vous devez utiliser le code suivant :

[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample3.cs)]

Le gestionnaire d’événements commence par déterminer si l’option « Afficher/modifier tous les fournisseurs » a été sélectionnée. Si tel était le cas, il définit le `SupplierDetails` DetailsView s `DataSourceID` sur `AllSuppliersDataSource` et retourne l’utilisateur au premier enregistrement de l’ensemble de fournisseurs en affectant à la propriété `PageIndex` la valeur 0. Toutefois, si l’utilisateur a sélectionné un fournisseur particulier dans le DropDownList, le `DataSourceID` DetailsView est assigné à `SingleSuppliersDataSource`. Quelle que soit la source de données utilisée, le mode de `SuppliersDetails` est rétabli en mode lecture seule et les données sont reliées au DetailsView par un appel à la méthode `DataBind()` du contrôle `SuppliersDetails`.

Lorsque ce gestionnaire d’événements est en place, le contrôle DetailsView affiche maintenant le fournisseur sélectionné, sauf si l’option « Afficher/modifier tous les fournisseurs » a été sélectionnée. dans ce cas, tous les fournisseurs peuvent être consultés via l’interface de pagination. La figure 9 illustre la page avec l’option « Afficher/modifier tous les fournisseurs » sélectionnée ; Notez que l’interface de pagination est présente, ce qui permet à l’utilisateur de consulter et de mettre à jour n’importe quel fournisseur. La figure 10 illustre la page avec le fournisseur de maison ma. Seules les informations de ma maison sont visibles et modifiables dans ce cas.

[![toutes les informations relatives aux fournisseurs peuvent être affichées et modifiées](limiting-data-modification-functionality-based-on-the-user-cs/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image25.png)

**Figure 9**: toutes les informations relatives aux fournisseurs peuvent être affichées et modifiées ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image27.png))

[![seules les informations sur les fournisseurs sélectionnés peuvent être affichées et modifiées](limiting-data-modification-functionality-based-on-the-user-cs/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image28.png)

**Figure 10**: seules les informations sur le fournisseur sélectionné peuvent être affichées et modifiées ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image30.png))

> [!NOTE]
> Pour ce didacticiel, les `EnableViewState` contrôle DropDownList et DetailsView doivent avoir la valeur `true` (valeur par défaut), car les modifications des objets DropDownList `SelectedIndex` et des propriétés du contrôle DetailsView `DataSourceID` s doivent être mémorisées sur les publications.

## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Étape 4 : affichage des produits des fournisseurs dans un contrôle GridView modifiable

Le contrôle DetailsView étant terminé, l’étape suivante consiste à inclure un GridView modifiable qui répertorie les produits fournis par le fournisseur sélectionné. Ce GridView doit autoriser uniquement les modifications apportées aux champs `ProductName` et `QuantityPerUnit`. En outre, si l’utilisateur visitant la page provient d’un fournisseur particulier, il ne doit autoriser que les mises à jour des produits qui ne sont *pas* interrompus. Pour ce faire, nous devons d’abord ajouter une surcharge de la méthode `ProductsBLL` Class s `UpdateProducts` qui accepte uniquement les champs `ProductID`, `ProductName`et `QuantityPerUnit` comme entrées. Nous avons parcouru ce processus au préalable dans de nombreux didacticiels. nous allons donc juste examiner le code, qui doit être ajouté à `ProductsBLL`:

[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample4.cs)]

Une fois cette surcharge créée, nous sommes prêts à ajouter le contrôle GridView et son ObjectDataSource associé. Ajoutez un nouveau contrôle GridView à la page, affectez à sa propriété `ID` la valeur `ProductsBySupplier`et configurez-le pour qu’il utilise un nouvel ObjectDataSource nommé `ProductsBySupplierDataSource`. Comme nous voulons que ce GridView répertorie ces produits par le fournisseur sélectionné, utilisez la méthode `ProductsBLL` classe s `GetProductsBySupplierID(supplierID)`. Mappez également la méthode `Update()` à la nouvelle surcharge `UpdateProduct` que nous venons de créer.

[![configurer ObjectDataSource pour utiliser la surcharge UpdateProduct que vous venez de créer](limiting-data-modification-functionality-based-on-the-user-cs/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image31.png)

**Figure 11**: configurer ObjectDataSource pour utiliser la surcharge de `UpdateProduct` que vous venez de créer ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image33.png))

Nous avons demandé à sélectionner la source de paramètre de la méthode `GetProductsBySupplierID(supplierID)` `supplierID` paramètre d’entrée. Étant donné que nous souhaitons afficher les produits pour le fournisseur sélectionné dans le contrôle DetailsView, utilisez la propriété `SuppliersDetails` DetailsView Control s `SelectedValue` comme source du paramètre.

[![utiliser la propriété SelectedValue SuppliersDetails DetailsView s comme source de paramètre](limiting-data-modification-functionality-based-on-the-user-cs/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image34.png)

**Figure 12**: utiliser la propriété `SuppliersDetails` DetailsView s `SelectedValue` en tant que source de paramètre ([Cliquer pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image36.png))

En retournant dans le GridView, supprimez tous les champs GridView à l’exception de `ProductName`, `QuantityPerUnit`et `Discontinued`, en marquant le `Discontinued` CheckBoxField en lecture seule. Cochez également l’option Activer la modification de la balise active GridView s. Une fois ces modifications effectuées, le balisage déclaratif pour les contrôles GridView et ObjectDataSource doit ressembler à ce qui suit :

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample5.aspx)]

Comme avec notre ObjectDataSources précédent, la propriété `OldValuesParameterFormatString` est définie sur `original_{0}`, ce qui entraîne des problèmes lors de la tentative de mise à jour d’un nom de produit ou d’une quantité par unité. Supprimez cette propriété de la syntaxe déclarative ou affectez-lui la valeur par défaut, `{0}`.

Une fois cette configuration terminée, notre page répertorie maintenant les produits fournis par le fournisseur sélectionné dans le contrôle GridView (voir figure 13). Actuellement, vous pouvez mettre à jour *n’importe quel* nom ou quantité de produit par unité. Toutefois, nous devons mettre à jour la logique de la page pour que cette fonctionnalité soit interdite pour les produits abandonnés pour les utilisateurs associés à un fournisseur particulier. Nous allons aborder ce dernier élément à l’étape 5.

[![les produits fournis par le fournisseur sélectionné s’affichent.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image37.png)

**Figure 13**: affichage des produits fournis par le fournisseur sélectionné ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image39.png))

> [!NOTE]
> Avec l’ajout de ce GridView modifiable, le gestionnaire d’événements `Suppliers` DropDownList s `SelectedIndexChanged` doit être mis à jour pour ramener le contrôle GridView à un État en lecture seule. Dans le cas contraire, si un autre fournisseur est sélectionné au milieu de la modification des informations sur le produit, l’index correspondant dans le GridView pour le nouveau fournisseur sera également modifiable. Pour éviter cela, il vous suffit de définir la propriété GridView s `EditIndex` sur `-1` dans le gestionnaire d’événements `SelectedIndexChanged`.

Rappelez-vous également qu’il est important que l’état d’affichage de GridView s soit activé (comportement par défaut). Si vous affectez à la propriété GridView s `EnableViewState` la valeur `false`, vous risquez d’avoir des utilisateurs simultanés qui suppriment ou modifient des enregistrements de manière involontaire. Consultez [Avertissement : problème d’accès concurrentiel avec ASP.NET 2,0 GridViews/DetailsView/FormViews qui prennent en charge l’édition et/ou la suppression et dont l’état d’affichage est désactivé](http://scottonwriting.net/sowblog/posts/10054.aspx) pour plus d’informations.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Étape 5 : interdire la modification des produits abandonnés lorsque l’option Afficher/modifier tous les fournisseurs n’est pas sélectionnée

Tandis que le `ProductsBySupplier` GridView est entièrement fonctionnel, il accorde actuellement un trop grand accès aux utilisateurs d’un fournisseur particulier. Conformément aux règles de votre entreprise, ces utilisateurs ne doivent pas être en mesure de mettre à jour les produits abandonnés. Pour l’appliquer, nous pouvons masquer (ou désactiver) le bouton modifier dans les lignes GridView avec les produits abandonnés lorsque la page est visitée par un utilisateur d’un fournisseur.

Créez un gestionnaire d’événements pour l’événement `RowDataBound` GridView s. Dans ce gestionnaire d’événements, nous devons déterminer si l’utilisateur est associé à un fournisseur particulier, ce qui, pour ce didacticiel, peut être déterminé en vérifiant la propriété de la `SelectedValue` DropDownList des fournisseurs, s’il s’agit d’une autre valeur que-1, l’utilisateur est associé à un fournisseur particulier. Pour ces utilisateurs, nous devons ensuite déterminer si le produit n’est plus disponible. Nous pouvons extraire une référence à l’instance réelle de `ProductRow` liée à la ligne GridView via la propriété `e.Row.DataItem`, comme indiqué dans le didacticiel [*affichage des informations de synthèse dans le pied de page de GridView s*](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) . Si le produit n’est plus disponible, nous pouvons saisir une référence par programmation au bouton modifier dans le contrôle de GridView s CommandField à l’aide des techniques présentées dans le didacticiel précédent, en [*ajoutant une confirmation côté client lors*](adding-client-side-confirmation-when-deleting-cs.md)de la suppression. Une fois que nous avons une référence, nous pouvons masquer ou désactiver le bouton.

[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample6.cs)]

Lorsque ce gestionnaire d’événements est en place, lorsque vous visitez cette page en tant qu’utilisateur d’un fournisseur spécifique, les produits qui ne sont plus disponibles ne sont pas modifiables, car le bouton modifier est masqué pour ces produits. Par exemple, chef Anton s Gumbo Mix est un produit abandonné pour le fournisseur de désoleils de Cajun-Orléans. Lorsque vous accédez à la page de ce fournisseur spécifique, le bouton modifier de ce produit est masqué (voir figure 14). Toutefois, lorsque vous vous rendez à l’aide du bouton « afficher/modifier tous les fournisseurs », le bouton modifier est disponible (voir figure 15).

[![pour les utilisateurs spécifiques à un fournisseur le bouton modifier pour chef Anton s Gumbo Mix est masqué](limiting-data-modification-functionality-based-on-the-user-cs/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image40.png)

**Figure 14**: pour les utilisateurs spécifiques au fournisseur, le bouton modifier pour le chef de la combinaison du chef de Gumbo s est masqué ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image42.png))

[![pour afficher/modifier tous les utilisateurs de fournisseurs, le bouton modifier pour chef Anton s Gumbo Mix est affiché](limiting-data-modification-functionality-based-on-the-user-cs/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image43.png)

**Figure 15**: pour afficher/modifier tous les utilisateurs de fournisseurs, le bouton modifier pour chef Anton s Gumbo Mix s’affiche ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image45.png))

## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Vérification des droits d’accès dans la couche de logique métier

Dans ce didacticiel, la page ASP.NET gère toute la logique en ce qui concerne les informations que l’utilisateur peut voir et les produits qu’il peut mettre à jour. Dans l’idéal, cette logique serait également présente dans la couche de logique métier. Par exemple, la méthode `SuppliersBLL` classe s `GetSuppliers()` (qui retourne tous les fournisseurs) peut inclure une vérification pour s’assurer que l’utilisateur actuellement connecté n’est *pas* associé à un fournisseur spécifique. De même, la méthode `UpdateSupplierAddress` peut inclure une vérification pour s’assurer que l’utilisateur actuellement connecté a travaillé pour notre société (et par conséquent peut mettre à jour toutes les informations d’adresse des fournisseurs) ou est associé au fournisseur dont les données sont mises à jour.

Je n’ai pas inclus de telles vérifications de couche BLL ici, car dans notre didacticiel, les droits des utilisateurs sont déterminés par un contrôle DropDownList sur la page, auquel les classes BLL ne peuvent pas accéder. Lors de l’utilisation du système d’appartenance ou de l’un des schémas d’authentification prêts à l’emploi fournis par ASP.NET (par exemple, l’authentification Windows), les informations sur les utilisateurs et les rôles de l’utilisateur actuellement connecté sont accessibles à partir de la couche BLL, ce qui rend ce type d’accès contrôles de droits possibles aux couches présentation et couche BLL.

## <a name="summary"></a>Récapitulatif

La plupart des sites qui fournissent des comptes d’utilisateur doivent personnaliser l’interface de modification des données en fonction de l’utilisateur connecté. Les utilisateurs administratifs peuvent être en mesure de supprimer et de modifier n’importe quel enregistrement, tandis que les utilisateurs non administratifs peuvent être limités à la mise à jour ou à la suppression des enregistrements qu’ils ont créés eux-mêmes. Quel que soit le scénario, les classes Data Web Controls, ObjectDataSource et de la couche de logique métier peuvent être étendues pour ajouter ou refuser certaines fonctionnalités en fonction de l’utilisateur connecté. Dans ce didacticiel, nous avons vu comment limiter les données affichables et modifiables selon que l’utilisateur a été associé à un fournisseur particulier ou s’il a travaillé pour notre entreprise.

Ce didacticiel conclut notre examen de l’insertion, de la mise à jour et de la suppression de données à l’aide des contrôles GridView, DetailsView et FormView. À partir du prochain didacticiel, nous allons attirer l’attention sur l’ajout de la prise en charge de la pagination et du tri.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](adding-client-side-confirmation-when-deleting-cs.md)
> [Suivant](an-overview-of-inserting-updating-and-deleting-data-vb.md)
