---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 'Didacticiel : diffusion sur le serveur avec ASP.NET Signalr 1. x | Microsoft Docs'
author: bradygaster
description: Ce didacticiel montre comment créer une application Web qui utilise ASP.NET Signalr pour fournir une fonctionnalité de diffusion serveur. La diffusion serveur signifie que la commune...
ms.author: bradyg
ms.date: 04/10/2013
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 68908be34f6b010e512677fe5f5e31bfdefab592
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579407"
---
# <a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a>Didacticiel : diffusion sur le serveur avec ASP.NET Signalr 1. x

de [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce didacticiel montre comment créer une application Web qui utilise ASP.NET Signalr pour fournir une fonctionnalité de diffusion serveur. La diffusion serveur signifie que les communications envoyées aux clients sont lancées par le serveur. Ce scénario nécessite une approche de programmation différente des scénarios d’égal à égal, tels que les applications de conversation, dans lesquelles les communications envoyées aux clients sont lancées par un ou plusieurs clients.
> 
> L’application que vous allez créer dans ce didacticiel simule un ticker boursier, un scénario classique de fonctionnalités de diffusion serveur.
> 
> Les commentaires sur ce didacticiel sont les bienvenus. Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com).

## <a name="overview"></a>Présentation

Le package NuGet [Microsoft. Aspnet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installe un exemple d’application de téléscripteur de cotations boursières simulé dans un projet Visual Studio. Dans la première partie de ce didacticiel, vous allez créer une version simplifiée de cette application à partir de zéro. Dans le reste du didacticiel, vous allez installer le package NuGet et passer en revue les fonctionnalités et le code supplémentaires qu’il crée.

L’application de cotations boursières est représentative d’une application en temps réel dans laquelle vous souhaitez régulièrement « pousser » ou diffuser des notifications du serveur vers tous les clients connectés.

L’application que vous allez créer dans la première partie de ce didacticiel affiche une grille contenant les données boursières.

![Version initiale de StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

Périodiquement, le serveur met à jour de façon aléatoire les cotations boursières et transmet les mises à jour à tous les clients connectés. Dans le navigateur, les nombres et les symboles de la **modification** et **%** colonnes changent de manière dynamique en réponse aux notifications du serveur. Si vous ouvrez des navigateurs supplémentaires sur la même URL, ils affichent tous les mêmes données et les mêmes modifications simultanément aux données.

Ce didacticiel contient les sections suivantes :

- [Conditions préalables](#prerequisites)
- [Création du projet](#createproject)
- [Ajouter les packages NuGet Signalr](#nugetpackages)
- [Configurer le code serveur](#server)
- [Configurer le code client](#client)
- [Testez l’application](#test)
- [Activer la journalisation](#enablelogging)
- [Installer et examiner l’exemple de StockTicker complet](#fullsample)
- [Étapes suivantes](#nextsteps)

> [!NOTE]
> Si vous ne souhaitez pas suivre les étapes de création de l’application, vous pouvez installer le package Signalr. Sample dans un nouveau projet d' **application Web ASP.net vide** et lire ces étapes pour obtenir des explications sur le code. La première partie du didacticiel couvre un sous-ensemble de Signalr. exemple de code, et la deuxième partie explique les fonctionnalités clés des fonctionnalités supplémentaires de Signalr. exemple de package.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Conditions préalables requises

Avant de commencer, assurez-vous que Visual Studio 2012 ou 2010 SP1 est installé sur votre ordinateur. Si vous ne disposez pas de Visual Studio, consultez [ASP.net downloads](https://www.asp.net/downloads) pour obtenir gratuitement Visual Studio 2012 Express pour le Web.

Si vous disposez de Visual Studio 2010, assurez-vous que [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) est installé.

<a id="createproject"></a>

## <a name="create-the-project"></a>Créer le projet

1. Dans le menu **Fichier**, cliquez sur **Nouveau projet**.
2. Dans la boîte de dialogue **nouveau projet** , **C#** développez sous **modèles** , puis sélectionnez **Web**.
3. Sélectionnez le modèle **application Web vide ASP.net** , nommez le projet *signalr. StockTicker*, puis cliquez sur **OK**.

    ![Boîte de dialogue Nouveau projet de test](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a>Ajouter les packages NuGet Signalr

### <a name="add-the-signalr-and-jquery-nuget-packages"></a>Ajouter les packages Signalr et JQuery NuGet

Vous pouvez ajouter une fonctionnalité Signalr à un projet en installant un package NuGet.

1. Cliquez sur **Outils | Gestionnaire de package NuGet | Console du gestionnaire de package**.
2. Dans le gestionnaire de package, entrez la commande suivante.

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    Le package Signalr installe un certain nombre d’autres packages NuGet en tant que dépendances. Une fois l’installation terminée, vous disposez de tous les composants serveur et client requis pour utiliser Signalr dans une application ASP.NET.

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Configurer le code du serveur

Dans cette section, vous configurez le code qui s’exécute sur le serveur.

### <a name="create-the-stock-class"></a>Créer la classe stock

Commencez par créer la classe de modèle stock que vous utiliserez pour stocker et transmettre des informations sur une action.

1. Créez un nouveau fichier de classe dans le dossier du projet, nommez-le *stock.cs*, puis remplacez le code du modèle par le code suivant :

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    Les deux propriétés que vous allez définir lorsque vous créez des actions sont le symbole (par exemple, MSFT pour Microsoft) et le prix. Les autres propriétés dépendent de la façon dont et quand vous définissez Price. La première fois que vous définissez Price, la valeur est propagée à DayOpen. Quand vous définissez Price, les valeurs de propriété change et PercentChange sont calculées en fonction de la différence entre Price et DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Créer les classes StockTicker et StockTickerHub

Vous allez utiliser l’API Hub Signalr pour gérer l’interaction de serveur à client. Une classe StockTickerHub qui dérive de la classe Hub Signalr va gérer la réception des connexions et des appels de méthode à partir des clients. Vous devez également mettre à jour les données boursières et exécuter un objet minuteur pour déclencher régulièrement des mises à jour de prix, indépendamment des connexions clientes. Vous ne pouvez pas placer ces fonctions dans une classe de concentrateur, car les instances de concentrateur sont temporaires. Une instance de classe de concentrateur est créée pour chaque opération sur le concentrateur, comme les connexions et les appels du client au serveur. Par conséquent, le mécanisme qui conserve les données boursières, met à jour les prix et diffuse les mises à jour de prix doit s’exécuter dans une classe distincte, que vous nommerez StockTicker.

![Diffusion à partir de StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

Vous souhaitez qu’une seule instance de la classe StockTicker s’exécute sur le serveur. vous devez donc configurer une référence de chaque instance StockTickerHub à l’instance StockTicker Singleton. La classe StockTicker doit pouvoir être diffusée aux clients, car elle contient les données boursières et déclenche les mises à jour, mais StockTicker n’est pas une classe de concentrateur. Par conséquent, la classe StockTicker doit obtenir une référence à l’objet de contexte de connexion du concentrateur Signalr. Il peut ensuite utiliser l’objet de contexte de connexion Signalr pour la diffusion vers les clients.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis cliquez sur **Ajouter un nouvel élément**.
2. Si vous avez Visual Studio 2012 avec la [mise à jour ASP.net et Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941), cliquez sur **Web** sous **Visual C#**  et sélectionnez le modèle d’élément de **classe du concentrateur signalr** . Dans le cas contraire, sélectionnez le modèle de **classe** .
3. Nommez la nouvelle classe *StockTickerHub.cs*, puis cliquez sur **Ajouter**.

    ![Ajouter StockTickerHub.cs](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. Remplacez le code du modèle par le code suivant :

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    La classe de [concentrateur](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) est utilisée pour définir les méthodes que les clients peuvent appeler sur le serveur. Vous définissez une méthode : `GetAllStocks()`. Lorsqu’un client se connecte initialement au serveur, il appelle cette méthode pour obtenir la liste de tous les stocks avec leurs prix actuels. La méthode peut s’exécuter de façon synchrone et retourner `IEnumerable<Stock>`, car elle retourne des données à partir de la mémoire. Si la méthode devait obtenir les données en réalisant une opération impliquant l’attente, par exemple une recherche de base de données ou un appel de service Web, vous devez spécifier `Task<IEnumerable<Stock>>` comme valeur de retour pour activer le traitement asynchrone. Pour plus d’informations, consultez Guide de l' [API hubs signalr ASP.net-serveur-quand exécuter de manière asynchrone](index.md).

    L’attribut HubName spécifie la manière dont le concentrateur sera référencé dans le code JavaScript sur le client. Le nom par défaut sur le client si vous n’utilisez pas cet attribut est une version en casse mixte du nom de la classe, qui est dans ce cas stockTickerHub.

    Comme vous le verrez plus tard lorsque vous créerez la classe StockTicker, une instance singleton de cette classe est créée dans sa propriété d’instance statique. Cette instance singleton de StockTicker reste en mémoire, quel que soit le nombre de clients connectés ou déconnectés, et cette instance est celle utilisée par la méthode GetAllStocks pour retourner les informations boursières actuelles.
5. Créez un nouveau fichier de classe dans le dossier du projet, nommez-le *stockticker.cs*, puis remplacez le code du modèle par le code suivant :

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    Étant donné que plusieurs threads exécuteront la même instance du code StockTicker, la classe StockTicker doit être threadsafe.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Stockage de l’instance singleton dans un champ statique

    Le code initialise le champ d’instance statique \_qui stocke la propriété d’instance avec une instance de la classe, et il s’agit de la seule instance de la classe qui peut être créée, car le constructeur est marqué comme privé. L' [initialisation tardive](https://msdn.microsoft.com/library/dd997286.aspx) est utilisée pour le champ d’instance \_, pas pour des raisons de performances, mais pour garantir que la création de l’instance est threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    Chaque fois qu’un client se connecte au serveur, une nouvelle instance de la classe StockTickerHub qui s’exécute dans un thread distinct obtient l’instance singleton de StockTicker à partir de la propriété statique de StockTicker. instance, comme vous l’avez vu précédemment dans la classe StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Stockage des données boursières dans un ConcurrentDictionary

    Le constructeur initialise la collection \_stocks avec des exemples de données stock et GetAllStocks retourne les actions. Comme vous l’avez vu précédemment, cette collection de stocks est à son tour retournée par StockTickerHub. GetAllStocks, qui est une méthode serveur dans la classe de concentrateur que les clients peuvent appeler.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    La collection d’actions est définie en tant que type [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) pour la sécurité des threads. Vous pouvez également utiliser un objet [dictionnaire](https://msdn.microsoft.com/library/xfhwa508.aspx) et verrouiller explicitement le dictionnaire lorsque vous y apportez des modifications.

    Pour cet exemple d’application, il est OK de stocker les données d’application en mémoire et de perdre les données lorsque l’instance de StockTicker est supprimée. Dans une application réelle, vous travaillez avec un magasin de données principal tel qu’une base de données.

    ### <a name="periodically-updating-stock-prices"></a>Mise à jour périodique des cours boursiers

    Le constructeur démarre un objet minuteur qui appelle périodiquement des méthodes qui mettent à jour les cotations boursières de manière aléatoire.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    UpdateStockPrices est appelé par le minuteur, qui passe la valeur null dans le paramètre d’État. Avant la mise à jour des tarifs, un verrou est pris sur l’objet \_updateStockPricesLock. Le code vérifie si un autre thread est déjà en cours de mise à jour des prix, puis appelle TryUpdateStockPrice sur chaque stock de la liste. La méthode TryUpdateStockPrice décide s’il faut modifier le cours de l’action et la quantité de modification. Si le cours de la bourse est modifié, BroadcastStockPrice est appelé pour diffuser la modification du cours de l’action à tous les clients connectés.

    L’indicateur \_updatingStockPrices est marqué comme [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) pour s’assurer que son accès est threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    Dans une application réelle, la méthode TryUpdateStockPrice appelle un service Web pour rechercher le prix. dans ce code, il utilise un générateur de nombres aléatoires pour apporter des modifications de manière aléatoire.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obtention du contexte Signalr afin que la classe StockTicker puisse être diffusée aux clients

    Étant donné que les changements de prix proviennent ici dans l’objet StockTicker, il s’agit de l’objet qui doit appeler une méthode updateStockPrice sur tous les clients connectés. Dans une classe de concentrateur, vous avez une API pour appeler des méthodes clientes, mais StockTicker ne dérive pas de la classe Hub et n’a pas de référence à un objet Hub. Par conséquent, pour la diffusion vers les clients connectés, la classe StockTicker doit obtenir l’instance de contexte Signalr pour la classe StockTickerHub et l’utiliser pour appeler des méthodes sur les clients.

    Le code obtient une référence au contexte Signalr lorsqu’il crée l’instance de classe singleton, passe cette référence au constructeur, et le constructeur le place dans la propriété clients.

    Il existe deux raisons pour lesquelles vous souhaitez obtenir le contexte une seule fois : l’obtention du contexte est une opération coûteuse et l’obtention d’une fois pour s’assurer que l’ordre prévu des messages envoyés aux clients est préservé.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    L’obtention de la propriété clients du contexte et sa mise dans la propriété StockTickerClient vous permet d’écrire du code pour appeler des méthodes clientes qui se présente comme dans une classe de concentrateur. Par exemple, pour diffuser sur tous les clients, vous pouvez écrire des clients. All. updateStockPrice (stock).

    La méthode updateStockPrice que vous appelez dans BroadcastStockPrice n’existe pas encore ; vous l’ajouterez ultérieurement lorsque vous écrirez du code qui s’exécute sur le client. Vous pouvez vous référer à updateStockPrice ici, car clients. All est dynamique, ce qui signifie que l’expression sera évaluée au moment de l’exécution. Lorsque cet appel de méthode s’exécute, Signalr envoie le nom de la méthode et la valeur du paramètre au client, et si le client a une méthode nommée updateStockPrice, cette méthode est appelée et la valeur du paramètre lui est transmise.

    Clients. All signifie envoyer à tous les clients. Signalr vous offre d’autres options pour spécifier les clients ou groupes de clients à envoyer à. Pour plus d’informations, consultez [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Inscrire l’itinéraire Signalr

Le serveur doit connaître l’URL à intercepter et diriger vers Signalr. Pour ce faire, vous allez ajouter du code au fichier *global. asax* .

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis cliquez sur **Ajouter un nouvel élément**.
2. Sélectionnez le modèle d’élément de **classe d’application globale** , puis cliquez sur **Ajouter**.

    ![Ajouter global. asax](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. Ajoutez le code d’inscription de l’itinéraire Signalr à l’application\_méthode Start :

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    Par défaut, l’URL de base pour tout le trafic Signalr est « /signalr » et « /signalr/hubs » est utilisé pour récupérer un fichier JavaScript généré dynamiquement qui définit des proxies pour tous les concentrateurs que vous avez dans votre application. La méthode MapHubs comprend des surcharges qui vous permettent de spécifier une URL de base différente et certaines options Signalr dans une instance de la classe [HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx) .
4. Ajoutez une instruction using en haut du fichier :

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. Enregistrez et fermez le fichier *global. asax* , puis générez le projet.

Vous avez maintenant terminé la configuration du code serveur. Dans la section suivante, vous allez configurer le client.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Configurer le code client

1. Créez un nouveau fichier HTML dans le dossier du projet, puis nommez-le *stockticker. html*.
2. Remplacez le code du modèle par le code suivant :

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    Le code HTML crée une table avec 5 colonnes, une ligne d’en-tête et une ligne de données avec une seule cellule qui s’étend sur cinq colonnes. La ligne de données affiche « chargement en cours... » et s’affichent momentanément uniquement au démarrage de l’application. Le code JavaScript supprimera cette ligne et ajoutera à ses lignes de place les données de stock extraites du serveur.

    Les balises de script spécifient le fichier de script jQuery, le fichier de script du noyau Signalr, le fichier de script des proxies de Signalr et un fichier de script StockTicker que vous allez créer ultérieurement. Le fichier de script des proxies Signalr, qui spécifie l’URL « /signalr/hubs », est généré dynamiquement et définit des méthodes de proxy pour les méthodes sur la classe de concentrateur, dans ce cas pour StockTickerHub. GetAllStocks. Si vous préférez, vous pouvez générer ce fichier JavaScript manuellement à l’aide des [utilitaires signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) et désactiver la création de fichiers dynamiques dans l’appel de la méthode MapHubs.
3. > [!IMPORTANT]
   > Assurez-vous que les références de fichier JavaScript dans *stockticker. html* sont correctes. Autrement dit, assurez-vous que la version jQuery dans votre balise de script (1.8.2 dans l’exemple) est identique à la version jQuery dans le dossier *scripts* de votre projet, et assurez-vous que la version de signalr dans votre balise de script est la même que la version de signalr dans le dossier *scripts* de votre projet. Modifiez les noms de fichiers dans les balises de script, si nécessaire.
4. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *stockticker. html*, puis cliquez sur **définir comme page de démarrage**.
5. Créez un nouveau fichier JavaScript dans le dossier du projet et nommez-le *stockticker. js*.
6. Remplacez le code du modèle par le code suivant :

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    $. Connection fait référence aux proxies Signalr. Le code obtient une référence au proxy pour la classe StockTickerHub et le place dans la variable du téléscripteur. Le nom du proxy est le nom qui a été défini par l’attribut [HubName] :

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    Une fois que toutes les variables et fonctions sont définies, la dernière ligne de code du fichier initialise la connexion Signalr en appelant la fonction de démarrage Signalr. La fonction Start s’exécute de façon asynchrone et retourne un [objet différé jQuery](http://api.jquery.com/category/deferred-object/), ce qui signifie que vous pouvez appeler la fonction Done pour spécifier la fonction à appeler lorsque l’opération asynchrone est terminée.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    La fonction init appelle la fonction getAllStocks sur le serveur et utilise les informations renvoyées par le serveur pour mettre à jour la table stock. Notez que, par défaut, vous devez utiliser la casse mixte sur le client bien que le nom de la méthode soit en respectant la casse sur le serveur. La règle de casse mixte s’applique uniquement aux méthodes, pas aux objets. Par exemple, vous faites référence à stock. Symbole et stock. Prix, non stock. Symbol ou stock. Price.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    Si vous souhaitez utiliser la casse Pascal sur le client, ou si vous souhaitez utiliser un nom de méthode complètement différent, vous pouvez décorer la méthode de concentrateur avec l’attribut HubMethodName de la même façon que vous avez décoré la classe de concentrateur proprement dite avec l’attribut HubName.

    Dans la méthode Init, le code HTML d’une ligne de table est créé pour chaque objet stock reçu du serveur en appelant formatStock pour mettre en forme les propriétés de l’objet stock, puis en appelant la méthode supplanté (qui est définie en haut de *stockticker. js*) pour remplacer les espaces réservés dans la variable rowTemplate par les valeurs de propriété de l’objet stock. Le code HTML qui en résulte est ensuite ajouté à la table stock.

    Vous appelez init en le transmettant en tant que fonction de rappel qui s’exécute une fois la fonction Start asynchrone terminée. Si vous avez appelé init en tant qu’instruction JavaScript distincte après avoir appelé Start, la fonction échoue, car elle s’exécute immédiatement sans attendre la fin de l’établissement de la connexion par la fonction Start. Dans ce cas, la fonction init tente d’appeler la fonction getAllStocks avant l’établissement de la connexion au serveur.

    Lorsque le serveur modifie le prix d’une action, il appelle le updateStockPrice sur les clients connectés. La fonction est ajoutée à la propriété client du proxy stockTicker afin de la rendre disponible aux appels à partir du serveur.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    La fonction updateStockPrice met en forme un objet stock reçu à partir du serveur dans une ligne de table de la même façon que dans la fonction init. Toutefois, au lieu d’ajouter la ligne à la table, elle trouve la ligne active du stock dans la table et remplace cette ligne par la nouvelle.

<a id="test"></a>

## <a name="test-the-application"></a>Tester l’application

1. Appuyez sur F5 pour exécuter l'application en mode débogage.

    La table stock affiche initialement le chargement... puis, après un court délai, les données boursières initiales sont affichées, puis les cours des actions commencent à changer.

    ![Chargement en cours](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![Table de stock initiale](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![Table stock recevant les modifications du serveur](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. Copiez l’URL à partir de la barre d’adresses du navigateur et collez-la dans une ou plusieurs nouvelles fenêtres de navigateur.

    L’affichage du stock initial est le même que celui du premier navigateur et les modifications se produisent simultanément.
3. Fermez tous les navigateurs et ouvrez un nouveau navigateur, puis accédez à la même URL.

    L’objet singleton de StockTicker a continué à s’exécuter sur le serveur. par conséquent, l’affichage de la table stock indique que les stocks ont continué à changer. (Vous ne voyez pas la table initiale avec des chiffres de modification zéro.)
4. Fermez le navigateur.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Activer la journalisation

Signalr possède une fonction de journalisation intégrée que vous pouvez activer sur le client pour faciliter la résolution des problèmes. Dans cette section, vous activez la journalisation et consultez des exemples qui montrent comment les journaux vous indiquent les méthodes de transport signalant les suivantes :

- [WebSockets](http://en.wikipedia.org/wiki/WebSocket), pris en charge par IIS 8 et les navigateurs actuels.
- [Événements envoyés par le serveur](http://en.wikipedia.org/wiki/Server-sent_events), pris en charge par les navigateurs autres qu’Internet Explorer.
- [Cadre illimité](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), pris en charge par Internet Explorer.
- [Interrogation longue Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), prise en charge par tous les navigateurs.

Pour une connexion donnée, Signalr choisit la meilleure méthode de transport que le serveur et le client prennent en charge.

1. Ouvrez *stockticker. js* et ajoutez une ligne de code pour activer la journalisation immédiatement avant le code qui initialise la connexion à la fin du fichier :

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. Appuyez sur F5 pour exécuter le projet.
3. Ouvrez la fenêtre outils de développement de votre navigateur, puis sélectionnez la console pour afficher les journaux. Vous devrez peut-être actualiser la page pour afficher les journaux de Signalr qui négocient la méthode de transport pour une nouvelle connexion.

    Si vous exécutez Internet Explorer 10 sur Windows 8 (IIS 8), la méthode de transport est WebSocket.

    ![Console Internet Explorer 10 IIS 8](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    Si vous exécutez Internet Explorer 10 sur Windows 7 (IIS 7,5), la méthode de transport est IFRAME.

    ![Console IE 10, IIS 7,5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    Dans Firefox, installez le complément Firebug pour obtenir une fenêtre de console. Si vous exécutez Firefox 19 sur Windows 8 (IIS 8), la méthode de transport est WebSocket.

    ![Firefox 19, WebSockets IIS 8](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    Si vous exécutez Firefox 19 sur Windows 7 (IIS 7,5), la méthode de transport est un événement envoyé par le serveur.

    ![Firefox 19 console IIS 7,5](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Installer et examiner l’exemple de StockTicker complet

L’application StockTicker qui est installée par le package NuGet [Microsoft. Aspnet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) contient plus de fonctionnalités que la version simplifiée que vous venez de créer à partir de zéro. Dans cette section du didacticiel, vous installez le package NuGet et passez en revue les nouvelles fonctionnalités et le code qui les implémente.

### <a name="install-the-signalrsample-nuget-package"></a>Installez Signalr. exemple de package NuGet

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis cliquez sur **gérer les packages NuGet**.
2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **en ligne**, entrez *signalr. Sample* dans la zone **Rechercher en ligne** , puis cliquez sur **installer** dans l’exemple de package **signalr.**

    ![Installer Signalr. exemple de package](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. Dans le fichier *global. asax* , commentez RouteTable. routes. MapHubs (); la ligne que vous avez ajoutée précédemment dans l’application\_méthode Start.

    Le code dans *global. asax* n’est plus nécessaire, car le package signalr. Sample enregistre l’itinéraire signalr dans le fichier *app\_Start/RegisterHubs. cs* :

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    La classe webactivateur référencée par l’attribut assembly est incluse dans le package NuGet WebActivatorEx, qui est installé en tant que dépendance de l’exemple de package Signalr.
4. Dans **Explorateur de solutions**, développez le dossier *signalr. Sample* qui a été créé en installant l’exemple de package signalr.
5. Dans le dossier *signalr. Sample* , cliquez avec le bouton droit sur *stockticker. html*, puis cliquez sur **définir comme page de démarrage**.

    > [!NOTE]
    > Installation de Signalr. l’exemple de package NuGet peut modifier la version de jQuery que vous avez dans votre dossier *scripts* . Le nouveau fichier *stockticker. html* que le package installe dans le dossier *signalr. Sample* sera synchronisé avec la version jQuery installée par le package, mais si vous souhaitez réexécuter votre fichier *stockticker. html* d’origine, vous devrez peut-être mettre à jour la référence jQuery dans la balise de script en premier.

### <a name="run-the-application"></a>Exécuter l'application

1. Appuyez sur F5 pour exécuter l'application.

    En plus de la grille que vous avez vue précédemment, l’application de cotations boursières complètes affiche une fenêtre de défilement horizontal qui affiche les mêmes données boursières. Lorsque vous exécutez l’application pour la première fois, le « marché » est « fermé » et vous voyez une grille statique et une fenêtre de téléscripteur qui ne défile pas.

    ![Démarrage de l’écran StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    Lorsque vous cliquez sur **ouvrir le marché**, la zone de cotations **boursières dynamiques** commence à faire défiler horizontalement et le serveur commence à diffuser régulièrement les modifications du cours de l’action sur une base aléatoire. Chaque fois qu’un cours boursier change, la grille de la **table des stocks** en direct et la zone de cotations **boursières en direct** sont mises à jour. Lorsque le changement de prix d’une action est positif, le brut s’affiche avec un arrière-plan vert et, lorsque la modification est négative, l’action est représentée par un arrière-plan rouge.

    ![Application StockTicker, ouverture du marché](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    Le bouton **Fermer le marché** arrête les modifications et arrête le défilement du téléscripteur, et le bouton **Réinitialiser** rétablit l’état initial de toutes les données de stock avant le début des changements de prix. Si vous ouvrez d’autres fenêtres de navigateur et que vous accédez à la même URL, les mêmes données sont mises à jour de manière dynamique en même temps dans chaque navigateur. Lorsque vous cliquez sur l’un des boutons, tous les navigateurs répondent de la même manière en même temps.

### <a name="live-stock-ticker-display"></a>Affichage de cotations boursières en direct

L’affichage de **cotations boursières en direct** est une liste non triée dans un élément div qui est mis en forme sur une seule ligne par les styles CSS. Le Ticker est initialisé et mis à jour de la même façon que la table : en remplaçant les espaces réservés dans une chaîne de modèle &lt;Li&gt; et en ajoutant dynamiquement les éléments &lt;Li&gt; à l’élément &lt;UL&gt;. Le défilement est effectué à l’aide de la fonction d’animation jQuery pour faire varier la marge à gauche de la liste non triée dans la balise div.

CODE HTML du ticker de stock :

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

CSS ticker ticker :

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

Code jQuery qui le fait défiler :

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Méthodes supplémentaires sur le serveur que le client peut appeler

La classe StockTickerHub définit quatre méthodes supplémentaires que le client peut appeler :

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

OpenMarket, CloseMarket et Reset sont appelés en réponse aux boutons situés en haut de la page. Elles illustrent le modèle d’un client déclenchant une modification de l’État qui est immédiatement propagée à tous les clients. Chacune de ces méthodes appelle une méthode dans la classe StockTicker qui affecte la modification de l’état du marché, puis diffuse le nouvel État.

Dans la classe StockTicker, l’état du marché est géré par une propriété MarketState qui retourne une valeur d’énumération MarketState :

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

Chacune des méthodes qui modifient l’état du marché le fait à l’intérieur d’un bloc de verrouillage, car la classe StockTicker doit être threadsafe :

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

Pour vous assurer que ce code est threadsafe, le champ marketState \_qui stocke la propriété MarketState est marqué comme volatile,

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

Les méthodes BroadcastMarketStateChange et BroadcastMarketReset sont similaires à la méthode BroadcastStockPrice que vous avez déjà vue, à ceci près qu’elles appellent différentes méthodes définies sur le client :

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Fonctions supplémentaires sur le client que le serveur peut appeler

La fonction updateStockPrice gère désormais la grille et l’affichage du téléscripteur, et utilise jQuery. Color pour faire clignoter les couleurs rouges et vertes.

De nouvelles fonctions dans *signalr. stockticker. js* activent et désactivent les boutons en fonction de l’état du marché, et ils arrêtent ou démarrent le défilement horizontal de la fenêtre du téléscripteur. Étant donné que plusieurs fonctions sont ajoutées à Ticker. client, la [fonction d’extension jQuery](http://api.jquery.com/jQuery.extend/) est utilisée pour les ajouter.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Configuration supplémentaire du client après l’établissement de la connexion

Une fois que le client a établi la connexion, il a d’autres tâches à effectuer : Déterminez si le marché est ouvert ou fermé afin d’appeler la fonction marketOpened ou marketClosed appropriée, et joignez les appels de la méthode serveur aux boutons.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

Les méthodes de serveur ne sont pas connectées aux boutons tant que la connexion n’est pas établie, de sorte que le code ne peut pas essayer d’appeler les méthodes du serveur avant qu’elles ne soient disponibles.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à programmer une application Signalr qui diffuse des messages à partir du serveur à tous les clients connectés, à la fois sur une base périodique et en réponse aux notifications de n’importe quel client. Le modèle d’utilisation d’une instance Singleton multithread pour maintenir l’état du serveur peut également être utilisé dans les scénarios de jeux en ligne multi-joueurs. Pour obtenir un exemple, consultez [le jeu de pousseurs basé sur signalr](https://github.com/NTaylorMullen/ShootR).

Pour obtenir des didacticiels montrant les scénarios de communication d’égal à égal, consultez [prise en main avec signalr](index.md) et [mise à jour en temps réel avec signalr](index.md).

Pour en savoir plus sur les concepts de développement Signalr plus avancés, visitez les sites suivants pour le code source et les ressources Signalr :

- [ASP.NET SignalR](https://asp.net/signalr/)
- [Projet signalr](http://signalr.net/)
- [Signalr GitHub et exemples](https://github.com/SignalR/SignalR)
- [Wiki signalr](https://github.com/SignalR/SignalR/wiki)
