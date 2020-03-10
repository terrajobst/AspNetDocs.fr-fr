---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: Résilience des connexions ASP.NET Web Forms et interception des commandes | Microsoft Docs
author: Erikre
description: Ce didacticiel explique comment modifier un exemple d’application pour prendre en charge la résilience des connexions et l’interception des commandes.
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 95f0b5635c12d5ef88622e5766c1278c6570dd4d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598426"
---
# <a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Résilience des connexions et interception des commandes Web Forms ASP.NET

par [Erik Reitan](https://github.com/Erikre)

Dans ce didacticiel, vous allez modifier l’exemple d’application Wingtip Toys pour prendre en charge la résilience des connexions et l’interception des commandes. En activant la résilience des connexions, l’exemple d’application Wingtip Toys réessaie automatiquement les appels de données lorsque des erreurs temporaires typiques d’un environnement Cloud se produisent. En outre, en implémentant l’interception des commandes, l’exemple d’application Wingtip Toys détecte toutes les requêtes SQL envoyées à la base de données afin de les enregistrer ou de les modifier.

> [!NOTE] 
> 
> Ce didacticiel de Web Forms a été basé sur le didacticiel MVC suivant de Tom Dykstra :  
> [Résilience des connexions et interception des commandes avec l’Entity Framework dans une application MVC ASP.NET](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment assurer la résilience des connexions.
- Comment implémenter l’interception des commandes.

## <a name="prerequisites"></a>Conditions préalables requises

Avant de commencer, assurez-vous que les logiciels suivants sont installés sur votre ordinateur :

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Le .NET Framework est installé automatiquement.
- L’exemple de projet Wingtip Toys, afin que vous puissiez implémenter les fonctionnalités mentionnées dans ce didacticiel au sein du projet Wingtip Toys. Le lien suivant fournit des détails sur le téléchargement :

    - [Prise en main avec ASP.net 4.5.1 Web Forms-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Avant de suivre ce didacticiel, pensez à consulter la série de didacticiels associés, [prise en main avec ASP.NET 4,5 Web Forms et Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). La série de didacticiels vous aidera à vous familiariser avec le projet et le code **WingtipToys** .

## <a name="connection-resiliency"></a>Résilience de la connexion

Lorsque vous envisagez de déployer une application sur Windows Azure, vous pouvez envisager de déployer la base de données sur **windows** **Azure SQL Database**, un service de base de données Cloud. Les erreurs de connexion temporaires sont généralement plus fréquentes lorsque vous vous connectez à un service de base de données Cloud que lorsque votre serveur Web et votre serveur de base de données sont directement connectés ensemble dans le même centre de données. Même si un serveur Web Cloud et un service de base de données Cloud sont hébergés dans le même centre de données, il existe plus de connexions réseau entre eux qui peuvent avoir des problèmes, tels que des équilibreurs de charge.

En outre, un service Cloud est généralement partagé par d’autres utilisateurs, ce qui signifie que sa réactivité peut leur être affectée. Et votre accès à la base de données peut être soumis à une limitation. La limitation signifie que le service de base de données lève des exceptions lorsque vous essayez d’y accéder plus fréquemment que ce qui est autorisé dans votre *contrat de niveau de service* (SLA).

Un grand nombre ou la plupart des problèmes de connexion qui se produisent lorsque vous accédez à un service Cloud sont temporaires, c’est-à-dire qu’ils se résolvent eux-mêmes sur une période de temps limitée. Ainsi, lorsque vous essayez d’effectuer une opération de base de données et que vous recevez un type d’erreur généralement temporaire, vous pouvez recommencer l’opération après une brève attente et l’opération peut être réussie. Vous pouvez fournir une meilleure expérience à vos utilisateurs si vous traitez des erreurs temporaires en réessayant automatiquement, en les rendant invisibles au client. La fonctionnalité de résilience des connexions dans Entity Framework 6 automatise ce processus de nouvelle tentative d’échec de requêtes SQL.

La fonctionnalité de résilience des connexions doit être configurée de manière appropriée pour un service de base de données spécifique :

1. Il doit savoir quelles exceptions sont susceptibles d’être temporaires. Vous souhaitez réessayer les erreurs causées par une perte temporaire de la connectivité réseau, et non les erreurs provoquées par les bogues de programme, par exemple.
2. Il doit attendre un laps de temps approprié entre les nouvelles tentatives d’une opération ayant échoué. Vous pouvez attendre plus longtemps entre les nouvelles tentatives d’un traitement par lots que pour une page Web en ligne où un utilisateur attend une réponse.
3. Il doit réessayer un nombre de fois approprié avant d’abandonner. Vous souhaiterez peut-être réessayer plus souvent dans un processus de traitement par lots que dans une application en ligne.

Vous pouvez configurer ces paramètres manuellement pour tout environnement de base de données pris en charge par un fournisseur de Entity Framework.

Pour activer la résilience des connexions, il vous suffit de créer une classe dans votre assembly qui dérive de la classe `DbConfiguration` et, dans cette classe, de définir la stratégie d’exécution SQL Database, qui, dans Entity Framework, est un autre terme pour la stratégie de nouvelle tentative.

### <a name="implementing-connection-resiliency"></a>Implémentation de la résilience des connexions

1. Téléchargez et ouvrez l’exemple d’application [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) Web Forms dans Visual Studio.
2. Dans le dossier *logique* de l’application **WingtipToys** , ajoutez un fichier de classe nommé *WingtipToysConfiguration.cs*.
3. Remplacez le code existant par celui-ci :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Le Entity Framework exécute automatiquement le code qu’il trouve dans une classe qui dérive de `DbConfiguration`. Vous pouvez utiliser la classe `DbConfiguration` pour effectuer des tâches de configuration dans le code que vous feriez autrement dans le fichier *Web. config* . Pour plus d’informations, consultez la section relative [à la configuration basée sur le code EntityFramework](https://msdn.microsoft.com/data/jj680699).

1. Dans le dossier *logique* , ouvrez le fichier *AddProducts.cs* .
2. Ajoutez une instruction `using` pour `System.Data.Entity.Infrastructure` comme indiqué en jaune :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Ajoutez un bloc de `catch` à la méthode `AddProduct` afin que le `RetryLimitExceededException` soit enregistré comme étant mis en surbrillance en jaune :   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

En ajoutant l’exception `RetryLimitExceededException`, vous pouvez fournir une meilleure journalisation ou afficher un message d’erreur à l’utilisateur où il peut choisir de retenter le processus. En interceptant l’exception `RetryLimitExceededException`, les seules erreurs susceptibles d’être temporaires ont déjà été tentées et ont échoué plusieurs fois. L’exception réelle retournée est encapsulée dans l’exception `RetryLimitExceededException`. En outre, vous avez également ajouté un bloc catch général. Pour plus d’informations sur l’exception `RetryLimitExceededException`, consultez [Entity Framework résilience des connexions/logique de nouvelle tentative](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Interception des commandes

Maintenant que vous avez activé une stratégie de nouvelle tentative, comment effectuer un test pour vérifier qu’elle fonctionne comme prévu ? Il n’est pas tellement facile de forcer une erreur temporaire, en particulier lorsque vous exécutez localement, et il serait particulièrement difficile d’intégrer les erreurs temporaires réelles dans un test unitaire automatisé. Pour tester la fonctionnalité de résilience des connexions, vous avez besoin d’un moyen d’intercepter les requêtes que Entity Framework envoie à SQL Server et de remplacer la réponse SQL Server par un type d’exception généralement temporaire.

Vous pouvez également utiliser l’interception des requêtes afin d’implémenter une meilleure pratique pour les applications Cloud : consigner la latence et la réussite ou l’échec de tous les appels aux services externes, tels que les services de base de données.

Dans cette section du didacticiel, vous allez utiliser la fonctionnalité d' [*interception*](https://msdn.microsoft.com/data/dn469464) de Entity Framework pour la journalisation et pour la simulation des erreurs temporaires.

### <a name="create-a-logging-interface-and-class"></a>Créer une interface et une classe de journalisation

Pour la journalisation, il est recommandé de le faire à l’aide d’un [`interface`](https://msdn.microsoft.com/library/ms173156.aspx) plutôt que de coder en dur des appels à `System.Diagnostics.Trace` ou une classe de journalisation. Cela facilite la modification de votre mécanisme de journalisation ultérieurement si vous avez besoin de le faire. Ainsi, dans cette section, vous allez créer l’interface de journalisation et une classe pour l’implémenter.

Sur la base de la procédure ci-dessus, vous avez téléchargé et ouvert l’exemple d’application **WingtipToys** dans Visual Studio.

1. Créez un dossier dans le projet **WingtipToys** et nommez-le *journalisation*.
2. Dans le dossier de *journalisation* , créez un fichier de classe nommé *ILogger.cs* et remplacez le code par défaut par le code suivant :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   L’interface fournit trois niveaux de suivi pour indiquer l’importance relative des journaux, et un autre conçu pour fournir des informations de latence pour les appels de service externe, tels que les requêtes de base de données. Les méthodes de journalisation ont des surcharges qui vous permettent de passer une exception. Ainsi, les informations sur les exceptions, y compris la trace de la pile et les exceptions internes, sont journalisées de façon fiable par la classe qui implémente l’interface, au lieu de s’appuyer sur cette opération dans chaque appel de méthode de journalisation au sein de l’application.  
  
   Les méthodes `TraceApi` vous permettent d’effectuer le suivi de la latence de chaque appel à un service externe, tel que SQL Database.
3. Dans le dossier de *journalisation* , créez un fichier de classe nommé *logger.cs* et remplacez le code par défaut par le code suivant :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

L’implémentation utilise `System.Diagnostics` pour effectuer le suivi. Il s’agit d’une fonctionnalité intégrée de .NET qui facilite la génération et l’utilisation des informations de suivi. Il existe de nombreux écouteurs &quot;&quot; vous pouvez utiliser avec le suivi `System.Diagnostics`, pour écrire des journaux dans des fichiers, par exemple, ou pour les écrire dans le stockage d’objets BLOB dans Windows Azure. Pour plus d’informations sur la [résolution des problèmes liés aux sites Web Windows Azure dans Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio), consultez certaines options et liens vers d’autres ressources. Pour ce didacticiel, vous allez examiner uniquement les journaux dans la fenêtre **sortie** de Visual Studio.

Dans une application de production, vous souhaiterez peut-être envisager d’utiliser des frameworks de suivi autres que `System.Diagnostics`, et l’interface `ILogger` rend relativement facile de basculer vers un mécanisme de suivi différent si vous décidez de le faire.

### <a name="create-interceptor-classes"></a>Créer des classes d’intercepteur

Ensuite, vous allez créer les classes que le Entity Framework appellera à chaque fois qu’il enverra une requête à la base de données, une pour simuler des erreurs temporaires et une pour la journalisation. Ces classes d’intercepteur doivent dériver de la classe `DbCommandInterceptor`. Dans ceux-ci, vous écrivez des substitutions de méthode qui sont appelées automatiquement lorsque la requête est sur le paragraphe d’être exécutée. Dans ces méthodes, vous pouvez examiner ou enregistrer la requête qui est envoyée à la base de données, et vous pouvez modifier la requête avant de l’envoyer à la base de données ou retourner un objet à Entity Framework vous-même sans même passer la requête à la base de données.

1. Pour créer la classe d’intercepteur qui journalise chaque requête SQL avant de l’envoyer à la base de données, créez un fichier de classe nommé *InterceptorLogging.cs* dans le dossier *logique* et remplacez le code par défaut par le code suivant :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Pour les requêtes ou les commandes ayant abouti, ce code écrit un journal d’informations avec des informations de latence. Pour les exceptions, il crée un journal des erreurs.
2. Pour créer la classe d’intercepteur qui génère des erreurs temporaires factices lorsque vous entrez &quot;lève&quot; dans la zone de texte **nom** de la page nommée *AdminPage. aspx*, créez un fichier de classe nommé *InterceptorTransientErrors.cs* dans le dossier *logique* et remplacez le code par défaut par le code suivant :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Ce code substitue uniquement la méthode `ReaderExecuting`, qui est appelée pour les requêtes qui peuvent retourner plusieurs lignes de données. Si vous souhaitez vérifier la résilience des connexions pour d’autres types de requêtes, vous pouvez également remplacer les méthodes `NonQueryExecuting` et `ScalarExecuting`, comme le fait l’intercepteur de journalisation.  
  
   Plus tard, vous ouvrirez une session en tant qu’administrateur et sélectionnerez le lien d' **administration** dans la barre de navigation supérieure. Ensuite, sur la page *AdminPage. aspx* , vous allez ajouter un produit nommé &quot;lève&quot;. Le code crée une exception de SQL Database factice pour le numéro d’erreur 20, un type connu généralement comme temporaire. Les autres numéros d’erreur actuellement reconnus comme temporaires sont 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 et 40613, mais ceux-ci sont susceptibles d’être modifiés dans les nouvelles versions de SQL Database. Le produit sera renommé en « TransientErrorExample », que vous pouvez suivre dans le code du fichier *InterceptorTransientErrors.cs* .  
  
   Le code retourne l’exception à Entity Framework au lieu d’exécuter la requête et de renvoyer les résultats. L’exception temporaire est retournée *quatre* fois, puis le code revient à la procédure normale de transmission de la requête à la base de données.

    Étant donné que tout est journalisé, vous pouvez voir que Entity Framework tente d’exécuter la requête quatre fois avant la fin de l’opération, et la seule différence dans l’application est qu’il faut plus de temps pour afficher une page avec les résultats de la requête.  
  
   Le nombre de nouvelles tentatives de Entity Framework peut être configuré ; le code spécifie quatre fois, car il s’agit de la valeur par défaut pour la stratégie d’exécution SQL Database. Si vous modifiez la stratégie d’exécution, vous modifiez également le code qui spécifie le nombre de fois où des erreurs temporaires sont générées. Vous pouvez également modifier le code pour générer plus d’exceptions afin que Entity Framework lève l’exception `RetryLimitExceededException`.
3. Dans *global. asax*, ajoutez les instructions using suivantes :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Ensuite, ajoutez les lignes en surbrillance à la méthode `Application_Start` :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Ces lignes de code provoquent l’exécution de votre code d’intercepteur quand Entity Framework envoie des requêtes à la base de données. Notez que, étant donné que vous avez créé des classes d’intercepteur distinctes pour la simulation et la journalisation des erreurs temporaires, vous pouvez les activer et les désactiver indépendamment.   
  
 Vous pouvez ajouter des intercepteurs à l’aide de la méthode `DbInterception.Add` n’importe où dans votre code ; elle n’a pas besoin d’être dans la méthode `Application_Start`. Une autre option, si vous n’avez pas ajouté d’intercepteurs dans la méthode `Application_Start`, consisterait à mettre à jour ou à ajouter la classe nommée *WingtipToysConfiguration.cs* et à placer le code ci-dessus à la fin du constructeur de la classe `WingtipToysConfiguration`.

Quel que soit l’endroit où vous placez ce code, veillez à ne pas exécuter plusieurs `DbInterception.Add` pour le même intercepteur, sinon vous obtiendrez des instances d’intercepteur supplémentaires. Par exemple, si vous ajoutez deux fois l’intercepteur de journalisation, vous verrez deux journaux pour chaque requête SQL.

Les intercepteurs sont exécutés dans l’ordre d’inscription (l’ordre dans lequel la méthode `DbInterception.Add` est appelée). L’ordre peut être important en fonction de ce que vous effectuez dans l’intercepteur. Par exemple, un intercepteur peut modifier la commande SQL qu’il obtient dans la propriété `CommandText`. Si la commande SQL est modifiée, l’intercepteur suivant obtient la commande SQL modifiée, et non la commande SQL d’origine.

Vous avez écrit le code de simulation d’erreur temporaire d’une manière qui vous permet de générer des erreurs temporaires en entrant une valeur différente dans l’interface utilisateur. Vous pouvez également écrire le code de l’intercepteur pour toujours générer la séquence d’exceptions transitoires sans vérifier une valeur de paramètre particulière. Vous pouvez ensuite ajouter l’intercepteur uniquement lorsque vous souhaitez générer des erreurs temporaires. Toutefois, si vous procédez ainsi, n’ajoutez pas l’intercepteur jusqu’à la fin de l’initialisation de la base de données. En d’autres termes, effectuez au moins une opération de base de données telle qu’une requête sur l’un de vos jeux d’entités avant de commencer à générer des erreurs temporaires. Le Entity Framework exécute plusieurs requêtes pendant l’initialisation de la base de données, et elles ne sont pas exécutées dans une transaction ; par conséquent, les erreurs au cours de l’initialisation peuvent amener le contexte à passer à un état incohérent.

## <a name="test-logging-and-connection-resiliency"></a>Journalisation des tests et résilience des connexions

1. Dans Visual Studio, appuyez sur **F5** pour exécuter l’application en mode débogage, puis connectez-vous en tant que « admin » en utilisant « Pa $ $Word » comme mot de passe.
2. Sélectionnez **administrateur** dans la barre de navigation en haut.
3. Entrez un nouveau produit nommé « Throw » avec la description, le prix et le fichier image appropriés.
4. Appuyez sur le bouton **Ajouter un produit** .  
   Vous remarquerez que le navigateur semble se bloquer pendant plusieurs secondes alors que Entity Framework retente plusieurs fois la requête. La première tentative se produit très rapidement, puis l’attente augmente avant chaque nouvelle tentative supplémentaire. Ce processus d’attente plus longtemps avant chaque nouvelle tentative est appelé interruption *exponentielle* .
5. Attendez jusqu’à ce que la page ne tente plus de charger.
6. Arrêtez le projet et examinez la fenêtre **sortie** de Visual Studio pour afficher la sortie de traçage. Vous pouvez trouver la fenêtre **sortie** en sélectionnant **déboguer** -&gt; -de **sortie**de &gt; **Windows** . Vous devrez peut-être faire défiler plusieurs autres journaux écrits par votre enregistreur d’événements.  
  
   Notez que vous pouvez voir les requêtes SQL réelles envoyées à la base de données. Vous voyez des requêtes et des commandes initiales que Entity Framework fait pour commencer, en vérifiant la version de la base de données et la table de l’historique de la migration.   
    ![Sortie (fenêtre)](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Notez que vous ne pouvez pas répéter ce test, sauf si vous arrêtez l’application et que vous la redémarrez. Si vous souhaitez être en mesure de tester la résilience de connexion plusieurs fois dans une seule exécution de l’application, vous pouvez écrire du code pour réinitialiser le compteur d’erreurs dans `InterceptorTransientErrors`.
7. Pour voir la différence entre la stratégie d’exécution (stratégie de nouvelle tentative), commentez la ligne de `SetExecutionStrategy` dans le fichier *WingtipToysConfiguration.cs* dans le dossier *logique* , exécutez à nouveau la page **admin** en mode débogage, puis ajoutez le produit nommé &quot;lever à nouveau&quot;.  
  
   Cette fois, le débogueur s’arrête immédiatement sur la première exception générée lorsqu’il essaie d’exécuter la requête la première fois.  
    ![Débogage-afficher les détails](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Supprimez les marques de commentaire de la ligne de `SetExecutionStrategy` dans le fichier *WingtipToysConfiguration.cs* .

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez vu comment modifier un exemple d’application Web Forms pour prendre en charge la résilience des connexions et l’interception des commandes.

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez revu la résilience des connexions et l’interception des commandes dans ASP.NET Web Forms, consultez les [méthodes asynchrones ASP.NET Web Forms rubrique dans ASP.NET 4,5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). La rubrique vous apprend les bases de la création d’une application ASP.NET Web Forms asynchrone à l’aide de Visual Studio.
