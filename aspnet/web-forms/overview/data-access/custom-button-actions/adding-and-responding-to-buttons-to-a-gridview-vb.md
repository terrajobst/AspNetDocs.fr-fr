---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Ajout et réponse à des boutons sur un GridView (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner comment ajouter des boutons personnalisés, à la fois à un modèle et aux champs d’un contrôle GridView ou DetailsView. En particulier, nous allons le...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 8727d8faead02340d223c75845bf29f63d1a0834
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600958"
---
# <a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Ajout et réponse à des boutons sur un GridView (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) ou [Télécharger le PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> Dans ce didacticiel, nous allons examiner comment ajouter des boutons personnalisés, à la fois à un modèle et aux champs d’un contrôle GridView ou DetailsView. En particulier, nous allons créer une interface avec un contrôle FormView qui permet à l’utilisateur de parcourir les fournisseurs.

## <a name="introduction"></a>Introduction

Si de nombreux scénarios de création de rapports impliquent un accès en lecture seule aux données du rapport, il n’est pas rare que les rapports incluent la possibilité d’effectuer des actions basées sur les données affichées. En général, cela impliquait l’ajout d’un contrôle Web Button, LinkButton ou ImageButton avec chaque enregistrement affiché dans le rapport qui, lorsque vous cliquez dessus, provoque une publication (postback) et appelle du code côté serveur. La modification et la suppression des données au moyen d’un enregistrement par enregistrement est l’exemple le plus courant. En fait, comme nous l’avons vu depuis la [Présentation de l’insertion, de la mise à jour et de la suppression du](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) didacticiel sur les données, la modification et la suppression sont si fréquentes que les contrôles GridView, DetailsView et FormView peuvent prendre en charge ces fonctionnalités sans avoir à écrire une seule ligne de code.

Outre les boutons modifier et supprimer, les contrôles GridView, DetailsView et FormView peuvent également inclure des boutons, LinkButtons ou ImageButtons qui, lorsque vous cliquez dessus, exécutent une logique personnalisée côté serveur. Dans ce didacticiel, nous allons examiner comment ajouter des boutons personnalisés, à la fois à un modèle et aux champs d’un contrôle GridView ou DetailsView. En particulier, nous allons créer une interface avec un contrôle FormView qui permet à l’utilisateur de parcourir les fournisseurs. Pour un fournisseur donné, le contrôle FormView affiche des informations sur le fournisseur avec un contrôle Web Button qui, si vous cliquez dessus, marque tous les produits associés comme abandonnés. En outre, un GridView répertorie les produits fournis par le fournisseur sélectionné, avec chaque ligne contenant les boutons augmenter le prix et prix de remise qui, si vous cliquez dessus, élèvent ou réduisent les `UnitPrice` du produit de 10% (voir figure 1).

[![les contrôles FormView et GridView contiennent des boutons qui effectuent des actions personnalisées](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Figure 1**: FormView et GridView contiennent des boutons qui effectuent des actions personnalisées ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))

## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Étape 1 : ajout des pages Web du didacticiel du bouton

Avant d’examiner comment ajouter des boutons personnalisés, commençons par prendre un moment pour créer les pages ASP.NET dans notre projet de site Web dont nous aurons besoin pour ce didacticiel. Commencez par ajouter un nouveau dossier nommé `CustomButtons`. Ensuite, ajoutez les deux pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page à la page maître `Site.master` :

- `Default.aspx`
- `CustomButtons.aspx`

![Ajouter les pages ASP.NET pour les didacticiels associés aux boutons personnalisés](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Figure 2**: ajouter les pages ASP.net pour les didacticiels associés aux boutons personnalisés

Comme dans les autres dossiers, `Default.aspx` dans le dossier `CustomButtons` répertorie les didacticiels de la section. N’oubliez pas que le contrôle utilisateur `SectionLevelTutorialListing.ascx` fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser de l’Explorateur de solutions sur la page s Mode Création.

[![ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Figure 3**: ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))

Enfin, ajoutez les pages en tant qu’entrées au fichier `Web.sitemap`. Plus précisément, ajoutez le balisage suivant après la pagination et le tri `<siteMapNode>`:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Après avoir mis à jour `Web.sitemap`, prenez un moment pour afficher le site Web des didacticiels via un navigateur. Le menu à gauche contient maintenant des éléments pour les didacticiels de modification, d’insertion et de suppression.

![Le plan de site comprend désormais l’entrée du didacticiel sur les boutons personnalisés](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Figure 4**: le plan de site comprend désormais l’entrée du didacticiel sur les boutons personnalisés

## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Étape 2 : ajout d’un FormView qui répertorie les fournisseurs

Commençons par ce didacticiel en ajoutant le FormView qui répertorie les fournisseurs. Comme nous l’avons vu dans l’introduction, ce FormView permettra à l’utilisateur de parcourir les fournisseurs, en présentant les produits fournis par le fournisseur dans un GridView. En outre, ce FormView inclut un bouton qui, lorsque vous cliquez dessus, marque tous les produits du fournisseur comme abandonnés. Avant de nous soucier de l’ajout du bouton personnalisé au FormView, n’hésitez pas à créer le FormView pour qu’il affiche les informations du fournisseur.

Commencez par ouvrir la page `CustomButtons.aspx` dans le dossier `CustomButtons`. Ajoutez un contrôle FormView à la page en le faisant glisser de la boîte à outils vers le concepteur et définissez sa propriété `ID` sur `Suppliers`. À partir de la balise active FormView s, choisissez de créer une nouvelle ObjectDataSource nommée `SuppliersDataSource`.

[![créer un ObjectDataSource nommé SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Figure 5**: créer un ObjectDataSource nommé `SuppliersDataSource` ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))

Configurez ce nouvel ObjectDataSource de manière à ce qu’il interroge à partir de la méthode de `GetSuppliers()` `SuppliersBLL` classe s (voir figure 6). Étant donné que ce FormView ne fournit pas d’interface pour la mise à jour des informations sur les fournisseurs, sélectionnez l’option (aucune) dans la liste déroulante de l’onglet mettre à jour.

[![configurer la source de données pour utiliser la méthode SuppliersBLL de la classe s GetSuppliers ()](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Figure 6**: configurer la source de données pour utiliser la méthode de `GetSuppliers()` de la classe `SuppliersBLL` ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))

Après la configuration de l’ObjectDataSource, Visual Studio génère un `InsertItemTemplate`, `EditItemTemplate`et `ItemTemplate` pour le FormView. Supprimez le `InsertItemTemplate` et `EditItemTemplate` et modifiez le `ItemTemplate` afin qu’il affiche uniquement le nom de la société et le numéro de téléphone du fournisseur. Enfin, activez la prise en charge de la pagination pour le contrôle FormView en activant la case à cocher Activer la pagination dans sa balise active (ou en affectant à sa propriété `AllowPaging` la valeur `True`). Après ces modifications, le balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

La figure 7 illustre la page CustomButtons. aspx lorsque vous l’affichez dans un navigateur.

[![le FormView répertorie les champs CompanyName et Phone du fournisseur actuellement sélectionné](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Figure 7**: le FormView répertorie les champs `CompanyName` et `Phone` du fournisseur actuellement sélectionné ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))

## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Étape 3 : ajout d’un GridView qui répertorie les produits du fournisseur sélectionnés

Avant d’ajouter le bouton arrêter tous les produits au modèle FormView, nous allons d’abord ajouter un contrôle GridView sous le contrôle FormView qui répertorie les produits fournis par le fournisseur sélectionné. Pour ce faire, ajoutez un GridView à la page, affectez à sa propriété `ID` la valeur `SuppliersProducts`et ajoutez un nouvel ObjectDataSource nommé `SuppliersProductsDataSource`.

[![créer un ObjectDataSource nommé SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Figure 8**: créer un ObjectDataSource nommé `SuppliersProductsDataSource` ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))

Configurez cet ObjectDataSource pour utiliser la méthode `GetProductsBySupplierID(supplierID)` de la classe ProductsBLL (voir figure 9). Bien que ce GridView autorise l’ajustement du prix d’un produit, il n’utilise pas les fonctionnalités intégrées de modification ou de suppression du contrôle GridView. Par conséquent, nous pouvons définir la liste déroulante sur (aucune) pour les onglets mettre à jour, insérer et supprimer ObjectDataSource s.

[![configurer la source de données pour utiliser la méthode ProductsBLL de la classe s GetProductsBySupplierID (RéfFournisseur)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Figure 9**: configurer la source de données pour utiliser la méthode de `GetProductsBySupplierID(supplierID)` de la classe `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))

Étant donné que la méthode `GetProductsBySupplierID(supplierID)` accepte un paramètre d’entrée, l’Assistant ObjectDataSource nous invite à entrer la source de cette valeur de paramètre. Pour transmettre la valeur `SupplierID` à partir du FormView, définissez la liste déroulante source du paramètre sur Control et la liste déroulante ControlID sur `Suppliers` (ID du FormView créé à l’étape 2).

[![indiquer que le paramètre RéfFournisseur doit provenir du contrôle FormView Suppliers](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Figure 10**: indiquer que le paramètre *`supplierID`* doit provenir du contrôle FormView `Suppliers` ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))

Une fois l’exécution de l’Assistant ObjectDataSource terminée, le contrôle GridView contient un BoundField ou un CheckBoxField pour chacun des champs de données du produit. N’hésitez pas à les découper pour afficher uniquement les `ProductName` et `UnitPrice` BoundFields, ainsi que le `Discontinued` CheckBoxField ; en outre, nous allons mettre en forme le `UnitPrice` BoundField de telle sorte que son texte est mis en forme en tant que devise. Vos balises déclaratives GridView et `SuppliersProductsDataSource` ObjectDataSource s doivent ressembler à la balise suivante :

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

À ce stade, notre didacticiel affiche un rapport maître/détails qui permet à l’utilisateur de choisir un fournisseur à partir du contrôle FormView en haut et d’afficher les produits fournis par ce fournisseur par le biais du contrôle GridView situé en bas. La figure 11 illustre une capture d’écran de cette page lors de la sélection du fournisseur Tokyo traders à partir du FormView.

[![les produits des fournisseurs sélectionnés s’affichent dans le contrôle GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Figure 11**: les produits du fournisseur sélectionné s’affichent dans le GridView ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))

## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Étape 4 : création de méthodes DAL et BLL pour interrompre tous les produits d’un fournisseur

Avant de pouvoir ajouter un bouton au FormView qui, lorsque vous cliquez dessus, interrompt tous les produits du fournisseur, nous devons d’abord ajouter une méthode à la couche DAL et à la couche BLL qui effectue cette action. En particulier, cette méthode sera nommée `DiscontinueAllProductsForSupplier(supplierID)`. Quand l’utilisateur clique sur le bouton FormView s, nous appellerons cette méthode dans la couche de logique métier, en transmettant le `SupplierID`du fournisseur sélectionné ; la couche BLL appellera ensuite la méthode de la couche d’accès aux données correspondante, qui émettra une instruction `UPDATE` à la base de données qui interrompt les produits du fournisseur spécifiés.

Comme nous l’avons fait dans nos didacticiels précédents, nous allons utiliser une approche ascendante, en commençant par créer la méthode DAL, puis la méthode BLL et enfin en implémentant les fonctionnalités dans la page ASP.NET. Ouvrez le `Northwind.xsd` DataSet typé dans le dossier `App_Code/DAL` et ajoutez une nouvelle méthode au `ProductsTableAdapter` (cliquez avec le bouton droit sur le `ProductsTableAdapter`, puis choisissez Ajouter une requête). Cela entraînera l’ajout de l’Assistant Configuration de requêtes TableAdapter, qui nous guide tout au long du processus d’ajout de la nouvelle méthode. Commencez par indiquer que notre méthode DAL utilisera une instruction SQL ad hoc.

[![créer la méthode DAL à l’aide d’une instruction SQL ad hoc](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Figure 12**: créer la méthode dal à l’aide d’une instruction SQL ad hoc ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))

Ensuite, l’Assistant nous invite à spécifier le type de requête à créer. Dans la mesure où la méthode `DiscontinueAllProductsForSupplier(supplierID)` doit mettre à jour la table de base de données `Products`, en affectant la valeur 1 au champ `Discontinued` pour tous les produits fournis par le *`supplierID`* spécifié, nous devons créer une requête qui met à jour les données.

[![choisir le type de requête de mise à jour](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Figure 13**: choisir le type de requête de mise à jour ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))

L’écran suivant de l’Assistant fournit l’instruction `UPDATE` du TableAdapter existante, qui met à jour chacun des champs définis dans le DataTable `Products`. Remplacez le texte de cette requête par l’instruction suivante :

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Après avoir entré cette requête et cliqué sur suivant, le dernier écran de l’Assistant vous demande le nom de la nouvelle méthode use `DiscontinueAllProductsForSupplier`. Terminez l’Assistant en cliquant sur le bouton Terminer. Lors du retour au concepteur de DataSet, vous devez voir une nouvelle méthode dans la `ProductsTableAdapter` nommée `DiscontinueAllProductsForSupplier(@SupplierID)`.

[![nommez la nouvelle méthode DAL DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Figure 14**: nommer la nouvelle méthode dal `DiscontinueAllProductsForSupplier` ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))

Avec la méthode `DiscontinueAllProductsForSupplier(supplierID)` créée dans la couche d’accès aux données, la tâche suivante consiste à créer la méthode `DiscontinueAllProductsForSupplier(supplierID)` dans la couche de logique métier. Pour ce faire, ouvrez le fichier de classe `ProductsBLL` et ajoutez ce qui suit :

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Cette méthode appelle simplement la méthode `DiscontinueAllProductsForSupplier(supplierID)` dans la couche DAL, en passant la valeur de paramètre *`supplierID`* fournie. S’il existe des règles d’entreprise qui permettaient uniquement la fin de la mise en œuvre des produits d’un fournisseur dans certaines circonstances, ces règles doivent être implémentées ici, dans la couche BLL.

> [!NOTE]
> Contrairement à la `UpdateProduct` surcharges de la classe `ProductsBLL`, la signature de la méthode `DiscontinueAllProductsForSupplier(supplierID)` n’inclut pas l’attribut `DataObjectMethodAttribute` (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Cela exclut la méthode `DiscontinueAllProductsForSupplier(supplierID)` de la liste déroulante de l’Assistant de configuration de la source de données ObjectDataSource s dans l’onglet mettre à jour. J’ai omis cet attribut, car nous appellerons directement la méthode `DiscontinueAllProductsForSupplier(supplierID)` à partir d’un gestionnaire d’événements dans notre page ASP.NET.

## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Étape 5 : ajout d’un bouton arrêter tous les produits au FormView

Une fois la méthode `DiscontinueAllProductsForSupplier(supplierID)` dans la couche BLL et la couche DAL terminées, la dernière étape pour ajouter la possibilité d’interrompre tous les produits pour le fournisseur sélectionné consiste à ajouter un contrôle Web Button à la `ItemTemplate`FormView s. Ajoutez un bouton de ce type sous le numéro de téléphone du fournisseur avec le texte du bouton, discontinue tous les produits et une valeur de propriété de `ID` de `DiscontinueAllProductsForSupplier`. Vous pouvez ajouter ce contrôle Web de bouton par le biais du concepteur en cliquant sur le lien modifier les modèles dans la balise active FormView s (voir figure 15) ou directement à l’aide de la syntaxe déclarative.

[![ajouter un bouton arrêter tous les produits sur le contrôle Web de la vue ItemTemplate s](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Figure 15**: ajouter un bouton de commande arrêter tous les produits à la `ItemTemplate` de la vue FormView ([cliquez pour afficher l’image en plein écran](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))

Lorsque vous cliquez sur le bouton d’un utilisateur visitant la page, une publication est déclenchée et l' [événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) FormView s`ItemCommand` se déclenche. Pour exécuter du code personnalisé en réponse à un clic sur ce bouton, nous pouvons créer un gestionnaire d’événements pour cet événement. Sachez cependant que l’événement `ItemCommand` se déclenche chaque fois que vous cliquez sur un bouton, un LinkButton ou *un* contrôle Web ImageButton dans le FormView. Cela signifie que lorsque l’utilisateur passe d’une page à une autre dans le FormView, l’événement `ItemCommand` se déclenche ; même chose quand l’utilisateur clique sur nouveau, modifier ou supprimer dans un FormView qui prend en charge l’insertion, la mise à jour ou la suppression.

Étant donné que le `ItemCommand` se déclenche indépendamment du bouton sur lequel l’utilisateur clique, dans le gestionnaire d’événements, nous avons besoin d’un moyen de déterminer si l’utilisateur a cliqué sur le bouton arrêter tous les produits ou s’il s’agit d’un autre bouton. Pour ce faire, nous pouvons définir la propriété Button Web Control s `CommandName` sur une valeur d’identification. Lorsque l’utilisateur clique sur le bouton, cette `CommandName` valeur est transmise dans le gestionnaire d’événements `ItemCommand`, ce qui nous permet de déterminer si l’utilisateur a cliqué sur le bouton arrêter tous les produits. Définissez le bouton discontinue All Products s `CommandName` la propriété sur DiscontinueProducts.

Enfin, utilisez une boîte de dialogue de confirmation côté client pour vous assurer que l’utilisateur souhaite vraiment arrêter les produits du fournisseur sélectionné. Comme nous l’avons vu dans la confirmation de l' [Ajout côté client lors](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) de la suppression du didacticiel, cela peut être accompli avec un peu de JavaScript. En particulier, affectez à la propriété Button Web Control s OnClientClick la valeur `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Une fois ces modifications apportées, la syntaxe déclarative de FormView s doit ressembler à ce qui suit :

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Ensuite, créez un gestionnaire d’événements pour l’événement FormView s `ItemCommand`. Dans ce gestionnaire d’événements, nous devons d’abord déterminer si l’utilisateur a cliqué sur le bouton discontinue All Products. Si c’est le cas, nous souhaitons créer une instance de la classe `ProductsBLL` et appeler sa méthode `DiscontinueAllProductsForSupplier(supplierID)`, en passant la `SupplierID` du FormView sélectionné :

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Notez que le `SupplierID` du fournisseur actuellement sélectionné dans le FormView est accessible à l’aide de la propriété FormView s [`SelectedValue`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). La propriété `SelectedValue` retourne la première valeur de clé de données pour l’enregistrement affiché dans le FormView. La propriété FormView s [`DataKeyNames`](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), qui indique les champs de données à partir desquels les valeurs de clés de données sont extraites, a été automatiquement définie sur `SupplierID` par Visual Studio lors de la liaison de ObjectDataSource au FormView à l’étape 2.

Une fois le gestionnaire d’événements `ItemCommand` créé, prenez un moment pour tester la page. Accédez au fournisseur Cooperativa de quesos « las Cabras » (il s’agit du cinquième fournisseur du FormView pour moi). Ce fournisseur fournit deux produits, Queso Cabrales et queso Manchego la Pastora, qui ne sont *pas* encore abandonnés.

Imaginez que Cooperativa de quesos « las Cabras » n’est plus opérationnel et, par conséquent, ses produits ne seront plus disponibles. Cliquez sur le bouton arrêter tous les produits. La boîte de dialogue confirmer côté client s’affiche (voir figure 16).

[![Cooperativa de quesos las Cabras fournit deux produits actifs](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Figure 16**: Cooperativa de quesos las Cabras fournit deux produits actifs ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))

Si vous cliquez sur OK dans la boîte de dialogue confirmer côté client, l’envoi du formulaire se poursuit, provoquant une publication dans laquelle l’événement FormView s `ItemCommand` se déclenchera. Le gestionnaire d’événements que nous avons créé exécute ensuite, en appelant la méthode `DiscontinueAllProductsForSupplier(supplierID)` et en arrêtant à la fois les produits Queso Cabrales et queso Manchego la Pastora.

Si vous avez désactivé l’état d’affichage de GridView s, le GridView est relié à la Banque de données sous-jacente à chaque publication (postback) et, par conséquent, est immédiatement mis à jour pour refléter le fait que ces deux produits sont désormais abandonnés (voir figure 17). Toutefois, si vous n’avez pas désactivé l’état d’affichage dans le GridView, vous devrez lier manuellement les données au GridView après avoir apporté cette modification. Pour ce faire, effectuez simplement un appel à la méthode GridView s `DataBind()` immédiatement après l’appel de la méthode `DiscontinueAllProductsForSupplier(supplierID)`.

[![après avoir cliqué sur le bouton arrêter tous les produits, les produits du fournisseur sont mis à jour en conséquence.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Figure 17**: après avoir cliqué sur le bouton arrêter tous les produits, les produits du fournisseur sont mis à jour en conséquence ([cliquez pour afficher l’image en plein écran](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))

## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Étape 6 : création d’une surcharge UpdateProduct dans la couche de logique métier pour ajuster le prix d’un produit

Comme avec le bouton arrêter tous les produits dans le contrôle FormView, afin d’ajouter des boutons permettant d’augmenter et de réduire le prix d’un produit dans le contrôle GridView, nous devons d’abord ajouter la couche d’accès aux données et les méthodes de couche logique métier appropriées. Étant donné que nous avons déjà une méthode qui met à jour une ligne de produit unique dans la couche DAL, nous pouvons fournir une telle fonctionnalité en créant une nouvelle surcharge pour la méthode `UpdateProduct` dans la couche BLL.

Les surcharges précédentes de `UpdateProduct` ont été prises dans une combinaison de champs de produit comme valeurs d’entrée scalaires et ont ensuite mis à jour uniquement ces champs pour le produit spécifié. Pour cette surcharge, nous allons varier légèrement de cette norme et transmettre à la place le `ProductID` du produit et le pourcentage d’ajustement de la `UnitPrice` (par opposition au passage du nouveau `UnitPrice`, ajusté). Cette approche simplifie le code dont nous avons besoin pour écrire dans la classe code-behind de la page ASP.NET, car nous n’avons pas à vous soucier de la détermination du `UnitPrice`du produit actuel.

La surcharge `UpdateProduct` pour ce didacticiel est illustrée ci-dessous :

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Cette surcharge récupère des informations sur le produit spécifié par le biais de la méthode de `GetProductByProductID(productID)` DAL s. Il vérifie ensuite si une valeur de `NULL` de base de données est affectée au `UnitPrice` du produit. Si c’est le cas, le prix reste inchangé. Toutefois, si la valeur de `UnitPrice` n’est pas`NULL`, la méthode met à jour le `UnitPrice` du produit par le pourcentage spécifié (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Étape 7 : ajout des boutons d’augmentation et de réduction au contrôle GridView

Le contrôle GridView (et DetailsView) est constitué d’une collection de champs. En plus de BoundFields, CheckBoxFields et TemplateFields, ASP.NET comprend le ButtonField, qui, comme son nom l’indique, est rendu sous la forme d’une colonne avec un bouton, LinkButton ou ImageButton pour chaque ligne. Semblable au FormView, le fait de cliquer sur *un* bouton dans les boutons de pagination de GridView, les boutons modifier ou supprimer, les boutons de tri, etc. entraîne une publication et déclenche l' [événement`RowCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx)GridView.

ButtonField a une propriété `CommandName` qui affecte la valeur spécifiée à chacun de ses boutons `CommandName` propriétés. Comme avec le FormView, la valeur `CommandName` est utilisée par le gestionnaire d’événements `RowCommand` pour déterminer le bouton sur lequel l’utilisateur a cliqué.

Ajoutez deux nouveaux ButtonFields au contrôle GridView, l’un avec un prix texte de bouton + 10% et l’autre avec le texte Price-10%. Pour ajouter ces ButtonFields, cliquez sur le lien modifier les colonnes dans la balise active GridView s, sélectionnez le type de champ ButtonField dans la liste en haut à gauche, puis cliquez sur le bouton Ajouter.

![Ajouter deux ButtonFields au GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Figure 18**: ajouter deux ButtonFields au GridView

Déplacez les deux ButtonFields afin qu’ils apparaissent comme les deux premiers champs GridView. Ensuite, définissez les propriétés de `Text` de ces deux ButtonFields sur Price + 10% et Price-10% et les propriétés `CommandName` sur IncreasePrice et DecreasePrice, respectivement. Par défaut, un ButtonField affiche sa colonne de boutons en tant que LinkButtons. Cela peut toutefois être modifié par le biais de la propriété ButtonField s [`ButtonType`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Faites en sorte que ces deux ButtonFields soient rendues comme des boutons de commande standard ; par conséquent, affectez à la propriété `ButtonType` la valeur `Button`. La figure 19 affiche la boîte de dialogue champs après que ces modifications ont été apportées. Voici le balisage déclaratif s de GridView.

![Configurer les propriétés Text, CommandName et ButtonType de ButtonFields](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Figure 19**: configurer les propriétés ButtonFields `Text`, `CommandName`et `ButtonType`

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Une fois ces ButtonFields créés, la dernière étape consiste à créer un gestionnaire d’événements pour l’événement `RowCommand` GridView s. Ce gestionnaire d’événements, s’il est déclenché parce que les boutons Price + 10% ou Price-10% ont fait l’objet d’un clic, doit déterminer la `ProductID` de la ligne dont le bouton a été cliqué, puis appeler la méthode `ProductsBLL` classe s `UpdateProduct`, en passant l’ajustement de pourcentage de `UnitPrice` approprié avec la `ProductID`. Le code suivant effectue ces tâches :

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Pour déterminer la `ProductID` pour la ligne sur laquelle l’utilisateur a cliqué sur le prix + 10% ou le bouton Price-10%, nous devons consulter la collection de `DataKeys` GridView s. Cette collection contient les valeurs des champs spécifiés dans la propriété `DataKeyNames` pour chaque ligne GridView. Dans la mesure où la propriété GridView s `DataKeyNames` a été définie sur ProductID par Visual Studio lors de la liaison de ObjectDataSource au GridView, `DataKeys(rowIndex).Value` fournit le `ProductID` pour le *rowIndex*spécifié.

Le ButtonField transmet automatiquement le *rowIndex* de la ligne dont le bouton a été cliqué via le paramètre `e.CommandArgument`. Par conséquent, pour déterminer la `ProductID` pour la ligne dont le bouton Price + 10% ou Price-10% a été cliqué, nous utilisons : `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Comme avec le bouton arrêter tous les produits, si vous avez désactivé l’état d’affichage GridView s, le GridView est relié à la Banque de données sous-jacente à chaque publication (postback) et, par conséquent, est immédiatement mis à jour pour refléter un changement de prix qui se produit en cliquant sur l’un ou l’autre des boutons. Toutefois, si vous n’avez pas désactivé l’état d’affichage dans le GridView, vous devrez lier manuellement les données au GridView après avoir apporté cette modification. Pour ce faire, effectuez simplement un appel à la méthode GridView s `DataBind()` immédiatement après l’appel de la méthode `UpdateProduct`.

La figure 20 illustre la page lors de l’affichage des produits fournis par Homestead de grand-mère. La figure 21 illustre les résultats une fois que vous avez cliqué deux fois sur le bouton Price + 10% pour la propagation Boysenberry de grand-mère et le bouton Price-10% une fois pour Northwood Cranberry sauce.

[![le contrôle GridView contient les boutons Price + 10% et Price-10%](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Figure 20**: le contrôle GridView contient les boutons Price + 10% et Price-10% ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))

[![les prix pour le premier et le troisième produit ont été mis à jour à l’aide des boutons Price + 10% et Price-10%](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Figure 21**: les prix pour le premier et le troisième produit ont été mis à jour à l’aide des boutons Price + 10% et Price-10% ([cliquez pour afficher l’image en taille réelle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))

> [!NOTE]
> Les contrôles GridView (et DetailsView) peuvent également avoir des boutons, des LinkButtons ou des ImageButtons ajoutés à leur TemplateFields. Comme avec BoundField, ces boutons, quand vous cliquez dessus, provoquent une publication (postback), déclenchant l’événement `RowCommand` GridView s. Toutefois, lors de l’ajout de boutons dans un TemplateField, le bouton s `CommandArgument` n’est pas automatiquement défini sur l’index de la ligne comme c’est le cas lors de l’utilisation de ButtonFields. Si vous avez besoin de déterminer l’index de ligne du bouton sur lequel l’utilisateur a cliqué dans le gestionnaire d’événements `RowCommand`, vous devez définir manuellement la propriété Button s `CommandArgument` dans sa syntaxe déclarative au sein du TemplateField, à l’aide d’un code tel que :  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.

## <a name="summary"></a>Récapitulatif

Les contrôles GridView, DetailsView et FormView peuvent tous inclure des boutons, LinkButtons ou ImageButtons. De tels boutons, quand vous cliquez dessus, provoquent une publication (postback) et déclenchent l’événement `ItemCommand` dans les contrôles FormView et DetailsView et l’événement `RowCommand` dans le GridView. Ces contrôles Web de données ont des fonctionnalités intégrées permettant de gérer des actions courantes liées aux commandes, telles que la suppression ou la modification d’enregistrements. Toutefois, nous pouvons également utiliser des boutons qui, lorsque vous cliquez dessus, répondent à l’exécution de notre propre code personnalisé.

Pour ce faire, nous devons créer un gestionnaire d’événements pour l’événement `ItemCommand` ou `RowCommand`. Dans ce gestionnaire d’événements, nous vérifions tout d’abord la valeur de `CommandName` entrant pour déterminer le bouton sur lequel l’utilisateur a cliqué, puis j’ai effectué l’action personnalisée appropriée. Dans ce didacticiel, nous avons vu comment utiliser des boutons et des ButtonFields pour interrompre tous les produits pour un fournisseur spécifique ou pour augmenter ou réduire le prix d’un produit particulier de 10%.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](adding-and-responding-to-buttons-to-a-gridview-cs.md)
