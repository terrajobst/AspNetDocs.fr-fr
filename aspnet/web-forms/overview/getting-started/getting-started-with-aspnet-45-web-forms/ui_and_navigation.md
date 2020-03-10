---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Interface utilisateur et navigation | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: ac1dcaf1ba911fdcaeb3845c6836ec771733d93e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78643555"
---
# <a name="ui-and-navigation"></a>Interface utilisateur et navigation

par [Erik Reitan](https://github.com/Erikre)

[Télécharger l’exemple de projet WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Télécharger le livre électronique (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour le Web. Un [projet Visual Studio 2013 avec C# le code source](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.

Dans ce didacticiel, vous allez modifier l’interface utilisateur de l’application Web par défaut afin de prendre en charge les fonctionnalités de l’application Windows Store pour Wingtip Toys. En outre, vous allez ajouter des navigations simples et liées aux données. Ce didacticiel s’appuie sur le didacticiel précédent « créer la couche d’accès aux données » et fait partie de la série de didacticiels sur Wingtip Toys.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment modifier l’interface utilisateur pour prendre en charge les fonctionnalités de l’application Windows Store pour Wingtip Toys.
- Comment configurer un élément HTML5 pour inclure la navigation entre les pages.
- Comment créer un contrôle piloté par les données pour accéder à des données de produit spécifiques.
- Comment afficher les données d’une base de données créée à l’aide de Entity Framework Code First.

ASP.NET Web Forms vous permettent de créer du contenu dynamique pour votre application Web. Chaque page Web ASP.NET est créée d’une manière similaire à une page Web HTML statique (une page qui n’inclut pas le traitement basé sur le serveur), mais la page Web ASP.NET inclut des éléments supplémentaires que ASP.NET reconnaît et traite pour générer du code HTML lorsque la page s’exécute.

Avec une page HTML statique (fichier *. htm* ou *. html* ), le serveur effectue une demande de `Web` en lisant le fichier et en l’envoyant tel quel au navigateur. En revanche, lorsqu’un utilisateur demande une page Web ASP.NET (fichier *. aspx* ), la page s’exécute en tant que programme sur le serveur Web. Lorsque la page est en cours d’exécution, elle peut exécuter n’importe quelle tâche dont votre site Web a besoin, y compris le calcul des valeurs, la lecture ou l’écriture d’informations sur la base de données ou l’appel d’autres programmes. Comme sortie, la page produit dynamiquement le balisage (par exemple, les éléments en HTML) et envoie cette sortie dynamique au navigateur.

## <a name="modifying-the-ui"></a>Modification de l’interface utilisateur

Vous allez poursuivre cette série de didacticiels en modifiant la page *default. aspx* . Vous allez modifier l’interface utilisateur qui est déjà établie par le modèle par défaut utilisé pour créer l’application. Le type de modifications que vous effectuerez est normal lors de la création d’une application Web Forms. Pour ce faire, vous devez modifier le titre, remplacer du contenu et supprimer le contenu par défaut superflu.

1. Ouvrez ou basculez vers la page *default. aspx* .
2. Si la page s’affiche en mode **conception** , basculez en mode **source** .
3. En haut de la page de la `@Page` directive, remplacez l’attribut `Title` par « Welcome », comme indiqué en jaune ci-dessous. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Également sur la page *default. aspx* , remplacez tout le contenu par défaut contenu dans la balise `<asp:Content>` afin que le balisage apparaisse comme indiqué ci-dessous. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Enregistrez la page *default. aspx* en sélectionnant **Enregistrer default. aspx** dans le menu **fichier** .

   La page *default. aspx* qui en résulte s’affiche comme suit : 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

Dans l’exemple, vous avez défini l’attribut `Title` de la directive `@Page`. Lorsque le code HTML est affiché dans un navigateur, le `<%: Page.Title %>` de code du serveur correspond au contenu de l’attribut `Title`.

La page d’exemple comprend les éléments de base qui constituent une page Web ASP.NET. La page contient du texte statique comme vous pouvez le faire dans une page HTML, ainsi que des éléments spécifiques à ASP.NET. Le contenu de la page *default. aspx* sera intégré au contenu de la page maître, ce qui sera expliqué plus loin dans ce didacticiel.

### <a name="page-directive"></a>@Page directive

Les Web Forms ASP.NET contiennent généralement des directives qui vous permettent de spécifier les propriétés de page et les informations de configuration de la page. Les directives sont utilisées par ASP.NET comme instructions sur le traitement de la page, mais elles ne sont pas rendues dans le balisage envoyé au navigateur.

La directive la plus couramment utilisée est la directive `@Page`, qui vous permet de spécifier de nombreuses options de configuration pour la page, y compris les suivantes :

1. Langage de programmation serveur du code dans la page, tel que C#.
2. Si la page est une page avec du code serveur directement dans la page, appelée page à fichier unique, ou s’il s’agit d’une page contenant du code dans un fichier de classe distinct, appelé page code-behind.
3. Indique si la page est associée à une page maître et doit donc être traitée comme une page de contenu.
4. Options de débogage et de traçage.

Si vous n’incluez pas de directive `@Page` dans la page, ou si la directive n’inclut pas de paramètre spécifique, un paramètre est hérité du fichier de configuration *Web. config* ou du fichier de configuration *machine. config* . Le fichier *machine. config* fournit des paramètres de configuration supplémentaires pour toutes les applications qui s’exécutent sur un ordinateur.

> [!NOTE] 
> 
> Le *fichier machine. config* fournit également des détails sur tous les paramètres de configuration possibles.

### <a name="web-server-controls"></a>Contrôles serveur Web

Dans la plupart des ASP.NET Web Forms applications, vous allez ajouter des contrôles qui permettent à l’utilisateur d’interagir avec la page, comme des boutons, des zones de texte, des listes, etc. Ces contrôles serveur Web sont similaires aux boutons HTML et aux éléments d’entrée. Toutefois, ils sont traités sur le serveur, ce qui vous permet d’utiliser le code serveur pour définir leurs propriétés. Ces contrôles déclenchent également des événements que vous pouvez gérer dans le code serveur.

Les contrôles serveur utilisent une syntaxe spéciale que ASP.NET reconnaît lorsque la page s’exécute. Le nom de balise pour les contrôles serveur ASP.NET commence par un préfixe `asp:`. Cela permet à ASP.NET de reconnaître et de traiter ces contrôles serveur. Le préfixe peut être différent si le contrôle ne fait pas partie du .NET Framework. Outre le préfixe `asp:`, les contrôles serveur ASP.NET incluent également l’attribut `runat="server"` et un `ID` que vous pouvez utiliser pour référencer le contrôle dans le code serveur.

Lorsque la page s’exécute, ASP.NET identifie les contrôles serveur et exécute le code associé à ces contrôles. De nombreux contrôles affichent du code HTML ou autre balisage dans la page lorsqu’il est affiché dans un navigateur.

### <a name="server-code"></a>Code du serveur

La plupart des applications ASP.NET Web Forms incluent du code qui s’exécute sur le serveur lors du traitement de la page. Comme indiqué ci-dessus, le code serveur peut être utilisé pour effectuer diverses opérations, telles que l’ajout de données à un contrôle ListView. ASP.NET prend en charge de nombreux langages à exécuter sur C#le serveur, notamment, Visual Basic, J# et d’autres.

ASP.NET prend en charge deux modèles pour l’écriture de code serveur pour une page Web. Dans le modèle à fichier unique, le code de la page se trouve dans un élément script dans lequel la balise d’ouverture comprend l’attribut `runat="server"`. Vous pouvez également créer le code de la page dans un fichier de classe distinct, appelé modèle code-behind. Dans ce cas, la page de Web Forms ASP.NET ne contient généralement aucun code serveur. Au lieu de cela, la directive `@Page` contient des informations qui relient la page *. aspx* avec son fichier code-behind associé.

L’attribut `CodeBehind` contenu dans la directive `@Page` spécifie le nom du fichier de classe distinct, et l’attribut `Inherits` spécifie le nom de la classe dans le fichier code-behind qui correspond à la page.

### <a name="updating-the-master-page"></a>Mise à jour de la page maître

Dans ASP.NET Web Forms, les pages maîtres vous permettent de créer une mise en page homogène pour les pages de votre application. Une seule page maître définit la présentation et le comportement standard voulus pour toutes les pages (ou un groupe de pages) dans votre application. Vous pouvez ensuite créer des pages de contenu individuelles qui contiennent le contenu que vous souhaitez afficher, comme expliqué ci-dessus. Lorsque les utilisateurs demandent les pages de contenu, ASP.NET les fusionne avec la page maître pour produire une sortie qui associe la disposition de la page maître et le contenu de la page de contenu.

Le nouveau site a besoin d’un seul logo à afficher sur chaque page. Pour ajouter ce logo, vous pouvez modifier le code HTML sur la page maître.

1. Dans l' **Explorateur de solutions**, recherchez et ouvrez la page **Site.Master** .
2. Si la page est en mode **conception** , basculez en mode **source** .
3. Mettez à jour la page maître en **modifiant ou en ajoutant** le balisage mis en surbrillance en jaune : 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Ce code HTML affiche l’image nommée *logo. jpg* à partir du dossier *images* de l’application Web, que vous ajouterez ultérieurement. Quand une page qui utilise la page maître s’affiche dans un navigateur, le logo s’affiche. Si un utilisateur clique sur le logo, l’utilisateur accède à la page *default. aspx* . La balise d’ancrage HTML `<a>` encapsule le contrôle serveur d’images et autorise l’inclusion de l’image dans le cadre du lien. L’attribut `href` pour la balise d’ancrage spécifie le «`~/`» racine du site Web en tant qu’emplacement du lien. Par défaut, la page *default. aspx* s’affiche lorsque l’utilisateur accède à la racine du site Web. L' **Image** `<asp:Image>` contrôle serveur comprend des propriétés d’addition, telles que des `BorderStyle`, qui s’affichent au format html lorsqu’elles sont affichées dans un navigateur.

### <a name="master-pages"></a>Pages maîtres

Une page maître est un fichier ASP.NET avec l’extension. Master (par exemple, *site. Master*) avec une disposition prédéfinie qui peut inclure du texte statique, des éléments HTML et des contrôles serveur. La page maître est identifiée par une directive de `@Master` spéciale qui remplace la directive `@Page` utilisée pour les pages *. aspx* ordinaires.

En plus de la directive `@Master`, la page maître contient également tous les éléments HTML de niveau supérieur d’une page, comme `html`, `head`et `form`. Par exemple, sur la page maître que vous avez ajoutée ci-dessus, vous utilisez un `table` HTML pour la disposition, un élément `img` pour le logo de l’entreprise, du texte statique et des contrôles serveur pour gérer l’appartenance commune à votre site. Vous pouvez utiliser n’importe quel élément HTML et tout élément ASP.NET dans le cadre de votre page maître.

En plus du texte statique et des contrôles qui s’affichent sur toutes les pages, la page maître comprend également un ou plusieurs contrôles **ContentPlaceHolder** . Ces contrôles d’espace réservé définissent les régions dans lesquelles le contenu remplaçable s’affichera. Le contenu remplaçable est ensuite défini dans les pages de contenu, par exemple *default. aspx*, à l’aide du contrôle **content** Server.

#### <a name="adding-image-files"></a>Ajout de fichiers image

L’image de logo référencée ci-dessus, ainsi que toutes les images de produit, doivent être ajoutées à l’application Web afin qu’elles puissent être affichées lorsque le projet est affiché dans un navigateur.

#### <a name="download-from-msdn-samples-site"></a>Télécharger à partir du site d’exemples MSDN :

[Prise en main avec ASP.NET 4,5 Web Forms et Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

Le téléchargement comprend des ressources dans le dossier *WingtipToys-biens* utilisées pour créer l’exemple d’application.

1. Si vous ne l’avez pas déjà fait, téléchargez les exemples de fichiers compressés à l’aide du lien ci-dessus à partir du site d’exemples MSDN.
2. Une fois téléchargé, ouvrez le fichier. zip et copiez le contenu dans un dossier local sur votre ordinateur.
3. Recherchez et ouvrez le dossier *WingtipToys-biens* .
4. Par glisser-déplacer, copiez le dossier de *catalogue* à partir de votre dossier local vers la racine du projet d’application Web dans le **Explorateur de solutions** de Visual Studio. 

    ![Interface utilisateur et navigation-copier des fichiers](ui_and_navigation/_static/image1.png)
5. Ensuite, créez un dossier nommé *images* en cliquant avec le bouton droit sur le projet **WingtipToys** dans **Explorateur de solutions** et en sélectionnant **Ajouter** -&gt; **nouveau dossier**.
6. Copiez le fichier *logo. jpg* du dossier *WingtipToys-biens* de l' **Explorateur de fichiers** dans le dossier *Images* du projet d’application Web dans **Explorateur de solutions** de Visual Studio.
7. Cliquez sur l’option **Afficher tous les fichiers** en haut de **Explorateur de solutions** pour mettre à jour la liste des fichiers si vous ne voyez pas les nouveaux fichiers.  
  
    **Explorateur de solutions** affiche maintenant les fichiers de projet mis à jour. 

    ![Interface utilisateur et navigation-Explorateur de solutions](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Ajout de pages

Avant d’ajouter la navigation à l’application Web, vous devez d’abord ajouter deux nouvelles pages vers lesquelles vous allez accéder. Plus loin dans cette série de didacticiels, vous afficherez les produits et les détails du produit sur ces nouvelles pages.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **WingtipToys**, cliquez sur **Ajouter**, puis sur **nouvel élément**.   
 La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sélectionnez le **groupe C# Visual** -&gt; des modèles **Web** sur la gauche. Ensuite, sélectionnez **Web Form avec page maître** dans la liste du milieu, puis nommez-le *ProductList. aspx*. 

    ![Interface utilisateur et navigation-boîte de dialogue Ajouter un nouvel élément](ui_and_navigation/_static/image3.png)
3. Sélectionnez **site. Master** pour attacher la page maître à la page *. aspx* nouvellement créée. 

    ![Interface utilisateur et navigation-sélectionner la page maître](ui_and_navigation/_static/image4.png)
4. Ajoutez une page supplémentaire nommée *ProductDetails. aspx* en suivant la même procédure.

### <a name="updating-bootstrap"></a>Mise à jour de bootstrap

Les modèles de projet Visual Studio 2013 utilisent [bootstrap](http://getbootstrap.com/), une disposition et une infrastructure de thèmes créés par Twitter. Bootstrap utilise CSS3 pour fournir une conception réactive, ce qui signifie que les dispositions peuvent s’adapter de manière dynamique à différentes tailles de fenêtre de navigateur. Vous pouvez également utiliser la fonctionnalité de thèmes d’amorçage pour effectuer facilement une modification de l’apparence de l’application. Par défaut, le modèle d’application Web ASP.NET dans Visual Studio 2013 comprend le démarrage en tant que package NuGet.

Dans ce didacticiel, vous allez modifier l’apparence de l’application Wingtip Toys en remplaçant les fichiers de démarrage CSS.

1. Dans **Explorateur de solutions**, ouvrez le dossier *content* .
2. Cliquez avec le bouton droit sur le fichier *bootstrap. CSS* et renommez-le *bootstrap-original. CSS*.
3. Renommez le *bootstrap. min. CSS* en *bootstrap-original. min. CSS*.
4. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *content* et sélectionnez **ouvrir le dossier dans l’Explorateur de fichiers**.  
   L’Explorateur de fichiers s’affiche. Vous allez enregistrer un fichier CSS de démarrage téléchargé à cet emplacement.
5. Dans votre navigateur, accédez à [https://bootswatch.com/3/](https://bootswatch.com/3/).
6. Faites défiler la fenêtre du navigateur jusqu’à ce que le thème Cerulean s’affiche. 

    ![Interface utilisateur et navigation-thème Cerulean](ui_and_navigation/_static/image5.png)
7. Téléchargez le fichier *bootstrap. CSS* et le fichier *bootstrap. min. CSS* dans le dossier *content* . Utilisez le chemin d’accès au dossier de contenu qui s’affiche dans la fenêtre de l' **Explorateur de fichiers** que vous avez ouverte précédemment.
8. Dans **Visual Studio** en haut de **Explorateur de solutions**, sélectionnez l’option **Afficher tous les fichiers** pour afficher les nouveaux fichiers dans le dossier Content. 

    ![Interface utilisateur et navigation-Explorateur de solutions](ui_and_navigation/_static/image6.png)

   Vous verrez les deux nouveaux fichiers CSS dans le dossier **content** , mais notez que l’icône en regard de chaque nom de fichier est grisée. Cela signifie que le fichier n’a pas encore été ajouté au projet.
9. Cliquez avec le bouton droit sur les fichiers *bootstrap. CSS* et *bootstrap. min. CSS* et sélectionnez **inclure dans le projet**.   
   Quand vous exécutez l’application Wingtip Toys plus loin dans ce didacticiel, la nouvelle interface utilisateur s’affiche.

> [!NOTE] 
> 
> Le modèle d’application Web ASP.NET utilise le fichier *bundle. config* à la racine du projet pour stocker le chemin d’accès aux fichiers CSS de démarrage.

### <a name="modifying-the-default-navigation"></a>Modification de la navigation par défaut

La navigation par défaut pour chaque page de l’application peut être modifiée en modifiant l’élément de la liste de navigation non triée qui se trouve dans la page *site. Master* .

1. Dans **Explorateur de solutions**, recherchez et ouvrez la page *site. Master* .
2. Ajoutez le lien de navigation supplémentaire mis en surbrillance en jaune à la liste non triée affichée ci-dessous :   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Comme vous pouvez le voir dans le code HTML ci-dessus, vous avez modifié chaque élément de ligne `<li>` contenant une balise d’ancrage `<a>` avec un attribut `href` de lien. Chaque `href` pointe vers une page de l’application Web. Dans le navigateur, lorsqu’un utilisateur clique sur l’un de ces liens (tels que **Products**), il accède à la page contenue dans le `href` (par exemple, **ProductList. aspx**). Vous allez exécuter l’application à la fin de ce didacticiel.

> [!NOTE] 
> 
> Le caractère tilde (`~`) est utilisé pour spécifier que le chemin d’accès `href` commence à la racine du projet.

### <a name="adding-a-data-control-to-display-navigation-data"></a>Ajout d’un contrôle de données pour afficher les données de navigation

Ensuite, vous allez ajouter un contrôle pour afficher toutes les catégories de la base de données. Chaque catégorie fera office de lien vers la page *ProductList. aspx* . Lorsqu’un utilisateur clique sur un lien de catégorie dans le navigateur, il accède à la page produits et affiche uniquement les produits associés à la catégorie sélectionnée.

Vous utiliserez un contrôle **ListView** pour afficher toutes les catégories contenues dans la base de données. Pour ajouter un contrôle **ListView** à la page maître :

1. Dans la page *site. Master* , ajoutez l’élément `<div>` en surbrillance suivant **après** l’élément `<div>` contenant le `id="TitleContent"` que vous avez ajouté précédemment :  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Ce code affiche toutes les catégories de la base de données. Le contrôle **ListView** affiche chaque nom de catégorie sous forme de texte de lien et comprend un lien vers la page *ProductList. aspx* avec une valeur de chaîne de requête contenant le `ID` de la catégorie. En définissant la propriété `ItemType` dans le contrôle **ListView** , l’expression de liaison de données `Item` est disponible dans le nœud `ItemTemplate` et le contrôle est fortement typé. Vous pouvez sélectionner les détails de l’objet `Item` à l’aide d’IntelliSense, tels que la spécification du `CategoryName`. Ce code est contenu à l’intérieur du conteneur `<%#: %>` qui marque une expression de liaison de données. En ajoutant le ( :) à la fin du préfixe `<%#`, le résultat de l’expression de liaison de données est encodé au format HTML. Lorsque le résultat est encodé au format HTML, votre application est mieux protégée contre les attaques par injection de script entre sites (XSS) et par injection de code HTML.

> [!NOTE] 
> 
> **Conseil**
> 
> Quand vous ajoutez du code en tapant lors du développement, vous pouvez être certain qu’un membre valide d’un objet est trouvé, car les contrôles de données fortement typés affichent les membres disponibles en fonction d’IntelliSense. IntelliSense offre des choix de code contextuels à mesure que vous tapez du code, comme des propriétés, des méthodes et des objets.

À l’étape suivante, vous allez implémenter la méthode `GetCategories` pour récupérer des données.

### <a name="linking-the-data-control-to-the-database"></a>Liaison du contrôle de données à la base de données

Avant de pouvoir afficher des données dans le contrôle Data, vous devez lier le contrôle Data à la base de données. Pour créer le lien, vous pouvez modifier le code-behind du fichier *site.Master.cs* .

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la page *site. Master* , puis cliquez sur **afficher le code**. Le fichier *site.Master.cs* s’ouvre dans l’éditeur.
2. Vers le début du fichier *site.Master.cs* , ajoutez deux espaces de noms supplémentaires afin que tous les espaces de noms inclus apparaissent comme suit :  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Ajoutez la méthode `GetCategories` en surbrillance après le gestionnaire d’événements `Page_Load` comme suit :  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Le code ci-dessus est exécuté lorsqu’une page qui utilise la page maître est chargée dans le navigateur. Le contrôle `ListView` (nommé « categoryList ») que vous avez ajouté précédemment dans ce didacticiel utilise la liaison de modèle pour sélectionner des données. Dans le balisage du contrôle `ListView`, vous avez défini la propriété `SelectMethod` du contrôle sur la méthode `GetCategories`, comme indiqué ci-dessus. Le contrôle `ListView` appelle la méthode `GetCategories` au moment approprié dans le cycle de vie de la page et lie automatiquement les données retournées. Vous en apprendrez davantage sur la liaison de données dans le didacticiel suivant.

### <a name="running-the-application-and-creating-the-database"></a>Exécution de l’application et création de la base de données

Plus haut dans cette série de didacticiels, vous avez créé une classe d’initialiseur (nommée « ProductDatabaseInitializer ») et spécifié cette classe dans le fichier *global.asax.cs* . Le Entity Framework générera la base de données lorsque l’application sera exécutée la première fois, car la méthode `Application_Start` contenue dans le fichier *global.asax.cs* appellera la classe d’initialiseur. La classe d’initialiseur utilisera les classes de modèle (`Category` et `Product`) que vous avez ajoutées précédemment dans cette série de didacticiels pour créer la base de données.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la page *default. aspx* et sélectionnez **définir comme page de démarrage**.
2. Dans Visual Studio, appuyez sur **F5**.   
 Il faut un peu de temps pour configurer tout ce qui se passe lors de la première exécution.   
    ![l’interface utilisateur et la navigation-fenêtres du navigateur](ui_and_navigation/_static/image7.png)  
 Lorsque vous exécutez l’application, l’application est compilée et la base de données nommée *wingtiptoys. mdf* est créée dans le dossier de données de l' *application\_* . Dans le navigateur, vous verrez un menu de navigation de catégorie. Ce menu a été généré en extrayant les catégories de la base de données. Dans le didacticiel suivant, vous allez implémenter la navigation.
3. Fermez le navigateur pour arrêter l’application en cours d’exécution.

### <a name="reviewing-the-database"></a>Vérification de la base de données

Ouvrez le fichier *Web. config* et examinez la section de la chaîne de connexion. Vous pouvez voir que la valeur `AttachDbFilename` dans la chaîne de connexion pointe vers la `DataDirectory` pour le projet d’application Web. La valeur `|DataDirectory|` est une valeur réservée qui représente le dossier de données de l' *application\_* dans le projet. Ce dossier est l’emplacement où se trouve la base de données qui a été créée à partir de vos classes d’entité.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Si le dossier de données de l' *application\_* n’est pas visible ou si le dossier est vide, sélectionnez l’icône d' **actualisation** , puis l’icône **Afficher tous les fichiers** en haut de la fenêtre de **Explorateur de solutions** . L’agrandissement de la largeur des fenêtres de **Explorateur de solutions** peut être nécessaire pour afficher toutes les icônes disponibles.

Vous pouvez maintenant inspecter les données contenues dans le fichier de base de données *wingtiptoys. mdf* à l’aide de la fenêtre **Explorateur de serveurs** .

1. Développez le dossier données de l' *application\_* . Si le dossier de données de l' *application\_* n’est pas visible, consultez la remarque ci-dessus.
2. Si le fichier de base de données *wingtiptoys. mdf* n’est pas visible, sélectionnez l’icône d' **actualisation** , puis l’icône **Afficher tous les fichiers** en haut de la fenêtre de **Explorateur de solutions** .
3. Cliquez avec le bouton droit sur le fichier de base de données *wingtiptoys. mdf* , puis sélectionnez **ouvrir**.  
    **Explorateur de serveurs** s’affiche. 

    ![Interface utilisateur et navigation-Explorateur de serveurs](ui_and_navigation/_static/image8.png)
4. Développez le dossier *Tables* .
5. Cliquez avec le bouton droit sur la table **Products**et sélectionnez **afficher les données**de la table.  
 La table **Products** s’affiche. 

    ![Interface utilisateur et navigation-table des produits](ui_and_navigation/_static/image9.png)
6. Cette vue vous permet de visualiser et de modifier les données de la table **Products** à la main.
7. Fermez la fenêtre de la table **Products** .
8. Dans le **Explorateur de serveurs**, cliquez à nouveau avec le bouton droit sur la table **Products** et sélectionnez **ouvrir la définition de table**.  
 La conception des données pour la table **Products** s’affiche. 

    ![Interface utilisateur et navigation-conception des produits](ui_and_navigation/_static/image10.png)
9. Dans l’onglet **T-SQL** , vous verrez l’instruction DDL SQL qui a été utilisée pour créer la table. Vous pouvez également utiliser l’interface utilisateur sous l’onglet **conception** pour modifier le schéma.
10. Dans le **Explorateur de serveurs**, cliquez avec le bouton droit sur base de données **WingtipToys** , puis sélectionnez **Fermer la connexion**.   
 Si vous détachez la base de données de Visual Studio, le schéma de base de données pourra être modifié plus tard dans cette série de didacticiels.
11. Revenez à **Explorateur de solutions**en sélectionnant l’onglet **Explorateur de solutions** au bas de la fenêtre de **Explorateur de serveurs** .

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel de la série, vous avez ajouté une interface utilisateur de base, des graphiques, des pages et une navigation. En outre, vous avez exécuté l’application Web, qui a créé la base de données à partir des classes de données que vous avez ajoutées dans le didacticiel précédent. Vous pouvez également afficher le contenu de la table *Products* de la base de données en consultant directement la base de données. Dans le didacticiel suivant, vous allez afficher les éléments de données et les détails de la base de données.

## <a name="additional-resources"></a>Ressources supplémentaires

[Introduction à la programmation de pages Web ASP.NET](https://msdn.microsoft.com/library/ms178125.aspx)   
[Vue d’ensemble des contrôles serveur Web ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Didacticiel CSS](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Précédent](create_the_data_access_layer.md)
> [Suivant](display_data_items_and_details.md)
