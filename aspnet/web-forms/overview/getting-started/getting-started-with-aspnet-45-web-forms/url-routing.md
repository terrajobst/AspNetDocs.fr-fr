---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Routage d’URL | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 66b727b69ca4f9a3d35b67f492f9a554146e09ef
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74590707"
---
# <a name="url-routing"></a>Routage d'URL

par [Erik Reitan](https://github.com/Erikre)

[Télécharger l’exemple de projet WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Télécharger le livre électronique (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour le Web. Un [projet Visual Studio 2013 avec C# le code source](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.

Dans ce didacticiel, vous allez modifier l’exemple d’application Wingtip Toys pour prendre en charge le routage d’URL. Le routage permet à votre application Web d’utiliser des URL conviviales, faciles à mémoriser et mieux prises en charge par les moteurs de recherche. Ce didacticiel s’appuie sur le didacticiel précédent « appartenance et administration » et fait partie de la série de didacticiels Wingtip Toys.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment inscrire des itinéraires pour une application ASP.NET Web Forms.
- Comment ajouter des itinéraires à une page Web.
- Comment sélectionner des données dans une base de données pour prendre en charge les itinéraires.

## <a name="aspnet-routing-overview"></a>Vue d’ensemble du routage ASP.NET

Le routage d’URL vous permet de configurer une application pour accepter des URL de requête qui ne sont pas mappées à des fichiers physiques. Une URL de demande est simplement l’URL entrée par un utilisateur dans son navigateur pour rechercher une page sur votre site Web. Vous utilisez le routage pour définir des URL qui sont sémantiquement significatives pour les utilisateurs et qui peuvent aider à l’optimisation du moteur de recherche (SEO).

Par défaut, le modèle Web Forms comprend des [URL conviviales ASP.net](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Une grande partie du travail de routage de base sera implémentée à l’aide d' *URL conviviales*. Toutefois, dans ce didacticiel, vous allez ajouter des fonctionnalités de routage personnalisées.

Avant de personnaliser le routage des URL, l’exemple d’application Wingtip Toys peut créer un lien vers un produit à l’aide de l’URL suivante :

`https://localhost:44300/ProductDetails.aspx?productID=2`

En personnalisant le routage des URL, l’exemple d’application Wingtip Toys crée un lien vers le même produit à l’aide d’une URL plus facile à lire :

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Routes

Un itinéraire est un modèle d’URL qui est mappé à un gestionnaire. Le gestionnaire peut être un fichier physique, tel qu’un fichier. aspx dans une application Web Forms. Un gestionnaire peut également être une classe qui traite la requête. Pour définir un itinéraire, vous créez une instance de la classe de routage en spécifiant le modèle d’URL, le gestionnaire et éventuellement un nom pour l’itinéraire.

Pour ajouter l’itinéraire à l’application, ajoutez l’objet `Route` à la propriété statique `Routes` de la classe `RouteTable`. La propriété routes est un objet `RouteCollection` qui stocke tous les itinéraires de l’application.

### <a name="url-patterns"></a>Modèles d’URL

Un modèle d’URL peut contenir des valeurs littérales et des espaces réservés de variables (appelés paramètres d’URL). Les littéraux et les espaces réservés se trouvent dans des segments de l’URL qui sont délimités par le caractère barre oblique (`/`).

Quand une demande à votre application Web est effectuée, l’URL est analysée en segments et en espaces réservés, et les valeurs des variables sont fournies au gestionnaire de demandes. Ce processus est similaire à la façon dont les données d’une chaîne de requête sont analysées et transmises au gestionnaire de requêtes. Dans les deux cas, les informations sur les variables sont incluses dans l’URL et transmises au gestionnaire sous la forme de paires clé-valeur. Pour les chaînes de requête, les clés et les valeurs sont dans l’URL. Pour les itinéraires, les clés sont les noms des espaces réservés définis dans le modèle d’URL, et seules les valeurs sont dans l’URL.

Dans un modèle d’URL, vous définissez des espaces réservés en les plaçant entre accolades (`{` et `}`). Vous pouvez définir plusieurs espaces réservés dans un segment, mais ceux-ci doivent être séparés par une valeur littérale. Par exemple, `{language}-{country}/{action}` est un modèle d’itinéraire valide. Toutefois, `{language}{country}/{action}` n’est pas un modèle valide, car il n’existe pas de valeur littérale ou de délimiteur entre les espaces réservés. Par conséquent, le routage ne peut pas déterminer où séparer la valeur de l’espace réservé de langue de la valeur de l’espace réservé Country.

### <a name="mapping-and-registering-routes"></a>Mappage et inscription des itinéraires

Avant de pouvoir inclure des itinéraires vers les pages de l’exemple d’application Wingtip Toys, vous devez inscrire les itinéraires au démarrage de l’application. Pour inscrire les itinéraires, vous allez modifier le gestionnaire d’événements `Application_Start`.

1. Dans **Explorateur de solutions**de Visual Studio, recherchez et ouvrez le fichier *global.asax.cs* .
2. Ajoutez le code mis en surbrillance en jaune au fichier *global.asax.cs* comme suit :   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Lorsque l’exemple d’application Wingtip Toys démarre, il appelle le gestionnaire d’événements `Application_Start`. À la fin de ce gestionnaire d’événements, la méthode `RegisterCustomRoutes` est appelée. La méthode `RegisterCustomRoutes` ajoute chaque itinéraire en appelant la méthode `MapPageRoute` de l’objet `RouteCollection`. Les itinéraires sont définis à l’aide d’un nom d’itinéraire, d’une URL de routage et d’une URL physique.

Le premier paramètre («`ProductsByCategoryRoute`») est le nom de l’itinéraire. Elle est utilisée pour appeler l’itinéraire quand cela est nécessaire. Le deuxième paramètre («`Category/{categoryName}`») définit l’URL de remplacement conviviale qui peut être dynamique en fonction du code. Vous utilisez cet itinéraire lorsque vous remplissez un contrôle de données avec des liens qui sont générés en fonction de données. Un itinéraire s’affiche comme suit :

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Le deuxième paramètre de l’itinéraire contient une valeur dynamique spécifiée par des accolades (`{ }`). Dans ce cas, le `categoryName` est une variable qui sera utilisée pour déterminer le chemin de routage approprié.

> [!NOTE] 
> 
> **Optional**
> 
> Il peut s’avérer plus facile de gérer votre code en déplaçant la méthode `RegisterCustomRoutes` dans une classe distincte. Dans le dossier *logique* , créez une classe `RouteActions` distincte. Déplacez la méthode `RegisterCustomRoutes` ci-dessus à partir du fichier *global.asax.cs* vers la nouvelle classe `RoutesActions`. Utilisez la classe `RoleActions` et la méthode `createAdmin` comme exemple d’appel de la méthode `RegisterCustomRoutes` à partir du fichier *global.asax.cs* .

Vous avez peut-être également remarqué l’appel de la méthode `RegisterRoutes` à l’aide de l’objet `RouteConfig` au début du gestionnaire d’événements `Application_Start`. Cet appel est effectué pour implémenter le routage par défaut. Il a été inclus comme code par défaut lorsque vous avez créé l’application à l’aide du modèle de Web Forms de Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Récupération et utilisation des données d’itinéraire

Comme indiqué ci-dessus, les itinéraires peuvent être définis. Le code que vous avez ajouté au gestionnaire d’événements `Application_Start` dans le fichier *global.asax.cs* charge les itinéraires définissables.

### <a name="setting-routes"></a>Définition des itinéraires

Les itinéraires vous obligent à ajouter du code supplémentaire. Dans ce didacticiel, vous allez utiliser la liaison de modèle pour récupérer un objet `RouteValueDictionary` utilisé lors de la génération des itinéraires à l’aide des données d’un contrôle de données. L’objet `RouteValueDictionary` contient une liste de noms de produits appartenant à une catégorie de produits spécifique. Un lien est créé pour chaque produit en fonction des données et de l’itinéraire.

#### <a name="enable-routes-for-categories-and-products"></a>Activer les itinéraires pour les catégories et les produits

Ensuite, vous allez mettre à jour l’application pour utiliser le `ProductsByCategoryRoute` pour déterminer l’itinéraire correct à inclure pour chaque lien de catégorie de produit. Vous allez également mettre à jour la page *ProductList. aspx* pour inclure un lien routé pour chaque produit. Les liens s’affichent tels qu’ils étaient avant la modification, mais les liens utilisent désormais le routage d’URL.

1. Dans **Explorateur de solutions**, ouvrez la page *site. Master* si elle n’est pas déjà ouverte.
2. Mettez à jour le contrôle **ListView** nommé «`categoryList`» avec les modifications mises en surbrillance en jaune, afin que le balisage apparaisse comme suit :   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. Dans **Explorateur de solutions**, ouvrez la page *ProductList. aspx* .
4. Mettez à jour l’élément `ItemTemplate` de la page *ProductList. aspx* avec les mises à jour mises en surbrillance en jaune, afin que le balisage apparaisse comme suit :   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Ouvrez le code-behind de *ProductList.aspx.cs* et ajoutez l’espace de noms suivant mis en surbrillance en jaune :  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Remplacez la méthode `GetProducts` de code-behind (*ProductList.aspx.cs*) par le code suivant :   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Ajouter du code pour les détails du produit

À présent, mettez à jour le code-behind (*ProductDetails.aspx.cs*) de la page *ProductDetails. aspx* pour utiliser les données d’itinéraire. Notez que la nouvelle méthode `GetProduct` accepte également une valeur de chaîne de requête pour le cas où l’utilisateur a un lien favori qui utilise l’ancienne URL non conviviale et non routée.

1. Remplacez la méthode `GetProduct` de code-behind (*ProductDetails.aspx.cs*) par le code suivant :   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Exécution de l'application

Vous pouvez exécuter l’application maintenant pour voir les itinéraires mis à jour.

1. Appuyez sur **F5** pour exécuter l’exemple d’application Wingtip Toys.  
 Le navigateur s’ouvre et affiche la page *default. aspx* .
2. Cliquez sur le lien **produits** en haut de la page.  
 Tous les produits s’affichent sur la page *ProductList. aspx* . L’URL suivante (à l’aide de votre numéro de port) s’affiche pour le navigateur :  
    `https://localhost:44300/ProductList`
3. Ensuite, cliquez sur le lien catégorie **Cars** près du haut de la page.  
 Seules les voitures sont affichées sur la page *ProductList. aspx* . L’URL suivante (à l’aide de votre numéro de port) s’affiche pour le navigateur :  
    `https://localhost:44300/Category/Cars`
4. Cliquez sur le lien contenant le nom de la première voiture figurant sur la page («**voiture convertible**») pour afficher les détails du produit.  
 L’URL suivante (à l’aide de votre numéro de port) s’affiche pour le navigateur :  
    `https://localhost:44300/Product/Convertible%20Car`
5. Ensuite, entrez l’URL non routée suivante (à l’aide de votre numéro de port) dans le navigateur :  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Le code reconnaît toujours une URL qui inclut une chaîne de requête, pour le cas où un utilisateur a un lien favori.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez ajouté des itinéraires pour les catégories et les produits. Vous avez appris comment intégrer des itinéraires à des contrôles de données qui utilisent la liaison de modèle. Dans le didacticiel suivant, vous allez implémenter la gestion globale des erreurs.

## <a name="additional-resources"></a>Ressources supplémentaires

[URL conviviales ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Déployer une application ASP.NET Web Forms sécurisée avec appartenance, OAuth et SQL Database à Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure-essai gratuit](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Précédent](membership-and-administration.md)
> [Suivant](aspnet-error-handling.md)
