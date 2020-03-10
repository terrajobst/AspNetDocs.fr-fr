---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Mise en cache | Microsoft Docs
author: microsoft
description: Une bonne compréhension de la mise en cache est importante pour une application ASP.NET efficace. ASP.NET 1. x propose trois options différentes pour la mise en cache ; ,... de mise en cache de sortie
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 4f0b021ca6ca151544dd9fb0587ed9e0cf14ff65
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575935"
---
# <a name="caching"></a>Mise en cache

par [Microsoft](https://github.com/microsoft)

> Une bonne compréhension de la mise en cache est importante pour une application ASP.NET efficace. ASP.NET 1. x propose trois options différentes pour la mise en cache ; mise en cache de la sortie, mise en cache des fragments et API du cache.

Une bonne compréhension de la mise en cache est importante pour une application ASP.NET efficace. ASP.NET 1. x propose trois options différentes pour la mise en cache ; mise en cache de la sortie, mise en cache des fragments et API du cache. ASP.NET 2,0 propose les trois méthodes suivantes, mais il ajoute des fonctionnalités supplémentaires importantes. Il existe plusieurs nouvelles dépendances de cache et les développeurs ont désormais la possibilité de créer également des dépendances de cache personnalisées. La configuration de la mise en cache a également été améliorée de manière significative dans ASP.NET 2,0.

## <a name="new-features"></a>Nouvelles fonctionnalités

## <a name="cache-profiles"></a>Profils de cache

Les profils de cache permettent aux développeurs de définir des paramètres de cache spécifiques qui peuvent ensuite être appliqués à des pages individuelles. Par exemple, si certaines pages doivent expirer du cache après 12 heures, vous pouvez facilement créer un profil de cache qui peut être appliqué à ces pages. Pour ajouter un nouveau profil de cache, utilisez la section &lt;outputCacheSettings&gt; dans le fichier de configuration. Par exemple, voici la configuration d’un profil de cache appelé *twoday* qui configure une durée de cache de 12 heures.

[!code-xml[Main](caching/samples/sample1.xml)]

Pour appliquer ce profil de cache à une page particulière, utilisez l’attribut CacheProfile de la directive @ OutputCache comme indiqué ci-dessous :

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Dépendances de cache personnalisées

ASP.NET 1. x développeurs effectué pour les dépendances de cache personnalisées. Dans ASP.NET 1. x, la classe CacheDependency était sealed, ce qui empêchait les développeurs de dériver leurs propres classes à partir de celle-ci. Dans ASP.NET 2,0, cette limitation est supprimée et les développeurs sont libres de développer leurs propres dépendances de cache personnalisées. La classe CacheDependency permet de créer une dépendance de cache personnalisée basée sur des fichiers, des répertoires ou des clés de cache.

Par exemple, le code ci-dessous crée une dépendance de cache personnalisée basée sur un fichier appelé stuff. xml situé à la racine de l’application Web :

[!code-csharp[Main](caching/samples/sample3.cs)]

Dans ce scénario, lorsque le fichier stuff. xml change, l’élément mis en cache est invalidé.

Il est également possible de créer une dépendance de cache personnalisée à l’aide de clés de cache. À l’aide de cette méthode, la suppression de la clé de cache invalidera les données mises en cache. L'exemple suivant illustre ce comportement :

[!code-csharp[Main](caching/samples/sample4.cs)]

Pour invalider l’élément qui a été inséré précédemment, supprimez simplement l’élément qui a été inséré dans le cache pour agir en tant que clé de cache.

[!code-csharp[Main](caching/samples/sample5.cs)]

Notez que la clé de l’élément qui joue le rôle de clé de cache doit être la même que la valeur ajoutée au tableau de clés de cache.

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>Dépendances de cache SQL basées sur l’interrogation (également appelées dépendances basées sur une table)

SQL Server 7 et 2000 utilisent le modèle d’interrogation pour les dépendances de cache SQL. Le modèle basé sur l’interrogation utilise un déclencheur sur une table de base de données qui est déclenchée lorsque les données de la table changent. Ce déclencheur met à jour un champ **correspondant changeid** dans la table de notification que ASP.net vérifie régulièrement. Si le champ **correspondant changeid** a été mis à jour, ASP.net sait que les données ont changé et invalide les données mises en cache.

> [!NOTE]
> SQL Server 2005 peut également utiliser le modèle d’interrogation, mais comme le modèle basé sur l’interrogation n’est pas le modèle le plus efficace, il est recommandé d’utiliser un modèle basé sur une requête (abordé plus tard) avec SQL Server 2005.

Pour qu’une dépendance de cache SQL utilisant le modèle d’interrogation fonctionne correctement, les notifications doivent être activées sur les tables. Cela peut être accompli par programmation à l’aide de la classe SqlCacheDependencyAdmin ou à l’aide de l’utilitaire ASPNET\_RegSql. exe.

La ligne de commande suivante inscrit la table Products dans la base de données Northwind située sur une instance SQL Server nommée *dBASE* pour la dépendance de cache SQL.

[!code-console[Main](caching/samples/sample6.cmd)]

Vous trouverez ci-dessous une explication des commutateurs de ligne de commande utilisés dans la commande ci-dessus :

| **Commutateur de ligne de commande** | **Fonction** |
| --- | --- |
| -S *server* | Spécifie le nom du serveur. |
| -Ed | Spécifie que la base de données doit être activée pour la dépendance de cache SQL. |
| *nom de\_de base de données* -d | Spécifie le nom de la base de données qui doit être activé pour la dépendance de cache SQL. |
| -E | Spécifie que le RegSql ASPNET\_doit utiliser l’authentification Windows lors de la connexion à la base de données. |
| -et | Spécifie que nous allons activer une table de base de données pour la dépendance de cache SQL. |
| -t *table\_nom* | Spécifie le nom de la table de base de données à activer pour la dépendance de cache SQL. |

> [!NOTE]
> D’autres commutateurs sont disponibles pour ASPNET\_RegSql. exe. Pour obtenir la liste complète, exécutez ASPNET\_RegSql. exe- ? à partir d’une ligne de commande.

Lorsque cette commande est exécutée, les modifications suivantes sont apportées à la base de données SQL Server :

- Une table **AspNet\_SqlCacheTablesForChangeNotification** est ajoutée. Cette table contient une ligne pour chaque table de la base de données pour laquelle une dépendance de cache SQL a été activée.
- Les procédures stockées suivantes sont créées à l’intérieur de la base de données :

| SqlCachePollingStoredProcedure AspNet\_ | Interroge la table AspNet\_SqlCacheTablesForChangeNotification et retourne toutes les tables qui sont activées pour la dépendance de cache SQL et la valeur de correspondant changeid pour chaque table. Cette procédure stockée est utilisée pour l’interrogation afin de déterminer si les données ont changé. |
| --- | --- |
| SqlCacheQueryRegisteredTablesStoredProcedure AspNet\_ | Retourne toutes les tables activées pour la dépendance de cache SQL en interrogeant la table AspNet\_SqlCacheTablesForChangeNotification et retourne toutes les tables activées pour la dépendance de cache SQL. |
| SqlCacheRegisterTableStoredProcedure AspNet\_ | Inscrit une table pour la dépendance de cache SQL en ajoutant l’entrée nécessaire dans la table de notification et ajoute le déclencheur. |
| SqlCacheUnRegisterTableStoredProcedure AspNet\_ | Annule l’inscription d’une table pour la dépendance de cache SQL en supprimant l’entrée dans la table de notification et supprime le déclencheur. |
| SqlCacheUpdateChangeIdStoredProcedure AspNet\_ | Met à jour la table de notification en incrémentant le correspondant changeid pour la table modifiée. ASP.NET utilise cette valeur pour déterminer si les données ont changé. Comme indiqué ci-dessous, cette procédure stockée est exécutée par le déclencheur créé lorsque la table est activée. |

- Un déclencheur de SQL Server appelé  **_table\_Name_\_AspNet\_SqlCacheNotification\_déclencheur** est créé pour la table. Ce déclencheur exécute le SqlCacheUpdateChangeIdStoredProcedure AspNet\_lorsqu’une insertion, une mise à jour ou une suppression est effectuée sur la table.
- Un rôle SQL Server nommé **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** est ajouté à la base de données.

Le rôle de SQL Server **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** dispose des autorisations d’exécution sur le SqlCachePollingStoredProcedure ASPNET\_. Pour que le modèle d’interrogation fonctionne correctement, vous devez ajouter votre compte de processus au rôle ASPNET\_ChangeNotification\_ReceiveNotificationsOnlyAccess. L’outil ASPNET\_RegSql. exe ne le fait pas pour vous.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Configuration des dépendances de cache SQL basées sur l’interrogation

Plusieurs étapes sont nécessaires pour configurer les dépendances de cache SQL basées sur l’interrogation. La première étape consiste à activer la base de données et la table, comme indiqué ci-dessus. Une fois cette étape terminée, le reste de la configuration est le suivant :

- Configuration du fichier de configuration ASP.NET.
- Configuration de SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Configuration du fichier de configuration ASP.NET

En plus d’ajouter une chaîne de connexion comme indiqué dans un module précédent, vous devez également configurer un élément de cache &lt;&gt; avec un élément &lt;sqlCacheDependency&gt; comme indiqué ci-dessous :

[!code-xml[Main](caching/samples/sample7.xml)]

Cette configuration active une dépendance de cache SQL sur la base de données *pubs* . Notez que l’attribut pollTime dans l’élément &lt;sqlCacheDependency&gt; a par défaut la valeur 60000 millisecondes ou 1 minute. (Cette valeur ne peut pas être inférieure à 500 millisecondes.) Dans cet exemple, la &lt;ajouter&gt; élément ajoute une nouvelle base de données et remplace pollTime, en lui affectant la valeur 9 millions millisecondes.

#### <a name="configuring-the-sqlcachedependency"></a>Configuration de SqlCacheDependency

L’étape suivante consiste à configurer SqlCacheDependency. Le moyen le plus simple d’y parvenir consiste à spécifier la valeur de l’attribut SqlDependency dans la directive @ OutAttribute, comme suit :

[!code-aspx[Main](caching/samples/sample8.aspx)]

Dans la directive @ OutputCache ci-dessus, une dépendance de cache SQL est configurée pour la table *Authors* dans la base de données *pubs* . Plusieurs dépendances peuvent être configurées en les séparant par un point-virgule comme suit :

[!code-aspx[Main](caching/samples/sample9.aspx)]

Une autre méthode de configuration de SqlCacheDependency consiste à le faire par programme. Le code suivant crée une dépendance de cache SQL sur la table *Authors* de la base de données *pubs* .

[!code-csharp[Main](caching/samples/sample10.cs)]

L’un des avantages de la définition par programmation de la dépendance de cache SQL est que vous pouvez gérer toutes les exceptions qui peuvent se produire. Par exemple, si vous tentez de définir une dépendance de cache SQL pour une base de données qui n’a pas été activée pour la notification, une exception **DatabaseNotEnabledForNotificationException** est levée. Dans ce cas, vous pouvez essayer d’activer la base de données pour les notifications en appelant la méthode **SqlCacheDependencyAdmin. EnableNotifications** et en lui transmettant le nom de la base de données.

De même, si vous tentez de définir une dépendance de cache SQL pour une table qui n’a pas été activée pour la notification, une **TableNotEnabledForNotificationException** est levée. Vous pouvez ensuite appeler la méthode **SqlCacheDependencyAdmin. EnableTableForNotifications** en lui transmettant le nom de la base de données et le nom de la table.

L’exemple de code suivant montre comment configurer correctement la gestion des exceptions lors de la configuration d’une dépendance de cache SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Plus d’informations : [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Dépendances de cache SQL basées sur une requête (SQL Server 2005 uniquement)

Lors de l’utilisation de SQL Server 2005 pour la dépendance de cache SQL, le modèle basé sur l’interrogation n’est pas nécessaire. Lorsqu’ils sont utilisés avec SQL Server 2005, les dépendances de cache SQL communiquent directement via les connexions SQL à l’instance de SQL Server (aucune configuration supplémentaire n’est nécessaire) à SQL Server l’aide des notifications de requêtes 2005.

La façon la plus simple d’activer la notification basée sur une requête est de le faire de façon déclarative en affectant à l’attribut **SqlCacheDependency** de l’objet source de données la valeur **CommandNotification** et en affectant à l’attribut **EnableCaching** la **valeur true**. À l’aide de cette méthode, aucun code n’est requis. Si le résultat d’une commande exécutée sur la source de données change, les données du cache sont invalidées.

L’exemple suivant configure un contrôle de source de données pour la dépendance de cache SQL :

[!code-aspx[Main](caching/samples/sample12.aspx)]

Dans ce cas, si la requête spécifiée dans **SelectCommand** retourne un résultat différent de celui qu’elle avait à l’origine, les résultats mis en cache sont invalidés.

Vous pouvez également spécifier que toutes vos sources de données soient activées pour les dépendances de cache SQL en affectant à l’attribut **SqlDependency** de la directive **@ OutputCache** la valeur **CommandNotification**. L’exemple ci-dessous illustre cela.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Pour plus d’informations sur les notifications de requêtes dans SQL Server 2005, consultez la Documentation en ligne de SQL Server.

Une autre méthode de configuration d’une dépendance de cache SQL basée sur une requête consiste à le faire par programme à l’aide de la classe SqlCacheDependency. L’exemple de code suivant illustre cette procédure.

[!code-csharp[Main](caching/samples/sample14.cs)]

Plus d’informations : [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Substitution après le cache

La mise en cache d’une page peut augmenter considérablement les performances d’une application Web. Toutefois, dans certains cas, vous avez besoin que la plus grande partie de la page soit mise en cache et que certains fragments de la page soient dynamiques. Par exemple, si vous créez une page de nouvelles histoires qui est entièrement statique pour des périodes définies, vous pouvez définir la page entière à mettre en cache. Si vous souhaitez inclure une bannière publicitaire en rotation qui a changé à chaque demande de page, alors la partie de la page contenant la publication doit être dynamique. Pour vous permettre de mettre en cache une page, mais de remplacer dynamiquement du contenu, vous pouvez utiliser la substitution ASP.NET après le cache. Avec la substitution après le cache, la page entière est mise en cache avec des parties spécifiques marquées comme exemptes de mise en cache. Dans l’exemple des bannières Active Directory, le contrôle AdRotator vous permet de tirer parti de la substitution postérieure au cache afin que les publicités créées dynamiquement pour chaque utilisateur et pour chaque page soient actualisées.

Il existe trois façons d’implémenter la substitution après le cache :

- De façon déclarative, à l’aide du contrôle de substitution.
- Par programmation, à l’aide de l’API de contrôle de substitution.
- Implicitement, à l’aide du contrôle AdRotator.

### <a name="substitution-control"></a>Contrôle de substitution

Le contrôle de substitution ASP.NET spécifie une section d’une page mise en cache qui est créée dynamiquement au lieu d’être mise en cache. Vous placez un contrôle de substitution à l’emplacement de la page où vous souhaitez que le contenu dynamique apparaisse. Au moment de l’exécution, le contrôle de substitution appelle une méthode que vous spécifiez avec la propriété MethodName. La méthode doit retourner une chaîne, qui remplace ensuite le contenu du contrôle de substitution. La méthode doit être une méthode statique sur la page conteneur ou le contrôle UserControl. L’utilisation du contrôle de substitution entraîne la modification de la mise en cache côté client vers la mise en cache du serveur, afin que la page ne soit pas mise en cache sur le client. Cela garantit que les demandes ultérieures à la page appellent à nouveau la méthode pour générer du contenu dynamique.

### <a name="substitution-api"></a>API de substitution

Pour créer du contenu dynamique pour une page mise en cache par programmation, vous pouvez appeler la méthode [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) dans votre code de page, en lui transmettant le nom d’une méthode en tant que paramètre. La méthode qui gère la création du contenu dynamique accepte un seul paramètre [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) et retourne une chaîne. La chaîne de retour est le contenu qui sera remplacé à l’emplacement donné. L’un des avantages de l’appel de la méthode WriteSubstitution au lieu d’utiliser le contrôle de substitution de façon déclarative est que vous pouvez appeler une méthode d’un objet arbitraire plutôt que d’appeler une méthode statique de la page ou de l’objet UserControl.

L’appel de la méthode WriteSubstitution entraîne la modification de la mise en cache côté client vers la mise en cache du serveur, afin que la page ne soit pas mise en cache sur le client. Cela garantit que les demandes ultérieures à la page appellent à nouveau la méthode pour générer du contenu dynamique.

### <a name="adrotator-control"></a>Contrôle AdRotator

Le contrôle serveur AdRotator implémente la prise en charge de la substitution après le cache en interne. Si vous placez un contrôle AdRotator sur votre page, il affichera des publications uniques à chaque demande, que la page parent soit mise en cache ou non. Par conséquent, une page qui contient un contrôle AdRotator est uniquement mise en cache côté serveur.

## <a name="controlcachepolicy-class"></a>ControlCachePolicy, classe

La classe ControlCachePolicy permet le contrôle par programmation de la mise en cache de fragments à l’aide de contrôles utilisateur. ASP.NET incorpore les contrôles utilisateur dans une instance [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) . La classe BasePartialCachingControl représente un contrôle utilisateur pour lequel la mise en cache de sortie est activée.

Lorsque vous accédez à la propriété [BasePartialCachingControl. CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) d’un contrôle [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) , vous recevez toujours un objet ControlCachePolicy valide. Toutefois, si vous accédez à la propriété [UserControl. CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) d’un contrôle [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) , vous recevez un objet ControlCachePolicy valide uniquement si le contrôle utilisateur est déjà encapsulé par un contrôle BasePartialCachingControl. Si elle n’est pas encapsulée, l’objet ControlCachePolicy retourné par la propriété lève des exceptions lorsque vous tentez de la manipuler, car aucun BasePartialCachingControl n’est associé. Pour déterminer si une instance de UserControl prend en charge la mise en cache sans générer d’exceptions, examinez la propriété [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) .

L’utilisation de la classe ControlCachePolicy est l’une des méthodes permettant d’activer la mise en cache de sortie. La liste suivante décrit les méthodes que vous pouvez utiliser pour activer la mise en cache de sortie :

- Utilisez la directive [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) pour activer la mise en cache de sortie dans les scénarios déclaratifs.
- Utilisez l’attribut [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) pour activer la mise en cache d’un contrôle utilisateur dans un fichier code-behind.
- Utilisez la classe ControlCachePolicy pour spécifier des paramètres de cache dans les scénarios de programmation dans lesquels vous travaillez avec des instances BasePartialCachingControl qui ont été activées pour le cache à l’aide de l’une des méthodes précédentes et chargées dynamiquement à l’aide de la méthode [System. Web. UI. TemplateControl. LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) .

Une instance ControlCachePolicy peut être manipulée avec succès uniquement entre les étapes d’initialisation et de prérendu du cycle de vie de contrôle. Si vous modifiez un objet ControlCachePolicy après la phase de prérendu, ASP.NET lève une exception, car toutes les modifications apportées après le rendu du contrôle ne peuvent pas réellement affecter les paramètres du cache (un contrôle est mis en cache pendant l’étape de rendu). Enfin, une instance de contrôle utilisateur (et par conséquent son objet ControlCachePolicy) est uniquement disponible pour la manipulation par programmation lorsqu’elle est réellement rendue.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Modifications apportées à la configuration de la mise en cache-l’élément&gt; Caching &lt;

Il existe plusieurs modifications à la configuration de la mise en cache dans ASP.NET 2,0. L’élément &lt;Caching&gt; est une nouveauté de ASP.NET 2,0 et vous permet d’apporter des modifications à la configuration de la mise en cache dans le fichier de configuration. Les attributs suivants sont disponibles.

| **Élément** | **Description** |
| --- | --- |
| **en** | Élément facultatif. Définit les paramètres globaux du cache d’application. |
| **Directive** | Élément facultatif. Spécifie les paramètres de cache de sortie à l’ensemble de l’application. |
| **outputCacheSettings** | Élément facultatif. Spécifie les paramètres de cache de sortie qui peuvent être appliqués aux pages de l’application. |
| **sqlCacheDependency** | Élément facultatif. Configure les dépendances de cache SQL pour une application ASP.NET. |

### <a name="the-ltcachegt-element"></a>Élément&gt; du cache &lt;

Les attributs suivants sont disponibles dans l’élément&gt; du cache &lt;:

| **Attribut** | **Description** |
| --- | --- |
| **disableMemoryCollection** | Attribut **Boolean** facultatif. Obtient ou définit une valeur indiquant si la collection de mémoires cache qui se produit lorsque l’ordinateur subit une sollicitation de la mémoire est désactivée. |
| **disableExpiration** | Attribut **Boolean** facultatif. Obtient ou définit une valeur indiquant si l’expiration du cache est désactivée. Lorsqu’elles sont désactivées, les éléments mis en cache n’expirent pas et le nettoyage en arrière-plan des éléments de cache expirés n’a pas lieu. |
| **privateBytesLimit** | Attribut **Int64** facultatif. Obtient ou définit une valeur indiquant la taille maximale des octets privés d’une application avant que le cache ne commence à vider les éléments arrivés à expiration et à tenter de récupérer de la mémoire. Cette limite comprend la mémoire utilisée par le cache ainsi que la surcharge de mémoire normale de l’application en cours d’exécution. La valeur zéro indique que ASP.NET utilise ses propres heuristiques pour déterminer quand démarrer la récupération de la mémoire. |
| **percentagePhysicalMemoryUsedLimit** | Attribut **Int32** facultatif. Obtient ou définit une valeur qui indique le pourcentage maximal de mémoire physique d’un ordinateur qui peut être consommé par une application avant que le cache ne démarre le vidage des éléments arrivés à expiration et tente de récupérer de la mémoire. cette utilisation de la mémoire comprend également la mémoire utilisée par le cache. comme utilisation normale de la mémoire de l’application en cours d’exécution. La valeur zéro indique que ASP.NET utilise ses propres heuristiques pour déterminer quand démarrer la récupération de la mémoire. |
| **privateBytesPollTime** | Attribut **TimeSpan** facultatif. Obtient ou définit une valeur indiquant l’intervalle de temps entre les interrogations pour l’utilisation de la mémoire d’octets privés de l’application. |

### <a name="the-ltoutputcachegt-element"></a>Élément&gt; &lt;outputCache

Les attributs suivants sont disponibles pour l’élément&gt; &lt;outputCache.

|       <strong>Attribut</strong>        |                                                                                                                                                                                                                                                       <strong>Description</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Attribut <strong>Boolean</strong> facultatif. Active/désactive le cache de sortie de page. Si elle est désactivée, aucune page n’est mise en cache, quels que soient les paramètres de programmation ou déclaratifs. La valeur par défaut est <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Attribut <strong>Boolean</strong> facultatif. Active/désactive le cache de fragments de l’application. Si elle est désactivée, aucune page n’est mise en cache indépendamment de la directive [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) ou du profil de mise en cache utilisé. Comprend un en-tête Cache-Control indiquant que les serveurs proxy en amont et les clients de navigateur ne doivent pas tenter de mettre en cache la sortie de page. La valeur par défaut est <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Attribut <strong>Boolean</strong> facultatif. Obtient ou définit une valeur indiquant si l’en-tête <strong>Cache-Control : Private</strong> est envoyé par défaut par le module de cache de sortie. La valeur par défaut est <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Attribut <strong>Boolean</strong> facultatif. Active/désactive l’envoi d’un en-tête http «<strong>Vary : \</strong ><em>» dans la réponse. Avec le paramètre par défaut false, un</em>en-tête « * Vary : \*<strong>» est envoyé pour les pages en cache de sortie. Lorsque l’en-tête Vary est envoyé, il permet de mettre en cache différentes versions en fonction de ce qui est spécifié dans l’en-tête Vary. Par exemple, <em>Vary : user-agents</em> stocke différentes versions d’une page en fonction de l’agent utilisateur qui émet la demande. La valeur par défaut est * * false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>Élément &lt;outputCacheSettings&gt;

L’élément &lt;outputCacheSettings&gt; permet de créer des profils de cache comme décrit précédemment. Le seul élément enfant de l’élément &lt;outputCacheSettings&gt; est l’élément &lt;outputCacheProfiles&gt; pour la configuration des profils de cache.

### <a name="the-ltsqlcachedependencygt-element"></a>Élément&gt; &lt;sqlCacheDependency

Les attributs suivants sont disponibles pour l’élément &lt;sqlCacheDependency&gt;.

| **Attribut** | **Description** |
| --- | --- |
| **activé** | Attribut **booléen** obligatoire. Indique si les modifications sont en cours d’interrogation. |
| **pollTime** | Attribut **Int32** facultatif. Définit la fréquence à laquelle SqlCacheDependency interroge la table de base de données pour les modifications. Cette valeur correspond au nombre de millisecondes entre les interrogations consécutives. Elle ne peut pas être définie sur une valeur inférieure à 500 millisecondes. La valeur par défaut est 1 minute. |

### <a name="more-information"></a>Plus d'informations

Vous devez tenir compte des informations supplémentaires relatives à la configuration du cache.

- Si la limite d’octets privés du processus de travail n’est pas définie, le cache utilisera l’une des limites suivantes : 

    - x86 Go : 800 Mo ou 60% de RAM physique, selon celui qui est le plus petit
    - x86 3 Go : 1800MB ou 60% de RAM physique, selon celui qui est le plus petit
    - x64:1 téraoctet ou 60% de RAM physique, selon la valeur la plus faible
- Si les valeurs limite des octets privés du processus de travail et &lt;du cache privateBytesLimit/&gt; sont définies, le cache utilise le minimum des deux.
- Comme dans 1. x, nous abandonnons les entrées du cache et appelons GC. Collecter pour deux raisons : 

    - Nous sommes très proches de la limite d’octets privés
    - La mémoire disponible est proche ou inférieure à 10%.
- Vous pouvez désactiver la délimitation et le cache pour des conditions de mémoire insuffisante en définissant &lt;cache percentagePhysicalMemoryUseLimit/&gt; sur 100.
- Contrairement à 1. x, 2,0 interrompt le découpage et collecte les appels si le dernier GC. La collecte n’a pas réduit les octets privés ou la taille des tas managés de plus de 1% de la limite de mémoire (cache).

## <a name="lab1-custom-cache-dependencies"></a>Lab1 : dépendances de cache personnalisées

1. Créez un nouveau site Web.
2. Ajoutez un nouveau fichier XML appelé cache. xml et enregistrez-le à la racine de l’application Web.
3. Ajoutez le code suivant à la page\_méthode Load dans le code-behind de default. aspx : 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Ajoutez le code suivant au début du default. aspx en mode Source : 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Parcourez default. aspx. Que signifie l’heure ?
6. Actualisez le navigateur. Que signifie l’heure ?
7. Ouvrez le fichier cache. xml et ajoutez le code suivant : 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Enregistrez cache. Xml.
9. Actualisez votre navigateur. Que signifie l’heure ?
10. Explique pourquoi l’heure a été mise à jour au lieu d’afficher les valeurs précédemment mises en cache :

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Lab 2 : utilisation des dépendances de cache basées sur l’interrogation

Ce laboratoire utilise le projet que vous avez créé dans le module précédent, qui permet de modifier les données de la base de données Northwind via un contrôle GridView et DetailsView.

1. Ouvrez le projet dans Visual Studio 2005.
2. Exécutez l’utilitaire ASPNET\_RegSql sur la base de données Northwind pour activer la base de données et la table Products. Utilisez la commande suivante à partir d’une invite de commandes Visual Studio : 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Ajoutez le code suivant à votre fichier Web. config : 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Ajoutez un nouveau WebForm appelé ShowData. aspx.
5. Ajoutez la directive @ OutputCache suivante à la page ShowData. aspx : 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Ajoutez le code suivant à la page\_charge de ShowData. aspx : 

    [!code-html[Main](caching/samples/sample21.html)]
7. Ajoutez un nouveau contrôle SqlDataSource à ShowData. aspx et configurez-le pour utiliser la connexion de base de données Northwind. Cliquez sur Suivant.
8. Activez les cases à cocher ProductName et ProductID, puis cliquez sur suivant.
9. Cliquez sur Finish.
10. Ajoutez un nouveau contrôle GridView à la page ShowData. aspx.
11. Choisissez SqlDataSource1 dans la liste déroulante.
12. Enregistrez et parcourez ShowData. aspx. Prenez note de l’heure affichée.
