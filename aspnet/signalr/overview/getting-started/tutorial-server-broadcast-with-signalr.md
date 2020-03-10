---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Didacticiel : diffusion sur le serveur avec Signalr 2 | Microsoft Docs'
author: tdykstra
description: Ce didacticiel montre comment créer une application Web qui utilise ASP.NET Signalr 2 pour fournir une fonctionnalité de diffusion serveur.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536602"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Didacticiel : diffusion sur le serveur avec Signalr 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Ce didacticiel montre comment créer une application Web qui utilise ASP.NET Signalr 2 pour fournir une fonctionnalité de diffusion serveur. La diffusion serveur signifie que le serveur démarre les communications envoyées aux clients.

L’application que vous allez créer dans ce didacticiel simule un ticker boursier, un scénario classique de fonctionnalités de diffusion serveur. Régulièrement, le serveur met à jour de façon aléatoire les cotations boursières et diffuse les mises à jour à tous les clients connectés. Dans le navigateur, les nombres et les symboles de la **modification** et **%** colonnes changent de manière dynamique en réponse aux notifications du serveur. Si vous ouvrez des navigateurs supplémentaires sur la même URL, ils affichent tous les mêmes données et les mêmes modifications simultanément aux données.

![Créer un site Web](tutorial-server-broadcast-with-signalr/_static/image1.png)

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Créer le projet
> * Configurer le code du serveur
> * Examiner le code du serveur
> * Configurer le code client
> * Examiner le code client
> * Tester l’application
> * Activer la journalisation

> [!IMPORTANT]
> Si vous ne souhaitez pas suivre les étapes de création de l’application, vous pouvez installer le package Signalr. Sample dans un nouveau projet d’application Web ASP.NET vide. Si vous installez le package NuGet sans effectuer les étapes de ce didacticiel, vous devez suivre les instructions du fichier *Readme. txt* . Pour exécuter le package, vous devez ajouter une classe de démarrage OWIN qui appelle la méthode `ConfigureSignalR` dans le package installé. Vous recevrez une erreur si vous n’ajoutez pas la classe de démarrage OWIN. Consultez la section [installer l’exemple StockTicker](#install-the-stockticker-sample) de cet article.

## <a name="prerequisites"></a>Conditions préalables requises

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la charge de travail **Développement ASP.NET et web**.

## <a name="create-the-project"></a>Créer le projet

Cette section montre comment utiliser Visual Studio 2017 pour créer une application Web ASP.NET vide.

1. Dans Visual Studio, créez une application Web ASP.NET.

    ![Créer un site Web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. Dans la fenêtre **nouvelle application Web ASP.net-signalr. StockTicker** , laissez **vide** sélectionné et sélectionnez **OK**.

## <a name="set-up-the-server-code"></a>Configurer le code du serveur

Dans cette section, vous allez configurer le code qui s’exécute sur le serveur.

### <a name="create-the-stock-class"></a>Créer la classe stock

Commencez par créer la classe de modèle *stock* que vous utiliserez pour stocker et transmettre des informations sur une action.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **classe**.

1. Nommez la classe *stock* et ajoutez-la au projet.

1. Remplacez le code du fichier *stock.cs* par le code suivant :

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Les deux propriétés que vous allez définir lorsque vous créez des actions sont `Symbol` (par exemple, MSFT pour Microsoft) et `Price`. Les autres propriétés dépendent de la façon dont et quand vous définissez `Price`. La première fois que vous définissez `Price`, la valeur est propagée à `DayOpen`. Après cela, lorsque vous définissez `Price`, l’application calcule les valeurs de propriété `Change` et `PercentChange` en fonction de la différence entre `Price` et `DayOpen`.

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Créer les classes StockTickerHub et StockTicker

Vous allez utiliser l’API Hub Signalr pour gérer l’interaction de serveur à client. Une classe `StockTickerHub` qui dérive de la classe Signalr `Hub` va gérer la réception des connexions et des appels de méthode à partir des clients. Vous devez également gérer les données boursières et exécuter un objet `Timer`. L’objet `Timer` déclenche périodiquement des mises à jour de prix indépendamment des connexions clientes. Vous ne pouvez pas placer ces fonctions dans une classe `Hub`, car les hubs sont temporaires. L’application crée une instance de classe `Hub` pour chaque tâche sur le concentrateur, comme les connexions et les appels du client vers le serveur. Le mécanisme qui conserve les données boursières, met à jour les prix et diffuse les mises à jour de prix doit s’exécuter dans une classe distincte. Vous nommez la classe `StockTicker`.

![Diffusion à partir de StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Vous souhaitez qu’une seule instance de la classe `StockTicker` s’exécute sur le serveur. vous devez donc configurer une référence de chaque `StockTickerHub` instance sur l’instance de `StockTicker` Singleton. La classe `StockTicker` doit être diffusée aux clients, car elle contient les données boursières et déclenche les mises à jour, mais `StockTicker` n’est pas une classe `Hub`. La classe `StockTicker` doit obtenir une référence à l’objet de contexte de connexion du concentrateur Signalr. Il peut ensuite utiliser l’objet de contexte de connexion Signalr pour la diffusion vers les clients.

#### <a name="create-stocktickerhubcs"></a>Créer StockTickerHub.cs

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.

1. Dans **Ajouter un nouvel élément-signalr. StockTicker**, **Sélectionnez installé** > **Visual C#**  > **Web** > **signalr** , puis sélectionnez **classe de concentrateur signalr (v2)** .

1. Nommez la classe *StockTickerHub* et ajoutez-la au projet.

    Cette étape crée le fichier de classe *StockTickerHub.cs* . Simultanément, elle ajoute un ensemble de fichiers de script et de références d’assembly qui prend en charge Signalr au projet.

1. Remplacez le code du fichier *StockTickerHub.cs* par le code suivant :

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Enregistrez le fichier.

L’application utilise la classe de [concentrateur](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) pour définir les méthodes que les clients peuvent appeler sur le serveur. Vous définissez une méthode : `GetAllStocks()`. Lorsqu’un client se connecte initialement au serveur, il appelle cette méthode pour obtenir la liste de tous les stocks avec leurs prix actuels. La méthode peut s’exécuter de façon synchrone et retourner `IEnumerable<Stock>`, car elle retourne des données à partir de la mémoire.

Si la méthode devait obtenir les données en réalisant une opération impliquant l’attente, comme une recherche de base de données ou un appel de service Web, vous spécifiez `Task<IEnumerable<Stock>>` comme valeur de retour pour activer le traitement asynchrone. Pour plus d’informations, consultez Guide de l' [API hubs signalr ASP.net-serveur-quand exécuter de manière asynchrone](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

L’attribut `HubName` spécifie la manière dont l’application fait référence au hub dans le code JavaScript sur le client. Le nom par défaut sur le client si vous n’utilisez pas cet attribut, est une version la casse mixte du nom de la classe, qui dans ce cas est `stockTickerHub`.

Comme vous le verrez plus tard lorsque vous créerez la classe `StockTicker`, l’application crée une instance singleton de cette classe dans sa propriété statique `Instance`. Cette instance singleton de `StockTicker` est en mémoire, quel que soit le nombre de clients qui se connectent ou se déconnectent. Cette instance est celle utilisée par la méthode `GetAllStocks()` pour retourner les informations boursières actuelles.

#### <a name="create-stocktickercs"></a>Créer StockTicker.cs

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **classe**.

1. Nommez la classe *StockTicker* et ajoutez-la au projet.

1. Remplacez le code du fichier *stockticker.cs* par le code suivant :

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Étant donné que tous les threads exécuteront la même instance du code StockTicker, la classe StockTicker doit être thread-safe.

### <a name="examine-the-server-code"></a>Examiner le code du serveur

Si vous examinez le code du serveur, il vous aidera à comprendre le fonctionnement de l’application.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Stockage de l’instance singleton dans un champ statique

Le code initialise le champ `_instance` statique qui stocke la propriété `Instance` avec une instance de la classe. Étant donné que le constructeur est privé, il s’agit de la seule instance de la classe que l’application peut créer. L’application utilise l' [initialisation tardive](/dotnet/framework/performance/lazy-initialization) pour le champ `_instance`. Ce n’est pas pour des raisons de performances. Cela permet de s’assurer que la création de l’instance est thread-safe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Chaque fois qu’un client se connecte au serveur, une nouvelle instance de la classe StockTickerHub qui s’exécute dans un thread distinct obtient l’instance singleton de StockTicker à partir de la propriété statique `StockTicker.Instance`, comme vous l’avez vu précédemment dans la classe `StockTickerHub`.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Stockage des données boursières dans un ConcurrentDictionary

Le constructeur initialise la collection `_stocks` avec des exemples de données stock et `GetAllStocks` retourne les stocks. Comme vous l’avez vu précédemment, cette collection de stocks est retournée par `StockTickerHub.GetAllStocks`, qui est une méthode serveur dans la classe `Hub` que les clients peuvent appeler.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

La collection d’actions est définie en tant que type [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) pour la sécurité des threads. Vous pouvez également utiliser un objet [dictionnaire](https://msdn.microsoft.com/library/xfhwa508.aspx) et verrouiller explicitement le dictionnaire lorsque vous y apportez des modifications.

Pour cet exemple d’application, il est OK de stocker les données d’application en mémoire et de perdre les données quand l’application supprime l’instance de `StockTicker`. Dans une application réelle, vous travaillez avec un magasin de données principal comme une base de données.

#### <a name="periodically-updating-stock-prices"></a>Mise à jour périodique des cours boursiers

Le constructeur démarre un objet `Timer` qui appelle périodiquement des méthodes qui mettent à jour les cotations boursières de manière aléatoire.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` appelle `UpdateStockPrices`, qui passe la valeur null dans le paramètre d’État. Avant de mettre à jour les tarifs, l’application prend un verrou sur l’objet `_updateStockPricesLock`. Le code vérifie si un autre thread est déjà en cours de mise à jour des prix, puis appelle `TryUpdateStockPrice` sur chaque stock de la liste. La méthode `TryUpdateStockPrice` décide s’il faut modifier le cours de l’action et la quantité de modification. Si le cours de l’action change, l’application appelle `BroadcastStockPrice` pour diffuser la modification du cours de l’action à tous les clients connectés.

L’indicateur de `_updatingStockPrices` désigné comme [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) pour s’assurer qu’il est thread-safe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

Dans une application réelle, la méthode `TryUpdateStockPrice` appelle un service Web pour rechercher le prix. Dans ce code, l’application utilise un générateur de nombres aléatoires pour apporter des modifications de manière aléatoire.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obtention du contexte Signalr afin que la classe StockTicker puisse être diffusée aux clients

Étant donné que les changements de prix proviennent ici dans l’objet `StockTicker`, il s’agit de l’objet qui doit appeler une méthode `updateStockPrice` sur tous les clients connectés. Dans une classe `Hub`, vous avez une API pour appeler des méthodes clientes, mais `StockTicker` ne dérive pas de la classe `Hub` et n’a pas de référence à un objet `Hub`. Pour la diffusion vers des clients connectés, la classe `StockTicker` doit obtenir l’instance de contexte Signalr pour la classe `StockTickerHub` et l’utiliser pour appeler des méthodes sur les clients.

Le code obtient une référence au contexte de Signalr lorsqu’il crée l’instance de classe singleton, passe cette référence au constructeur, et le constructeur le place dans la propriété `Clients`.

Il existe deux raisons pour lesquelles vous ne souhaitez obtenir le contexte qu’une seule fois : l’obtention du contexte est une tâche coûteuse et l’obtention d’une fois garantit que l’application conserve l’ordre prévu des messages envoyés aux clients.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

L’obtention de la propriété `Clients` du contexte et son ajout dans la propriété `StockTickerClient` vous permet d’écrire du code pour appeler des méthodes clientes qui se présente comme dans une classe `Hub`. Par exemple, pour diffuser sur tous les clients, vous pouvez écrire des `Clients.All.updateStockPrice(stock)`.

La méthode `updateStockPrice` que vous appelez dans `BroadcastStockPrice` n’existe pas encore. Vous l’ajouterez ultérieurement lorsque vous écrirez du code qui s’exécute sur le client. Vous pouvez vous référer à `updateStockPrice` ici, car `Clients.All` est dynamique, ce qui signifie que l’application évalue l’expression au moment de l’exécution. Lorsque cet appel de méthode s’exécute, Signalr envoie le nom de la méthode et la valeur du paramètre au client, et si le client a une méthode nommée `updateStockPrice`, l’application appelle cette méthode et lui transmet la valeur de paramètre.

`Clients.All` signifie envoyer à tous les clients. Signalr vous offre d’autres options pour spécifier les clients ou groupes de clients à envoyer à. Pour plus d’informations, consultez [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Inscrire l’itinéraire Signalr

Le serveur doit connaître l’URL à intercepter et diriger vers Signalr. Pour ce faire, ajoutez une classe de démarrage OWIN :

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.

1. Dans **Ajouter un nouvel élément-signalr. StockTicker** , sélectionnez **installé** > **Visual C#**  > **Web** , puis sélectionnez **classe de démarrage OWIN**.

1. Nommez le *démarrage* de la classe, puis sélectionnez **OK**.

1. Remplacez le code par défaut dans le fichier *Startup.cs* par le code suivant :

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Vous avez maintenant terminé la configuration du code serveur. Dans la section suivante, vous allez configurer le client.

## <a name="set-up-the-client-code"></a>Configurer le code client

Dans cette section, vous allez configurer le code qui s’exécute sur le client.

### <a name="create-the-html-page-and-javascript-file"></a>Créer la page HTML et le fichier JavaScript

La page HTML affichera les données et le fichier JavaScript organisera les données.

#### <a name="create-stocktickerhtml"></a>Créer StockTicker. html

Tout d’abord, vous allez ajouter le client HTML.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **page HTML**.

1. Nommez le fichier *StockTicker* , puis sélectionnez **OK**.

1. Remplacez le code par défaut dans le fichier *stockticker. html* par le code suivant :

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    Le code HTML crée une table avec cinq colonnes, une ligne d’en-tête et une ligne de données avec une seule cellule qui s’étend sur les cinq colonnes. La ligne de données affiche « chargement en cours... » momentanément au démarrage de l’application. Le code JavaScript supprimera cette ligne et ajoutera à ses lignes de place les données de stock extraites du serveur.

    Les balises de script spécifient les éléments suivants :

    * Fichier de script jQuery.

    * Fichier de script du noyau Signalr.

    * Fichier de script des proxies Signalr.

    * Un fichier de script StockTicker que vous allez créer ultérieurement.

    L’application génère dynamiquement le fichier de script des proxies Signalr. Il spécifie l’URL « /signalr/hubs » et définit les méthodes proxy pour les méthodes sur la classe de concentrateur, dans ce cas, pour `StockTickerHub.GetAllStocks`. Si vous préférez, vous pouvez générer ce fichier JavaScript manuellement à l’aide des [utilitaires signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). N’oubliez pas de désactiver la création de fichiers dynamiques dans l’appel de méthode `MapHubs`.

1. Dans **Explorateur de solutions**, développez **scripts**.

    Les bibliothèques de scripts pour jQuery et Signalr sont visibles dans le projet.

    > [!IMPORTANT]
    > Le gestionnaire de package installe une version plus récente des scripts Signalr.

1. Mettez à jour les références de script dans le bloc de code afin qu’elles correspondent aux versions des fichiers de script dans le projet.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *stockticker. html*, puis sélectionnez **définir comme page de démarrage**.

#### <a name="create-stocktickerjs"></a>Créer StockTicker. js

Créez maintenant le fichier JavaScript.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **fichier JavaScript**.

1. Nommez le fichier *StockTicker* , puis sélectionnez **OK**.

1. Ajoutez ce code au fichier *stockticker. js* :

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Examiner le code client

Si vous examinez le code client, cela vous permettra d’apprendre comment le code client interagit avec le code serveur pour que l’application fonctionne.

#### <a name="starting-the-connection"></a>Démarrage de la connexion

`$.connection` fait référence aux proxies Signalr. Le code obtient une référence au proxy pour la classe `StockTickerHub` et le place dans la variable `ticker`. Le nom du proxy est le nom qui a été défini par l’attribut `HubName` :

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Une fois que vous avez défini toutes les variables et fonctions, la dernière ligne de code du fichier initialise la connexion Signalr en appelant la fonction Signalr `start`. La fonction `start` s’exécute de façon asynchrone et retourne un [objet différé jQuery](http://api.jquery.com/category/deferred-object/). Vous pouvez appeler la fonction Done pour spécifier la fonction à appeler lorsque l’application termine l’action asynchrone.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Obtention de tous les stocks

La fonction `init` appelle la fonction `getAllStocks` sur le serveur et utilise les informations renvoyées par le serveur pour mettre à jour la table stock. Notez que, par défaut, vous devez utiliser camelCasing sur le client, même si le nom de la méthode est en Pascal sur le serveur. La règle camelCasing s’applique uniquement aux méthodes, pas aux objets. Par exemple, vous faites référence à `stock.Symbol` et `stock.Price`, et non à `stock.symbol` ou `stock.price`.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

Dans la méthode `init`, l’application crée du code HTML pour une ligne de table pour chaque objet stock reçu du serveur en appelant `formatStock` pour mettre en forme les propriétés de l’objet `stock`, puis en appelant `supplant` pour remplacer les espaces réservés dans la variable `rowTemplate` par les valeurs de propriété de l’objet `stock`. Le code HTML qui en résulte est ensuite ajouté à la table stock.

> [!NOTE]
> Vous appelez `init` en le transmettant en tant que fonction `callback` qui s’exécute une fois la fonction de `start` asynchrone terminée. Si vous avez appelé `init` en tant qu’instruction JavaScript distincte après l’appel de `start`, la fonction échoue parce qu’elle s’exécute immédiatement sans attendre la fin de l’établissement de la connexion par la fonction Start. Dans ce cas, la fonction `init` essaiera d’appeler la fonction `getAllStocks` avant que l’application établisse une connexion au serveur.

#### <a name="getting-updated-stock-prices"></a>Obtention des cours mis à jour

Lorsque le serveur modifie le prix d’une action, il appelle la `updateStockPrice` sur les clients connectés. L’application ajoute la fonction à la propriété cliente du proxy `stockTicker` pour la rendre disponible aux appels à partir du serveur.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

La fonction `updateStockPrice` met en forme un objet stock reçu à partir du serveur dans une ligne de table de la même façon que dans la fonction `init`. Au lieu d’ajouter la ligne à la table, elle trouve la ligne active du stock dans la table et remplace cette ligne par la nouvelle.

## <a name="test-the-application"></a>Tester l’application

Vous pouvez tester l’application pour vous assurer qu’elle fonctionne correctement. Toutes les fenêtres de navigateur affichent la table des stocks en direct avec des fluctuations des cours.

1. Dans la barre d’outils, activez le **débogage de script** , puis sélectionnez le bouton de lecture pour exécuter l’application en mode débogage.

    ![Capture d’écran de l’activation du mode de débogage par l’utilisateur et de la sélection de lecture.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Une fenêtre de navigateur s’ouvre et affiche la **table des stocks en direct**. La table stock affiche initialement le chargement... puis, après une brève période, l’application affiche les données boursières initiales, puis les cours des actions commencent à changer.

1. Copiez l’URL à partir du navigateur, ouvrez deux autres navigateurs, puis collez les URL dans les barres d’adresses.

    L’affichage du stock initial est le même que celui du premier navigateur et les modifications se produisent simultanément.

1. Fermez tous les navigateurs, ouvrez un nouveau navigateur, puis accédez à la même URL.

    L’objet singleton de StockTicker a continué à s’exécuter sur le serveur. La **table** des stocks en direct indique que les stocks ont continué à changer. Vous ne voyez pas la table initiale avec des chiffres de modification zéro.

1. Fermez le navigateur.

## <a name="enable-logging"></a>Activer la journalisation

Signalr possède une fonction de journalisation intégrée que vous pouvez activer sur le client pour faciliter la résolution des problèmes. Dans cette section, vous allez activer la journalisation et voir des exemples montrant comment les journaux vous indiquent les méthodes de transport signalant les suivantes :

* [WebSockets](http://en.wikipedia.org/wiki/WebSocket), pris en charge par IIS 8 et les navigateurs actuels.

* [Événements envoyés par le serveur](http://en.wikipedia.org/wiki/Server-sent_events), pris en charge par les navigateurs autres qu’Internet Explorer.

* [Cadre illimité](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), pris en charge par Internet Explorer.

* [Interrogation longue Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), prise en charge par tous les navigateurs.

Pour une connexion donnée, Signalr choisit la meilleure méthode de transport que le serveur et le client prennent en charge.

1. Ouvrez *stockticker. js*.

1. Ajoutez cette ligne de code en surbrillance pour activer la journalisation juste avant le code qui initialise la connexion à la fin du fichier :

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Appuyez sur **F5** pour exécuter le projet.

1. Ouvrez la fenêtre outils de développement de votre navigateur, puis sélectionnez la console pour afficher les journaux. Vous devrez peut-être actualiser la page pour afficher les journaux de Signalr qui négocient la méthode de transport pour une nouvelle connexion.

    * Si vous exécutez Internet Explorer 10 sur Windows 8 (IIS 8), la méthode de transport est **WebSocket**.

    * Si vous exécutez Internet Explorer 10 sur Windows 7 (IIS 7,5), la méthode de transport est **IFRAME**.

    * Si vous exécutez Firefox 19 sur Windows 8 (IIS 8), la méthode de transport est **WebSocket**.

        > [!TIP]
        > Dans Firefox, installez le complément Firebug pour obtenir une fenêtre de console.

    * Si vous exécutez Firefox 19 sur Windows 7 (IIS 7,5), la méthode de transport est **un événement envoyé par le serveur** .

## <a name="install-the-stockticker-sample"></a>Installer l’exemple StockTicker

[Microsoft. Aspnet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installe l’application stockticker. Le package NuGet comprend plus de fonctionnalités que la version simplifiée que vous avez créée à partir de zéro. Dans cette section du didacticiel, vous installez le package NuGet et passez en revue les nouvelles fonctionnalités et le code qui les implémente.

> [!IMPORTANT]
> Si vous installez le package sans effectuer les étapes précédentes de ce didacticiel, vous devez ajouter une classe de démarrage OWIN à votre projet. Ce fichier Readme. txt pour le package NuGet explique cette étape.

### <a name="install-the-signalrsample-nuget-package"></a>Installez Signalr. exemple de package NuGet

1. Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Gérer les packages NuGet**.

1. Dans le **Gestionnaire de package NuGet : signalr. StockTicker**, sélectionnez **Parcourir**.

1. À partir de la **source du package**, sélectionnez **NuGet.org**.

1. Entrez *signalr. Sample* dans la zone de recherche et sélectionnez **Microsoft. Aspnet. signalr. Sample** > **installer**.

1. Dans **Explorateur de solutions**, développez le dossier *signalr. Sample* .

    Installation du package Signalr. l’exemple a créé le dossier et son contenu.

1. Dans le dossier *signalr. Sample* , cliquez avec le bouton droit sur *stockticker. html*, puis sélectionnez **définir comme page de démarrage**.

    > [!NOTE]
    > Installation de Signalr. l’exemple de package NuGet peut modifier la version de jQuery que vous avez dans votre dossier *scripts* . Le nouveau fichier *stockticker. html* que le package installe dans le dossier *signalr. Sample* sera synchronisé avec la version jQuery installée par le package, mais si vous souhaitez réexécuter votre fichier *stockticker. html* d’origine, vous devrez peut-être mettre à jour la référence jQuery dans la balise de script en premier.

### <a name="run-the-application"></a>Exécuter l'application

 La table que vous avez vues dans la première application avait des fonctionnalités utiles. L’application Full stock ticker affiche de nouvelles fonctionnalités : une fenêtre de défilement horizontal qui affiche les données boursières et les stocks qui changent de couleur à mesure qu’ils s’élèvent et chutent.

1. Appuyez sur **F5** pour exécuter l'application.

     Lorsque vous exécutez l’application pour la première fois, le « marché » est « fermé » et vous voyez une table statique et une fenêtre de téléscripteur qui ne défile pas.

1. Sélectionnez **Open Market**.

    ![Capture d’écran du téléscripteur en direct.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * La zone de cotations **boursières dynamiques** commence à faire défiler horizontalement et le serveur commence à diffuser régulièrement les modifications du cours de l’action sur une base aléatoire.

    * Chaque fois qu’un cours boursier change, l’application met à jour la **table des stocks en direct** et le **téléscripteur en direct**.

    * Lorsque le changement de prix d’une action est positif, l’application affiche l’action avec un arrière-plan vert.

    * Lorsque la modification est négative, l’application affiche le brut avec un arrière-plan rouge.

1. Sélectionnez **Fermer le marché**.

    * La table met à jour stop.

    * Le téléscripteur cesse de faire défiler.

1. Sélectionnez **Réinitialiser**.

    * Toutes les données boursières sont réinitialisées.

    * L’application restaure l’état initial avant le début des changements de prix.

1. Copiez l’URL à partir du navigateur, ouvrez deux autres navigateurs, puis collez les URL dans les barres d’adresses.

1. Les mêmes données sont mises à jour de manière dynamique en même temps dans chaque navigateur.

1. Lorsque vous sélectionnez l’un des contrôles, tous les navigateurs répondent de la même manière en même temps.

### <a name="live-stock-ticker-display"></a>Affichage de cotations boursières en direct

L’affichage de **cotations boursières en direct** est une liste non triée dans un élément `<div>` mis en forme en une seule ligne par les styles CSS. L’application initialise et met à jour le téléscripteur de la même façon que la table : en remplaçant les espaces réservés dans une chaîne de modèle `<li>` et en ajoutant dynamiquement les éléments `<li>` à l’élément `<ul>`. L’application comprend le défilement à l’aide de la fonction jQuery `animate` pour faire varier la marge à gauche de la liste non triée dans le `<div>`.

#### <a name="signalrsample-stocktickerhtml"></a>Signalr. exemple de StockTicker. html

Code HTML du téléscripteur de stock :

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>Signalr. exemple de StockTicker. CSS

Code CSS du ticker de stock :

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>Signalr. exemple Signalr. StockTicker. js

Code jQuery qui le fait défiler :

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Méthodes supplémentaires sur le serveur que le client peut appeler

Pour ajouter de la flexibilité à l’application, il existe des méthodes supplémentaires que l’application peut appeler.

#### <a name="signalrsample-stocktickerhubcs"></a>Signalr. exemple StockTickerHub.cs

La classe `StockTickerHub` définit quatre méthodes supplémentaires que le client peut appeler :

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

L’application appelle `OpenMarket`, `CloseMarket`et `Reset` en réponse aux boutons situés en haut de la page. Elles illustrent le modèle d’un client déclenchant une modification de l’État immédiatement propagé à tous les clients. Chacune de ces méthodes appelle une méthode dans la classe `StockTicker` qui provoque la modification de l’état du marché, puis diffuse le nouvel État.

#### <a name="signalrsample-stocktickercs"></a>Signalr. exemple StockTicker.cs

Dans la classe `StockTicker`, l’application maintient l’état du marché avec une propriété `MarketState` qui retourne une valeur d’énumération `MarketState` :

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Chacune des méthodes qui modifient l’état du marché le fait à l’intérieur d’un bloc de verrouillage, car la classe `StockTicker` doit être thread-safe :

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Pour vous assurer que ce code est thread-safe, le champ `_marketState` qui stocke la propriété `MarketState` désignée `volatile`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Les méthodes `BroadcastMarketStateChange` et `BroadcastMarketReset` sont similaires à la méthode BroadcastStockPrice que vous avez déjà vue, à ceci près qu’elles appellent différentes méthodes définies sur le client :

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Fonctions supplémentaires sur le client que le serveur peut appeler

La fonction `updateStockPrice` gère désormais à la fois la table et l’affichage du téléscripteur, et elle utilise `jQuery.Color` pour flasher les couleurs rouges et vertes.

Nouvelles fonctions dans *signalr. stockticker. js* activent et désactivent les boutons en fonction de l’état du marché. Ils arrêtent ou démarrent également le défilement horizontal des cotations **boursières dynamiques** . Étant donné que de nombreuses fonctions sont ajoutées à `ticker.client`, l’application utilise la [fonction d’extension jQuery](http://api.jquery.com/jQuery.extend/) pour les ajouter.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Configuration supplémentaire du client après l’établissement de la connexion

Une fois que le client a établi la connexion, il a d’autres tâches à effectuer :

* Déterminez si le marché est ouvert ou fermé pour appeler la fonction de `marketOpened` ou de `marketClosed` appropriée.

* Attachez les appels de méthode serveur aux boutons.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Les méthodes de serveur ne sont pas raccordées aux boutons tant que l’application n’a pas établi la connexion. C’est pourquoi le code ne peut pas appeler les méthodes du serveur avant qu’elles ne soient disponibles.

## <a name="additional-resources"></a>Ressources supplémentaires

Dans ce didacticiel, vous avez appris à programmer une application Signalr qui diffuse les messages du serveur à tous les clients connectés. Vous pouvez désormais diffuser des messages régulièrement et en réponse aux notifications de n’importe quel client. Vous pouvez utiliser le concept d’instance de Singleton multithread pour maintenir l’état du serveur dans les scénarios de jeux en ligne multi-joueurs. Pour obtenir un exemple, consultez [le jeu de pousseurs basé sur signalr](https://github.com/NTaylorMullen/ShootR).

Pour obtenir des didacticiels montrant les scénarios de communication d’égal à égal, consultez [prise en main avec signalr](introduction-to-signalr.md) et [mise à jour en temps réel avec signalr](tutorial-high-frequency-realtime-with-signalr.md).

Pour plus d’informations sur Signalr, consultez les ressources suivantes :

* [ASP.NET SignalR](../../index.md)
* [Projet signalr](http://signalr.net/)
* [Signalr GitHub et exemples](https://github.com/SignalR/SignalR)
* [Wiki signalr](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Création du projet
> * Configurer le code du serveur
> * Examen du code du serveur
> * Configurer le code client
> * Examen du code client
> * Test de l’application
> * Journalisation activée

Passez à l’article suivant pour apprendre à créer une application Web en temps réel qui utilise ASP.NET Signalr 2.
> [!div class="nextstepaction"]
> [Créer une application Web en temps réel avec Signalr](real-time-web-applications-with-signalr.md)
