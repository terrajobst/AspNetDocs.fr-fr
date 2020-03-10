---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Didacticiel : utiliser la résilience des connexions et l’interception des commandes avec EF dans une application MVC ASP.NET'
author: tdykstra
description: Dans ce didacticiel, vous allez apprendre à utiliser la résilience des connexions et l’interception des commandes. Il s’agit de deux fonctionnalités importantes de Entity Framework 6.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 276266f8ae9df38529d44742ebe6ac0dc8e79727
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583453"
---
# <a name="tutorial-use-connection-resiliency-and-command-interception-with-entity-framework-in-an-aspnet-mvc-app"></a>Didacticiel : utiliser la résilience des connexions et l’interception des commandes avec Entity Framework dans une application MVC ASP.NET

Jusqu’à présent, l’application s’exécutait localement dans IIS Express sur votre ordinateur de développement. Pour mettre une application réelle à la disposition d’autres personnes à utiliser sur Internet, vous devez la déployer sur un fournisseur d’hébergement Web, et vous devez déployer la base de données sur un serveur de base de données.

Dans ce didacticiel, vous allez apprendre à utiliser la résilience des connexions et l’interception des commandes. Il s’agit de deux fonctionnalités importantes de Entity Framework 6 qui sont particulièrement utiles lors du déploiement dans l’environnement cloud : résilience des connexions (nouvelles tentatives automatiques pour les erreurs temporaires) et interception des commandes (intercepter toutes les requêtes SQL envoyées à la base de données). afin de les enregistrer ou de les modifier).

Ce didacticiel sur la résilience des connexions et l’interception des commandes est facultatif. Si vous ignorez ce didacticiel, quelques ajustements mineurs devront être effectués dans les didacticiels suivants.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Activer la résilience des connexions
> * Activer l’interception des commandes
> * Tester la nouvelle configuration

## <a name="prerequisites"></a>Conditions préalables requises

* [Tri, filtrage et pagination](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-connection-resiliency"></a>Activer la résilience des connexions

Lorsque vous déployez l’application sur Windows Azure, vous déployez la base de données sur Windows Azure SQL Database, un service de base de données Cloud. Les erreurs de connexion temporaires sont généralement plus fréquentes lorsque vous vous connectez à un service de base de données Cloud que lorsque votre serveur Web et votre serveur de base de données sont directement connectés ensemble dans le même centre de données. Même si un serveur Web Cloud et un service de base de données Cloud sont hébergés dans le même centre de données, il existe plus de connexions réseau entre eux qui peuvent avoir des problèmes, tels que des équilibreurs de charge.

En outre, un service Cloud est généralement partagé par d’autres utilisateurs, ce qui signifie que sa réactivité peut leur être affectée. Et votre accès à la base de données peut être soumis à une limitation. La limitation signifie que le service de base de données lève des exceptions lorsque vous essayez d’y accéder plus fréquemment que ce qui est autorisé dans votre Contrat de niveau de service (SLA).

Un grand nombre ou la plupart des problèmes de connexion lorsque vous accédez à un service Cloud sont temporaires, c’est-à-dire qu’ils se résolvent eux-mêmes sur une période de temps limitée. Ainsi, lorsque vous essayez d’effectuer une opération de base de données et que vous recevez un type d’erreur généralement temporaire, vous pouvez recommencer l’opération après une brève attente et l’opération peut être réussie. Vous pouvez fournir une meilleure expérience à vos utilisateurs si vous traitez des erreurs temporaires en réessayant automatiquement, en les rendant invisibles au client. La fonctionnalité de résilience des connexions dans Entity Framework 6 automatise ce processus de nouvelle tentative d’échec de requêtes SQL.

La fonctionnalité de résilience des connexions doit être configurée de manière appropriée pour un service de base de données spécifique :

- Il doit savoir quelles exceptions sont susceptibles d’être temporaires. Vous souhaitez réessayer les erreurs causées par une perte temporaire de la connectivité réseau, et non les erreurs provoquées par les bogues de programme, par exemple.
- Il doit attendre un laps de temps approprié entre les nouvelles tentatives d’une opération ayant échoué. Vous pouvez attendre plus longtemps entre les nouvelles tentatives d’un traitement par lots que pour une page Web en ligne où un utilisateur attend une réponse.
- Il doit réessayer un nombre de fois approprié avant d’abandonner. Vous souhaiterez peut-être réessayer plus souvent dans un processus de traitement par lots que dans une application en ligne.

Vous pouvez configurer ces paramètres manuellement pour n’importe quel environnement de base de données pris en charge par un fournisseur de Entity Framework, mais les valeurs par défaut qui fonctionnent généralement correctement pour une application en ligne qui utilise Windows Azure SQL Database ont déjà été configurées pour vous, et Il s’agit des paramètres que vous allez implémenter pour l’application Contoso University.

Pour activer la résilience des connexions, il vous suffit de créer une classe dans votre assembly qui dérive de la classe [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) . dans cette classe, vous définissez la *stratégie d’exécution*de SQL Database, qui, dans EF, est un autre terme pour la *stratégie de nouvelle tentative*.

1. Dans le dossier DAL, ajoutez un fichier de classe nommé *SchoolConfiguration.cs*.
2. Remplacez le code du modèle par le code suivant :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Le Entity Framework exécute automatiquement le code qu’il trouve dans une classe qui dérive de `DbConfiguration`. Vous pouvez utiliser la classe `DbConfiguration` pour effectuer des tâches de configuration dans le code que vous feriez autrement dans le fichier *Web. config* . Pour plus d’informations, consultez la section relative [à la configuration basée sur le code EntityFramework](https://msdn.microsoft.com/data/jj680699).
3. Dans *StudentController.cs*, ajoutez une instruction `using` pour `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Modifiez tous les blocs de `catch` qui interceptent `DataException` exceptions afin qu’ils interceptent `RetryLimitExceededException` exceptions à la place. Exemple :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Vous utilisiez `DataException` pour tenter d’identifier les erreurs qui peuvent être temporaires afin d’obtenir un message « réessayer » convivial. Mais maintenant que vous avez activé une stratégie de nouvelle tentative, les seules erreurs susceptibles d’être temporaires ont déjà été tentées et échouent plusieurs fois et l’exception réelle retournée est encapsulée dans l’exception `RetryLimitExceededException`.

Pour plus d’informations, consultez [Entity Framework la résilience des connexions/logique de nouvelle tentative](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Activer l’interception des commandes

Maintenant que vous avez activé une stratégie de nouvelle tentative, comment effectuer un test pour vérifier qu’elle fonctionne comme prévu ? Il n’est pas tellement facile de forcer une erreur temporaire, en particulier lorsque vous exécutez localement, et il serait particulièrement difficile d’intégrer les erreurs temporaires réelles dans un test unitaire automatisé. Pour tester la fonctionnalité de résilience des connexions, vous avez besoin d’un moyen d’intercepter les requêtes que Entity Framework envoie à SQL Server et de remplacer la réponse SQL Server par un type d’exception généralement temporaire.

Vous pouvez également utiliser l’interception des requêtes afin d’implémenter une meilleure pratique pour les applications Cloud : [consigner la latence et la réussite ou l’échec de tous les appels aux services externes](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) , tels que les services de base de données. EF6 fournit une [API de journalisation dédiée](https://msdn.microsoft.com/data/dn469464) qui facilite la journalisation, mais dans cette section du didacticiel, vous allez apprendre à utiliser la [fonctionnalité d’interception](https://msdn.microsoft.com/data/dn469464) de Entity Framework directement, à la fois pour la journalisation et pour la simulation des erreurs temporaires.

### <a name="create-a-logging-interface-and-class"></a>Créer une interface et une classe de journalisation

[Pour la journalisation](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) , il est recommandé de le faire à l’aide d’une interface plutôt que de coder en dur des appels à System. Diagnostics. trace ou à une classe de journalisation. Cela facilite la modification de votre mécanisme de journalisation ultérieurement si vous avez besoin de le faire. Ainsi, dans cette section, vous allez créer l’interface de journalisation et une classe pour l’implémenter./p >

1. Créez un dossier dans le projet et nommez-le *journalisation*.
2. Dans le dossier de *journalisation* , créez un fichier de classe nommé *ILogger.cs*et remplacez le code du modèle par le code suivant :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    L’interface fournit trois niveaux de suivi pour indiquer l’importance relative des journaux, et un autre conçu pour fournir des informations de latence pour les appels de service externe, tels que les requêtes de base de données. Les méthodes de journalisation ont des surcharges qui vous permettent de passer une exception. Ainsi, les informations sur les exceptions, y compris la trace de la pile et les exceptions internes, sont journalisées de façon fiable par la classe qui implémente l’interface, au lieu de s’appuyer sur cette opération dans chaque appel de méthode de journalisation au sein de l’application.

    Les méthodes TraceApi vous permettent d’effectuer le suivi de la latence de chaque appel à un service externe, tel que SQL Database.
3. Dans le dossier de *journalisation* , créez un fichier de classe nommé *logger.cs*et remplacez le code du modèle par le code suivant :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    L’implémentation utilise System. Diagnostics pour effectuer le suivi. Il s’agit d’une fonctionnalité intégrée de .NET qui facilite la génération et l’utilisation des informations de suivi. Il existe de nombreux « écouteurs » que vous pouvez utiliser avec le suivi System. Diagnostics, pour écrire des journaux dans des fichiers, par exemple, ou pour les écrire dans le stockage d’objets BLOB dans Azure. Pour plus d’informations sur la [résolution des problèmes liés aux sites Web Azure dans Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio), consultez certaines options et liens vers d’autres ressources. Pour ce didacticiel, vous allez examiner uniquement les journaux dans la fenêtre **sortie** de Visual Studio.

    Dans une application de production, vous souhaiterez peut-être envisager des packages de suivi autres que System. Diagnostics, et l’interface ILogger permet de basculer relativement facilement vers un mécanisme de suivi différent si vous décidez de le faire.

### <a name="create-interceptor-classes"></a>Créer des classes d’intercepteur

Ensuite, vous allez créer les classes que le Entity Framework appellera à chaque fois qu’il enverra une requête à la base de données, une pour simuler des erreurs temporaires et une pour la journalisation. Ces classes d’intercepteur doivent dériver de la classe `DbCommandInterceptor`. Dans, vous écrivez des substitutions de méthode qui sont appelées automatiquement quand la requête est sur le paragraphe d’être exécutée. Dans ces méthodes, vous pouvez examiner ou enregistrer la requête qui est envoyée à la base de données, et vous pouvez modifier la requête avant de l’envoyer à la base de données ou retourner un objet à Entity Framework vous-même sans même passer la requête à la base de données.

1. Pour créer la classe d’intercepteur qui journalise chaque requête SQL envoyée à la base de données, créez un fichier de classe nommé *SchoolInterceptorLogging.cs* dans le dossier *dal* , puis remplacez le code du modèle par le code suivant :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Pour les requêtes ou les commandes ayant abouti, ce code écrit un journal d’informations avec des informations de latence. Pour les exceptions, il crée un journal des erreurs.
2. Pour créer la classe d’intercepteur qui génère des erreurs temporaires factices lorsque vous entrez « Throw » dans la zone de **recherche** , créez un fichier de classe nommé *SchoolInterceptorTransientErrors.cs* dans le dossier *dal* , puis remplacez le code du modèle par le code suivant :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Ce code substitue uniquement la méthode `ReaderExecuting`, qui est appelée pour les requêtes qui peuvent retourner plusieurs lignes de données. Si vous souhaitez vérifier la résilience des connexions pour d’autres types de requêtes, vous pouvez également remplacer les méthodes `NonQueryExecuting` et `ScalarExecuting`, comme le fait l’intercepteur de journalisation.

    Quand vous exécutez la page Student et que vous entrez « Throw » comme chaîne de recherche, ce code crée une exception de SQL Database factice pour l’erreur numéro 20, un type connu généralement comme temporaire. Les autres numéros d’erreur actuellement reconnus comme temporaires sont 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 et 40613, mais ceux-ci sont susceptibles d’être modifiés dans les nouvelles versions de SQL Database.

    Le code retourne l’exception à Entity Framework au lieu d’exécuter la requête et de renvoyer les résultats de la requête. L’exception temporaire est retournée quatre fois, puis le code revient à la procédure normale de transmission de la requête à la base de données.

    Étant donné que tout est journalisé, vous pouvez voir que Entity Framework tente d’exécuter la requête quatre fois avant la fin de l’opération, et la seule différence dans l’application est qu’il faut plus de temps pour afficher une page avec les résultats de la requête.

    Le nombre de nouvelles tentatives de Entity Framework peut être configuré ; le code spécifie quatre fois, car il s’agit de la valeur par défaut pour la stratégie d’exécution SQL Database. Si vous modifiez la stratégie d’exécution, vous modifiez également le code qui spécifie le nombre de fois où des erreurs temporaires sont générées. Vous pouvez également modifier le code pour générer plus d’exceptions afin que Entity Framework lève l’exception `RetryLimitExceededException`.

    La valeur que vous entrez dans la zone de recherche est `command.Parameters[0]` et `command.Parameters[1]` (l’un est utilisé pour le prénom et l’autre pour le nom de famille). Quand la valeur « % Throw% » est trouvée, « Throw » est remplacé dans ces paramètres par « an » afin que certains élèves soient recherchés et retournés.

    Il s’agit simplement d’un moyen pratique de tester la résilience des connexions en fonction de la modification d’une entrée de l’interface utilisateur de l’application. Vous pouvez également écrire du code qui génère des erreurs transitoires pour toutes les requêtes ou mises à jour, comme expliqué plus loin dans les commentaires sur la méthode *DbInterception. Add* .
3. Dans *global. asax*, ajoutez les instructions `using` suivantes :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Ajoutez les lignes en surbrillance à la méthode `Application_Start` :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Ces lignes de code provoquent l’exécution de votre code d’intercepteur quand Entity Framework envoie des requêtes à la base de données. Notez que, étant donné que vous avez créé des classes d’intercepteur distinctes pour la simulation et la journalisation des erreurs temporaires, vous pouvez les activer et les désactiver indépendamment.

    Vous pouvez ajouter des intercepteurs à l’aide de la méthode `DbInterception.Add` n’importe où dans votre code ; elle n’a pas besoin d’être dans la méthode `Application_Start`. Une autre option consiste à placer ce code dans la classe DbConfiguration que vous avez créée précédemment pour configurer la stratégie d’exécution.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Quel que soit l’endroit où vous placez ce code, veillez à ne pas exécuter plusieurs `DbInterception.Add` pour le même intercepteur, sinon vous obtiendrez des instances d’intercepteur supplémentaires. Par exemple, si vous ajoutez deux fois l’intercepteur de journalisation, vous verrez deux journaux pour chaque requête SQL.

    Les intercepteurs sont exécutés dans l’ordre d’inscription (l’ordre dans lequel la méthode `DbInterception.Add` est appelée). L’ordre peut être important en fonction de ce que vous effectuez dans l’intercepteur. Par exemple, un intercepteur peut modifier la commande SQL qu’il obtient dans la propriété `CommandText`. Si la commande SQL est modifiée, l’intercepteur suivant obtient la commande SQL modifiée, et non la commande SQL d’origine.

    Vous avez écrit le code de simulation d’erreur temporaire d’une manière qui vous permet de générer des erreurs temporaires en entrant une valeur différente dans l’interface utilisateur. Vous pouvez également écrire le code de l’intercepteur pour toujours générer la séquence d’exceptions transitoires sans vérifier une valeur de paramètre particulière. Vous pouvez ensuite ajouter l’intercepteur uniquement lorsque vous souhaitez générer des erreurs temporaires. Toutefois, si vous procédez ainsi, n’ajoutez pas l’intercepteur jusqu’à la fin de l’initialisation de la base de données. En d’autres termes, effectuez au moins une opération de base de données telle qu’une requête sur l’un de vos jeux d’entités avant de commencer à générer des erreurs temporaires. Le Entity Framework exécute plusieurs requêtes pendant l’initialisation de la base de données, et elles ne sont pas exécutées dans une transaction ; par conséquent, les erreurs au cours de l’initialisation peuvent amener le contexte à passer à un état incohérent.

## <a name="test-the-new-configuration"></a>Tester la nouvelle configuration

1. Appuyez sur **F5** pour exécuter l’application en mode débogage, puis cliquez sur l’onglet **élèves** .
2. Examinez la fenêtre **sortie** de Visual Studio pour afficher la sortie de traçage. Vous devrez peut-être faire défiler vers le haut certaines erreurs JavaScript pour accéder aux journaux écrits par votre enregistreur d’événements.

    Notez que vous pouvez voir les requêtes SQL réelles envoyées à la base de données. Vous voyez des requêtes et des commandes initiales que Entity Framework fait pour démarrer, vérifier la version de la base de données et la table de l’historique de la migration (vous en apprendrez plus sur les migrations dans le didacticiel suivant). Et vous voyez une requête pour la pagination, pour connaître le nombre d’élèves, et enfin, vous voyez la requête qui obtient les données de l’étudiant.

    ![Journalisation pour une requête normale](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Dans la page **étudiants** , entrez « Throw » comme chaîne de recherche, puis cliquez sur **Rechercher**.

    ![Chaîne de recherche de levée](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Vous remarquerez que le navigateur semble se bloquer pendant plusieurs secondes alors que Entity Framework retente plusieurs fois la requête. La première tentative se produit très rapidement, puis l’attente avant chaque nouvelle tentative supplémentaire. Ce processus d’attente plus longtemps avant chaque nouvelle tentative est appelé interruption *exponentielle*.

    Lorsque la page s’affiche, affichant les étudiants dont le nom contient « an », examinez la fenêtre sortie et vous verrez que la même requête a été tentée cinq fois, les quatre premières fois qui retournent des exceptions transitoires. Pour chaque erreur temporaire, vous verrez le journal que vous écrivez lors de la génération de l’erreur temporaire dans la classe `SchoolInterceptorTransientErrors` (« retour d’une erreur temporaire pour la commande... ») et vous verrez le journal écrit lorsque `SchoolInterceptorLogging` obtient l’exception.

    ![Sortie de journalisation avec nouvelles tentatives](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Étant donné que vous avez entré une chaîne de recherche, la requête qui retourne les données d’étudiant est paramétrable :

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Vous n’êtes pas en train d’enregistrer la valeur des paramètres, mais vous pouvez le faire. Si vous souhaitez afficher les valeurs des paramètres, vous pouvez écrire le code de journalisation pour obtenir les valeurs de paramètre à partir de la propriété `Parameters` de l’objet `DbCommand` que vous obtenez dans les méthodes d’intercepteur.

    Notez que vous ne pouvez pas répéter ce test, sauf si vous arrêtez l’application et que vous la redémarrez. Si vous souhaitez être en mesure de tester la résilience de connexion plusieurs fois dans une seule exécution de l’application, vous pouvez écrire du code pour réinitialiser le compteur d’erreurs dans `SchoolInterceptorTransientErrors`.
4. Pour voir la différence entre la stratégie d’exécution (stratégie de nouvelle tentative), commentez la ligne `SetExecutionStrategy` dans *SchoolConfiguration.cs*, réexécutez la page Students en mode débogage et recherchez « throw ».

    Cette fois, le débogueur s’arrête immédiatement sur la première exception générée lorsqu’il essaie d’exécuter la requête la première fois.

    ![Exception factice](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Supprimez les marques de commentaire de la ligne *SetExecutionStrategy* dans *SchoolConfiguration.cs*.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Des liens vers d’autres ressources de Entity Framework sont disponibles dans [accès aux données ASP.net-ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Résilience de connexion activée
> * Interception de commande activée
> * Test de la nouvelle configuration

Passez à l’article suivant pour en savoir plus sur les migrations de Code First et le déploiement d’Azure.
> [!div class="nextstepaction"]
> [Migrations de Code First et déploiement Azure](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
