---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: Utilisation de méthodes asynchrones dans ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC asynchrone à l’aide de Visual Studio Express 2012 pour le Web, ce qui est une opération gratuite...
ms.author: riande
ms.date: 06/06/2012
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 5df6a9c136b1934b3afd731eb0ceac1e0faa483e
ms.sourcegitcommit: 6f0e10e4ca61a1e5534b09c655fd35cdc6886c8a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115079"
---
# <a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>Utilisation de méthodes asynchrones dans ASP.NET MVC 4

par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC asynchrone à l’aide de [Visual Studio Express 2012 pour le Web](https://www.microsoft.com/visualstudio/11), qui est une version gratuite de Microsoft Visual Studio. Vous pouvez également utiliser [Visual Studio 2012](https://www.microsoft.com/visualstudio/11).
> 
> Un exemple complet est fourni pour ce didacticiel sur GitHub [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)

La classe de [contrôleur](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx) ASP.NET MVC 4 en combinaison [.net 4,5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) vous permet d’écrire des méthodes d’action asynchrones qui retournent un objet de type [Task&lt;ActionResult&gt;](https://msdn.microsoft.com/library/dd321424(VS.110).aspx). Le .NET Framework 4 a introduit un concept de programmation asynchrone appelé [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) et ASP.NET MVC 4 prend en charge la [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Les tâches sont représentées par le type de **tâche** et les types associés dans l’espace de noms [System. Threading. Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) . La .NET Framework 4,5 s’appuie sur cette prise en charge asynchrone avec les mots clés [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) et [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) qui rendent l’utilisation des objets de [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) bien moins complexe que les approches asynchrones précédentes. Le mot clé [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) est un raccourci de syntaxe pour indiquer qu’un morceau de code doit attendre de façon asynchrone un autre morceau de code. Le mot clé [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) représente un indicateur que vous pouvez utiliser pour marquer des méthodes en tant que méthodes asynchrones basées sur des tâches. La combinaison d' **await**, **Async**et de l’objet de **tâche** facilite grandement l’écriture de code asynchrone dans .net 4,5. Le nouveau modèle pour les méthodes asynchrones est appelé *modèle asynchrone basé sur les tâches* (**Tap**). Ce didacticiel part du principe que vous êtes familiarisé avec les programmations asynchrones à l’aide des mots clés [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) et [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) et de l’espace de noms de [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) .

Pour plus d’informations sur l’utilisation des mots clés [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) et [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) et l’espace de noms de [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) , consultez les références suivantes.

- [Livre blanc : asynchronie dans .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [FAQ Async/await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programmation asynchrone Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Mode de traitement des demandes par le pool de threads

Sur le serveur Web, le .NET Framework gère un pool de threads utilisés pour traiter les demandes ASP.NET. Lorsqu’une requête arrive, un thread du pool est distribué pour traiter cette demande. Si la demande est traitée de façon synchrone, le thread qui traite la demande est occupé pendant le traitement de la demande et ce thread ne peut pas traiter une autre demande.   
  
Cela peut ne pas être un problème, car le pool de threads peut être rendu suffisamment grand pour accueillir de nombreux threads occupés. Toutefois, le nombre de threads dans le pool de threads est limité (la valeur maximale par défaut pour .NET 4,5 est 5 000). Dans les applications de grande taille avec un haut niveau de concurrence des requêtes à long terme, tous les threads disponibles peuvent être occupés. Cette condition est connue sous le nom de privation de thread. Lorsque cette condition est atteinte, le serveur Web met en file d’attente les demandes. Si la file d’attente de demandes est pleine, le serveur Web rejette les demandes avec un état HTTP 503 (serveur encombré). Le pool de threads CLR présente des limitations sur les nouvelles injections de thread. Si la concurrence est éclatée (autrement dit, votre site Web peut obtenir soudainement un grand nombre de requêtes) et que tous les threads de requête disponibles sont occupés en raison des appels du backend avec une latence élevée, le taux d’injection de threads limité peut rendre votre application plus difficile à résoudre. En outre, chaque nouveau thread ajouté au pool de threads a une charge mémoire (par exemple, 1 Mo de mémoire de pile). Une application Web utilisant des méthodes synchrones pour traiter les appels à latence élevée où le pool de threads atteint le nombre maximal de 5 000 threads par défaut de .NET 4,5 consomme environ 5 Go de mémoire par rapport à une application capable de traiter les mêmes demandes à l’aide de les méthodes asynchrones et uniquement les threads 50. Lorsque vous effectuez un travail asynchrone, vous n’utilisez pas toujours un thread. Par exemple, lorsque vous effectuez une demande de service Web asynchrone, ASP.NET n’utilise aucun thread entre l’appel de méthode **Async** et l' **expression await**. L’utilisation du pool de threads pour traiter les demandes avec une latence élevée peut entraîner un grand encombrement mémoire et une mauvaise utilisation du matériel serveur.

## <a name="processing-asynchronous-requests"></a>Traitement des demandes asynchrones

Dans une application Web qui voit un grand nombre de demandes simultanées au démarrage ou a une charge de rafale (où l’accès concurrentiel augmente soudainement), le fait d’appeler le service Web de façon asynchrone augmente la réactivité de l’application. Une requête asynchrone prend le même temps de traitement qu’une demande synchrone. Si une demande effectue un appel de service Web qui nécessite deux secondes, la demande prend deux secondes, qu’elle soit exécutée de façon synchrone ou asynchrone. Toutefois, lors d’un appel asynchrone, un thread ne peut pas répondre à d’autres requêtes pendant qu’il attend la fin de la première requête. Par conséquent, les requêtes asynchrones empêchent la mise en file d’attente des demandes et la croissance du pool de threads lorsqu’il existe de nombreuses demandes simultanées qui appellent des opérations de longue durée

## <a id="ChoosingSyncVasync"></a>Choix de méthodes d’action synchrones ou asynchrones

Cette section répertorie les instructions relatives à l’utilisation des méthodes d’action synchrones ou asynchrones. Il s’agit simplement de recommandations. Examinez chaque application individuellement pour déterminer si les méthodes asynchrones permettent d’obtenir des performances optimales.

En général, utilisez des méthodes synchrones pour les conditions suivantes :

- Les opérations sont simples ou à exécution rapide.
- La simplicité est plus importante que l’efficacité.
- Les opérations sont principalement des opérations de processeur plutôt que des opérations qui impliquent une surcharge de disque ou de réseau étendue. L’utilisation de méthodes d’action asynchrones sur les opérations liées à l’UC n’offre aucun avantage et aboutit à une plus grande surcharge.

En général, utilisez des méthodes asynchrones pour les conditions suivantes :

- Vous appelez des services qui peuvent être utilisés par le biais de méthodes asynchrones, et vous utilisez .NET 4,5 ou une version ultérieure.
- Les opérations sont liées au réseau ou à des e/s au lieu d’être liées à l’UC.
- Le parallélisme est plus important que la simplicité du code.
- Vous souhaitez fournir un mécanisme qui permet aux utilisateurs d’annuler une demande longue.
- Lorsque l’avantage du changement de threads compense le coût du changement de contexte. En général, vous devez faire en sorte qu’une méthode soit asynchrone si la méthode synchrone attend sur le thread de demande ASP.NET en ne procédant à aucun travail. En effectuant l’appel de manière asynchrone, le thread de demande ASP.NET n’est pas bloqué et n’effectue aucun travail pendant qu’il attend la fin de la demande de service Web.
- Les tests montrent que les opérations bloquantes représentent un goulot d’étranglement dans les performances de site et qu’IIS peut traiter davantage de demandes en utilisant des méthodes asynchrones pour ces appels bloquant.

L’exemple téléchargeable montre comment utiliser efficacement des méthodes d’action asynchrones. L’exemple fourni a été conçu pour fournir une démonstration simple de la programmation asynchrone dans ASP.NET MVC 4 à l’aide de .NET 4,5. L’exemple n’est pas destiné à être une architecture de référence pour la programmation asynchrone dans ASP.NET MVC. L’exemple de programme appelle [API Web ASP.net](../../../web-api/index.md) méthodes qui à son tour appellent [Task. Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) pour simuler des appels de service Web à long terme. La plupart des applications de production n’illustrent pas les avantages évidents de l’utilisation des méthodes d’action asynchrones.   
  
Certaines applications requièrent que toutes les méthodes d’action soient asynchrones. Souvent, la conversion de quelques méthodes d’action synchrones en méthodes asynchrones offre une meilleure augmentation de l’efficacité pour la quantité de travail requise.

## <a id="SampleApp"></a>L’exemple d’application

Vous pouvez télécharger l’exemple d’application à partir de [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET) sur le site [GitHub](https://github.com/) . Le référentiel se compose de trois projets :

- *Mvc4Async*: projet ASP.NET MVC 4 qui contient le code utilisé dans ce didacticiel. Il effectue des appels d’API Web au service **WebAPIpgw** .
- *WebAPIpgw*: projet d’API Web ASP.NET MVC 4 qui implémente les contrôleurs de `Products, Gizmos and Widgets`. Il fournit les données pour le projet *WebAppAsync* et le projet *Mvc4Async* .
- *WebAppAsync*: le projet ASP.NET Web Forms utilisé dans un autre didacticiel.

## <a id="GizmosSynch"></a>Méthode d’action synchrone gizmos

 Le code suivant illustre la `Gizmos` méthode d’action synchrone utilisée pour afficher une liste de gizmos. (Pour cet article, un gizmo est un appareil mécanique fictif.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

Le code suivant illustre la méthode `GetGizmos` du service gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

La méthode `GizmoService GetGizmos` passe un URI à un service HTTP API Web ASP.NET qui retourne une liste de données gizmos. Le projet *WebAPIpgw* contient l’implémentation de l’API Web `gizmos, widget` et `product` contrôleurs.  
L’illustration suivante montre la vue gizmos de l’exemple de projet.

![Gizmos](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Création d’une méthode d’action gizmos asynchrone

L’exemple utilise les nouveaux mots clés [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) et [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) (disponibles dans .net 4,5 et Visual Studio 2012) pour permettre au compilateur de gérer les transformations compliquées nécessaires à la programmation asynchrone. Le compilateur vous permet d’écrire du code C#à l’aide des constructions de flux de contrôle synchrones de et le compilateur applique automatiquement les transformations nécessaires pour utiliser les rappels afin d’éviter les threads de blocage.

Le code suivant montre les `Gizmos` méthode synchrone et `GizmosAsync` méthode asynchrone. Si votre navigateur prend en charge l' [élément `<mark>` HTML 5](http://www.w3.org/wiki/HTML/Elements/mark), vous verrez les modifications apportées à `GizmosAsync` en surbrillance en jaune.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 Les modifications suivantes ont été appliquées pour permettre l' `GizmosAsync` asynchrone.

- La méthode est marquée avec le mot clé [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , qui indique au compilateur de générer des rappels pour les parties du corps et de créer automatiquement une `Task<ActionResult>` retournée.
- &quot;&quot; Async a été ajouté au nom de la méthode. L’ajout de « Async » n’est pas obligatoire, mais il s’agit de la Convention lors de l’écriture de méthodes asynchrones.
- Le type de retour a été modifié de `ActionResult` à `Task<ActionResult>`. Le type de retour de `Task<ActionResult>` représente un travail en cours et fournit aux appelants de la méthode un handle par le biais duquel attendre l’achèvement de l’opération asynchrone. Dans ce cas, l’appelant est le service Web. `Task<ActionResult>` représente le travail en cours avec le résultat de `ActionResult.`
- Le mot clé [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) a été appliqué à l’appel de service Web.
- L’API de service Web asynchrone a été appelée (`GetGizmosAsync`).

Dans le corps de la méthode `GetGizmosAsync` une autre méthode asynchrone, `GetGizmosAsync` est appelée. `GetGizmosAsync` retourne immédiatement un `Task<List<Gizmo>>` qui finit par se terminer lorsque les données sont disponibles. Étant donné que vous ne souhaitez rien faire d’autre tant que vous n’avez pas les données Gizmo, le code attend la tâche (à l’aide du mot clé **await** ). Vous pouvez utiliser le mot clé **await** uniquement dans les méthodes annotées avec le mot clé **Async** .

Le mot clé **await** ne bloque pas le thread tant que la tâche n’est pas terminée. Il inscrit le reste de la méthode en tant que rappel sur la tâche et retourne immédiatement. Lorsque la tâche attendue finit par se terminer, elle appelle ce rappel et, par conséquent, reprend l’exécution de la méthode juste là où elle s’était arrêtée. Pour plus d’informations sur l’utilisation des mots clés [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) et [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) et de l’espace de noms de [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) , consultez [références Async](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Le code suivant illustre les méthodes `GetGizmos` et `GetGizmosAsync`.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 Les modifications asynchrones sont similaires à celles apportées aux **GizmosAsync** ci-dessus. 

- La signature de la méthode a été annotée avec le mot clé [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , le type de retour a été remplacé par `Task<List<Gizmo>>`et *Async* a été ajouté au nom de la méthode.
- La classe [httpclient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) asynchrone est utilisée à la place de la classe [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) .
- Le mot clé [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) a été appliqué aux méthodes asynchrones [httpclient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) .

L’illustration suivante montre la vue Gizmo asynchrone.

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

La présentation des navigateurs des données gizmos est identique à la vue créée par l’appel synchrone. La seule différence est que la version asynchrone peut être plus performante sous des charges lourdes.

## <a id="Parallel"></a>Exécution de plusieurs opérations en parallèle

Les méthodes d’action asynchrones présentent un avantage significatif par rapport aux méthodes synchrones lorsqu’une action doit effectuer plusieurs opérations indépendantes. Dans l’exemple fourni, la méthode synchrone `PWG`(pour Products, widgets et gizmos) affiche les résultats de trois appels de service Web pour obtenir la liste des produits, des widgets et des gizmos. Le projet [API Web ASP.net](../../../web-api/index.md) qui fournit ces services utilise [Task. Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) pour simuler la latence ou des appels réseau lents. Lorsque le délai est défini sur 500 millisecondes, la méthode de `PWGasync` asynchrone prend un peu plus de 500 millisecondes pour s’exécuter, tandis que la version de `PWG` synchrone prend plus de 1 500 millisecondes. La méthode de `PWG` synchrone est illustrée dans le code suivant.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

La méthode `PWGasync` asynchrone est présentée dans le code suivant.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

L’illustration suivante montre la vue retournée par la méthode **PWGasync** .

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>Utilisation d’un jeton d’annulation

Les méthodes d’action asynchrones qui retournent `Task<ActionResult>`peuvent être annulées, c’est-à-dire qu’elles prennent un paramètre [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) lorsque l’une d’elles est fournie avec l’attribut [AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx) . Le code suivant illustre la méthode `GizmosCancelAsync` avec un délai d’expiration de 150 millisecondes.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

Le code suivant illustre la surcharge GetGizmosAsync, qui prend un paramètre [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) .

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

Dans l’exemple d’application fourni, la sélection du lien de *démonstration de jeton d’annulation* appelle la méthode `GizmosCancelAsync` et montre l’annulation de l’appel asynchrone.

## <a id="ServerConfig"></a>Configuration du serveur pour les appels de service Web avec concurrence élevée/latence élevée

Pour tirer parti des avantages d’une application Web asynchrone, vous devrez peut-être apporter des modifications à la configuration du serveur par défaut. Gardez à l’esprit les points suivants lors de la configuration et du test de stress de votre application Web asynchrone.

- Windows 7, Windows Vista et tous les systèmes d’exploitation clients Windows ont un maximum de 10 demandes simultanées. Vous aurez besoin d’un système d’exploitation Windows Server pour voir les avantages des méthodes asynchrones sous une charge élevée.
- Inscrire .NET 4,5 avec IIS à partir d’une invite de commandes avec élévation de privilèges :  
  %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis-i  
  Consultez [ASP.NET IIS Registration Tool (Aspnet\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Vous devrez peut-être augmenter la limite de file d’attente [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) de la valeur par défaut 1 000 à 5 000. Si le paramètre est trop bas, vous pouvez voir que [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) rejette les demandes avec un état HTTP 503. Pour modifier la limite de file d’attente HTTP. sys :

    - Ouvrez le gestionnaire des services Internet et accédez au volet pools d’applications.
    - Cliquez avec le bouton droit sur le pool d’applications cible et sélectionnez **Paramètres avancés**.  
        ![](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png) avancé
    - Dans la boîte de dialogue **Paramètres avancés** , changez la longueur de la *file d’attente* de 1 000 à 5 000.  
        longueur de la file d’attente ![](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
  Remarque dans les images ci-dessus, le .NET Framework est listé comme v 4.0, même si le pool d’applications utilise .NET 4,5. Pour comprendre cette différence, consultez les rubriques suivantes :

    - [Contrôle de version .NET et multi-ciblage-.NET 4,5 est une mise à niveau sur place vers .NET 4,0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [Comment définir une application IIS ou un pool d’applications pour utiliser ASP.NET 3,5 au lieu de 2,0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [Versions et dépendances de .NET Framework](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Si votre application utilise des services Web ou System.NET pour communiquer avec un serveur principal via HTTP, vous devrez peut-être augmenter l’élément [connectionManagement/MaxConnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) . Pour les applications ASP.NET, cela est limité par la fonctionnalité de configuration automatique à 12 fois le nombre de processeurs. Cela signifie que sur un Quad-proc, vous pouvez avoir au maximum 12 \* 4 = 48 connexions simultanées à un point de terminaison IP. Étant donné que cela est lié à la [configuration automatique](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), le moyen le plus simple d’augmenter `maxconnection` dans une application ASP.net consiste à définir [System .net. ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) par programmation dans la méthode from `Application_Start` dans le fichier *global. asax* . Consultez l’exemple de téléchargement pour obtenir un exemple.
- Dans .NET 4,5, la valeur par défaut 5000 pour [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) doit être correcte.
