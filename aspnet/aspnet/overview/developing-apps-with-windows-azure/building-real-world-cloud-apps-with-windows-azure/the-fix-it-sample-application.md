---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Annexe : exemple d’application Fix it (création d’applications Cloud réalistes avec Azure) | Microsoft Docs'
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 896196bdb6a6b0d12a6c798ead510e37dd38a9fc
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456879"
---
# <a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Annexe : exemple d’application Fix it (création d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur l’e-Book, consultez [le premier chapitre](introduction.md).

Cette annexe à la création d’applications Cloud réelles avec Azure e-Book contient les sections suivantes qui fournissent des informations supplémentaires sur l’exemple d’application Fix it que vous pouvez télécharger :

- [Problèmes connus](#knownissues)
- [Bonnes pratiques](#bestpractices)
- [Comment exécuter l’application à partir de Visual Studio sur votre ordinateur local](#run-in-vs)
- [Comment déployer l’application de base pour Azure App Service Web Apps à l’aide des scripts Windows PowerShell](#deploybase)
- [Résolution des problèmes liés aux scripts Windows PowerShell](#troubleshooting)
- [Comment déployer l’application avec le traitement de la file d’attente pour Azure App Service Web Apps et un service Cloud Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problèmes connus

L’application Fix it a été développée à l’origine afin d’illustrer le plus simplement possible certains des modèles présentés dans ce livre électronique. Toutefois, dans la mesure où l’e-Book concerne la création d’applications réelles, nous avons soumis le code de correction informatique à un processus de révision et de test similaire à ce que nous faisons pour les logiciels publiés. Nous avons trouvé un certain nombre de problèmes et, comme avec n’importe quelle application réelle, certaines d’entre elles ont été corrigées et certaines d’entre elles ont été reportées vers une version ultérieure.

La liste suivante répertorie les problèmes qui doivent être résolus dans une application de production, mais pour une raison ou une autre, nous avons décidé de ne pas traiter dans la version initiale de l’exemple d’application Fix it.

### <a name="security"></a>Sécurité

- Assurez-vous que vous ne pouvez pas assigner une tâche à un propriétaire inexistant.
- Assurez-vous que vous pouvez afficher et modifier uniquement les tâches que vous avez créées ou qui vous sont attribuées.
- Utilisez le protocole HTTPs pour les pages de connexion et les cookies d’authentification.
- Spécifiez une limite de temps pour les cookies d’authentification.

### <a name="input-validation"></a>Validation des entrées

En général, une application de production effectue davantage de validation d’entrée que l’application Fix it. Par exemple, la taille de l’image/la taille du fichier image autorisée pour le téléchargement doit être limitée.

### <a name="administrator-functionality"></a>Fonctionnalité administrateur

Un administrateur doit être en mesure de modifier la propriété des tâches existantes. Par exemple, le créateur d’une tâche peut quitter l’entreprise, sans qu’il soit autorisé à gérer la tâche, sauf si l’accès administratif est activé.

### <a name="queue-message-processing"></a>Traitement des messages de la file d’attente

Le traitement des messages en file d’attente dans l’application Fix it a été conçu pour être simple afin d’illustrer le modèle de travail centré sur la file d’attente avec une quantité minimale de code. Ce code simple n’est pas adapté à une application de production réelle.

- Le code ne garantit pas que chaque message de file d’attente sera traité au plus une fois. Lorsque vous recevez un message de la file d’attente, il y a un délai d’attente pendant lequel le message est invisible pour d’autres écouteurs de file d’attente. Si le délai d’attente expire avant que le message ne soit supprimé, le message redevient visible. Par conséquent, si une instance de rôle de travail passe beaucoup de temps à traiter un message, il est théoriquement possible que le même message soit traité deux fois, ce qui entraîne une tâche dupliquée dans la base de données. Pour plus d’informations sur ce problème, consultez [utilisation des files d’attente stockage Azure](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- La logique d’interrogation de la file d’attente peut être plus économique, en regroupant par lot la récupération des messages. Chaque fois que vous appelez [CloudQueue. GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), il y a un coût de transaction. Au lieu de cela, vous pouvez appeler [CloudQueue. GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (Notez le pluriel), qui obtient plusieurs messages dans une transaction unique. Les coûts des transactions pour les files d’attente Azure Storage sont très faibles, l’impact sur les coûts n’est donc pas important dans la plupart des scénarios.
- La boucle serrée dans le code de traitement de message de file d’attente provoque une affinité du processeur, qui n’utilise pas efficacement les machines virtuelles multicœurs. Une meilleure conception utilise le parallélisme des tâches pour exécuter plusieurs tâches asynchrones en parallèle.
- Le traitement des messages en file d’attente n’a que la gestion rudimentaire des exceptions. Par exemple, le code ne gère pas les [messages incohérents](https://msdn.microsoft.com/library/ms789028.aspx). (Lorsque le traitement des messages provoque une exception, vous devez consigner l’erreur et supprimer le message, ou le rôle de travail tentera de le traiter à nouveau et la boucle se poursuivra indéfiniment.)

### <a name="sql-queries-are-unbounded"></a>Les requêtes SQL ne sont pas liées

Le code de correction actuel n’impose aucune limite au nombre de lignes que les requêtes pour les pages d’index peuvent retourner. Si un grand nombre de tâches est entré dans la base de données, la taille des listes résultantes reçues peut entraîner des problèmes de performances. La solution consiste à implémenter la pagination. Pour obtenir un exemple, consultez [Tri, filtrage et pagination avec le Entity Framework dans une application MVC ASP.net](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Afficher les modèles recommandés

L’application Fix It utilise la classe d’entité FixItTask pour transmettre des informations entre le contrôleur et la vue. Une meilleure pratique consiste à utiliser des modèles de vue. Le modèle de domaine (par exemple, la classe d’entité FixItTask) est conçu autour de ce qui est nécessaire pour la persistance des données, tandis qu’un modèle de vue peut être conçu pour la présentation des données. Pour plus d’informations, consultez [12 pratiques recommandées pour MVC ASP.net](https://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Objet blob d’images sécurisées recommandé

L’application Fix it stocke les images chargées comme étant publiques, ce qui signifie que toute personne qui trouve l’URL peut accéder aux images. Les images peuvent être sécurisées au lieu d’être publiques.

### <a name="no-powershell-automation-scripts-for-queues"></a>Aucun script d’automatisation PowerShell pour les files d’attente

Les exemples de scripts d’automatisation PowerShell ont été écrits uniquement pour la version de base de Fix it qui s’exécute entièrement dans Azure App Service Web Apps. Nous n’avons pas fourni de scripts pour la configuration et le déploiement de l’application Web, plus l’environnement de service Cloud requis pour le traitement de la file d’attente.

### <a name="special-handling-for-html-codes-in-user-input"></a>Gestion spéciale des codes HTML dans l’entrée utilisateur

ASP.NET empêche automatiquement les utilisateurs malveillants de tenter d’effectuer des attaques de script entre sites en entrant un script dans les zones de texte entrée utilisateur. Et le service d’assistance du `DisplayFor` MVC utilisé pour afficher les titres des tâches et les notes encodent automatiquement les valeurs qu’il envoie au navigateur. Mais dans une application de production, vous souhaiterez peut-être prendre des mesures supplémentaires. Pour plus d’informations, consultez [validation de la demande dans ASP.net](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Meilleures pratiques

Voici quelques problèmes qui ont été résolus après avoir été découverts dans la révision du code et les tests de la version d’origine de l’application Fix it. Certains ont été provoqués par le codeur d’origine qui n’a pas conscience d’une bonne pratique particulière, tout simplement parce que le code a été écrit rapidement et n’était pas destiné aux logiciels publiés. Nous répertorions les problèmes ici, au cas où nous avons appris cette revue et les tests, qui peuvent être utiles aux personnes qui développent également des applications Web.

### <a name="dispose-the-database-repository"></a>Supprimer le référentiel de base de données

La classe `FixItTaskRepository` doit supprimer l’instance `DbContext` Entity Framework. Nous l’avons fait en implémentant `IDisposable` dans la classe `FixItTaskRepository` :

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Notez que AutoFac supprimera automatiquement l’instance de `FixItTaskRepository`, nous n’avons donc pas besoin de la supprimer explicitement.

Une autre option consiste à supprimer la variable de membre `DbContext` de `FixItTaskRepository`et à créer une variable `DbContext` locale dans chaque méthode de dépôt, à l’intérieur d’une instruction `using`. Par exemple :

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Inscrire des singletons comme tels avec DI

Étant donné qu’une seule instance de la classe `PhotoService` et `Logger` classe est nécessaire, ces classes doivent être [inscrites en tant qu’instances uniques pour l’injection de dépendances](https://code.google.com/p/autofac/wiki/InstanceScope) dans *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Sécurité : ne pas afficher les détails de l’erreur aux utilisateurs

L’application de réparation d’origine ne contenait pas de page d’erreur générique et permettait simplement aux exceptions de se propager jusqu’à l’interface utilisateur. par conséquent, certaines exceptions, telles que les erreurs de connexion de base de données, peuvent entraîner l’affichage d’une trace de pile complète dans le navigateur. Les informations d’erreur détaillées peuvent parfois faciliter les attaques d’utilisateurs malveillants. La solution consiste à enregistrer les détails de l’exception et à afficher une page d’erreur pour l’utilisateur qui n’inclut pas les détails de l’erreur. L’application Fix It était déjà en cours d’enregistrement et, afin d’afficher une page d’erreur, nous avons ajouté `<customErrors mode=On>` dans le fichier Web. config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Par défaut, *Views\Shared\Error.cshtml* est affiché pour les erreurs. Vous pouvez personnaliser *Error. cshtml* ou créer votre propre affichage de page d’erreur et ajouter un attribut `defaultRedirect`. Vous pouvez également spécifier des pages d’erreurs différentes pour des erreurs spécifiques.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Sécurité : autoriser uniquement la modification d’une tâche par son créateur

La page d’index du tableau de bord affiche uniquement les tâches créées par l’utilisateur connecté, mais un utilisateur malveillant peut créer une URL avec un ID pour la tâche d’un autre utilisateur. Nous avons ajouté du code dans *DashboardController.cs* pour retourner un 404 dans ce cas :

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Ne pas absorber les exceptions

L’application de correction d’origine renvoyait la valeur null après la journalisation d’une exception résultant d’une requête SQL :

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Cela lui permettrait d’afficher l’utilisateur comme si la requête avait réussi, mais n’a pas renvoyé de lignes. La solution consiste à lever à nouveau l’exception après l’interception et la journalisation :

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Intercepter toutes les exceptions dans les rôles de travail

Toutes les exceptions non gérées dans un rôle de travail entraînent le recyclage de la machine virtuelle. vous devez donc encapsuler tout ce que vous faites dans un bloc try-catch et gérer toutes les exceptions.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Spécifier la longueur des propriétés de type chaîne dans les classes d’entité

Pour afficher du code simple, la version d’origine de l’application Fix it n’a pas spécifié de longueurs pour les champs de l’entité FixItTask et, par conséquent, elle a été définie en tant que varchar (max) dans la base de données. Par conséquent, l’interface utilisateur accepte presque n’importe quel nombre d’entrées. La spécification de longueurs définit des limites qui s’appliquent aux entrées d’utilisateur dans la page Web et à la taille des colonnes dans la base de données :

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Marquer les membres privés comme ReadOnly lorsqu’ils ne sont pas censés changer

Par exemple, dans la classe `DashboardController`, une instance de `FixItTaskRepository` est créée et n’est pas censée changer, donc nous l’avons définie comme [ReadOnly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Utilisez la liste. Any () au lieu de List. Count () &gt; 0

Si vous vous souciez de savoir si un ou plusieurs éléments d’une liste correspondent aux critères spécifiés, utilisez la méthode [any](https://msdn.microsoft.com/library/bb534972.aspx) , parce qu’elle retourne dès qu’un élément ajustant les critères est trouvé, tandis que la méthode `Count` doit toujours itérer au sein de chaque élément. Le fichier *index. cshtml* du tableau de bord contenait initialement ce code :

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Nous l’avons modifié comme suit :

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Générer des URL dans les vues MVC à l’aide des applications auxiliaires MVC

Pour le bouton **créer un correctif** sur la page d’hébergement, l’application Fix it a codé en dur un élément d’ancrage :

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Pour les liens affichage/action de ce type, il est préférable d’utiliser le programme d’assistance HTML [URL. action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) , par exemple :

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Utilisez Task. Delay au lieu de thread. Sleep dans le rôle worker

Le nouveau modèle de projet place `Thread.Sleep` dans l’exemple de code pour un rôle de travail, mais le fait de mettre le thread en veille peut entraîner la génération par le pool de threads d’autres threads inutiles. Vous pouvez éviter cela en utilisant [Task. Delay](https://msdn.microsoft.com/library/hh139096.aspx) à la place.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Éviter Async void

Si une méthode Async n’a pas besoin de retourner une valeur, retournez un type `Task` plutôt que `void`.

Cet exemple provient de la classe `FixItQueueManager` :

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Vous devez utiliser `async void` uniquement pour les gestionnaires d’événements de niveau supérieur. Si vous définissez une méthode comme `async void`, l’appelant ne peut pas **attendre** la méthode ou intercepter les exceptions levées par la méthode. Pour plus d’informations, consultez [meilleures pratiques en matière de programmation asynchrone](https://msdn.microsoft.com/magazine/jj991977.aspx).

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Utiliser un jeton d’annulation pour interrompre la boucle de rôle de travail

En règle générale, la méthode **Run** sur un rôle de travail contient une boucle infinie. Lorsque le rôle de travail s’arrête, la méthode [RoleEntryPoint. OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) est appelée. Vous devez utiliser cette méthode pour annuler le travail effectué à l’intérieur de la méthode **Run** et quitter correctement. Dans le cas contraire, le processus peut se terminer au milieu d’une opération.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Refuser la procédure de détection MIME automatique

Dans certains cas, Internet Explorer signale un type MIME différent du type spécifié par le serveur Web. Par exemple, si Internet Explorer trouve du contenu HTML dans un fichier fourni avec l’en-tête de réponse HTTP Content-type : text/plain, Internet Explorer détermine que le contenu doit être rendu au format HTML. Malheureusement, cette « MIME-détection » peut également entraîner des problèmes de sécurité pour les serveurs qui hébergent du contenu non approuvé. Pour combattre ce problème, Internet Explorer 8 a apporté un certain nombre de modifications au code de la détermination de type MIME et permet aux développeurs d’applications de [refuser la détection MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). Le code suivant a été ajouté au fichier *Web. config* .

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Activer le regroupement et la minimisation

Lorsque Visual Studio crée un nouveau projet Web, le regroupement et la minimisation des fichiers JavaScript ne sont pas activés par défaut. Nous avons ajouté une ligne de code dans BundleConfig.cs :

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Définir un délai d’expiration pour les cookies d’authentification

Par défaut, les cookies d’authentification expirent dans deux semaines. Une durée plus petite est plus sécurisée. Vous pouvez modifier ce paramètre dans *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Comment exécuter l’application à partir de Visual Studio sur votre ordinateur local

Il existe deux façons d’exécuter l’application Fix it :

- Exécutez l’application de base qui écrit les nouvelles tâches directement dans la base de données SQL.
- Exécutez l’application à l’aide d’une file d’attente plus un service principal pour créer des tâches. Le modèle de file d’attente est décrit dans le chapitre [modèle de travail centré sur la file d’attente](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Exécuter l’application de base

1. Installer [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).
2. Installez le [Kit de développement logiciel (SDK) Azure pour .net pour Visual Studio](https://azure.microsoft.com/downloads/).
3. Téléchargez le fichier. zip à partir de la [Galerie de code MSDN](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. Dans l’Explorateur de fichiers, cliquez avec le bouton droit sur le fichier. zip, cliquez sur Propriétés, puis dans la Fenêtre Propriétés cliquez sur débloquer.
5. Décompressez le fichier.
6. Double-cliquez sur le fichier. sln pour lancer Visual Studio.
7. Dans le menu **Outils** , cliquez sur **Gestionnaire de package NuGet**, puis sur **console du gestionnaire de package**.
8. Dans la console du gestionnaire de package (PMC), cliquez sur restaurer.
9. Quittez Visual Studio.
10. Démarrez l' [émulateur de stockage Azure](/azure/storage/common/storage-use-emulator).
11. Redémarrez Visual Studio, en ouvrant le fichier solution que vous avez fermé à l’étape précédente.
12. Assurez-vous que le projet FixIt est défini comme projet de démarrage, puis appuyez sur CTRL + F5 pour exécuter le projet.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Exécuter l’application avec traitement de la file d’attente

1. Suivez les instructions pour [exécuter l’application de base](#runbase), puis fermez le navigateur et fermez Visual Studio.
2. Démarrez Visual Studio avec des privilèges d’administrateur. (Vous allez utiliser l’émulateur de calcul Azure et cela nécessite des privilèges d’administrateur.)
3. Dans le fichier *Web. config* de l’application dans le projet *MyFixIt* (le projet Web), remplacez la valeur de `appSettings/UseQueues` par « true » :

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Si l' [émulateur de stockage Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) n’est pas encore en cours d’exécution, redémarrez-le.
5. Exécutez le projet Web FixIt et le projet MyFixItCloudService simultanément.

    À l’aide de Visual Studio :

   1. Appuyez sur **F5** pour exécuter le projet FixIt.
   2. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet MyFixItCloudService, puis cliquez sur **déboguer** > **Démarrer une nouvelle instance**.

    Utilisation de Visual Studio 2013 Express pour le Web :

   3. Dans Explorateur de solutions, cliquez avec le bouton droit sur la solution FixIt et sélectionnez **Propriétés**.
   4. Sélectionnez **plusieurs projets de démarrage**.
   5. Dans la liste déroulante **action** sous MyFixIt et MyFixItCloudService, sélectionnez **Démarrer**.
   6. Cliquez sur **OK**.
   7. Appuyez sur **F5** pour exécuter les deux projets.

      Quand vous exécutez le projet MyFixItCloudService, Visual Studio démarre l’émulateur de calcul Azure. Selon la configuration de votre pare-feu, vous devrez peut-être autoriser l’émulateur via le pare-feu.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Comment déployer l’application de base pour Azure App Service Web Apps à l’aide des scripts Windows PowerShell

Pour illustrer le modèle [automatiser tout](automate-everything.md) , l’application Fix it est fournie avec des scripts qui configurent un environnement dans Azure et déploient le projet dans le nouvel environnement. Les instructions suivantes expliquent comment utiliser les scripts.

Si vous souhaitez exécuter dans Azure sans utiliser de files d’attente et que vous avez apporté les modifications à exécuter localement avec les files d’attente, veillez à rétablir la valeur du appSetting UseQueues sur false avant de passer aux instructions suivantes.

Ces instructions supposent que vous avez déjà téléchargé et exécuté la solution Fix it localement, et que vous disposez d’un compte Azure ou d’un abonnement Azure que vous êtes autorisé à gérer.

1. Installez la console **Azure PowerShell** . Pour obtenir des instructions, consultez la rubrique [Installation et configuration d'Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Cette console personnalisée est configurée pour fonctionner avec votre abonnement Azure. Le module Azure est installé dans le répertoire *Program Files* et est automatiquement importé à chaque utilisation de la console Azure PowerShell.

    Si vous préférez travailler dans un autre programme hôte, tel que Windows PowerShell ISE, veillez à utiliser l’applet de commande [import-module](https://go.microsoft.com/fwlink/?LinkID=141553) pour importer le module Azure ou utilisez une commande dans le module Azure pour déclencher l’importation automatique du module.
2. Démarrez Azure PowerShell avec l’option **exécuter en tant qu’administrateur** .
3. Exécutez l’applet de commande [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) pour définir la stratégie d’exécution de Azure PowerShell sur `RemoteSigned`. Entrez **Y** (pour Oui) pour terminer la modification de la stratégie.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Ce paramètre vous permet d’exécuter des scripts locaux qui ne sont pas signés numériquement. (Vous pouvez également définir la stratégie d’exécution sur `Unrestricted`, ce qui élimine le besoin d’une étape de déblocage plus tard, mais cela n’est pas recommandé pour des raisons de sécurité.)
4. Exécutez l’applet de commande `Add-AzureAccount` pour configurer PowerShell avec les informations d’identification de votre compte.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Ces informations d’identification expirent après un certain laps de temps et vous devez exécuter à nouveau l’applet de commande `Add-AzureAccount`. Pendant l’écriture de ce livre électronique, la limite de temps avant l’expiration des informations d’identification est de 12 heures.
5. Si vous avez plusieurs abonnements, utilisez l’applet de commande SELECT-AzureSubscription pour spécifier l’abonnement dans lequel vous souhaitez créer l’environnement de test.
6. Importez un certificat de gestion pour le même abonnement Azure à l’aide des applets de commande `Get-AzurePublishSettingsFile` et `Import-AzurePublishSettingsFile`. La première de ces applets de commande télécharge un fichier de certificat et, dans la deuxième, vous spécifiez l’emplacement de ce fichier afin de l’importer. > [!IMPORTANT]
   > Conservez le fichier téléchargé dans un emplacement sûr ou supprimez-le lorsque vous n’en avez plus besoin, car il contient un certificat qui peut être utilisé pour gérer vos services Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Le certificat est utilisé pour un appel d’API REST qui détecte l’adresse IP de l’ordinateur de développement afin de définir une règle de pare-feu sur le serveur de SQL Database.
7. Exécutez l’applet de commande [set-location](https://go.microsoft.com/fwlink/p/?linkid=293912) (les alias sont `cd`, `chdir`et `sl`) pour accéder au répertoire qui contient les scripts. (Ils se trouvent dans le dossier *Automation* du dossier Fix it solution.) Placez le chemin d’accès entre guillemets si l’un des noms de répertoire contient des espaces. Par exemple, pour accéder au répertoire `c:\Sample Apps\FixIt\Automation`, vous pouvez entrer la commande suivante :

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Pour permettre à Windows PowerShell d’exécuter ces scripts, utilisez l’applet de commande [Unblock-file](https://go.microsoft.com/fwlink/p/?linkid=294021) . (Les scripts sont bloqués, car ils ont été téléchargés à partir d’Internet.)

    > [!WARNING]
    > Sécurité : avant d’exécuter `Unblock-File` sur un script ou un fichier exécutable, ouvrez le fichier dans le bloc-notes, examinez les commandes et assurez-vous qu’elles ne contiennent pas de code malveillant.

    Par exemple, la commande suivante exécute l’applet de commande `Unblock-File` sur tous les scripts dans le répertoire actif.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Pour créer l’application Web pour le correctif de base (sans traitement de files d’attente), exécutez le script de création de l’environnement.

    Le paramètre `Name` requis spécifie le nom de la base de données et est également utilisé pour le compte de stockage créé par le script. Le nom doit être globalement unique dans le domaine azurewebsites.net. Si vous spécifiez un nom qui n’est pas unique, comme FixIt ou test (ou même comme dans l’exemple, fixitdemo), l’applet de commande `New-AzureWebsite` échoue avec une erreur interne qui signale un conflit. Le script convertit le nom en minuscules pour se conformer aux spécifications de noms pour les applications Web, les comptes de stockage et les bases de données.

    Le paramètre `SqlDatabasePassword` requis spécifie le mot de passe du compte d’administrateur qui sera créé pour SQL Database. N’incluez pas de caractères XML spéciaux dans le mot de passe (&amp; &lt; &gt; ;). Il s’agit d’une limitation de la façon dont les scripts ont été écrits, et non d’une limitation d’Azure.

    Par exemple, si vous souhaitez créer une application Web nommée « fixitdemo » et utiliser un mot de passe d’administrateur SQL Server « Passw0rd1 », vous pouvez entrer la commande suivante :

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Le nom doit être unique dans le domaine azurewebsites.net et le mot de passe doit répondre aux exigences SQL Database pour la complexité du mot de passe. (L’exemple Passw0rd1 répond à la configuration requise.)

    Notez que la commande commence par «.\". Pour empêcher l’exécution malveillante des scripts, Windows PowerShell exige que vous fournissiez le chemin d’accès complet au fichier de script lorsque vous exécutez un script. Vous pouvez utiliser un point pour indiquer le répertoire actif (".\") ou fournissez le chemin d’accès complet, par exemple :

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Pour plus d’informations sur le script, utilisez l’applet de commande `Get-Help`.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Vous pouvez utiliser les paramètres `Detailed`, `Full`, `Parameters`et `Examples` de l’applet de commande « obtenir-Help » pour filtrer l’aide qui est retournée.

    Si le script échoue ou génère des erreurs, telles que « New-AzureWebsite : Call Set-AzureSubscription et SELECT-AzureSubscription First », vous n’avez peut-être pas terminé la configuration de Azure PowerShell.

    Une fois le script terminé, vous pouvez utiliser le Portail de gestion Azure pour voir les ressources qui ont été créées, comme indiqué dans le chapitre [automatiser tout](automate-everything.md) .
10. Pour déployer le projet FixIt dans le nouvel environnement Azure, utilisez le script *AzureWebsite. ps1* . Par exemple :

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Une fois le déploiement terminé, le navigateur s’ouvre avec le correctif exécuté dans Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Résolution des problèmes liés aux scripts Windows PowerShell

Les erreurs les plus courantes rencontrées lors de l’exécution de ces scripts sont liées aux autorisations. Assurez-vous que `Add-AzureAccount` et `Import-AzurePublishSettingsFile` réussissent et que vous les avez utilisés pour le même abonnement Azure. Même si `Add-AzureAccount` a réussi, vous devrez peut-être l’exécuter à nouveau. Les autorisations ajoutées par `Add-AzureAccount` expirent dans 12 heures.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>La référence d'objet n'a pas la valeur d'une instance d'un objet.

Si le script retourne des erreurs, telles que « la référence d’objet n’est pas définie à une instance d’un objet », ce qui signifie que Windows PowerShell ne peut pas trouver un objet à traiter (il s’agit d’une exception de référence null), exécutez l’applet de commande `Add-AzureAccount` et recommencez le script.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError : le serveur a rencontré une erreur interne.

L’applet de commande `New-AzureWebsite` retourne une erreur interne lorsque le nom n’est pas unique dans le domaine azurewebsites.net. Pour résoudre l’erreur, utilisez une valeur différente pour le nom, qui est dans le paramètre Name de *New-AzureWebsiteEnv. ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Redémarrage du script

Si vous devez redémarrer le script *New-AzureWebsiteEnv. ps1* parce qu’il a échoué avant d’imprimer le message « le script est terminé », vous pouvez supprimer les ressources que le script a créées avant de s’arrêter. Par exemple, si le script a déjà créé l’application Web ContosoFixItDemo et que vous exécutez à nouveau le script avec le même nom, le script échouera car le nom est en cours d’utilisation.

Pour déterminer les ressources que le script a créées avant de s’arrêter, utilisez les applets de commande suivantes :

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: pour exécuter cette applet de commande, dirigez le nom du serveur de base de données vers `Get-AzureSqlDatabase`: `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Pour supprimer ces ressources, utilisez les commandes suivantes. Notez que si vous supprimez le serveur de base de données, vous supprimez automatiquement les bases de données associées au serveur.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Comment déployer l’application avec le traitement de la file d’attente pour Azure App Service Web Apps et un service Cloud Azure

Pour activer les files d’attente, apportez la modification suivante dans le fichier MyFixIt\Web.config. Sous `appSettings`, remplacez la valeur de `UseQueues` par « true » :

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Déployez ensuite l’application MVC sur une application Web dans Azure App Service, comme décrit [précédemment](#deploybase).

Ensuite, créez un service Cloud Azure. Les scripts fournis avec l’application Fix it ne créent ni ne déploient le service Cloud. vous devez donc utiliser Portail Azure pour cela. Dans le portail, cliquez sur **nouveau** -- **Compute** – **service Cloud** -- **création rapide**, puis entrez une URL et un emplacement de centre de données. Utilisez le même centre de données que celui dans lequel vous avez déployé l’application Web.

![](the-fix-it-sample-application/_static/image1.png)

Avant de pouvoir déployer le service Cloud, vous devez mettre à jour certains fichiers de configuration.

Dans MyFixIt. WorkerRole\app.config, sous `connectionStrings`, remplacez la valeur de la chaîne de connexion `appdb` par la chaîne de connexion réelle pour le SQL Database. Vous pouvez récupérer la chaîne de connexion à partir du portail. Dans le portail, cliquez sur **bases de données SQL** - **appdb** - **Afficher SQL Database chaînes de connexion pour ADO .net, ODBC, php et JDBC**. Copiez la chaîne de connexion ADO.NET et collez la valeur dans le fichier app. config. Remplacez « {Your\_Password\_here} » par votre mot de passe de base de données. (En supposant que vous avez utilisé les scripts pour déployer l’application MVC, vous avez spécifié le mot de passe de base de données dans le paramètre de script `SqlDatabasePassword`.)

Le résultat doit ressembler à ce qui suit :

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

Dans le même fichier MyFixIt. WorkerRole\app.config, sous `appSettings`, remplacez les deux valeurs d’espace réservé pour le compte de stockage Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Vous pouvez récupérer la clé d’accès à partir du portail. Consultez [Comment gérer des comptes de stockage](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

Dans MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, remplacez les deux mêmes valeurs d’espaces réservés pour le compte de stockage Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Vous êtes maintenant prêt à déployer le service Cloud. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet MyFixItCloudService et sélectionnez **publier**. Pour plus d’informations, consultez «[déployer l’application sur Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)», qui est dans la partie 2 de [ce didacticiel](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Précédent](more-patterns-and-guidance.md)
