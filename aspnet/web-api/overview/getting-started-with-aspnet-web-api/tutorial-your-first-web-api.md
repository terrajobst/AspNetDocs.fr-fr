---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Prise en main de API Web ASP.NET 2C#()-ASP.net 4. x
author: MikeWasson
description: Didacticiel avec code. Utilisez API Web ASP.NET pour créer une API Web qui retourne une liste de produits.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 3e35c2bc0e46dfdb4544b772775eddd533f27be3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556797"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a>Prise en main de API Web ASP.NET 2C#()

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

Dans ce didacticiel, vous allez utiliser API Web ASP.NET pour créer une API Web qui retourne une liste de produits.

HTTP n’est pas seulement destiné au traitement des pages Web. HTTP est également une plateforme puissante pour la création d’API qui exposent des services et des données. HTTP est simple, flexible et omniprésent. Presque toutes les plateformes que vous pouvez considérer ont une bibliothèque HTTP, de sorte que les services HTTP peuvent atteindre un large éventail de clients, notamment des navigateurs, des appareils mobiles et des applications de bureau traditionnelles.

API Web ASP.NET est une infrastructure permettant de créer des API Web en plus des .NET Framework. 

## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- API Web 2

Pour obtenir une version plus récente de ce didacticiel [, consultez créer une API Web avec ASP.net Core et Visual Studio pour Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) .

## <a name="create-a-web-api-project"></a>Créer un projet d’API Web

Dans ce didacticiel, vous allez utiliser API Web ASP.NET pour créer une API Web qui retourne une liste de produits. La page Web frontale utilise jQuery pour afficher les résultats.

![](tutorial-your-first-web-api/_static/image1.png)

Démarrez Visual Studio et sélectionnez **nouveau projet** dans la page de **démarrage** . Ou, dans le menu **fichier** , sélectionnez **nouveau** , puis **projet**.

Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  . Sous **visuel C#** , sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **application Web ASP.net**. Nommez le projet « ProductsApp », puis cliquez sur **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le modèle **vide** . Sous &quot;ajouter des dossiers et des références principales pour&quot;, vérifiez **API Web**. Cliquez sur **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Vous pouvez également créer un projet d’API Web à l’aide du modèle de&quot; d’API Web &quot;. Le modèle d’API Web utilise ASP.NET MVC pour fournir les pages d’aide de l’API. J’utilise le modèle vide pour ce didacticiel, car je souhaite afficher l’API Web sans MVC. En général, vous n’avez pas besoin de connaître ASP.NET MVC pour utiliser l’API Web.

## <a name="adding-a-model"></a>Ajout d’un modèle

Un *modèle* est un objet qui représente les données dans votre application. API Web ASP.NET pouvez sérialiser automatiquement votre modèle en JSON, XML ou un autre format, puis écrire les données sérialisées dans le corps du message de réponse HTTP. Tant qu’un client peut lire le format de sérialisation, il peut désérialiser l’objet. La plupart des clients peuvent analyser XML ou JSON. En outre, le client peut indiquer le format souhaité en définissant l’en-tête Accept dans le message de requête HTTP.

Commençons par créer un modèle simple qui représente un produit.

Si l’Explorateur de solutions n’est pas déjà visible, cliquez sur le menu **Vue** et sélectionnez **Explorateur de solutions**. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier Modèles. Dans le menu contextuel, sélectionnez **Ajouter**, puis **Classe**.

![](tutorial-your-first-web-api/_static/image4.png)

Nommez la classe &quot;&quot;de produit. Ajoutez les propriétés suivantes à la classe `Product`.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Ajour d’un contrôleur

Dans l’API web, un *contrôleur* est un objet qui gère les requêtes HTTP. Nous allons ajouter un contrôleur qui peut retourner une liste de produits ou un seul produit spécifié par ID.

> [!NOTE]
> Si vous avez utilisé ASP.NET MVC, vous êtes déjà familiarisé avec les contrôleurs. Les contrôleurs d’API Web sont similaires aux contrôleurs MVC, mais héritent de la classe **ApiController** au lieu de la classe **Controller** .

Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier Contrôleurs. Sélectionnez **Ajouter**, puis **Contrôleur**.

![](tutorial-your-first-web-api/_static/image5.png)

Dans la boîte de dialogue **Ajouter une structure**, cliquez sur **Web API Controller - Empty** (Contrôleur d’API web - Vide). Cliquez sur **Ajouter**.

![](tutorial-your-first-web-api/_static/image6.png)

Dans la boîte de dialogue **Ajouter un contrôleur** , nommez le contrôleur &quot;&quot;ProductsController. Cliquez sur **Ajouter**.

![](tutorial-your-first-web-api/_static/image7.png)

La génération de modèles automatique crée un fichier nommé ProductsController.cs dans le dossier Controllers.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Vous n’avez pas besoin de placer vos contrôleurs dans un dossier nommé contrôleurs. Le nom du dossier est simplement un moyen pratique d’organiser vos fichiers sources.

Si ce fichier n’est pas déjà ouvert, double-cliquez dessus pour l’ouvrir. Remplacez le code de ce fichier par ce qui suit :

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Pour que l’exemple reste simple, les produits sont stockés dans un tableau fixe à l’intérieur de la classe Controller. Bien sûr, dans une application réelle, vous interrogez une base de données ou vous utilisez une autre source de données externe.

Le contrôleur définit deux méthodes qui retournent des produits :

- La méthode `GetAllProducts` retourne l’intégralité de la liste de produits sous la forme d’un type de **&gt;de produit IEnumerable&lt;** .
- La méthode `GetProduct` recherche un seul produit par son ID.

C’est tout ! Vous disposez d’une API Web opérationnelle. Chaque méthode sur le contrôleur correspond à un ou plusieurs URI :

| Méthode du contrôleur | URI |
| --- | --- |
| GetAllProducts | /api/products |
| GetProduct | *ID* /API/Products/ |

Pour la méthode `GetProduct`, l' *ID* de l’URI est un espace réservé. Par exemple, pour obtenir le produit avec l’ID 5, l’URI est `api/products/5`.

Pour plus d’informations sur la façon dont l’API Web achemine les requêtes HTTP vers les méthodes de contrôleur, consultez [routage dans API Web ASP.net](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Appel de l’API Web avec JavaScript et jQuery

Dans cette section, nous allons ajouter une page HTML qui utilise AJAX pour appeler l’API Web. Nous allons utiliser jQuery pour effectuer les appels AJAX et également pour mettre à jour la page avec les résultats.

Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter**, puis sélectionnez **nouvel élément**.

![](tutorial-your-first-web-api/_static/image9.png)

Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez le nœud **Web** sous **visuel C#** , puis sélectionnez l’élément de **page HTML** . Nommez la page &quot;&quot;index. html.

![](tutorial-your-first-web-api/_static/image10.png)

Remplacez tout ce qui suit dans ce fichier par le code suivant :

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Il existe plusieurs façons d’obtenir jQuery. Dans cet exemple, j’ai utilisé le [CDN Microsoft Ajax](../../../ajax/cdn/overview.md). Vous pouvez également le télécharger à partir de [http://jquery.com/](http://jquery.com/), et le modèle de projet ASP.NET « API Web » comprend également jQuery.

### <a name="getting-a-list-of-products"></a>Obtention d’une liste de produits

Pour obtenir la liste des produits, envoyez une requête HTTP-r à &quot;&quot;/API/Products.

La fonction jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) envoie une requête Ajax. La réponse contient un tableau d’objets JSON. La fonction `done` spécifie un rappel qui est appelé si la demande réussit. Dans le rappel, nous mettons à jour le DOM avec les informations sur le produit.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Obtention d’un produit par ID

Pour obtenir un produit par ID, envoyez une requête HTTP-to à &quot;*ID* /API/Products/&quot;, où *ID* est l’ID du produit.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Nous appelons toujours `getJSON` pour envoyer la demande AJAX, mais cette fois-ci, nous plaçons l’ID dans l’URI de la demande. La réponse de cette demande est une représentation JSON d’un produit unique.

## <a name="running-the-application"></a>Exécution de l'application

Appuyez sur F5 pour lancer le débogage de l’application. La page Web doit ressembler à ce qui suit :

![](tutorial-your-first-web-api/_static/image11.png)

Pour obtenir un produit par ID, entrez l’ID, puis cliquez sur Rechercher :

![](tutorial-your-first-web-api/_static/image12.png)

Si vous entrez un ID non valide, le serveur renvoie une erreur HTTP :

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Utilisation de la touche F12 pour afficher la requête et la réponse HTTP

Lorsque vous utilisez un service HTTP, il peut être très utile de voir les messages de requête et de demande HTTP. Pour ce faire, vous pouvez utiliser les outils de développement F12 dans Internet Explorer 9. Dans Internet Explorer 9, appuyez sur **F12** pour ouvrir les outils. Cliquez sur l’onglet **réseau** , puis appuyez sur **Démarrer la capture**. Revenez maintenant à la page Web et appuyez sur **F5** pour recharger la page Web. Internet Explorer capture le trafic HTTP entre le navigateur et le serveur Web. La vue Résumé affiche tout le trafic réseau d’une page :

![](tutorial-your-first-web-api/_static/image14.png)

Recherchez l’entrée de l’URI relatif « API/Products/ ». Sélectionnez cette entrée, puis cliquez sur **accéder à la vue détaillée**. Dans la vue détaillée, il existe des onglets pour afficher les en-têtes et les corps de la demande et de la réponse. Par exemple, si vous cliquez sur l’onglet **en-têtes de demande** , vous pouvez voir que le client a demandé &quot;application/JSON&quot; dans l’en-tête Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Si vous cliquez sur l’onglet corps de la réponse, vous pouvez voir comment la liste de produits a été sérialisée au format JSON. D’autres navigateurs ont des fonctionnalités similaires. [Fiddler](http://www.fiddler2.com/fiddler2/)est un autre outil utile : le proxy de débogage Web. Vous pouvez utiliser Fiddler pour afficher votre trafic HTTP, ainsi que pour composer des requêtes HTTP, ce qui vous donne un contrôle total sur les en-têtes HTTP de la requête.

## <a name="see-this-app-running-on-azure"></a>Voir cette application en cours d’exécution sur Azure

Voulez-vous que le site terminé s’exécute en tant qu’application Web en direct ? Vous pouvez déployer une version complète de l’application sur votre compte Azure en cliquant simplement sur le bouton suivant.

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure. Si vous n’avez pas encore de compte, vous disposez des options suivantes :

- [Ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : vous recevez des crédits que vous pouvez utiliser pour tester les services Azure payants, et même après leur utilisation, vous pouvez conserver le compte et utiliser les services Azure gratuits.
- [Activer les avantages des abonnés MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.

## <a name="next-steps"></a>Étapes suivantes

- Pour obtenir un exemple plus complet d’un service HTTP qui prend en charge les actions de publication, PUT et suppression et écrit dans une base de données, consultez [utilisation de l’API Web 2 avec Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Pour plus d’informations sur la création d’applications Web fluides et réactives au-dessus d’un service HTTP, consultez [application à page unique ASP.net](../../../single-page-application/index.md).
- Pour plus d’informations sur le déploiement d’un projet Web Visual Studio sur Azure App Service, consultez [créer une application web ASP.net dans Azure App service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
