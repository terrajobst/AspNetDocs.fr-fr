---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Appartenance et administration | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: ab00bc90bfc767d06e747be6dfb973245b5aae88
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546738"
---
# <a name="membership-and-administration"></a>Appartenance et administration

par [Erik Reitan](https://github.com/Erikre)

[Télécharger l’exemple de projet WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Télécharger le livre électronique (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour le Web. Un [projet Visual Studio 2013 avec C# le code source](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.

Ce didacticiel vous montre comment mettre à jour l’exemple d’application Wingtip Toys pour ajouter un rôle personnalisé et utiliser ASP.NET Identity. Elle vous montre également comment implémenter une page d’administration à partir de laquelle l’utilisateur doté d’un rôle personnalisé peut ajouter et supprimer des produits sur le site Web.

[ASP.net Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) est le système d’appartenance utilisé pour créer l’application Web ASP.net et est disponible dans ASP.net 4,5. ASP.NET Identity est utilisé dans le modèle de projet Visual Studio 2013 Web Forms, ainsi que les modèles pour [ASP.NET MVC](../../../../mvc/index.md), [API Web ASP.net](../../../../web-api/index.md)et [ASP.net application à page unique](../../../../single-page-application/index.md). Vous pouvez également installer spécifiquement le système ASP.NET Identity à l’aide de NuGet lorsque vous démarrez avec une application Web vide. Toutefois, dans cette série de didacticiels, vous utilisez le **Web Forms**ProjectTemplate, qui comprend le système ASP.net Identity. ASP.NET Identity facilite l’intégration des données de profil spécifiques à l’utilisateur avec les données d’application. ASP.NET Identity vous permet également de choisir le modèle de persistance pour les profils utilisateur dans votre application. Vous pouvez stocker les données dans une base de données SQL Server ou dans un autre magasin de données, y compris des magasins de données *NoSQL* tels que des tables de stockage Windows Azure.

Ce didacticiel s’appuie sur le didacticiel précédent intitulé « Checkout and Payment with PayPal » dans la série de didacticiels Wingtip Toys.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment utiliser du code pour ajouter un rôle personnalisé et un utilisateur à l’application.
- Comment restreindre l’accès au dossier et à la page d’administration.
- Comment fournir une navigation pour l’utilisateur qui appartient au rôle personnalisé.
- Comment utiliser la liaison de modèle pour remplir un contrôle [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) avec des catégories de produits.
- Téléchargement d’un fichier vers l’application Web à l’aide du contrôle [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) .
- Comment utiliser les contrôles de validation pour implémenter la validation des entrées.
- Comment ajouter et supprimer des produits dans l’application.

## <a name="these-features-are-included-in-the-tutorial"></a>Ces fonctionnalités sont incluses dans le didacticiel :

- ASP.NET Identity
- Configuration et autorisation
- Liaison de modèle
- Validation discrète

ASP.NET Web Forms fournit des fonctionnalités d’appartenance. En utilisant le modèle par défaut, vous disposez d’une fonctionnalité d’appartenance intégrée que vous pouvez utiliser immédiatement lorsque l’application s’exécute. Ce didacticiel vous montre comment utiliser ASP.NET Identity pour ajouter un rôle personnalisé et affecter un utilisateur à ce rôle. Vous allez apprendre à restreindre l’accès au dossier d’administration. Vous allez ajouter une page au dossier d’administration qui permet à un utilisateur disposant d’un rôle personnalisé d’ajouter et de supprimer des produits, et d’afficher un aperçu d’un produit une fois qu’il a été ajouté.

## <a name="adding-a-custom-role"></a>Ajout d’un rôle personnalisé

À l’aide de ASP.NET Identity, vous pouvez ajouter un rôle personnalisé et affecter un utilisateur à ce rôle à l’aide de code.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *logique* et créez une nouvelle classe.
2. Nommez la nouvelle classe *RoleActions.cs*.
3. Modifiez le code pour qu’il apparaisse comme suit :  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. Dans **Explorateur de solutions**, ouvrez le fichier *global.asax.cs* .
5. Modifiez le fichier *global.asax.cs* en ajoutant le code mis en surbrillance en jaune afin qu’il apparaisse comme suit :  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Notez que `AddUserAndRole` est souligné en rouge. Double-cliquez sur le code AddUserAndRole.  
   La lettre « A » au début de la méthode mise en surbrillance est soulignée.
7. Pointez sur la lettre « A », puis cliquez sur l’interface utilisateur qui vous permet de générer un stub de méthode pour la méthode `AddUserAndRole`. 

    ![Appartenance et administration-générer un stub de méthode](membership-and-administration/_static/image1.png)
8. Cliquez sur l’option intitulée :  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Ouvrez le fichier *RoleActions.cs* à partir du dossier *logique* .  
   La méthode `AddUserAndRole` a été ajoutée au fichier de classe.
10. Modifiez le fichier *RoleActions.cs* en supprimant le `NotImplementedException` et en ajoutant le code mis en surbrillance en jaune, afin qu’il apparaisse comme suit :  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Le code ci-dessus établit tout d’abord un contexte de base de données pour la base de données d’appartenance. La base de données d’appartenance est également stockée sous la forme d’un fichier *. mdf* dans le dossier de données de l' *application\_* . Vous serez en mesure d’afficher cette base de données une fois que le premier utilisateur s’est connecté à cette application Web. 

> [!NOTE] 
> 
> Si vous souhaitez stocker les données d’appartenance avec les données du produit, vous pouvez envisager d’utiliser le **DbContext** que vous avez utilisé pour stocker les données du produit dans le code ci-dessus.

 Le mot clé *Internal* est un modificateur d’accès pour les types (tels que les classes) et les membres de type (tels que les méthodes ou les propriétés). Les types ou membres internes sont accessibles uniquement dans les fichiers contenus dans le même assembly *(fichier. dll* ). Quand vous générez votre application, un fichier d’assembly *(. dll*) est créé et contient le code qui est exécuté lors de l’exécution de votre application. 

Un objet `RoleStore`, qui assure la gestion des rôles, est créé en fonction du contexte de la base de données.

> [!NOTE] 
> 
> Notez que lorsque l’objet `RoleStore` est créé, il utilise un type de `IdentityRole` générique. Cela signifie que la `RoleStore` est autorisée uniquement à contenir des objets `IdentityRole`. En outre, à l’aide de génériques, les ressources en mémoire sont mieux gérées.

Ensuite, l’objet `RoleManager` est créé en fonction de l’objet `RoleStore` que vous venez de créer. l’objet `RoleManager` expose l’API associée au rôle qui peut être utilisée pour enregistrer automatiquement les modifications apportées au `RoleStore`. Le `RoleManager` est uniquement autorisé à contenir des objets `IdentityRole`, car le code utilise le type générique `<IdentityRole>`.

Vous appelez la méthode `RoleExists` pour déterminer si le rôle « canEdit » est présent dans la base de données d’appartenance. Si ce n’est pas le cas, vous créez le rôle.

La création de l’objet `UserManager` semble plus complexe que le contrôle `RoleManager`, mais il est quasiment identique. Il est simplement codé sur une seule ligne plutôt que sur plusieurs. Ici, le paramètre que vous passez est instancié en tant que nouvel objet contenu dans la parenthèse.

Ensuite, vous créez l’utilisateur « canEditUser » en créant un objet de `ApplicationUser`. Ensuite, si vous avez correctement créé l’utilisateur, vous ajoutez l’utilisateur au nouveau rôle.

> [!NOTE] 
> 
> La gestion des erreurs sera mise à jour au cours du didacticiel « gestion des erreurs ASP.NET » plus loin dans cette série de didacticiels.

La prochaine fois que l’application démarre, l’utilisateur nommé « canEditUser » sera ajouté en tant que rôle nommé « canEdit » de l’application. Plus loin dans ce didacticiel, vous allez vous connecter en tant qu’utilisateur « canEditUser » pour afficher les fonctionnalités supplémentaires que vous ajouterez au cours de ce didacticiel. Pour plus d’informations sur les API relatives aux ASP.NET Identity, consultez l' [espace de noms Microsoft. Aspnet. Identity](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Pour plus d’informations sur l’initialisation du système de ASP.NET Identity, consultez [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Restriction de l’accès à la page d’administration

L’exemple d’application Wingtip Toys permet aux utilisateurs anonymes et aux utilisateurs connectés d’afficher et d’acheter des produits. Toutefois, l’utilisateur connecté disposant du rôle « canEdit » personnalisé peut accéder à une page restreinte afin d’ajouter et de supprimer des produits.

#### <a name="add-an-administration-folder-and-page"></a>Ajouter un dossier et une page d’administration

Ensuite, vous allez créer un dossier nommé *admin* pour l’utilisateur « canEditUser » appartenant au rôle personnalisé de l’exemple d’application Wingtip Toys.

1. Cliquez avec le bouton droit sur le nom du projet (**Wingtip Toys**) dans **Explorateur de solutions** , puis sélectionnez **Ajouter** -&gt; **nouveau dossier**.
2. Nommez le nouveau dossier *admin*.
3. Cliquez avec le bouton droit sur le dossier *admin* , puis sélectionnez **Ajouter** -&gt; **nouvel élément**.   
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
4. Sélectionnez le <strong>groupe C# Visual</strong>-&gt; des modèles <strong>Web</strong> sur la gauche. Dans la liste du milieu, sélectionnez <strong>Web Form avec page maître</strong>, nommez-le <em>AdminPage. aspx</em><strong>,</strong> puis sélectionnez <strong>Ajouter</strong>.
5. Sélectionnez le fichier *site. Master* comme page maître, puis choisissez **OK**.

#### <a name="add-a-webconfig-file"></a>Ajouter un fichier Web. config

En ajoutant un fichier *Web. config* au dossier *admin* , vous pouvez restreindre l’accès à la page contenue dans le dossier.

1. Cliquez avec le bouton droit sur le dossier *admin* et sélectionnez **Ajouter** -&gt; **nouvel élément**.  
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Dans la liste des modèles C# Web visuels, sélectionnez <strong>fichier de configuration Web</strong>dans la liste du milieu, acceptez le nom par défaut de <em>Web. config</em><strong>,</strong> puis sélectionnez <strong>Ajouter</strong>.
3. Remplacez le contenu XML existant dans le fichier *Web.config* par le code suivant :  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Enregistrez le fichier *Web.config* . Le fichier *Web. config* spécifie que seul l’utilisateur appartenant au rôle « canEdit » de l’application peut accéder à la page contenue dans le dossier *admin* .

### <a name="including-custom-role-navigation"></a>Inclusion de la navigation dans les rôles personnalisés

Pour permettre à l’utilisateur du rôle « canEdit » personnalisé d’accéder à la section d’administration de l’application, vous devez ajouter un lien vers la page *site. Master* . Seuls les utilisateurs qui appartiennent au rôle « canEdit » seront en mesure de voir le lien d' **administration** et d’accéder à la section Administration.

1. Dans Explorateur de solutions, recherchez et ouvrez la page *site. Master* .
2. Pour créer un lien pour l’utilisateur du rôle « canEdit », ajoutez la balise mise en surbrillance en jaune à la liste non triée `<ul>` élément suivant afin que la liste apparaisse comme suit :  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Ouvrez le fichier *site.Master.cs* . Rendez le lien d' **administration** visible uniquement pour l’utilisateur « canEditUser » en ajoutant le code mis en surbrillance en jaune au gestionnaire de `Page_Load`. Le gestionnaire de `Page_Load` s’affiche comme suit :   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Lors du chargement de la page, le code vérifie si l’utilisateur connecté a le rôle « canEdit ». Si l’utilisateur appartient au rôle « canEdit », l’élément span contenant le lien vers la page *AdminPage. aspx* (et, par conséquent, le lien à l’intérieur de l’étendue) est rendu visible.

### <a name="enabling-product-administration"></a>Activation de l’administration du produit

Jusqu’à présent, vous avez créé le rôle « canEdit » et ajouté un utilisateur « canEditUser », un dossier d’administration et une page d’administration. Vous avez défini des droits d’accès pour le dossier et la page d’administration et vous avez ajouté un lien de navigation pour l’utilisateur du rôle « canEdit » à l’application. Ensuite, vous allez ajouter le balisage à la page *AdminPage. aspx* et le code au fichier code-behind *AdminPage.aspx.cs* qui permettra à l’utilisateur disposant du rôle « canEdit » d’ajouter et de supprimer des produits.

1. Dans **Explorateur de solutions**, ouvrez le fichier *AdminPage. aspx* à partir du dossier *admin* .
2. Remplacez le balisage existant par le code suivant :  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Ensuite, ouvrez le fichier code-behind *AdminPage.aspx.cs* en cliquant avec le bouton droit sur *AdminPage. aspx* et en cliquant sur **afficher le code**.
4. Remplacez le code existant dans le fichier code-behind *AdminPage.aspx.cs* par le code suivant :  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

Dans le code que vous avez entré pour le fichier code-behind *AdminPage.aspx.cs* , une classe appelée `AddProducts` effectue le travail réel d’ajout de produits à la base de données. Cette classe n’existe pas encore. vous allez donc la créer maintenant.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *logique* , puis sélectionnez **Ajouter** -&gt; **nouvel élément**.   
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sélectionnez le **groupe C# Visual** -&gt; de modèles de **code** sur la gauche. Ensuite, sélectionnez **classe**dans la liste du milieu, puis nommez-la *AddProducts.cs*.   
   Le nouveau fichier de classe s’affiche.
3. Remplacez le code existant par le code ci-dessous :  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

La page *AdminPage. aspx* permet à l’utilisateur appartenant au rôle « canEdit » d’ajouter et de supprimer des produits. Quand un nouveau produit est ajouté, les détails sur le produit sont validés, puis entrés dans la base de données. Le nouveau produit est immédiatement disponible pour tous les utilisateurs de l’application Web.

#### <a name="unobtrusive-validation"></a>Validation discrète

Les détails du produit que l’utilisateur fournit sur la page *AdminPage. aspx* sont validés à l’aide des contrôles de validation (`RequiredFieldValidator` et `RegularExpressionValidator`). Ces contrôles utilisent automatiquement la validation discrète. Une validation discrète permet aux contrôles de validation d’utiliser JavaScript pour la logique de validation côté client, ce qui signifie que la page ne nécessite pas de trajet vers le serveur à valider. Par défaut, la validation discrète est incluse dans le fichier *Web. config* en fonction du paramètre de configuration suivant :

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Expressions régulières

Le prix du produit sur la page *AdminPage. aspx* est validé à l’aide d’un contrôle **RegularExpressionValidator** . Ce contrôle vérifie si la valeur du contrôle d’entrée associé (zone de texte « AddProductPrice ») correspond au modèle spécifié par l’expression régulière. Une expression régulière est une notation de critères spéciaux qui vous permet de rechercher et de faire correspondre rapidement des modèles de caractères spécifiques. Le contrôle **RegularExpressionValidator** inclut une propriété nommée `ValidationExpression` qui contient l’expression régulière utilisée pour valider l’entrée de prix, comme indiqué ci-dessous :

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>FileUpload, contrôle

En plus des contrôles d’entrée et de validation, vous avez ajouté le contrôle **FileUpload** à la page *AdminPage. aspx* . Ce contrôle permet de télécharger des fichiers. Dans ce cas, vous autorisez uniquement le téléchargement des fichiers image. Dans le fichier code-behind (*AdminPage.aspx.cs*), quand l’utilisateur clique sur le `AddProductButton`, le code vérifie la propriété `HasFile` du contrôle **FileUpload** . Si le contrôle a un fichier et que le type de fichier (basé sur l’extension de fichier) est autorisé, l’image est enregistrée dans le dossier *images* et le dossier *images/thumbs* de l’application.

#### <a name="model-binding"></a>Liaison de modèle

Plus haut dans cette série de didacticiels, vous avez utilisé la liaison de modèle pour remplir un contrôle **ListView** , un contrôle **FormsView** , un contrôle **GridView** et un contrôle **DetailView** . Dans ce didacticiel, vous utilisez la liaison de modèle pour remplir un contrôle **DropDownList** avec une liste de catégories de produits.

Le balisage que vous avez ajouté au fichier *AdminPage. aspx* contient un contrôle **DropDownList** appelé `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Vous utilisez la liaison de modèle pour remplir ce **DropDownList** en définissant l’attribut `ItemType` et l’attribut `SelectMethod`. L’attribut `ItemType` spécifie que vous utilisez le type `WingtipToys.Models.Category` lors du remplissage du contrôle. Vous avez défini ce type au début de cette série de didacticiels en créant la classe `Category` (illustrée ci-dessous). La classe `Category` se trouve dans le dossier *Models* à l’intérieur du fichier *Category.cs* .

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

L’attribut `SelectMethod` du contrôle **DropDownList** spécifie que vous utilisez la méthode `GetCategories` (illustrée ci-dessous) qui est incluse dans le fichier code-behind (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Cette méthode spécifie qu’une interface `IQueryable` est utilisée pour évaluer une requête sur un type `Category`. La valeur retournée est utilisée pour remplir le **DropDownList** dans le balisage de la page (*AdminPage. aspx*).

Le texte affiché pour chaque élément de la liste est spécifié en définissant l’attribut `DataTextField`. L’attribut `DataTextField` utilise la `CategoryName` de la classe `Category` (illustrée ci-dessus) pour afficher chaque catégorie dans le contrôle **DropDownList** . La valeur réelle qui est passée lorsqu’un élément est sélectionné dans le contrôle **DropDownList** est basée sur l’attribut `DataValueField`. L’attribut `DataValueField` est défini sur le `CategoryID` comme défini dans la classe `Category` (voir ci-dessus).

### <a name="how-the-application-will-work"></a>Fonctionnement de l’application

Lorsque l’utilisateur qui appartient au rôle « canEdit » accède à la page pour la première fois, le contrôle `DropDownAddCategory`**DropDownList** est rempli comme décrit ci-dessus. Le contrôle `DropDownRemoveProduct`**DropDownList** est également rempli avec les produits qui utilisent la même approche. L’utilisateur qui appartient au rôle « canEdit » sélectionne le type de catégorie et ajoute des détails sur le produit (**nom**, **Description**, **prix**et **fichier image**). Lorsque l’utilisateur qui appartient au rôle « canEdit » clique sur le bouton **Add Product** , le gestionnaire d’événements `AddProductButton_Click` est déclenché. Le gestionnaire d’événements `AddProductButton_Click` situé dans le fichier code-behind (*AdminPage.aspx.cs*) vérifie le fichier image pour s’assurer qu’il correspond aux types de fichiers autorisés *(. gif*, *. png*, *. jpeg*ou *. jpg*). Ensuite, le fichier image est enregistré dans un dossier de l’exemple d’application Wingtip Toys. Le nouveau produit est ensuite ajouté à la base de données. Pour ajouter un nouveau produit, une nouvelle instance de la classe `AddProducts` est créée et nommée Products. La classe `AddProducts` a une méthode nommée `AddProduct`, et l’objet Products appelle cette méthode pour ajouter des produits à la base de données.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Si le code ajoute correctement le nouveau produit à la base de données, la page est rechargée avec la valeur de chaîne de requête `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Lors du rechargement de la page, la chaîne de requête est incluse dans l’URL. En rechargeant la page, l’utilisateur qui appartient au rôle « canEdit » peut immédiatement voir les mises à jour dans les contrôles **DropDownList** sur la page *AdminPage. aspx* . En outre, en incluant la chaîne de requête avec l’URL, la page peut afficher un message de réussite à l’utilisateur appartenant au rôle « canEdit ».

Lors du rechargement de la page *AdminPage. aspx* , l’événement `Page_Load` est appelé.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

Le gestionnaire d’événements `Page_Load` vérifie la valeur de chaîne de requête et détermine s’il faut afficher un message de réussite.

## <a name="running-the-application"></a>Exécution de l'application

Vous pouvez exécuter l’application maintenant pour voir comment vous pouvez ajouter, supprimer et mettre à jour des éléments dans le panier d’achat. Le total du panier d’achat reflète le coût total de tous les articles dans le panier d’achat.

1. Dans Explorateur de solutions, appuyez sur **F5** pour exécuter l’exemple d’application Wingtip Toys.  
   Le navigateur s’ouvre et affiche la page *default. aspx* .
2. Cliquez sur le lien **se connecter en** haut de la page. 

    ![Appartenance et administration-lien de connexion](membership-and-administration/_static/image2.png)

   La page *login. aspx* s’affiche.
3. Utilisez le nom d’utilisateur et le mot de passe suivants :  
   Nom d’utilisateur : canEditUser@wingtiptoys.com  
   Mot de passe : PA $ $word 1 

    ![Appartenance et administration-page de connexion](membership-and-administration/_static/image3.png)
4. Cliquez sur le bouton **se connecter dans** la partie inférieure de la page.
5. En haut de la page suivante, sélectionnez le lien **administrateur** pour accéder à la page *AdminPage. aspx* . 

    ![Appartenance et administration-lien d’administration](membership-and-administration/_static/image4.png)
6. Pour tester la validation d’entrée, cliquez sur le bouton **Ajouter un produit** sans ajouter de détails sur le produit. 

    ![Appartenance et administration-page d’administration](membership-and-administration/_static/image5.png)

   Notez que les messages de champ requis s’affichent.
7. Ajoutez les détails d’un nouveau produit, puis cliquez sur le bouton **Ajouter un produit** . 

    ![Appartenance et administration-ajouter un produit](membership-and-administration/_static/image6.png)
8. Sélectionnez **produits** dans le menu de navigation supérieur pour afficher le nouveau produit que vous avez ajouté. 

    ![Appartenance et administration-afficher le nouveau produit](membership-and-administration/_static/image7.png)
9. Cliquez sur le lien **administrateur** pour revenir à la page d’administration.
10. Dans la section **supprimer le produit** de la page, sélectionnez le nouveau produit que vous avez ajouté dans le **DropDownListBox**.
11. Cliquez sur le bouton **supprimer le produit** pour supprimer le nouveau produit de l’application. 

    ![Appartenance et administration-supprimer le produit](membership-and-administration/_static/image8.png)
12. Sélectionnez **produits** dans le menu de navigation supérieur pour confirmer que le produit a été supprimé.
13. Cliquez sur Fermer la **session** pour le mode d’administration existant.   
    Notez que le volet de navigation supérieur n’affiche plus l’élément de menu **admin** .

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez ajouté un rôle personnalisé et un utilisateur appartenant au rôle personnalisé, limité l’accès au dossier et à la page d’administration et fourni la navigation pour l’utilisateur appartenant au rôle personnalisé. Vous avez utilisé la liaison de modèle pour remplir un contrôle **DropDownList** avec des données. Vous avez implémenté le contrôle **FileUpload** et les contrôles de validation. En outre, vous avez appris à ajouter et à supprimer des produits d’une base de données. Dans le didacticiel suivant, vous apprendrez à implémenter le routage ASP.NET.

## <a name="additional-resources"></a>Ressources supplémentaires

[Élément Web. config-Authorization](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Déployer une application ASP.NET Web Forms sécurisée avec appartenance, OAuth et SQL Database à un site Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure-essai gratuit](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Précédent](checkout-and-payment-with-paypal.md)
> [Suivant](url-routing.md)
