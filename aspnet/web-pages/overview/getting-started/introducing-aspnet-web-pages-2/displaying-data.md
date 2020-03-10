---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Présentation de l’affichage des données de pages Web ASP.NET | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer une base de données dans WebMatrix et comment afficher les données d’une base de données dans une page lorsque vous utilisez pages Web ASP.NET (Razor). Il suppose y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 9e665ca8dd064c23a8b8bd3593014969d0c3da48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525220"
---
# <a name="introducing-aspnet-web-pages---displaying-data"></a>Présentation de l’affichage des données de pages Web ASP.NET

par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre comment créer une base de données dans WebMatrix et comment afficher les données d’une base de données dans une page lorsque vous utilisez pages Web ASP.NET (Razor). Il part du principe que vous avez terminé la série jusqu’à la [Présentation de la programmation de pages Web ASP.net](../introducing-razor-syntax-c.md).
> 
> Ce que vous allez apprendre :
> 
> - Comment utiliser les outils WebMatrix pour créer une base de données et des tables de base de données.
> - Comment utiliser les outils WebMatrix pour ajouter des données à une base de données.
> - Comment afficher des données de la base de données sur une page.
> - Comment exécuter des commandes SQL dans pages Web ASP.NET.
> - Comment personnaliser l’assistance `WebGrid` pour modifier l’affichage des données et ajouter la pagination et le tri.
>   
> 
> Fonctionnalités/technologies présentées :
> 
> - Outils de base de données WebMatrix.
> - `WebGrid` Helper.

## <a name="what-youll-build"></a>Contenu

Dans le didacticiel précédent, vous avez été mis à pages Web ASP.NET (fichiers *. cshtml* ), aux principes de base de syntaxe Razor et aux applications d’assistance. Dans ce didacticiel, vous allez commencer à créer l’application Web que vous utiliserez pour le reste de la série. L’application est une application de cinéma simple qui vous permet d’afficher, d’ajouter, de modifier et de supprimer des informations sur les films.

Lorsque vous avez terminé ce didacticiel, vous pouvez afficher une liste de films ressemblant à cette page :

![Affichage WebGrid qui comprend les paramètres définis sur les noms de classe CSS](displaying-data/_static/image1.png)

Mais pour commencer, vous devez créer une base de données.

## <a name="a-very-brief-introduction-to-databases"></a>Une brève introduction aux bases de données

Ce didacticiel ne fournit que la présentation la plus rapide des bases de données. Si vous avez une expérience de base de données, vous pouvez ignorer cette petite section.

Une base de données contient une ou plusieurs tables qui contiennent des informations &mdash; par exemple, des tables pour les clients, les commandes et les fournisseurs, ou pour les élèves, les enseignants, les classes et les notes. Structurellement, une table de base de données est similaire à une feuille de calcul. Imaginez un carnet d’adresses classique. Pour chaque entrée dans le carnet d’adresses (c’est-à-dire, pour chaque personne), vous avez plusieurs éléments d’information, tels que le prénom, le nom, l’adresse, l’adresse de messagerie et le numéro de téléphone.

![Exemple de table de base de données sous la forme d’une grille simple](displaying-data/_static/image2.png)

(Les lignes sont parfois appelées *enregistrements*, et les colonnes sont parfois appelées *champs*.)

Pour la plupart des tables de base de données, la table doit contenir une colonne qui contient une valeur unique, telle qu’un numéro de client, un numéro de compte, etc. Cette valeur est appelée *clé primaire*de la table et vous l’utilisez pour identifier chaque ligne de la table. Dans l’exemple, la colonne ID est la clé primaire du carnet d’adresses indiqué dans l’exemple précédent.

La majeure partie du travail que vous effectuez dans les applications Web consiste à lire des informations dans la base de données et à les afficher sur une page. Vous pouvez également collecter des informations auprès des utilisateurs et les ajouter à une base de données, ou modifier les enregistrements qui se trouvent déjà dans la base de données. (Nous aborderons toutes ces opérations dans le cadre de ce didacticiel.)

Le travail de base de données peut être extrêmement complexe et peut nécessiter des connaissances spécialisées. Pour cet ensemble de didacticiels, toutefois, vous devez comprendre les concepts de base, qui seront tous expliqués au fur et à mesure.

## <a name="creating-a-database"></a>Création d’une base de données

WebMatrix comprend des outils qui facilitent la création d’une base de données et la création de tables dans la base de données. (La structure d’une base de données est appelée *schéma*de la base de données.) Pour cet ensemble de didacticiels, vous allez créer une base de données qui ne contient qu’une seule table &mdash; films.

Ouvrez WebMatrix si vous ne l’avez pas déjà fait, puis ouvrez le site WebPagesMovies que vous avez créé dans le didacticiel précédent.

Dans le volet gauche, cliquez sur l’espace **de travail base de données** .

![Onglet espace de travail de base de données WebMatrix](displaying-data/_static/image3.png)

Le ruban change pour afficher les tâches liées à la base de données. Dans le ruban, cliquez sur **nouvelle base de données**.

![Bouton’nouvelle base de données’dans le ruban de WebMatrix](displaying-data/_static/image4.png)

WebMatrix crée une base de données SQL Server CE (un fichier *. sdf* ) qui porte le même nom que votre site &mdash; *WebPagesMovies. sdf*. (Vous ne le faites pas ici, mais vous pouvez renommer le fichier comme vous le souhaitez, à condition qu’il ait une extension *. sdf* .)

## <a name="creating-a-table"></a>Création d’une table

Dans le ruban, cliquez sur **nouvelle table**. WebMatrix ouvre le concepteur de tables sous un nouvel onglet. (si l’option nouvelle table n’est pas disponible, assurez-vous que la nouvelle base de données est sélectionnée dans l’arborescence à gauche).

![Bouton « nouvelle table » dans le ruban de WebMatrix](displaying-data/_static/image5.png)

Dans la zone de texte située en haut (où le filigrane indique « entrez le nom de la table »), entrez « films ».

![Saisie d’un nom de table dans le concepteur de base de données WebMatrix](displaying-data/_static/image6.png)

Le volet situé sous le nom de la table vous permet de définir des colonnes individuelles. Pour le tableau *films* de ce didacticiel, vous allez créer uniquement quelques colonnes : *ID*, *titre*, *genre*et *année*.

Dans la zone **nom** , entrez « ID ». L’entrée d’une valeur ici active tous les contrôles de la nouvelle colonne.

Appuyez sur la touche Tab pour afficher la liste des **types de données** et choisissez **int**. Cette valeur spécifie que la colonne ID contiendra des données de type entier (Number).

> [!NOTE]
> Nous ne l’appellerons pas ici (en grande partie), mais vous pouvez utiliser des gestes de clavier Windows standard pour naviguer dans cette grille. Par exemple, vous pouvez utiliser la touche Tab entre les champs, mais vous pouvez simplement taper pour sélectionner un élément dans une liste, et ainsi de suite.

Tab au-delà de la zone de **valeur par défaut** (autrement dit, laissez-la vide). À la case à cocher **est une clé primaire** et sélectionnez-la. Cette option indique à la base de données que la colonne d' *ID* contient les données qui identifient des lignes individuelles. (Autrement dit, chaque ligne aura une valeur unique dans la colonne ID que vous pouvez utiliser pour rechercher cette ligne.)

Choisissez l’option **est l’identité** . Cette option indique à la base de données qu’elle doit générer automatiquement le numéro séquentiel suivant pour chaque nouvelle ligne. (L’option **is Identity** fonctionne uniquement si vous avez également sélectionné l’option **est une clé primaire** .)

Cliquez dans la ligne suivante de la grille ou appuyez deux fois sur Tab pour terminer la ligne actuelle. L’un ou l’autre geste enregistre la ligne actuelle et démarre la suivante. Notez que la colonne **valeur par défaut** indique désormais la valeur **null**. (Null est la valeur par défaut de la valeur par défaut, donc pour parler.)

Une fois que vous avez fini de définir la nouvelle colonne **ID** , le concepteur se présente comme suit :

![Concepteur de base de données WebMatrix après avoir défini la colonne ID pour le tableau films](displaying-data/_static/image7.png)

Pour créer la colonne suivante, cliquez dans la zone de la colonne **nom** . Entrez « title » pour la colonne, puis sélectionnez **nvarchar** comme valeur de **type de données** . La partie « var » de **nvarchar** indique à la base de données que les données de cette colonne seront une chaîne dont la taille peut varier d’un enregistrement à l’autre. (Le préfixe « n » représente « national », ce qui indique que le champ peut contenir des données de type caractère pour n’importe quel alphabet ou système d’écriture (autrement dit, le champ contient des données Unicode).

Lorsque vous choisissez **nvarchar**, une autre zone s’affiche, dans laquelle vous pouvez entrer la longueur maximale du champ. Entrez 50, en partant du principe qu’aucun titre de film avec lequel vous allez travailler dans ce didacticiel ne dépasse 50 caractères.

Ignorez la **valeur par défaut** et désactivez l’option **autoriser les valeurs NULL** . Vous ne souhaitez pas que la base de données autorise la saisie de films dans la base de données qui n’ont pas de titre.

Lorsque vous avez terminé et que vous passez à la ligne suivante, le concepteur ressemble à l’illustration suivante :

![Concepteur de base de données WebMatrix après avoir défini la colonne title pour le tableau films](displaying-data/_static/image8.png)

Répétez ces étapes pour créer une colonne nommée « genre », à l’exception de la longueur, définissez-la sur 30 uniquement.

Créez une autre colonne nommée « Year ». Pour le type de données, choisissez **nchar** (non **nvarchar**) et définissez la longueur sur 4. Pour l’année, vous allez utiliser un nombre à 4 chiffres comme « 1995 » ou « 2010 », de sorte que vous n’avez pas besoin d’une colonne de taille variable.

Voici à quoi ressemble la conception terminée :

![Concepteur de bases de données WebMatrix une fois tous les champs définis pour le tableau films](displaying-data/_static/image9.png)

Appuyez sur CTRL + S ou cliquez sur le bouton **Enregistrer** dans la barre d’outils accès rapide. Fermez le concepteur de base de données en fermant l’onglet.

## <a name="adding-some-example-data"></a>Ajout de quelques exemples de données

Plus loin dans cette série de didacticiels, vous allez créer une page dans laquelle vous pouvez entrer de nouveaux films dans un formulaire. Pour l’instant, toutefois, vous pouvez ajouter des exemples de données que vous pouvez ensuite afficher sur une page.

Dans l’espace **de travail base de données** de WebMatrix, Notez qu’il existe une arborescence qui vous montre le fichier *. sdf* que vous avez créé précédemment. Ouvrez le nœud de votre nouveau fichier *. sdf* , puis ouvrez le nœud **tables** .

![Espace de travail de base de données WebMatrix avec arborescence ouvrir au tableau films](displaying-data/_static/image10.png)

Cliquez avec le bouton droit sur le nœud **films** , puis choisissez **données**. WebMatrix ouvre une grille dans laquelle vous pouvez entrer des données pour le tableau *films* :

![Grille d’entrée de base de données dans WebMatrix (vide)](displaying-data/_static/image11.png)

Cliquez sur la colonne **title** et entrez « when Harry satisfaction Catherine ». Accédez à la colonne **genre** (vous pouvez utiliser la touche TAB) et entrez « romantique comédie ». Accédez à la colonne **year** et entrez « 1989 » :

![Grille d’entrée de base de données dans WebMatrix avec un enregistrement](displaying-data/_static/image12.png)

Appuyez sur entrée et WebMatrix enregistre le nouveau film. Notez que la colonne **ID** a été renseignée.

![Grille d’entrée de base de données dans WebMatrix avec un enregistrement et un ID généré automatiquement](displaying-data/_static/image13.png)

Entrez un autre film (par exemple, « passé avec le vent », « fiction », « 1939 »). La colonne ID est de nouveau remplie :

![Grille d’entrée de base de données dans WebMatrix avec deux enregistrements et des ID générés automatiquement](displaying-data/_static/image14.png)

Entrez un troisième film (par exemple, « Ghostbusters », « comédie »). En guise d’expérience, laissez la colonne **year** vide, puis appuyez sur entrée. Étant donné que vous avez désélectionné l’option **autoriser les valeurs NULL** , la base de données affiche une erreur :

![L’erreur « données non valides » s’affiche si une valeur de colonne requise est laissée vide](displaying-data/_static/image15.png)

Cliquez sur **OK** pour revenir en arrière et corriger l’entrée (l’année pour « Ghostbusters » est 1984), puis appuyez sur entrée.

Renseignez plusieurs films jusqu’à 8. (Si vous entrez 8, il est plus facile d’utiliser la pagination ultérieurement. Mais si cela est trop important, entrez-en quelques-uns pour l’instant.) Les données réelles n’ont pas d’importance.

![Grille d’entrée de base de données dans WebMatrix avec les deux enregistrements](displaying-data/_static/image16.png)

Si vous avez entré tous les films sans erreur, les valeurs d’ID sont séquentielles. Si vous avez essayé d’enregistrer un enregistrement de film incomplet, les numéros d’identification risquent de ne pas être séquentiels. Si c’est le cas, c’est parfait. Les nombres n’ont aucune signification inhérente, et la seule chose qui est importante est qu’ils sont uniques dans le tableau *films* .

Fermez l’onglet qui contient le concepteur de base de données.

Vous pouvez maintenant activer l’affichage de ces données sur une page Web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Affichage des données dans une page à l’aide du programme d’assistance WebGrid

Pour afficher des données dans une page, vous allez utiliser le programme d’assistance `WebGrid`. Cette application d’assistance produit un affichage dans une grille ou une table (lignes et colonnes). Comme vous le verrez, vous pourrez affiner la grille avec une mise en forme et d’autres fonctionnalités.

Pour exécuter la grille, vous devez écrire quelques lignes de code. Ces quelques lignes serviront de type de modèle pour presque tous les accès aux données que vous effectuez dans ce didacticiel.

> [!NOTE]
> En fait, vous disposez de nombreuses options pour afficher les données sur une page ; la `WebGrid` Helper n’est qu’une seule. Nous l’avons choisi pour ce didacticiel, car il s’agit de la méthode la plus simple pour afficher les données et parce qu’elles sont raisonnablement flexibles. Dans le prochain didacticiel, vous verrez comment utiliser une méthode plus « manuelle » pour travailler avec des données dans la page, ce qui vous donne un contrôle plus direct sur la façon d’afficher les données.

Dans le volet gauche de WebMatrix, cliquez sur l’espace de travail **fichiers** .

La nouvelle base de données que vous avez créée se trouve dans le dossier de données de l' *application\_* . Si le dossier n’existe pas déjà, WebMatrix l’a créé pour votre nouvelle base de données. (Le dossier peut avoir existé si vous aviez précédemment installé des applications auxiliaires.)

Dans l’arborescence, sélectionnez la racine du site Web. Vous devez sélectionner la racine du site Web. dans le cas contraire, le nouveau fichier peut être ajouté au dossier de données de l’application\_.

Dans le ruban, cliquez sur **nouveau**. Dans la zone **choisir un type de fichier** , choisissez **cshtml**.

Dans la zone **nom** , nommez la nouvelle page « movies. cshtml » :

![Boîte de dialogue Choisir un type de fichier avec la page « films »](displaying-data/_static/image17.png)

Cliquez sur le bouton **OK** . WebMatrix ouvre un nouveau fichier contenant quelques éléments squelettes. Tout d’abord, vous allez écrire du code pour accéder aux données de la base de données. Vous ajouterez ensuite le balisage à la page pour afficher les données.

### <a name="writing-the-data-query-code"></a>Écriture du code de requête de données

En haut de la page, entre les caractères `@{` et `}`, entrez le code suivant. (Veillez à entrer ce code entre les accolades ouvrantes et fermantes.)

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

La première ligne ouvre la base de données que vous avez créée précédemment, qui est toujours la première étape avant d’effectuer une opération sur la base de données. Vous indiquez le nom de la méthode `Database.Open` de la base de données à ouvrir. Notez que vous n’incluez pas *. sdf* dans le nom. La méthode `Open` suppose qu’elle recherche un fichier *. sdf* (autrement dit, *WebPagesMovies. sdf*) et que le fichier *. sdf* se trouve dans le dossier de *données de l’application\_* . (Précédemment, nous avons noté que le dossier de données de l' *application\_* est réservé ; ce scénario est l’un des endroits où ASP.NET fait des hypothèses sur ce nom.)

Lorsque la base de données est ouverte, une référence à celle-ci est placée dans la variable nommée `db`. (Qui pourrait être nommé n’importe quoi.) La variable `db` vous permet d’utiliser l’interaction avec la base de données.

La deuxième ligne extrait en fait les données de la base de données à l’aide de la méthode `Query`. Notez le fonctionnement de ce code : la variable `db` a une référence à la base de données ouverte et vous appelez la méthode `Query` à l’aide de la variable `db` (`db.Query`).

La requête elle-même est une instruction SQL `Select`. (Pour un peu plus d’informations sur SQL, reportez-vous à l’explication ultérieurement.) Dans l’instruction, `Movies` identifie la table à interroger. Le caractère `*` spécifie que la requête doit retourner toutes les colonnes de la table. (Vous pouvez également répertorier les colonnes individuellement, séparées par des virgules.)

Les résultats de la requête, le cas échéant, sont renvoyés et rendus disponibles dans la variable `selectedData`. Là encore, la variable peut être nommée n’importe quoi.

Enfin, la troisième ligne indique à ASP.NET que vous souhaitez utiliser une instance du programme d’assistance `WebGrid`. Vous créez (*instanciez*) l’objet d’assistance à l’aide du mot clé `new` et vous lui transmettez les résultats de la requête via la variable `selectedData`. Le nouvel objet `WebGrid`, ainsi que les résultats de la requête de base de données, sont rendus disponibles dans la variable `grid`. Vous aurez besoin de ce résultat dans un moment pour afficher les données dans la page.

À ce niveau, la base de données a été ouverte, vous avez obtenu les données de votre choix et vous avez préparé l’assistance `WebGrid` avec ces données. Ensuite, créez le balisage dans la page.

> [!TIP] 
> 
> **SQL (Structured Query Language)**
> 
> SQL est un langage utilisé dans la plupart des bases de données relationnelles pour la gestion des données dans une base de données. Il comprend des commandes qui vous permettent de récupérer des données et de les mettre à jour, et qui vous permettent de créer, modifier et gérer des données dans des tables de base de données. SQL est différent d’un langage de programmation ( C#comme). Avec SQL, vous indiquez à la base de données ce que vous souhaitez, et c’est la tâche de la base de données de déterminer comment obtenir les données ou effectuer la tâche. Voici quelques exemples de commandes SQL et ce qu’elles font :
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> La première instruction `Select` obtient toutes les colonnes (spécifiées par `*`) à partir du tableau *films* .
> 
> La deuxième instruction `Select` extrait les colonnes ID, Name et Price des enregistrements de la table *Product* dont la valeur de la colonne Price est supérieure à 10. La commande retourne les résultats par ordre alphabétique en fonction des valeurs de la colonne Name. Si aucun enregistrement ne correspond aux critères de prix, la commande retourne un ensemble vide.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Cette commande insère un nouvel enregistrement dans la table *Product* , en affectant à la colonne Name la valeur « croissant », à la colonne Description la valeur « a-de la satisfaction » et le prix à 1,99.
> 
> Notez que lorsque vous spécifiez une valeur non numérique, la valeur est placée entre guillemets simples (pas de guillemets doubles, comme dans C#). Vous utilisez ces guillemets autour des valeurs de texte ou de date, mais pas autour des nombres.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Cette commande supprime les enregistrements de la table *Product* dont la colonne Date d’expiration est antérieure au 1er janvier 2008. (La commande suppose que la table *Product* possède une telle colonne, bien évidemment.) La date est entrée ici au format MM/JJ/AAAA, mais elle doit être entrée au format utilisé pour vos paramètres régionaux.
> 
> Les commandes `Insert` et `Delete` ne retournent pas de jeux de résultats. Au lieu de cela, ils retournent un nombre qui indique le nombre d’enregistrements affectés par la commande.
> 
> Pour certaines de ces opérations (telles que l’insertion et la suppression d’enregistrements), le processus qui demande l’opération doit disposer des autorisations appropriées dans la base de données. C’est pourquoi, pour les bases de données de production, vous devez souvent fournir un nom d’utilisateur et un mot de passe lorsque vous vous connectez à la base de données.
> 
> Il existe des dizaines de commandes SQL, mais elles suivent toutes un modèle comme les commandes que vous voyez ici. Vous pouvez utiliser des commandes SQL pour créer des tables de base de données, compter le nombre d’enregistrements dans une table, calculer les prix et effectuer de nombreuses opérations supplémentaires.

### <a name="adding-markup-to-display-the-data"></a>Ajout de balises pour afficher les données

À l’intérieur de l’élément `<head>`, définissez le contenu de l’élément `<title>` sur « movies » :

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

À l’intérieur de l’élément `<body>` de la page, ajoutez ce qui suit :

[!code-html[Main](displaying-data/samples/sample3.html)]

C'est tout ! La variable `grid` est la valeur que vous avez créée lors de la création de l’objet `WebGrid` dans le code précédent.

Dans l’arborescence WebMatrix, cliquez avec le bouton droit sur la page et sélectionnez **lancer dans le navigateur**. Vous verrez une page semblable à celle-ci :

![Sortie par défaut de la grille d’assistance WebGrid du tableau films](displaying-data/_static/image18.png)

Cliquez sur un lien d’en-tête de colonne pour trier sur cette colonne. Le fait de pouvoir effectuer un tri en cliquant sur un en-tête est une fonctionnalité intégrée à l’application auxiliaire de la **grille** .

La méthode `GetHtml`, comme son nom le suggère, génère un balisage qui affiche les données. Par défaut, la méthode `GetHtml` génère un élément HTML `<table>`. (Si vous le souhaitez, vous pouvez vérifier le rendu en regardant la source de la page dans le navigateur.)

## <a name="modifying-the-look-of-the-grid"></a>Modification de l’apparence de la grille

L’utilisation du programme d’assistance `WebGrid` comme vous l’avez simplement fait est simple, mais l’affichage résultant est simple. Le `WebGrid` Helper possède toutes sortes d’options qui vous permettent de contrôler la façon dont les données sont affichées. Il y a beaucoup trop d’exploration dans ce didacticiel, mais cette section vous donnera une idée de certaines de ces options. Quelques options supplémentaires seront abordées dans les didacticiels ultérieurs de cette série.

### <a name="specifying-individual-columns-to-display"></a>Spécification de colonnes individuelles à afficher

Pour commencer, vous pouvez spécifier que vous souhaitez afficher uniquement certaines colonnes. Par défaut, comme vous l’avez vu, la grille affiche les quatre colonnes de la table *films* .

Dans le fichier *movies. cshtml* , remplacez le balisage `@grid.GetHtml()` que vous venez d’ajouter par ce qui suit :

[!code-css[Main](displaying-data/samples/sample4.css)]

Pour indiquer à l’application auxiliaire les colonnes à afficher, vous devez inclure un paramètre `columns` pour la méthode `GetHtml` et transmettre une collection de colonnes. Dans la collection, vous spécifiez chaque colonne à inclure. Vous spécifiez une colonne individuelle à afficher en incluant un objet `grid.Column` et vous transmettez le nom de la colonne de données de votre choix. (Ces colonnes doivent être incluses dans les résultats de la requête SQL ; le programme d’assistance `WebGrid` ne peut pas afficher les colonnes qui n’ont pas été retournées par la requête.)

Lancez à nouveau la page *movies. cshtml* dans le navigateur, et cette fois, vous recevez un affichage comme celui qui suit (Notez qu’aucune colonne ID n’est affichée) :

![Affichage WebGrid affichant uniquement les colonnes sélectionnées](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Modification de l’apparence de la grille

Il existe d’autres options pour afficher des colonnes, dont certaines seront explorées dans des didacticiels ultérieurs dans cet ensemble. Pour l’instant, cette section vous présente les différentes façons dont vous pouvez appliquer un style à l’ensemble de la grille.

À l’intérieur de la section `<head>` de la page, juste avant la balise de fermeture `</head>`, ajoutez l’élément `<style>` suivant :

[!code-css[Main](displaying-data/samples/sample5.css)]

Ce balisage CSS définit des classes nommées `grid`, `head`et ainsi de suite. Vous pouvez également placer ces définitions de style dans un fichier *. CSS* distinct et les lier à la page. (En fait, vous le ferez ultérieurement dans ce didacticiel.) Mais pour faciliter les choses dans ce didacticiel, celles-ci se trouvent à l’intérieur de la même page qui affiche les données.

Vous pouvez désormais obtenir le `WebGrid` Helper pour utiliser ces classes de style. L’application auxiliaire a un certain nombre de propriétés (par exemple, `tableStyle`) à cet effet : vous lui affectez un nom de classe de style CSS et ce nom de classe est restitué dans le cadre du balisage rendu par le programme d’assistance.

Modifiez le balisage `grid.GetHtml` pour qu’il ressemble maintenant à ce code :

[!code-css[Main](displaying-data/samples/sample6.css)]

Nouveautés ici : vous avez ajouté des paramètres `tableStyle`, `headerStyle`et `alternatingRowStyle` à la méthode `GetHtml`. Ces paramètres ont été définis sur les noms des styles CSS que vous avez ajoutés il y a un instant.

Exécutez la page. cette fois, vous voyez une grille qui semble bien moins simple qu’avant :

![Affichage WebGrid qui comprend les paramètres définis sur les noms de classe CSS](displaying-data/_static/image20.png)

Pour voir ce que la méthode `GetHtml` générée, vous pouvez consulter la source de la page dans le navigateur. Nous n’aborderons pas les détails ici, mais le point important est qu’en spécifiant des paramètres comme `tableStyle`, vous avez provoqué la génération par la grille de balises HTML comme suit :

`<table class="grid">`

Un attribut `class` a été ajouté à la balise `<table>`, qui fait référence à l’une des règles CSS que vous avez ajoutées précédemment. Ce code vous montre le modèle de base &mdash; différents paramètres pour la méthode `GetHtml` vous permettent de référencer des classes CSS que la méthode génère ensuite avec le balisage. C’est ce que vous faites avec les classes CSS.

## <a name="adding-paging"></a>Ajout de pagination

Au cours de la dernière tâche de ce didacticiel, vous allez ajouter la pagination à la grille. À ce stade, il n’y a aucun problème pour afficher tous vos films en même temps. Toutefois, si vous avez ajouté des centaines de films, l’affichage de la page est long.

Dans le code de la page, remplacez la ligne qui crée l’objet `WebGrid` par le code suivant :

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

La seule différence par rapport à est que vous avez ajouté un paramètre `rowsPerPage` défini sur 3.

Exécutez la page. La grille affiche 3 lignes à la fois, ainsi que des liens de navigation qui vous permettent de parcourir les films de votre base de données :

![Affichage de WebGrid avec pagination](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>À venir suivant

Dans le didacticiel suivant, vous apprendrez à utiliser Razor et C# code pour obtenir une entrée utilisateur dans un formulaire. Vous allez ajouter une zone de recherche à la page films afin de pouvoir Rechercher des films par titre ou par genre.

## <a name="complete-listing-for-movies-page"></a>Page complète de la liste des films

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Présentation de la programmation Web ASP.NET à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Précédent](intro-to-web-pages-programming.md)
> [Suivant](form-basics.md)
