---
uid: web-pages/overview/data/5-working-with-data
title: Présentation de l’utilisation d’une base de données dans des sites pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre décrit comment accéder aux données d’une base de données et les afficher à l’aide de pages Web ASP.NET.
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 45e988d037465e59ad352bb9444af2c69fd3cd70
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586533"
---
# <a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Présentation de l’utilisation d’une base de données dans des sites pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment utiliser les outils Microsoft WebMatrix pour créer une base de données dans un site Web pages Web ASP.NET (Razor) et comment créer des pages qui vous permettent d’afficher, d’ajouter, de modifier et de supprimer des données.
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment créer une base de données.
> - Comment se connecter à une base de données.
> - Comment afficher des données dans une page Web.
> - Comment insérer, mettre à jour et supprimer des enregistrements de base de données.
> 
> Les fonctionnalités présentées dans l’article sont les suivantes :
> 
> - Utilisation d’une base de données Microsoft SQL Server Compact Edition.
> - Utilisation de requêtes SQL.
> - La classe `Database`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> Ce didacticiel fonctionne également avec WebMatrix 3. Vous pouvez utiliser pages Web ASP.NET 3 et Visual Studio 2013 (ou Visual Studio Express 2013 pour le Web); Toutefois, l’interface utilisateur sera différente.

## <a name="introduction-to-databases"></a>Présentation des bases de données

Imaginez un carnet d’adresses classique. Pour chaque entrée dans le carnet d’adresses (c’est-à-dire, pour chaque personne), vous avez plusieurs éléments d’information, tels que le prénom, le nom, l’adresse, l’adresse de messagerie et le numéro de téléphone.

Une façon classique d’imager des données comme celle-ci est une table avec des lignes et des colonnes. En termes de base de données, chaque ligne est souvent appelée un enregistrement. Chaque colonne (parfois appelée champs) contient une valeur pour chaque type de données : prénom, nom, etc.

| **ID** | **FirstName** | **LastName** | **Adresse** | **Courrier électronique** | **Téléphone** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100 ème er SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Beaune | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Pour la plupart des tables de base de données, la table doit contenir une colonne qui contient un identificateur unique, tel qu’un numéro de client, un numéro de compte, etc. C’est ce que l’on appelle la *clé primaire*de la table et vous l’utilisez pour identifier chaque ligne dans la table. Dans l’exemple, la colonne ID est la clé primaire du carnet d’adresses.

Avec cette compréhension de base des bases de données, vous êtes prêt à apprendre à créer une base de données simple et à effectuer des opérations telles que l’ajout, la modification et la suppression de données.

> [!TIP] 
> 
> **Bases de données relationnelles**
> 
> Vous pouvez stocker des données de nombreuses façons, notamment des fichiers texte et des feuilles de calcul. Pour la plupart des utilisations commerciales, cependant, les données sont stockées dans une base de données relationnelle.
> 
> Cet article ne s’approfondit pas dans les bases de données. Toutefois, il peut être utile d’en comprendre un peu plus. Dans une base de données relationnelle, les informations sont logiquement divisées en tables distinctes. Par exemple, une base de données pour une école peut contenir des tables distinctes pour les élèves et pour les offres de classe. Le logiciel de base de données (tel que SQL Server) prend en charge des commandes puissantes qui vous permettent d’établir dynamiquement des relations entre les tables. Par exemple, vous pouvez utiliser la base de données relationnelle pour établir une relation logique entre les élèves et les classes, afin de créer une planification. Le stockage de données dans des tables distinctes réduit la complexité de la structure de la table et réduit la nécessité de conserver des données redondantes dans les tables.

## <a name="creating-a-database"></a>Création d’une base de données

Cette procédure vous montre comment créer une base de données nommée SmallBakery à l’aide de l’outil de conception de base de données SQL Server Compact inclus dans WebMatrix. Bien que vous puissiez créer une base de données à l’aide du code, il est plus courant de créer la base de données et les tables de base de données à l’aide d’un outil de conception comme WebMatrix.

1. Démarrez WebMatrix, puis dans la page Démarrage rapide, cliquez sur **site à partir d’un modèle**.
2. Sélectionnez **site vide**, puis dans la zone **nom du site** , entrez « SmallBakery », puis cliquez sur **OK**. Le site est créé et affiché dans WebMatrix.
3. Dans le volet gauche, cliquez sur l’espace **de travail bases de données** .
4. Dans le ruban, cliquez sur **nouvelle base de données**. Une base de données vide est créée avec le même nom que votre site.
5. Dans le volet gauche, développez le nœud **SmallBakery. sdf** , puis cliquez sur **tables**.
6. Dans le ruban, cliquez sur **nouvelle table**. WebMatrix ouvre le concepteur de tables.

    ![[image]](5-working-with-data/_static/image1.jpg)
7. Cliquez dans la colonne **nom** et entrez &quot;ID&quot;.
8. Dans la colonne **type de données** , sélectionnez **int**.
9. Définissez les options **est la clé primaire** et **est identifier ?** sur **Oui**.

    Comme son nom l’indique, **est une clé primaire** qui indique à la base de données qu’il s’agit de la clé primaire de la table. **Is Identity** indique à la base de données de créer automatiquement un numéro d’identification pour chaque nouvel enregistrement et de lui attribuer le numéro séquentiel suivant (à partir de 1).
10. Cliquez sur dans la ligne suivante. L’éditeur démarre une nouvelle définition de colonne.
11. Pour la valeur nom, entrez &quot;nom&quot;.
12. Pour **type de données**, choisissez &quot;nvarchar&quot; et définissez la longueur sur 50. La partie *var* de `nvarchar` indique à la base de données que les données de cette colonne seront une chaîne dont la taille peut varier d’un enregistrement à l’autre. (Le préfixe *n* représente *national*, ce qui indique que le champ peut contenir des données de type caractère représentant &#8212; n’importe quel ordre alphabétique ou système d’écriture qui est, que le champ contient des données Unicode.)
13. Affectez la valeur **non**à l’option **autoriser les valeurs NULL** . Cela permet d’appliquer que la colonne *Name* n’est pas vide.
14. À l’aide de ce même processus, créez une colonne nommée *Description*. Définissez **type de données** sur « nvarchar » et 50 pour la longueur, et affectez à **autoriser les valeurs NULL** la valeur false.
15. Créez une colonne nommée *Price*. Affectez **à type de données la valeur « Money »** et affectez à **autoriser les valeurs NULL** la valeur false.
16. Dans la zone située en haut, nommez la table &quot;&quot;produit.

    Lorsque vous avez terminé, la définition se présente comme suit :

    ![[image]](5-working-with-data/_static/image2.png)
17. Appuyez sur CTRL + S pour enregistrer la table.

## <a name="adding-data-to-the-database"></a>Ajout de données à la base de données

Vous pouvez maintenant ajouter des exemples de données à votre base de données que vous utiliserez plus loin dans cet article.

1. Dans le volet gauche, développez le nœud **SmallBakery. sdf** , puis cliquez sur **tables**.
2. Cliquez avec le bouton droit sur la table Product, puis cliquez sur **données**.
3. Dans le volet d’édition, entrez les enregistrements suivants :

    | **Name** | **Description** | **Compétitif** |
    | --- | --- | --- |
    | Pain | Cuit à nouveau tous les jours. | 2.99 |
    | Frais Shortcake | Créé avec des frais organiques de notre jardin. | 9.99 |
    | Secteurs Apple | Deuxièmement uniquement dans le secteur de votre MOM. | 12.99 |
    | Pecan secteurs | Si vous aimez pecans, c’est pour vous. | 10.99 |
    | Camembert citron | Créé avec les meilleurs citrons dans le monde. | 11.99 |
    | Petits gâteaux | Vos enfants et l’enfant de vous les apprécieront. | 7.99 |

    N’oubliez pas que vous n’avez rien à saisir pour la colonne *ID* . Lorsque vous avez créé la colonne *ID* , vous affectez à sa propriété **is Identity** la valeur true, ce qui entraîne son remplissage automatique.

    Lorsque vous avez terminé d’entrer les données, le concepteur de tables se présente comme suit :

    ![[image]](5-working-with-data/_static/image3.jpg)
4. Fermez l’onglet qui contient les données de la base de données.

## <a name="displaying-data-from-a-database"></a>Affichage de données à partir d’une base de données

Une fois que vous disposez d’une base de données contenant des données, vous pouvez afficher les données dans une page Web ASP.NET. Pour sélectionner les lignes de table à afficher, vous utilisez une instruction SQL, qui est une commande que vous transmettez à la base de données.

1. Dans le volet gauche, cliquez sur l’espace de travail **fichiers** .
2. À la racine du site Web, créez une nouvelle page CSHTML nommée *ListProducts. cshtml*.
3. Remplacez le balisage existant par le code suivant :

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    Dans le premier bloc de code, vous ouvrez le fichier *SmallBakery. sdf* (base de données) que vous avez créé précédemment. La méthode `Database.Open` suppose que le fichier *. sdf* se trouve dans le dossier de *données de l’application\_* de votre site Web. (Notez que vous n’avez pas besoin de spécifier l’extension &#8212; *. sdf* en fait, si vous le faites, la méthode `Open` ne fonctionnera pas.)

    > [!NOTE]
    > Le dossier *application\_Data* est un dossier spécial dans ASP.net qui est utilisé pour stocker les fichiers de données. Pour plus d’informations, consultez [connexion à une base de données](#SB_ConnectingToADatabase) plus loin dans cet article.

    Vous effectuez ensuite une requête pour interroger la base de données à l’aide de l’instruction SQL `Select` suivante :

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    Dans l’instruction, `Product` identifie la table à interroger. Le caractère `*` spécifie que la requête doit retourner toutes les colonnes de la table. (Vous pouvez également répertorier les colonnes individuellement, séparées par des virgules, si vous souhaitez afficher uniquement certaines colonnes.) La clause `Order By` indique la manière dont les données doivent &#8212; être triées dans ce cas, par la colonne *nom* . Cela signifie que les données sont triées par ordre alphabétique en fonction de la valeur de la colonne *Name* pour chaque ligne.

    Dans le corps de la page, le balisage crée un tableau HTML qui sera utilisé pour afficher les données. À l’intérieur de l’élément `<tbody>`, vous utilisez une boucle `foreach` pour obtenir individuellement chaque ligne de données retournée par la requête. Pour chaque ligne de données, vous créez une ligne de tableau HTML (élément`<tr>`). Ensuite, vous créez des cellules de tableau HTML (éléments`<td>`) pour chaque colonne. Chaque fois que vous parcourez la boucle, la ligne suivante disponible dans la base de données se trouve dans la variable `row` (vous la configurez dans l’instruction `foreach`). Pour obtenir une colonne individuelle à partir de la ligne, vous pouvez utiliser `row.Name` ou `row.Description` ou n’importe quel nom de la colonne de votre choix.
4. Exécutez la page dans un navigateur. (Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.) La page affiche une liste semblable à la suivante :

    ![[image]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **SQL (Structured Query Language)**
> 
> SQL est un langage utilisé dans la plupart des bases de données relationnelles pour la gestion des données dans une base de données. Il comprend des commandes qui vous permettent de récupérer des données et de les mettre à jour, et qui vous permettent de créer, modifier et gérer des tables de base de données. SQL est différent d’un langage de programmation (comme celui que vous utilisez dans WebMatrix), car avec SQL, l’idée est que vous indiquez à la base de données ce que vous voulez, et c’est la tâche de la base de données de déterminer comment obtenir les données ou effectuer la tâche. Voici quelques exemples de commandes SQL et ce qu’elles font :
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Cette commande extrait les colonnes *ID*, *Name*et *Price* des enregistrements de la table *Product* si la valeur *Price* est supérieure à 10, et retourne les résultats par ordre alphabétique en fonction des valeurs de la colonne *Name* . Cette commande retourne un jeu de résultats qui contient les enregistrements qui répondent aux critères ou un ensemble vide si aucun enregistrement ne correspond.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Cela insère un nouvel enregistrement dans la table *Product* , en définissant la colonne *Name* sur &quot;croissant&quot;, la colonne *Description* pour &quot;un&quot;de la satisfaction des regards et le prix à 1,99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Cette commande supprime les enregistrements de la table *Product* dont la colonne Date d’expiration est antérieure au 1er janvier 2008. (Cela suppose que la table *Product* possède une telle colonne, bien évidemment.) La date est entrée ici au format MM/JJ/AAAA, mais elle doit être entrée au format utilisé pour vos paramètres régionaux.
> 
> Les commandes `Insert Into` et `Delete` ne retournent pas de jeux de résultats. Au lieu de cela, ils retournent un nombre qui indique le nombre d’enregistrements affectés par la commande.
> 
> Pour certaines de ces opérations (telles que l’insertion et la suppression d’enregistrements), le processus qui demande l’opération doit disposer des autorisations appropriées dans la base de données. C’est pour cette raison que, pour les bases de données de production, vous devez souvent fournir un nom d’utilisateur et un mot de passe lorsque vous vous connectez à la base de données.
> 
> Il existe des dizaines de commandes SQL, mais elles suivent toutes un modèle comme celui-ci. Vous pouvez utiliser des commandes SQL pour créer des tables de base de données, compter le nombre d’enregistrements dans une table, calculer les prix et effectuer de nombreuses opérations supplémentaires.

## <a name="inserting-data-in-a-database"></a>Insertion de données dans une base de données

Cette section explique comment créer une page qui permet aux utilisateurs d’ajouter un nouveau produit à la table de base de données du *produit* . Après l’insertion d’un nouvel enregistrement de produit, la page affiche la table mise à jour à l’aide de la page *ListProducts. cshtml* que vous avez créée dans la section précédente.

La page comprend une validation pour s’assurer que les données entrées par l’utilisateur sont valides pour la base de données. Par exemple, le code de la page permet de s’assurer qu’une valeur a été entrée pour toutes les colonnes requises.

1. Dans le site Web, créez un nouveau fichier CSHTML nommé *InsertProducts. cshtml*.
2. Remplacez le balisage existant par le code suivant :

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Le corps de la page contient un formulaire HTML avec trois zones de texte qui permettent aux utilisateurs d’entrer un nom, une description et un prix. Quand les utilisateurs cliquent sur le bouton **Insérer** , le code en haut de la page ouvre une connexion à la base de données *SmallBakery. sdf* . Vous pouvez ensuite récupérer les valeurs que l’utilisateur a soumises à l’aide de l’objet `Request` et affecter ces valeurs à des variables locales.

    Pour confirmer que l’utilisateur a entré une valeur pour chaque colonne requise, vous devez inscrire chaque `<input>` élément que vous souhaitez valider :

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    Le `Validation` Helper vérifie qu’il existe une valeur dans chacun des champs que vous avez enregistrés. Vous pouvez vérifier si tous les champs ont été validés en vérifiant `Validation.IsValid()`, que vous faites généralement avant de traiter les informations que vous recevez de l’utilisateur :

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (L’opérateur `&&` signifie et — ce test est *s’il s’agit d’une soumission de formulaire et que tous les champs ont été validés*.)

    Si toutes les colonnes sont validées (aucune n’était vide), vous allez créer une instruction SQL pour insérer les données et les exécuter comme indiqué ci-dessous :

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Pour les valeurs à insérer, vous incluez des espaces réservés de paramètre (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Par mesure de sécurité, transmettez toujours des valeurs à une instruction SQL à l’aide de paramètres, comme vous pouvez le voir dans l’exemple précédent. Cela vous donne la possibilité de valider les données de l’utilisateur, et cela vous permet de vous protéger contre les tentatives d’envoi de commandes malveillantes à votre base de données (parfois appelées « attaques par injection SQL »).

    Pour exécuter la requête, vous utilisez cette instruction, en lui transmettant les variables qui contiennent les valeurs pour remplacer les espaces réservés :

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Après l’exécution de l’instruction `Insert Into`, vous envoyez l’utilisateur à la page qui répertorie les produits à l’aide de cette ligne :

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Si la validation n’a pas réussi, vous ignorez l’insertion. Au lieu de cela, vous disposez d’une application d’assistance dans la page qui peut afficher les messages d’erreur accumulés (le cas échéant) :

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Notez que le bloc de style dans le balisage comprend une définition de classe CSS nommée `.validation-summary-errors`. Il s’agit du nom de la classe CSS qui est utilisée par défaut pour l’élément `<div>` qui contient des erreurs de validation. Dans ce cas, la classe CSS spécifie que les erreurs de résumé de validation s’affichent en rouge et en gras, mais vous pouvez définir la classe `.validation-summary-errors` pour afficher la mise en forme de votre choix.

### <a name="testing-the-insert-page"></a>Test de la page d’insertion

1. Affichez la page dans un navigateur. La page affiche un formulaire similaire à celui présenté dans l’illustration suivante.

    ![[image]](5-working-with-data/_static/image5.jpg)
2. Entrez des valeurs pour toutes les colonnes, mais veillez à laisser la colonne *Price* vide.
3. Cliquez sur **Insérer**. La page affiche un message d’erreur, comme indiqué dans l’illustration suivante. (Aucun nouvel enregistrement n’est créé.)

    ![[image]](5-working-with-data/_static/image6.jpg)
4. Remplissez entièrement le formulaire, puis cliquez sur **Insérer**. Cette fois-ci, la page *ListProducts. cshtml* s’affiche et affiche le nouvel enregistrement.

## <a name="updating-data-in-a-database"></a>Mise à jour des données d’une base de données

Une fois les données entrées dans une table, vous devrez peut-être les mettre à jour. Cette procédure vous montre comment créer deux pages similaires à celles que vous avez créées pour l’insertion de données précédemment. La première page affiche les produits et permet aux utilisateurs de sélectionner un produit à modifier. La deuxième page permet aux utilisateurs d’apporter les modifications et de les enregistrer.

> [!NOTE] 
> 
> **Important** Dans un site Web de production, vous limitez généralement les personnes autorisées à apporter des modifications aux données. Pour plus d’informations sur la configuration de l’appartenance et sur les façons d’autoriser les utilisateurs à effectuer des tâches sur le site, consultez [Ajout de la sécurité et de l’appartenance à un site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202904).

1. Dans le site Web, créez un nouveau fichier CSHTML nommé *EditProducts. cshtml*.
2. Remplacez le balisage existant dans le fichier par le code suivant :

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    La seule différence entre cette page et la page *ListProducts. cshtml* de la version antérieure est que le tableau HTML de cette page comprend une colonne supplémentaire qui affiche un lien **modifier** . Lorsque vous cliquez sur ce lien, vous accédez à la page *UpdateProducts. cshtml* (que vous allez créer ensuite) dans laquelle vous pouvez modifier l’enregistrement sélectionné.

    Examinez le code qui crée le lien **modifier** :

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Cela crée un élément `<a>` HTML dont l’attribut `href` est défini dynamiquement. L’attribut `href` spécifie la page à afficher lorsque l’utilisateur clique sur le lien. Il passe également la valeur `Id` de la ligne actuelle au lien. Lorsque la page s’exécute, la source de la page peut contenir des liens comme suit :

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Notez que l’attribut `href` est défini sur `UpdateProducts/n`, où *n* est un numéro de produit. Lorsqu’un utilisateur clique sur l’un de ces liens, l’URL obtenue ressemble à ceci :

    `http://localhost:18816/UpdateProducts/6`

    En d’autres termes, le numéro de produit à modifier sera transmis à l’URL.
3. Affichez la page dans un navigateur. La page affiche les données dans un format similaire à ce qui suit :

    ![[image]](5-working-with-data/_static/image7.jpg)

    Ensuite, vous allez créer la page qui permet aux utilisateurs de mettre à jour les données. La page de mise à jour comprend une validation pour valider les données entrées par l’utilisateur. Par exemple, le code de la page permet de s’assurer qu’une valeur a été entrée pour toutes les colonnes requises.
4. Dans le site Web, créez un nouveau fichier CSHTML nommé *UpdateProducts. cshtml*.
5. Remplacez le balisage existant dans le fichier par le code suivant.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Le corps de la page contient un formulaire HTML dans lequel un produit est affiché et l’endroit où les utilisateurs peuvent le modifier. Pour obtenir le produit à afficher, utilisez l’instruction SQL suivante :

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Cela permet de sélectionner le produit dont l’ID correspond à la valeur passée dans le paramètre `@0`. (Étant donné que l' *ID* est la clé primaire et doit donc être unique, un seul enregistrement produit peut être sélectionné de cette façon.) Pour obtenir la valeur d’ID à transmettre à cette instruction `Select`, vous pouvez lire la valeur transmise à la page dans le cadre de l’URL, à l’aide de la syntaxe suivante :

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Pour récupérer réellement l’enregistrement du produit, vous utilisez la méthode `QuerySingle`, qui retourne un seul enregistrement :

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    La ligne unique est retournée dans la variable `row`. Vous pouvez récupérer des données de chaque colonne et les assigner à des variables locales comme suit :

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    Dans le balisage du formulaire, ces valeurs s’affichent automatiquement dans des zones de texte individuelles à l’aide d’un code incorporé comme suit :

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Cette partie du code affiche l’enregistrement du produit à mettre à jour. Une fois l’enregistrement affiché, l’utilisateur peut modifier des colonnes individuelles.

    Lorsque l’utilisateur envoie le formulaire en cliquant sur le bouton **mettre à jour** , le code du bloc `if(IsPost)` s’exécute. Cela obtient les valeurs de l’utilisateur de l’objet `Request`, stocke les valeurs dans les variables et valide que chaque colonne a été remplie. Si la validation réussit, le code crée l’instruction SQL UPDATE suivante :

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    Dans une instruction SQL `Update`, vous spécifiez chaque colonne à mettre à jour et la valeur à laquelle la définir. Dans ce code, les valeurs sont spécifiées à l’aide des espaces réservés de paramètre `@0`, `@1`, `@2`, et ainsi de suite. (Comme indiqué précédemment, pour la sécurité, vous devez toujours passer des valeurs à une instruction SQL à l’aide de paramètres.)

    Lorsque vous appelez la méthode `db.Execute`, vous transmettez les variables qui contiennent les valeurs dans l’ordre qui correspond aux paramètres dans l’instruction SQL :

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Après l’exécution de l’instruction `Update`, vous appelez la méthode suivante afin de rediriger l’utilisateur vers la page de modification :

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    L’effet est que l’utilisateur voit une liste mise à jour des données dans la base de données et peut modifier un autre produit.
6. Enregistrez la page.
7. Exécutez la page *EditProducts. cshtml* (pas la page mettre à jour), puis cliquez sur **modifier** pour sélectionner un produit à modifier. La page *UpdateProducts. cshtml* s’affiche, affichant l’enregistrement que vous avez sélectionné.

    ![[image]](5-working-with-data/_static/image8.jpg)
8. Apportez une modification, puis cliquez sur **mettre à jour**. La liste des produits s’affiche à nouveau avec vos données mises à jour.

## <a name="deleting-data-in-a-database"></a>Suppression de données dans une base de données

Cette section montre comment permettre aux utilisateurs de supprimer un produit de la table de base de données du *produit* . L’exemple se compose de deux pages. Dans la première page, les utilisateurs sélectionnent un enregistrement à supprimer. L’enregistrement à supprimer est ensuite affiché dans une deuxième page qui permet de confirmer qu’il souhaite supprimer l’enregistrement.

> [!NOTE] 
> 
> **Important** Dans un site Web de production, vous limitez généralement les personnes autorisées à apporter des modifications aux données. Pour plus d’informations sur la configuration de l’appartenance et sur les façons d’autoriser l’utilisateur à effectuer des tâches sur le site, consultez [Ajout de la sécurité et de l’appartenance à un site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202904).

1. Dans le site Web, créez un nouveau fichier CSHTML nommé *ListProductsForDelete. cshtml*.
2. Remplacez le balisage existant par le code suivant :

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Cette page est similaire à la page *EditProducts. cshtml* de la version antérieure. Toutefois, au lieu d’afficher un lien de **modification** pour chaque produit, il affiche un lien **supprimer** . Le lien **supprimer** est créé à l’aide du code incorporé suivant dans le balisage :

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Cela crée une URL qui ressemble à ce qui suit lorsque les utilisateurs cliquent sur le lien :

    `http://<server>/DeleteProduct/4`

    L’URL appelle une page nommée *DeleteProduct. cshtml* (que vous allez créer ensuite) et lui transmet l’ID du produit à supprimer (ici, 4).
3. Enregistrez le fichier, mais laissez-le ouvert.
4. Créez un autre fichier CHTML nommé *DeleteProduct. cshtml*. Remplacez le contenu existant par ce qui suit :

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Cette page est appelée par *ListProductsForDelete. cshtml* et permet aux utilisateurs de confirmer qu’ils veulent supprimer un produit. Pour répertorier le produit à supprimer, vous devez récupérer l’ID du produit à supprimer de l’URL à l’aide du code suivant :

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    La page demande ensuite à l’utilisateur de cliquer sur un bouton pour supprimer réellement l’enregistrement. Il s’agit d’une mesure de sécurité importante : lorsque vous effectuez des opérations sensibles sur votre site Web, telles que la mise à jour ou la suppression de données, ces opérations doivent toujours être effectuées à l’aide d’une opération de publication, et non d’une opération d’extraction. Si votre site est configuré de façon à ce qu’une opération de suppression puisse être effectuée à l’aide d’une opération d’extraction, tout le monde peut passer une URL comme `http://<server>/DeleteProduct/4` et supprimer tout ce qu’il veut de votre base de données. En ajoutant la confirmation et en codant la page de sorte que la suppression ne puisse être effectuée qu’à l’aide d’une publication, vous ajoutez une mesure de sécurité à votre site.

    L’opération de suppression réelle est effectuée à l’aide du code suivant, qui confirme d’abord qu’il s’agit d’une opération de publication et que l’ID n’est pas vide :

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Le code exécute une instruction SQL qui supprime l’enregistrement spécifié, puis redirige l’utilisateur vers la page de liste.
5. Exécutez *ListProductsForDelete. cshtml* dans un navigateur.

    ![[image]](5-working-with-data/_static/image9.jpg)
6. Cliquez sur le lien **supprimer** pour l’un des produits. La page *DeleteProduct. cshtml* s’affiche pour confirmer que vous souhaitez supprimer cet enregistrement.
7. Cliquez sur le bouton **Supprimer** . L’enregistrement du produit est supprimé et la page est actualisée avec une mise à jour de la liste des produits.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Connexion à une base de données
> 
> Vous pouvez vous connecter à une base de données de deux façons. La première consiste à utiliser la méthode `Database.Open` et à spécifier le nom du fichier de base de données (moins l’extension *. sdf* ) :
> 
> `var db = Database.Open("SmallBakery");`
> 
> La méthode `Open` suppose que le. le fichier *SDF* se trouve dans le dossier de données de l' *application\_* du site Web. Ce dossier est conçu spécifiquement pour contenir des données. Par exemple, il dispose des autorisations appropriées pour permettre au site Web de lire et d’écrire des données, et en tant que mesure de sécurité, WebMatrix n’autorise pas l’accès aux fichiers à partir de ce dossier.
> 
> La seconde consiste à utiliser une chaîne de connexion. Une chaîne de connexion contient des informations sur le mode de connexion à une base de données. Il peut s’agir d’un chemin d’accès à un fichier, ou il peut inclure le nom d’une base de données SQL Server sur un serveur local ou distant, ainsi qu’un nom d’utilisateur et un mot de passe pour se connecter à ce serveur. (Si vous conservez les données dans une version gérée de manière centralisée de SQL Server, par exemple sur le site d’un fournisseur d’hébergement, vous utilisez toujours une chaîne de connexion pour spécifier les informations de connexion à la base de données.)
> 
> Dans WebMatrix, les chaînes de connexion sont généralement stockées dans un fichier XML nommé *Web. config*. Comme son nom l’indique, vous pouvez utiliser un fichier *Web. config* à la racine de votre site Web pour stocker les informations de configuration du site, y compris les chaînes de connexion dont votre site peut avoir besoin. Voici un exemple de chaîne de connexion dans un fichier *Web. config* :
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> Dans l’exemple, la chaîne de connexion pointe vers une base de données dans une instance de SQL Server qui s’exécute sur un serveur quelque part (au lieu d’un fichier *. sdf* local). Vous devez substituer les noms appropriés pour `myServer` et `myDatabase`et spécifier SQL Server valeurs de connexion pour `username` et `password`. (Les valeurs nom d’utilisateur et mot de passe ne sont pas nécessairement les mêmes que celles de vos informations d’identification Windows ou de celles que votre fournisseur d’hébergement vous a donné pour vous connecter à leurs serveurs. Vérifiez auprès de l’administrateur les valeurs exactes dont vous avez besoin.)
> 
> La méthode `Database.Open` est flexible, car elle vous permet de transmettre le nom d’un fichier *. sdf* de base de données ou le nom d’une chaîne de connexion stockée dans le fichier *Web. config* . L’exemple suivant montre comment se connecter à la base de données à l’aide de la chaîne de connexion illustrée dans l’exemple précédent :
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Comme indiqué précédemment, la méthode `Database.Open` vous permet de transmettre un nom de base de données ou une chaîne de connexion, et de déterminer le nom à utiliser. Cela est très utile lorsque vous déployez (publiez) votre site Web. Vous pouvez utiliser un fichier *. sdf* dans le dossier de données de l' *application\_* lorsque vous développez et testez votre site. Ensuite, lorsque vous déplacez votre site vers un serveur de production, vous pouvez utiliser une chaîne de connexion dans le fichier *Web. config* portant le même nom que votre fichier *. sdf* , mais qui pointe vers la base &#8212; de données du fournisseur d’hébergement sans avoir à modifier votre code.
> 
> Enfin, si vous souhaitez travailler directement avec une chaîne de connexion, vous pouvez appeler la méthode `Database.OpenConnectionString` et la passer à la chaîne de connexion réelle plutôt qu’au nom d’une valeur dans le fichier *Web. config* . Cela peut être utile dans les situations où, pour une raison quelconque, vous n’avez pas accès à la chaîne de connexion (ou aux valeurs qu’elle contient, telles que le nom de fichier *. sdf* ) jusqu’à ce que la page soit en cours d’exécution. Toutefois, pour la plupart des scénarios, vous pouvez utiliser `Database.Open` comme décrit dans cet article.

## <a name="additional-resources"></a>Ressources supplémentaires

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Connexion à une base de données SQL Server ou MySQL dans WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Validation des entrées utilisateur dans les sites ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
