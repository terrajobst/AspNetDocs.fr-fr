---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: Interaction avec la page maître à partir de la page de contenu (VB) | Microsoft Docs
author: rick-anderson
description: Examine comment appeler des méthodes, définir des propriétés, etc. de la page maître à partir du code dans la page de contenu.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: fe1f7b80b26ff25c1ce744e4f823e3fb35eea074
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74577277"
---
# <a name="interacting-with-the-master-page-from-the-content-page-vb"></a>Interaction avec la page de contenu à partir de la page maître (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> Examine comment appeler des méthodes, définir des propriétés, etc. de la page maître à partir du code dans la page de contenu.

## <a name="introduction"></a>Introduction

Au cours des cinq derniers didacticiels, nous avons vu comment créer une page maître, définir des zones de contenu, lier des pages ASP.NET à une page maître et définir du contenu spécifique à la page. Lorsqu’un visiteur demande une page de contenu particulière, le balisage du contenu et des pages maîtres sont fusionnés au moment de l’exécution, ce qui entraîne le rendu d’une hiérarchie de contrôle unifiée. Par conséquent, nous avons déjà vu une manière dans laquelle la page maître et l’une de ses pages de contenu peuvent interagir : la page de contenu épele le balisage pour effectuer un transfusion dans les contrôles ContentPlaceHolder de la page maître.

Nous n’avons pas encore examiné comment la page maître et la page de contenu peuvent interagir par programmation. En plus de définir le balisage pour les contrôles ContentPlaceHolder de la page maître, une page de contenu peut également assigner des valeurs aux propriétés publiques de sa page maître et appeler ses méthodes publiques. De même, une page maître peut interagir avec ses pages de contenu. Bien que l’interaction par programmation entre une page maître et une page de contenu soit moins courante que l’interaction entre leurs balises déclaratives, il existe de nombreux scénarios dans lesquels une telle interaction est nécessaire.

Dans ce didacticiel, nous examinons comment une page de contenu peut interagir par programmation avec sa page maître. dans le didacticiel suivant, nous allons examiner comment la page maître peut interagir de manière similaire avec ses pages de contenu.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Exemples d’interactions par programmation entre une page de contenu et sa page maître

Quand une région particulière d’une page doit être configurée page par page, nous utilisons un contrôle ContentPlaceHolder. Mais qu’en est-il des situations où la majorité des pages doit émettre une sortie donnée, mais un petit nombre de pages doit le personnaliser pour afficher autre chose ? Un exemple, que nous avons examiné dans le didacticiel sur les [*ContentPlaceHolders et le contenu par défaut*](multiple-contentplaceholders-and-default-content-vb.md) , implique l’affichage d’une interface de connexion sur chaque page. Bien que la plupart des pages doivent inclure une interface de connexion, elles doivent être supprimées pour quelques pages, par exemple : la page de connexion principale (`Login.aspx`); la page créer un compte ; et d’autres pages qui sont uniquement accessibles aux utilisateurs authentifiés. Le didacticiel sur les [*multiples ContentPlaceHolders et le contenu par défaut*](multiple-contentplaceholders-and-default-content-vb.md) vous a montré comment définir le contenu par défaut d’un ContentPlaceHolder dans la page maître, puis comment le remplacer dans les pages où le contenu par défaut n’était pas souhaité.

Une autre option consiste à créer une propriété ou une méthode publique dans la page maître qui indique s’il faut afficher ou masquer l’interface de connexion. Par exemple, la page maître peut inclure une propriété publique nommée `ShowLoginUI` dont la valeur a été utilisée pour définir la propriété `Visible` du contrôle de connexion dans la page maître. Les pages de contenu où l’interface utilisateur de connexion doit être supprimée peuvent alors définir par programmation la propriété `ShowLoginUI` sur `False`.

L’exemple le plus courant d’interaction entre le contenu et la page maître se produit lorsque les données affichées dans la page maître doivent être actualisées après qu’une action s’est arrêtée dans la page de contenu. Prenons l’exemple d’une page maître qui contient un GridView qui affiche les cinq enregistrements ajoutés le plus récemment à partir d’une table de base de données particulière, et que l’une de ses pages de contenu comprend une interface permettant d’ajouter de nouveaux enregistrements à cette même table.

Lorsqu’un utilisateur visite la page pour ajouter un nouvel enregistrement, il voit les cinq enregistrements ajoutés le plus récemment affichés dans la page maître. Après avoir renseigné les valeurs des colonnes du nouvel enregistrement, elle envoie le formulaire. En supposant que le contrôle GridView dans la page maître a sa propriété `EnableViewState` définie sur true (valeur par défaut), son contenu est rechargé à partir de l’état d’affichage et, par conséquent, les cinq mêmes enregistrements sont affichés même si un enregistrement plus récent vient d’être ajouté à la base de données. Cela peut dérouter l’utilisateur.

> [!NOTE]
> Même si vous désactivez l’état d’affichage de GridView afin qu’il se reconnecte à sa source de données sous-jacente à chaque publication (postback), il n’affiche toujours pas l’enregistrement juste ajouté parce que les données sont liées au GridView plus tôt dans le cycle de vie de la page que lorsque le nouvel enregistrement est ajouté au datab équipement.

Pour remédier à cela afin que l’enregistrement juste ajouté s’affiche dans le GridView de la page maître lors de la publication (postback), nous devons demander à la classe GridView d’effectuer une nouvelle liaison à sa source de données *une fois que* le nouvel enregistrement a été ajouté à la base de données. Cela nécessite une interaction entre le contenu et les pages maîtres, car l’interface permettant d’ajouter le nouvel enregistrement (et ses gestionnaires d’événements) se trouve dans la page de contenu, mais le GridView qui doit être actualisé est dans la page maître.

Étant donné que l’actualisation de l’affichage de la page maître à partir d’un gestionnaire d’événements dans la page de contenu est l’un des besoins les plus courants en matière d’interaction du contenu et de la page maître, explorez cette rubrique plus en détail. Le téléchargement de ce didacticiel comprend une base de données Microsoft SQL Server 2005 Express Edition nommée `NORTHWIND.MDF` dans le dossier `App_Data` du site Web. La base de données Northwind stocke les informations sur les produits, les employés et les ventes d’une société fictive, Northwind Traders.

L’étape 1 vous guide dans l’affichage des cinq produits récemment ajoutés dans un GridView dans la page maître. L’étape 2 crée une page de contenu pour l’ajout de nouveaux produits. L’étape 3 explique comment créer des propriétés et des méthodes publiques dans la page maître, et l’étape 4 montre comment interagir par programmation avec ces propriétés et méthodes à partir de la page de contenu.

> [!NOTE]
> Ce didacticiel n’aborde pas les spécificités de l’utilisation des données dans ASP.NET. Les étapes de configuration de la page maître pour afficher des données et la page de contenu pour l’insertion de données sont terminées, mais Breezy. Pour plus d’informations sur l’affichage et l’insertion de données et sur l’utilisation des contrôles SqlDataSource et GridView, consultez les ressources dans la section lectures supplémentaires à la fin de ce didacticiel.

## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Étape 1 : affichage des cinq derniers produits ajoutés dans la page maître

Ouvrez la page maître site. Master et ajoutez une étiquette et un contrôle GridView au `leftContent` `<div>`. Effacez la propriété `Text` de l’étiquette, affectez à sa propriété `EnableViewState` la valeur `False`et à sa propriété `ID` la valeur `GridMessage`; Définissez la propriété `ID` de GridView sur `RecentProducts`. Ensuite, à partir du concepteur, développez la balise active de GridView et choisissez de la lier à une nouvelle source de données. Cela lance l’Assistant Configuration de source de données. Étant donné que la base de données Northwind dans le dossier `App_Data` est une base de données Microsoft SQL Server, choisissez de créer un SqlDataSource en sélectionnant (voir figure 1). Nommez le `RecentProductsDataSource`SqlDataSource.

[![lier le contrôle GridView à un contrôle SqlDataSource nommé RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**Figure 01**: lier le contrôle GridView à un contrôle SqlDataSource nommé `RecentProductsDataSource` ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))

L’étape suivante nous invite à spécifier la base de données à laquelle se connecter. Choisissez le `NORTHWIND.MDF` fichier de base de données dans la liste déroulante, puis cliquez sur suivant. Comme c’est la première fois que vous utilisez cette base de données, l’Assistant propose de stocker la chaîne de connexion dans `Web.config`. Stockez la chaîne de connexion en utilisant le nom `NorthwindConnectionString`.

[![se connecter à la base de données Northwind](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**Figure 02**: se connecter à la base de données Northwind ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))

L’Assistant Configuration de la source de données offre deux moyens permettant de spécifier la requête utilisée pour récupérer les données :

- En spécifiant une instruction SQL personnalisée ou une procédure stockée, ou
- En sélectionnant une table ou une vue et en spécifiant les colonnes à retourner

Étant donné que nous souhaitons retourner uniquement les cinq derniers produits ajoutés, nous devons spécifier une instruction SQL personnalisée. Utilisez la requête `SELECT` suivante :

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

Le mot clé `TOP 5` retourne uniquement les cinq premiers enregistrements de la requête. La clé primaire de la table `Products`, `ProductID`, est un `IDENTITY` colonne, qui nous garantit que chaque nouveau produit ajouté à la table aura une valeur supérieure à celle de l’entrée précédente. Par conséquent, le tri des résultats par `ProductID` dans l’ordre décroissant retourne les produits commençant par les derniers créés.

[![retourner les cinq derniers produits ajoutés](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**Figure 03**: retourner les cinq derniers produits ajoutés ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))

Une fois l’Assistant terminé, Visual Studio génère deux BoundFields pour le GridView afin d’afficher les champs `ProductName` et `UnitPrice` retournés par la base de données. À ce stade, le balisage déclaratif de votre page maître doit inclure un balisage similaire à ce qui suit :

[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

Comme vous pouvez le voir, le balisage contient : le contrôle Web étiquette (`GridMessage`); `RecentProducts`GridView, avec deux BoundFields ; et un contrôle SqlDataSource qui retourne les cinq produits récemment ajoutés.

Une fois ce GridView créé et son contrôle SqlDataSource configuré, visitez le site Web via un navigateur. Comme le montre la figure 4, vous verrez une grille dans le coin inférieur gauche qui répertorie les cinq produits récemment ajoutés.

[![le contrôle GridView affiche les cinq produits récemment ajoutés](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**Figure 04**: le contrôle GridView affiche les cinq derniers produits ajoutés ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))

> [!NOTE]
> N’hésitez pas à nettoyer l’apparence du contrôle GridView. Certaines suggestions incluent la mise en forme de la valeur `UnitPrice` affichée en tant que devise et l’utilisation des couleurs et des polices d’arrière-plan pour améliorer l’apparence de la grille.

## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Étape 2 : création d’une page de contenu pour ajouter de nouveaux produits

La tâche suivante consiste à créer une page de contenu à partir de laquelle un utilisateur peut ajouter un nouveau produit au tableau `Products`. Ajoutez une nouvelle page de contenu au dossier `Admin` nommé `AddProduct.aspx`, en veillant à le lier à la page maître `Site.master`. La figure 5 montre les Explorateur de solutions une fois que cette page a été ajoutée au site Web.

[![ajouter une nouvelle page ASP.NET au dossier Admin](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**Figure 05**: ajouter une nouvelle page ASP.net au dossier `Admin` ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))

Rappelez-vous que dans le didacticiel [*spécification du titre, des balises meta et d’autres en-têtes HTML dans le didacticiel de la page maître,* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) nous avons créé une classe de page de base personnalisée nommée `BasePage` qui a généré le titre de la page si elle n’a pas été définie explicitement. Accédez à la classe code-behind de la page de `AddProduct.aspx` et dérivez-la de `BasePage` (au lieu de `System.Web.UI.Page`).

Enfin, mettez à jour le fichier `Web.sitemap` pour inclure une entrée pour cette leçon. Ajoutez le balisage suivant sous le `<siteMapNode>` pour la leçon relative aux problèmes d’attribution de noms aux ID de contrôle :

[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

Comme illustré à la figure 6, l’ajout de cet élément `<siteMapNode>` est reflété dans la liste leçons.

Revenez à `AddProduct.aspx`. Dans le contrôle de contenu pour le `MainContent` ContentPlaceHolder, ajoutez un contrôle DetailsView et nommez-le `NewProduct`. Liez le DetailsView à un nouveau contrôle SqlDataSource nommé `NewProductDataSource`. Comme avec le SqlDataSource à l’étape 1, configurez l’Assistant pour qu’il utilise la base de données Northwind et choisissez de spécifier une instruction SQL personnalisée. Étant donné que le contrôle DetailsView est utilisé pour ajouter des éléments à la base de données, nous devons spécifier une instruction `SELECT` et une instruction `INSERT`. Utilisez la requête `SELECT` suivante :

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

Ensuite, dans l’onglet Insertion, ajoutez l’instruction `INSERT` suivante :

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

Après avoir terminé l’Assistant, accédez à la balise active du contrôle DetailsView et cochez la case « Activer l’insertion ». Cela ajoute une CommandField à DetailsView avec la propriété `ShowInsertButton` définie sur true. Étant donné que ce DetailsView sera utilisé uniquement pour l’insertion de données, affectez à la propriété `DefaultMode` du contrôle DetailsView la valeur `Insert`.

C’est aussi simple que cela ! Nous allons tester cette page. Visitez `AddProduct.aspx` via un navigateur, entrez un nom et un prix (voir figure 6).

[![ajouter un nouveau produit à la base de données](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**Figure 06**: ajouter un nouveau produit à la base de données ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))

Après avoir tapé le nom et le prix de votre nouveau produit, cliquez sur le bouton Insérer. Cela entraîne la publication du formulaire. Lors de la publication (postback), l’instruction `INSERT` du contrôle SqlDataSource est exécutée ; ses deux paramètres sont remplis avec les valeurs entrées par l’utilisateur dans les deux contrôles TextBox du contrôle DetailsView. Malheureusement, il n’existe aucun commentaire visuel indiquant qu’une insertion s’est produite. Il serait intéressant de faire en sorte qu’un message s’affiche, confirmant l’ajout d’un nouvel enregistrement. Je laisse cela en tant qu’exercice pour le lecteur. En outre, après avoir ajouté un nouvel enregistrement à partir du contrôle DetailsView, le contrôle GridView dans la page maître affiche toujours les cinq mêmes enregistrements qu’auparavant. elle n’inclut pas l’enregistrement qui vient d’être ajouté. Nous allons examiner comment y remédier dans les prochaines étapes.

> [!NOTE]
> En plus d’ajouter une forme de retour visuel que l’insertion a réussi, je vous conseille de mettre également à jour l’interface d’insertion du contrôle DetailsView pour inclure la validation. Actuellement, il n’y a aucune validation. Si un utilisateur entre une valeur non valide pour le champ `UnitPrice`, telle que « trop cher », une exception est levée lors de la publication (postback) lorsque le système tente de convertir cette chaîne en un nombre décimal. Pour plus d’informations sur la personnalisation de l’interface d’insertion, reportez-vous au [didacticiel *Personnalisation de l’interface de modification des données* ](https://asp.net/learn/data-access/tutorial-20-vb.aspx) de la [série didacticiel](../../data-access/index.md)sur les données.

## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Étape 3 : création de propriétés et de méthodes publiques dans la page maître

À l’étape 1, nous avons ajouté un contrôle Label Web nommé `GridMessage` au-dessus du contrôle GridView dans la page maître. Cette étiquette est destinée à afficher éventuellement un message. Par exemple, après avoir ajouté un nouvel enregistrement à la table `Products`, nous pourrions souhaiter afficher un message indiquant que «*ProductName* a été ajouté à la base de données ». Au lieu de coder en dur le texte de cette étiquette dans la page maître, il est possible que le message soit personnalisable par la page de contenu.

Étant donné que le contrôle Label est implémenté en tant que variable membre protégée dans la page maître, il n’est pas accessible directement à partir des pages de contenu. Pour pouvoir utiliser l’étiquette dans une page maître à partir de la page de contenu (ou, pour ce faire, de n’importe quel contrôle Web dans la page maître), nous devons créer une propriété publique dans la page maître qui expose le contrôle Web ou sert de proxy par lequel l’une de ses propriétés peut être  atteint. Ajoutez la syntaxe suivante à la classe code-behind de la page maître pour exposer la propriété `Text` de l’étiquette :

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

Lorsqu’un nouvel enregistrement est ajouté à la table `Products` à partir d’une page de contenu, le `RecentProducts` GridView de la page maître doit être reliée à sa source de données sous-jacente. Pour lier à nouveau le GridView, appelez sa méthode `DataBind`. Étant donné que le contrôle GridView dans la page maître n’est pas accessible par programmation aux pages de contenu, nous devons créer une méthode publique dans la page maître qui, lorsqu’elle est appelée, relit les données au GridView. Ajoutez la méthode suivante à la classe code-behind de la page maître :

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

Avec la propriété `GridMessageText` et la méthode `RefreshRecentProductsGrid` en place, toute page de contenu peut définir ou lire par programmation la valeur de la propriété `Text` de l’étiquette `GridMessage` ou lier les données au contrôle GridView `RecentProducts`. L’étape 4 explique comment accéder aux propriétés et méthodes publiques de la page maître à partir d’une page de contenu.

> [!NOTE]
> N’oubliez pas de marquer les propriétés et les méthodes de la page maître comme `Public`. Si vous ne spécifiez pas explicitement ces propriétés et méthodes comme `Public`, elles ne seront pas accessibles à partir de la page de contenu.

## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Étape 4 : appel des membres publics de la page maître à partir d’une page de contenu

Maintenant que la page maître possède les propriétés et méthodes publiques nécessaires, nous sommes prêts à appeler ces propriétés et méthodes à partir de la page de contenu `AddProduct.aspx`. Plus précisément, nous devons définir la propriété `GridMessageText` de la page maître et appeler sa méthode `RefreshRecentProductsGrid` une fois que le nouveau produit a été ajouté à la base de données. Tous les contrôles Web de données ASP.NET déclenchent immédiatement des événements avant et après l’exécution de différentes tâches, ce qui permet aux développeurs de pages d’effectuer des actions de programmation avant ou après la tâche. Par exemple, lorsque l’utilisateur final clique sur le bouton d’insertion du contrôle DetailsView, le contrôle DetailsView déclenche son événement `ItemInserting` avant de commencer le workflow d’insertion. Il insère ensuite l’enregistrement dans la base de données. À la suite de cela, le contrôle DetailsView déclenche son événement `ItemInserted`. Par conséquent, pour pouvoir utiliser la page maître après l’ajout du nouveau produit, créez un gestionnaire d’événements pour l’événement de `ItemInserted` du contrôle DetailsView.

Il existe deux façons pour une page de contenu d’interagir par programme avec sa page maître :

- À l’aide de la propriété `Page.Master`, qui retourne une référence faiblement typée à la page maître, ou
- Spécifiez le type de page maître ou le chemin d’accès du fichier de la page via une directive `@MasterType` ; Cela ajoute automatiquement une propriété fortement typée à la page nommée `Master`.

Examinons les deux approches.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Utilisation de la propriété de`Page.Master`faiblement typée

Toutes les pages Web ASP.NET doivent dériver de la classe `Page`, qui se trouve dans l’espace de noms `System.Web.UI`. La classe `Page` comprend une [propriété`Master`](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) qui retourne une référence à la page maître de la page. Si la page n’a pas de page maître `Master` retourne `Nothing`.

La propriété `Master` retourne un objet de type [`MasterPage`](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (également situé dans l’espace de noms `System.Web.UI`) qui est le type de base dont toutes les pages maîtres dérivent. Par conséquent, pour utiliser des propriétés ou des méthodes publiques définies dans la page maître de notre site Web, nous devons effectuer un cast de l’objet `MasterPage` renvoyé de la propriété `Master` vers le type approprié. Étant donné que nous avons nommé notre fichier de page maître `Site.master`, la classe code-behind était nommée `Site`. Par conséquent, le code suivant convertit la propriété `Page.Master` en une instance de la classe `Site`.

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

Maintenant que nous avons converti la propriété de `Page.Master` faiblement typé en type de site, nous pouvons référencer les propriétés et les méthodes spécifiques au site. Comme le montre la figure 7, la propriété publique `GridMessageText` s’affiche dans la liste déroulante IntelliSense.

[![IntelliSense affiche les propriétés et les méthodes publiques de notre page maître](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**Figure 07**: IntelliSense affiche les propriétés et les méthodes publiques de notre page maître ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))

> [!NOTE]
> Si vous avez nommé votre fichier de page maître `MasterPage.master` le nom de la classe code-behind de la page maître est `MasterPage`. Cela peut entraîner un code ambigu lors de la conversion du type `System.Web.UI.MasterPage` à votre classe `MasterPage`. En résumé, vous devez qualifier complètement le type vers lequel vous effectuez un cast, ce qui peut être un peu délicat lors de l’utilisation du modèle de projet de site Web. Ma suggestion serait de s’assurer que lorsque vous créez votre page maître, vous la nommez autrement que `MasterPage.master` ou, mieux encore, créez une référence fortement typée à la page maître.

### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Création d’une référence fortement typée avec la directive`@MasterType`

Si vous regardez de près, vous pouvez voir que la classe code-behind d’une page ASP.NET est une classe partielle (Notez le mot clé `Partial` dans la définition de classe). Les classes partielles ont C# été introduites dans et Visual Basic Framework with.NET 2,0 et, en résumé, permettent de définir les membres d’une classe sur plusieurs fichiers. Le fichier de classe code-behind `AddProduct.aspx.vb`, par exemple, contient le code que nous avons créé par le développeur de pages. En plus de notre code, le moteur ASP.NET crée automatiquement un fichier de classe distinct avec des propriétés et des gestionnaires d’événements dans qui traduisent le balisage déclaratif dans la hiérarchie de classes de la page.

La génération de code automatique qui se produit chaque fois qu’une page ASP.NET est visitée ouvre la voie à de nouvelles possibilités intéressantes et utiles. Dans le cas des pages maîtres, si nous indiquons au moteur ASP.NET quelle page maître est utilisée par notre page de contenu, il génère une propriété de `Master` fortement typée pour nous.

Utilisez la [directive`@MasterType`](https://msdn.microsoft.com/library/ms228274.aspx) pour informer le moteur ASP.net du type de page maître de la page de contenu. La directive `@MasterType` peut accepter le nom de type de la page maître ou son chemin d’accès au fichier. Pour spécifier que la page `AddProduct.aspx` utilise `Site.master` comme page maître, ajoutez la directive suivante au début de `AddProduct.aspx`:

[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

Cette directive indique au moteur ASP.NET d’ajouter une référence fortement typée à la page maître par le biais d’une propriété nommée `Master`. Une fois la directive `@MasterType` en place, nous pouvons appeler les propriétés et les méthodes publiques de la page maître `Site.master` directement via la propriété `Master` sans casts.

> [!NOTE]
> Si vous omettez la directive `@MasterType`, la syntaxe `Page.Master` et `Master` retourner la même chose : un objet faiblement typé à la page maître de la page. Si vous incluez la directive `@MasterType`, `Master` retourne une référence fortement typée à la page maître spécifiée. Toutefois, `Page.Master`, retourne toujours une référence faiblement typée. Pour plus d’informations sur la façon dont c’est le cas et sur la construction de la propriété `Master` lorsque la directive `@MasterType` est incluse, consultez le billet de blog [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) [`@MasterType` dans ASP.NET 2,0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).

### <a name="updating-the-master-page-after-adding-a-new-product"></a>Mise à jour de la page maître après l’ajout d’un nouveau produit

Maintenant que nous savons comment appeler les propriétés et les méthodes publiques d’une page maître à partir d’une page de contenu, nous sommes prêts à mettre à jour la page de `AddProduct.aspx` afin que la page maître soit actualisée après l’ajout d’un nouveau produit. Au début de l’étape 4, nous avons créé un gestionnaire d’événements pour l’événement `ItemInserting` du contrôle DetailsView, qui s’exécute immédiatement après l’ajout du nouveau produit à la base de données. Ajoutez le code suivant à ce gestionnaire d’événements :

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

Le code ci-dessus utilise à la fois la propriété de `Page.Master` faiblement typée et la propriété de `Master` fortement typée. Notez que la propriété `GridMessageText` est définie sur «*ProductName* ajouté à Grid... » Les valeurs du produit que vous venez d’ajouter sont accessibles via la collection `e.Values` ; comme vous pouvez le voir, la valeur `ProductName` ajoutée est accessible via `e.Values("ProductName")`.

La figure 8 illustre la page de `AddProduct.aspx` immédiatement après l’ajout d’un nouveau produit (Soda de Scott) à la base de données. Notez que le nom du produit que vous venez d’ajouter est indiqué dans l’étiquette de la page maître et que le GridView a été actualisé pour inclure le produit et son prix.

[![l’étiquette de la page maître et que GridView affiche le produit que vous venez d’ajouter](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**Figure 08**: l’étiquette de la page maître et GridView affichent le produit que vous venez d’ajouter ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))

## <a name="summary"></a>Récapitulatif

Dans l’idéal, une page maître et ses pages de contenu sont complètement séparées les unes des autres et ne nécessitent aucun niveau d’interaction. Bien que les pages maîtres et les pages de contenu doivent être conçues avec cet objectif, il existe un certain nombre de scénarios courants dans lesquels une page de contenu doit interagir avec sa page maître. L’une des raisons les plus courantes est consacrée à la mise à jour d’une partie spécifique de la page maître, en fonction d’une action qui s’est affichée dans la page de contenu.

La bonne nouvelle, c’est qu’il est relativement simple de faire en sorte qu’une page de contenu interagisse par programmation avec sa page maître. Commencez par créer des propriétés ou des méthodes publiques dans la page maître qui encapsulent les fonctionnalités qui doivent être appelées par une page de contenu. Ensuite, dans la page contenu, accédez aux propriétés et méthodes de la page maître via la propriété faiblement typée `Page.Master` ou utilisez la directive `@MasterType` pour créer une référence fortement typée à la page maître.

Dans le didacticiel suivant, nous examinons comment la page maître interagit par programmation avec l’une de ses pages de contenu.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Accès et mise à jour des données dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Pages maîtres ASP.NET : conseils, astuces et interruptions](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` dans ASP.NET 2,0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Passage d’informations entre le contenu et les pages maîtres](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Utilisation des données dans les didacticiels ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs ouvrages ASP/ASP. net et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 3,5 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott peut être contacté au [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Zack Jones. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, insérez une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](control-id-naming-in-content-pages-vb.md)
> [Suivant](interacting-with-the-content-page-from-the-master-page-vb.md)
