---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: Interaction avec la page de contenu à partir de la pageC#maître () | Microsoft Docs
author: rick-anderson
description: Examine comment appeler des méthodes, définir des propriétés, etc. de la page de contenu à partir du code de la page maître.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 2cf57665aa584285351d874267949d61db69c7fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78618194"
---
# <a name="interacting-with-the-content-page-from-the-master-page-c"></a>Interaction avec la page de contenu à partir de la page maître (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> Examine comment appeler des méthodes, définir des propriétés, etc. de la page de contenu à partir du code de la page maître.

## <a name="introduction"></a>Introduction

Le didacticiel précédent a examiné comment la page de contenu interagit par programmation avec sa page maître. Rappelez-vous que nous avons mis à jour la page maître pour inclure un contrôle GridView qui répertorie les cinq derniers produits ajoutés. Nous avons ensuite créé une page de contenu à partir de laquelle l’utilisateur peut ajouter un nouveau produit. Lors de l’ajout d’un nouveau produit, la page de contenu devait indiquer à la page maître d’actualiser son GridView afin qu’il inclue le produit juste ajouté. Cette fonctionnalité a été obtenue en ajoutant une méthode publique à la page maître qui a actualisé les données liées au GridView, puis en appelant cette méthode à partir de la page de contenu.

La forme la plus courante d’interaction entre le contenu et la page maître provient de la page de contenu. Toutefois, il est possible que la page maître Rouse la page de contenu actuelle en action, et de telles fonctionnalités peuvent être nécessaires si la page maître contient des éléments d’interface utilisateur qui permettent aux utilisateurs de modifier des données qui sont également affichées dans la page de contenu. Imaginez une page de contenu qui affiche les informations sur les produits dans un contrôle GridView et une page maître qui comprend un contrôle Button qui, lorsque vous cliquez dessus, double les prix de tous les produits. À l’instar de l’exemple du didacticiel précédent, le contrôle GridView doit être actualisé une fois que vous avez cliqué sur le bouton double prix pour afficher les nouveaux prix, mais dans ce scénario, il s’agit de la page maître qui doit Rouse la page de contenu en action.

Ce didacticiel décrit comment définir la fonctionnalité d’appel de la page maître dans la page de contenu.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Interaction de programmation à l’aide d’un gestionnaire d’événements et d’événements

L’appel de la fonctionnalité de page de contenu à partir d’une page maître est plus complexe que l’inverse. Étant donné qu’une page de contenu a une seule page maître, lorsque vous effectuez l’interaction par programmation à partir de la page de contenu, nous savons ce que sont les méthodes et les propriétés publiques à notre disposition. Toutefois, une page maître peut avoir de nombreuses pages de contenu différentes, chacune avec son propre ensemble de propriétés et de méthodes. Comment puis-je écrire du code dans la page maître pour effectuer une action dans sa page de contenu lorsque nous ne savons pas quelle page de contenu sera appelée jusqu’à l’exécution ?

Prenons l’exemple d’un contrôle Web ASP.NET, tel que le contrôle Button. Un contrôle bouton peut apparaître sur n’importe quel nombre de pages ASP.NET et nécessite un mécanisme par lequel il peut alerter la page sur laquelle l’utilisateur a cliqué. Cela s’effectue à l’aide d' *événements*. En particulier, le contrôle Button déclenche son événement `Click` lorsque l’utilisateur clique dessus ; la page ASP.NET qui contient le bouton peut éventuellement répondre à cette notification via un *Gestionnaire d’événements*.

Ce même modèle peut être utilisé pour qu’une page maître déclenche des fonctionnalités dans ses pages de contenu :

1. Ajoutez un événement à la page maître.
2. Déclenchez l’événement chaque fois que la page maître doit communiquer avec sa page de contenu. Par exemple, si la page maître doit avertir la page de contenu que l’utilisateur a doublé les prix, son événement est déclenché immédiatement après que les prix ont été doublés.
3. Créez un gestionnaire d’événements dans les pages de contenu qui doivent prendre des mesures.

Le reste de ce didacticiel met en œuvre l’exemple décrit dans l’introduction. à savoir, une page de contenu qui répertorie les produits de la base de données et une page maître qui comprend un contrôle bouton pour doubler les prix.

## <a name="step-1-displaying-products-in-a-content-page"></a>Étape 1 : affichage des produits dans une page de contenu

Notre premier ordre d’activité consiste à créer une page de contenu qui répertorie les produits de la base de données Northwind. (Nous avons ajouté la base de données Northwind au projet dans le didacticiel précédent, en [*interagissant avec la page maître à partir de la page de contenu*](interacting-with-the-master-page-from-the-content-page-cs.md).) Commencez par ajouter une nouvelle page ASP.NET au dossier `~/Admin` nommé `Products.aspx`, en veillant à le lier à la page maître `Site.master`. La figure 1 montre les Explorateur de solutions une fois que cette page a été ajoutée au site Web.

[![ajouter une nouvelle page ASP.NET au dossier Admin](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**Figure 01**: ajouter une nouvelle page ASP.net au dossier `Admin` ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))

Rappelez-vous que dans le didacticiel [*spécification du titre, des balises meta et d’autres en-têtes HTML dans le didacticiel de la page maître*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) , nous avons créé une classe de page de base personnalisée nommée `BasePage` qui génère le titre de la page s’il n’est pas explicitement défini. Accédez à la classe code-behind de la page de `Products.aspx` et dérivez-la de `BasePage` (au lieu de `System.Web.UI.Page`).

Enfin, mettez à jour le fichier `Web.sitemap` pour inclure une entrée pour cette leçon. Ajoutez le balisage suivant sous le `<siteMapNode>` pour la leçon relative à l’interaction du contenu vers la page maître :

[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

L’ajout de cet élément `<siteMapNode>` est reflété dans la liste leçons (voir figure 5).

Revenez à `Products.aspx`. Dans le contrôle de contenu pour `MainContent`, ajoutez un contrôle GridView et nommez-le `ProductsGrid`. Liez le contrôle GridView à un nouveau contrôle SqlDataSource nommé `ProductsDataSource`.

[![lier le contrôle GridView à un nouveau contrôle SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**Figure 02**: lier le contrôle GridView à un nouveau contrôle SqlDataSource ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))

Configurez l’Assistant pour qu’il utilise la base de données Northwind. Si vous avez utilisé le didacticiel précédent, vous devez déjà disposer d’une chaîne de connexion nommée `NorthwindConnectionString` dans `Web.config`. Choisissez cette chaîne de connexion dans la liste déroulante, comme illustré à la figure 3.

[![configurer le SqlDataSource pour utiliser la base de données Northwind](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**Figure 03**: configurer le SqlDataSource pour utiliser la base de données Northwind ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))

Ensuite, spécifiez l’instruction `SELECT` du contrôle de source de données en choisissant la table Products dans la liste déroulante et en retournant les colonnes `ProductName` et `UnitPrice` (voir figure 4). Cliquez sur suivant, puis sur Terminer pour terminer l’Assistant Configuration de la source de données.

[![retourner les champs ProductName et UnitPrice à partir de la table Products](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**Figure 04**: retourner les champs `ProductName` et `UnitPrice` de la table `Products` ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))

C’est aussi simple que cela ! À la fin de l’Assistant, Visual Studio ajoute deux BoundFields au GridView pour mettre en miroir les deux champs retournés par le contrôle SqlDataSource. Le balisage des contrôles GridView et SqlDataSource suit. La figure 5 montre les résultats lorsqu’ils sont affichés dans un navigateur.

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]

[![chaque produit et son prix sont indiqués dans le contrôle GridView](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**Figure 05**: chaque produit et son prix sont listés dans le contrôle GridView ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))

> [!NOTE]
> N’hésitez pas à nettoyer l’apparence du contrôle GridView. Certaines suggestions incluent la mise en forme de la valeur UnitPrice affichée en tant que devise et l’utilisation des couleurs et des polices d’arrière-plan pour améliorer l’apparence de la grille. Pour plus d’informations sur l’affichage et la mise en forme des données dans ASP.NET, reportez-vous à la [série didacticiel sur les données](../../data-access/index.md).

## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Étape 2 : ajout d’un bouton de double prix à la page maître

La tâche suivante consiste à ajouter un contrôle Web Button à la page maître qui, lorsque vous cliquez dessus, double le prix de tous les produits de la base de données. Ouvrez la page maître `Site.master` et faites glisser un bouton de la boîte à outils vers le concepteur, en la plaçant sous le contrôle `RecentProductsDataSource` SqlDataSource que nous avons ajouté dans le didacticiel précédent. Affectez à la propriété `ID` du bouton la valeur `DoublePrice` et à sa propriété `Text` la valeur « double Price Product ».

Ensuite, ajoutez un contrôle SqlDataSource à la page maître, en lui attribuant un nom `DoublePricesDataSource`. Ce SqlDataSource sera utilisé pour exécuter l’instruction `UPDATE` pour doubler tous les prix. Plus précisément, nous devons définir ses propriétés `ConnectionString` et `UpdateCommand` sur la chaîne de connexion appropriée et l’instruction `UPDATE`. Nous devons ensuite appeler la méthode `Update` de ce contrôle SqlDataSource quand l’utilisateur clique sur le bouton `DoublePrice`. Pour définir les propriétés `ConnectionString` et `UpdateCommand`, sélectionnez le contrôle SqlDataSource, puis accédez à la Fenêtre Propriétés. La propriété `ConnectionString` répertorie les chaînes de connexion déjà stockées dans `Web.config` dans une liste déroulante. Choisissez l’option `NorthwindConnectionString`, comme illustré à la figure 6.

[![configurer le SqlDataSource pour utiliser l’NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**Figure 06**: configurer le SqlDataSource pour utiliser le `NorthwindConnectionString` ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))

Pour définir la propriété `UpdateCommand`, recherchez l’option UpdateQuery dans le Fenêtre Propriétés. Cette propriété, lorsqu’elle est sélectionnée, affiche un bouton avec des ellipses. Cliquez sur ce bouton pour afficher la boîte de dialogue Éditeur de commandes et de paramètres illustrée à la figure 7. Tapez l’instruction de `UPDATE` suivante dans la zone de texte de la boîte de dialogue :

[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

Lorsqu’elle est exécutée, cette instruction double la valeur `UnitPrice` pour chaque enregistrement de la table `Products`.

[![définir la propriété UpdateCommand du SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**Figure 07**: définir la propriété `UpdateCommand` du SqlDataSource ([Cliquer pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))

Après avoir défini ces propriétés, le balisage déclaratif des contrôles Button et SqlDataSource doit ressembler à ce qui suit :

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

Il ne reste plus qu’à appeler sa méthode `Update` lorsque l’utilisateur clique sur le bouton `DoublePrice`. Créez un gestionnaire d’événements `Click` pour le bouton `DoublePrice` et ajoutez le code suivant :

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

Pour tester cette fonctionnalité, accédez à la page de `~/Admin/Products.aspx` que nous avons créée à l’étape 1, puis cliquez sur le bouton « prix des produits double ». Le fait de cliquer sur le bouton entraîne une publication (postback) et exécute le gestionnaire d’événements `Click` du bouton `DoublePrice`, en doublant les prix de tous les produits. La page est ensuite restituée à nouveau et le balisage est retourné et réaffiché dans le navigateur. Toutefois, le contrôle GridView dans la page de contenu répertorie les mêmes tarifs que ceux qui ont été sur le bouton « double Price Price ». Cela est dû au fait que les données initialement chargées dans le GridView avaient son état stocké dans l’état d’affichage, de sorte qu’elle n’est pas rechargée sur les publications, sauf indication contraire. Si vous visitez une page différente, puis revenez à la page `~/Admin/Products.aspx`, vous verrez les tarifs mis à jour.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Étape 3 : déclenchement d’un événement lorsque les prix sont doublés

Étant donné que le contrôle GridView dans la page de `~/Admin/Products.aspx` ne reflète pas immédiatement le prix doublement, un utilisateur peut considérer qu’il n’a pas cliqué sur le bouton « double prix du produit » ou qu’il n’a pas fonctionné. Ils peuvent essayer de cliquer sur le bouton plusieurs fois, en doublant les prix et de nouveau. Pour résoudre ce problème, nous devons faire en sorte que la grille dans la page de contenu affiche les nouveaux prix juste après leur double.

Comme nous l’avons vu précédemment dans ce didacticiel, nous devons déclencher un événement dans la page maître chaque fois que l’utilisateur clique sur le bouton `DoublePrice`. Un événement est un moyen pour une classe (un éditeur d’événements) de notifier à un autre jeu d’autres classes (les abonnés aux événements) qu’un événement intéressant s’est produit. Dans cet exemple, la page maître est l’éditeur d’événements ; les pages de contenu qui vous intéressent lorsque vous cliquez sur le bouton `DoublePrice` sont les abonnés.

Une classe s’abonne à un événement en créant un *Gestionnaire d’événements*, qui est une méthode exécutée en réponse à l’événement déclenché. Le serveur de publication définit les événements qu’il déclenche en définissant un *délégué d’événement*. Le délégué d’événement spécifie les paramètres d’entrée que le gestionnaire d’événements doit accepter. Dans le .NET Framework, les délégués d’événements ne retournent aucune valeur et acceptent deux paramètres d’entrée :

- Un `Object`, qui identifie la source de l’événement, et
- Classe dérivée de `System.EventArgs`

Le deuxième paramètre passé à un gestionnaire d’événements peut inclure des informations supplémentaires sur l’événement. Bien que la classe de base `EventArgs` ne transmette pas d’informations, la .NET Framework comprend un certain nombre de classes qui étendent `EventArgs` et englobent des propriétés supplémentaires. Par exemple, une `CommandEventArgs` instance est passée à des gestionnaires d’événements qui répondent à l’événement `Command` et comprend deux propriétés d’informations : `CommandArgument` et `CommandName`.

> [!NOTE]
> Pour plus d’informations sur la création, le déclenchement et la gestion des événements, consultez [événements et délégués](https://msdn.microsoft.com/library/17sde2xt.aspx) et [délégués d’événements en anglais simple](http://www.codeproject.com/KB/cs/eventdelegates.aspx).

Pour définir un événement, utilisez la syntaxe suivante :

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

Étant donné que nous avons uniquement besoin d’alerter la page de contenu quand l’utilisateur a cliqué sur le bouton `DoublePrice` et qu’il n’est pas nécessaire de transmettre d’autres informations supplémentaires, nous pouvons utiliser le délégué d’événement `EventHandler`, qui définit un gestionnaire d’événements qui accepte comme deuxième paramètre un objet de type `System.EventArgs`. Pour créer l’événement dans la page maître, ajoutez la ligne de code suivante à la classe code-behind de la page maître :

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

Le code ci-dessus ajoute un événement public à la page maître nommée `PricesDoubled`. Nous devons maintenant déclencher cet événement une fois que les prix ont été doublés. Pour déclencher un événement, utilisez la syntaxe suivante :

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

Où *sender* et *EventArgs* sont les valeurs que vous souhaitez passer au gestionnaire d’événements de l’abonné.

Mettez à jour le gestionnaire d’événements `DoublePrice` `Click` à l’aide du code suivant :

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

Comme précédemment, le gestionnaire d’événements `Click` démarre en appelant la méthode `Update` du contrôle `DoublePricesDataSource` SqlDataSource pour doubler les prix de tous les produits. Vous trouverez ci-après deux ajouts au gestionnaire d’événements. Tout d’abord, les données du `RecentProducts` GridView sont actualisées. Ce GridView a été ajouté à la page maître dans le didacticiel précédent et affiche les cinq produits récemment ajoutés. Nous devons actualiser cette grille pour afficher les prix juste-à-deux pour ces cinq produits. Après cela, l’événement `PricesDoubled` est déclenché. Une référence à la page maître elle-même (`this`) est envoyée au gestionnaire d’événements en tant que source de l’événement et un objet `EventArgs` vide est envoyé en tant qu’arguments d’événement.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Étape 4 : gestion de l’événement dans la page de contenu

À ce stade, la page maître déclenche son événement `PricesDoubled` chaque fois que l’utilisateur clique sur le contrôle `DoublePrice` bouton. Toutefois, ce n’est que la moitié de la bataille : nous devons toujours gérer l’événement dans l’abonné. Cela implique deux étapes : la création du gestionnaire d’événements et l’ajout d’un code de câblage d’événement afin que, lorsque l’événement est déclenché, le gestionnaire d’événements soit exécuté.

Commencez par créer un gestionnaire d’événements nommé `Master_PricesDoubled`. En raison de la façon dont nous avons défini l’événement `PricesDoubled` dans la page maître, les deux paramètres d’entrée du gestionnaire d’événements doivent être de type `Object` et `EventArgs`, respectivement. Dans le gestionnaire d’événements, appelez la méthode `DataBind` de `ProductsGrid` GridView pour relier les données à la grille.

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

Le code du gestionnaire d’événements est terminé, mais nous n’avons pas encore connecté l’événement `PricesDoubled` de la page maître à ce gestionnaire d’événements. L’abonné relie un événement à un gestionnaire d’événements via la syntaxe suivante :

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

le serveur de *publication* est une référence à l’objet qui offre l’événement *EventName*, et *MethodName* est le nom du gestionnaire d’événements défini dans l’abonné qui a une signature correspondant à *eventDelegate*. En d’autres termes, si le délégué d’événement est `EventHandler`, *MethodName* doit être le nom d’une méthode dans l’abonné qui ne retourne pas de valeur et accepte deux paramètres d’entrée de types `Object` et `EventArgs`, respectivement.

Ce code de câblage d’événement doit être exécuté sur la première page visitée et les publications (postbacks) suivantes, et doit se produire à un point du cycle de vie de la page qui précède lorsque l’événement peut être déclenché. Un bon moment pour ajouter le code de câblage des événements est à l’étape de préinitialisation, qui se produit très tôt dans le cycle de vie de la page.

Ouvrez `~/Admin/Products.aspx` et créez un gestionnaire d’événements `Page_PreInit` :

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

Pour pouvoir terminer ce code de câblage, nous avons besoin d’une référence par programmation à la page maître à partir de la page de contenu. Comme indiqué dans le didacticiel précédent, il existe deux façons de procéder :

- En effectuant un cast de la propriété de `Page.Master` faiblement typée vers le type de page maître approprié, ou
- En ajoutant une directive `@MasterType` dans la page `.aspx`, puis en utilisant la propriété fortement typée `Master`.

Utilisons cette dernière approche. Ajoutez la directive `@MasterType` suivante au début du balisage déclaratif de la page :

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

Ajoutez ensuite le code de câblage d’événement suivant dans le gestionnaire d’événements `Page_PreInit` :

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

Une fois ce code en place, le contrôle GridView dans la page de contenu est actualisé chaque fois que l’utilisateur clique sur le bouton `DoublePrice`.

Les figures 8 et 9 illustrent ce comportement. La figure 8 illustre la page lors de la première visite. Notez que les valeurs Price dans le `RecentProducts` GridView (dans la colonne de gauche de la page maître) et le contrôle GridView `ProductsGrid` (dans la page de contenu). La figure 9 présente le même écran immédiatement après que l’utilisateur a cliqué sur le bouton `DoublePrice`. Comme vous pouvez le voir, les nouveaux prix sont instantanément reflétés dans les deux GridViews.

[![les valeurs de prix initiales](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**Figure 08**: valeurs de prix initiaux ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))

[![les prix simplement doublés sont affichés dans les GridViews](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**Figure 09**: les prix juste-à-deux sont affichés dans les GridViews ([cliquez pour afficher l’image en taille réelle](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))

## <a name="summary"></a>Récapitulatif

Dans l’idéal, une page maître et ses pages de contenu sont complètement séparées les unes des autres et ne nécessitent aucun niveau d’interaction. Toutefois, si vous disposez d’une page maître ou d’une page de contenu qui affiche des données qui peuvent être modifiées à partir de la page maître ou de la page de contenu, vous devrez peut-être faire en sorte que la page maître alerte la page de contenu (ou inversement) lorsque les données sont modifiées afin que l’affichage puisse être mis à jour. Dans le didacticiel précédent, nous avons vu comment une page de contenu interagit par programmation avec sa page maître. dans ce didacticiel, nous avons vu comment une page maître initie l’interaction.

Alors que l’interaction par programmation entre un contenu et une page maître peut provenir de la page de contenu ou de la page maître, le modèle d’interaction utilisé dépend de l’origine. Les différences sont dues au fait qu’une page de contenu a une page maître unique, mais qu’une page maître peut avoir de nombreuses pages de contenu différentes. Plutôt que d’avoir une page maître interagissant directement avec une page de contenu, une meilleure approche consiste à faire en sorte que la page maître déclenche un événement pour signaler qu’une action a eu lieu. Les pages de contenu qui s’intéressent à l’action peuvent créer des gestionnaires d’événements.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Accès et mise à jour des données dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Événements et délégués](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Passage d’informations entre le contenu et les pages maîtres](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Utilisation des données dans les didacticiels ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs ouvrages ASP/ASP. net et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 3,5 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott peut être contacté au [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Teli Banerjee. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, insérez une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](interacting-with-the-master-page-from-the-content-page-cs.md)
> [Suivant](master-pages-and-asp-net-ajax-cs.md)
