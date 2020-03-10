---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Configuration et instrumentation | Microsoft Docs
author: microsoft
description: Des modifications majeures ont été apportées à la configuration et à l’instrumentation dans ASP.NET 2,0. La nouvelle API de configuration ASP.NET permet d’apporter des modifications de configuration...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: cd5bedce5459e8cf8e72df8de69ebd82f2d97789
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78626027"
---
# <a name="configuration-and-instrumentation"></a>Configuration et instrumentation

par [Microsoft](https://github.com/microsoft)

> Des modifications majeures ont été apportées à la configuration et à l’instrumentation dans ASP.NET 2,0. La nouvelle API de configuration ASP.NET permet d’effectuer des modifications de configuration par programme. En outre, de nombreux nouveaux paramètres de configuration existent pour les nouvelles configurations et l’instrumentation.

Des modifications majeures ont été apportées à la configuration et à l’instrumentation dans ASP.NET 2,0. La nouvelle API de configuration ASP.NET permet d’effectuer des modifications de configuration par programme. En outre, de nombreux nouveaux paramètres de configuration existent pour les nouvelles configurations et l’instrumentation.

Dans ce module, nous aborderons l’API de configuration de ASP.NET en ce qui concerne la lecture et l’écriture dans les fichiers de configuration ASP.NET, et nous aborderons également l’instrumentation ASP.NET. Nous aborderons également les nouvelles fonctionnalités disponibles dans le suivi ASP.NET.

## <a name="aspnet-configuration-api"></a>API de configuration ASP.NET

L’API de configuration ASP.NET vous permet de développer, de déployer et de gérer des données de configuration d’application à l’aide d’une seule interface de programmation. Vous pouvez utiliser l’API de configuration pour développer et modifier des configurations ASP.NET complètes par programme sans modifier directement le code XML dans les fichiers de configuration. En outre, vous pouvez utiliser l’API de configuration dans les applications console et les scripts que vous développez, dans les outils de gestion basés sur le Web et dans les composants logiciels enfichables de la console MMC (Microsoft Management Console).

Les deux outils de gestion de configuration suivants utilisent l’API de configuration et sont inclus avec le .NET Framework version 2,0 :

- Le composant logiciel enfichable MMC ASP.NET, qui utilise l’API de configuration pour simplifier les tâches d’administration, en fournissant une vue intégrée des données de configuration locales à partir de tous les niveaux de la hiérarchie de configuration.
- L’outil Administration de site Web, qui vous permet de gérer les paramètres de configuration pour les applications locales et distantes, y compris les sites hébergés.

L’API de configuration ASP.NET comprend un ensemble d’objets de gestion ASP.NET que vous pouvez utiliser pour configurer des sites Web et des applications par programme. Les objets de gestion sont implémentés sous la forme d’une bibliothèque de classes .NET Framework. Le modèle de programmation de l’API de configuration permet de garantir la cohérence et la fiabilité du code en appliquant des types de données au moment de la compilation. Pour faciliter la gestion des configurations d’application, l’API de configuration vous permet d’afficher les données fusionnées à partir de tous les points de la hiérarchie de configuration en tant que collection unique, au lieu d’afficher les données sous forme de collections distinctes de différentes fichiers de configuration. En outre, l’API de configuration vous permet de manipuler des configurations d’application entières sans modifier directement le code XML dans les fichiers de configuration. Enfin, l’API simplifie les tâches de configuration en prenant en charge les outils d’administration, tels que l’outil Administration de site Web. L’API de configuration simplifie le déploiement en prenant en charge la création de fichiers de configuration sur un ordinateur et en exécutant des scripts de configuration sur plusieurs ordinateurs.

> [!NOTE]
> L’API de configuration ne prend pas en charge la création d’applications IIS.

## <a name="working-with-local-and-remote-configuration-settings"></a>Utilisation des paramètres de configuration locaux et distants

Un objet de configuration représente l’affichage fusionné des paramètres de configuration qui s’appliquent à une entité physique spécifique, telle qu’un ordinateur, ou à une entité logique, telle qu’une application ou un site Web. L’entité logique spécifiée peut exister sur l’ordinateur local ou sur un serveur distant. Si aucun fichier de configuration n’existe pour une entité spécifiée, l’objet de configuration représente les paramètres de configuration par défaut définis par le fichier machine. config.

Vous pouvez obtenir un objet de configuration à l’aide de l’une des méthodes de configuration ouvertes des classes suivantes :

1. La classe ConfigurationManager, si votre entité est une application cliente.
2. La classe WebConfigurationManager, si votre entité est une application Web.

Ces méthodes retournent un objet de configuration qui, à son tour, fournit les méthodes et propriétés requises pour gérer les fichiers de configuration sous-jacents. Vous pouvez accéder à ces fichiers pour la lecture ou l’écriture.

### <a name="reading"></a>Lecture

Vous utilisez la méthode GetSection ou GetSectionGroup pour lire les informations de configuration. L’utilisateur ou le processus qui lit doit avoir des autorisations de lecture sur tous les fichiers de configuration de la hiérarchie.

> [!NOTE]
> Si vous utilisez une méthode GetSection statique qui accepte un paramètre Path, le paramètre Path doit faire référence à l’application dans laquelle le code s’exécute. Sinon, le paramètre est ignoré et les informations de configuration de l’application en cours d’exécution sont retournées.

### <a name="writing"></a>Écriture

Vous utilisez l’une des méthodes Save pour écrire des informations de configuration. L’utilisateur ou le processus qui écrit doit avoir des autorisations d’écriture sur le fichier de configuration et le répertoire au niveau de la hiérarchie de configuration actuelle, ainsi que des autorisations de lecture sur tous les fichiers de configuration de la hiérarchie.

Pour générer un fichier de configuration qui représente les paramètres de configuration hérités pour une entité spécifiée, utilisez l’une des méthodes Save-Configuration suivantes :

1. Méthode Save pour créer un nouveau fichier de configuration.
2. La méthode SaveAs pour générer un nouveau fichier de configuration à un autre emplacement.

## <a name="configuration-classes-and-namespaces"></a>Classes et espaces de noms de configuration

De nombreuses classes et méthodes de configuration sont similaires. Le tableau suivant décrit les classes de configuration et les espaces de noms les plus couramment utilisés.

| **Classe de configuration ou espace de noms** | **Description** |
| --- | --- |
| Espace de noms [System. Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) | Contient les classes de configuration principales pour toutes les applications .NET Framework. Les classes de gestionnaire de section permettent d’obtenir des données de configuration pour une section à partir de méthodes, telles que GetSection et GetSectionGroup. Ces deux méthodes sont non statiques. |
| System. Configuration. Configuration (classe) | Représente un ensemble de données de configuration pour un ordinateur, une application, un répertoire Web ou une autre ressource. Cette classe contient des méthodes utiles, telles que GetSection et GetSectionGroup, pour la mise à jour des paramètres de configuration et l’obtention de références à des sections et des groupes de sections. Cette classe est utilisée comme type de retour pour les méthodes qui obtiennent des données de configuration au moment du design, telles que les méthodes des classes WebConfigurationManager et ConfigurationManager. |
| Espace de noms System. Web. Configuration | Contient les classes de gestionnaire de section pour les sections de configuration ASP.NET définies dans [paramètres de configuration de ASP.net](https://msdn.microsoft.com/library/b5ysx397.aspx). Les classes de gestionnaire de section permettent d’obtenir des données de configuration pour une section à partir de méthodes, telles que GetSection et GetSectionGroup. |
| System. Web. Configuration. WebConfigurationManager, classe | Fournit des méthodes utiles pour obtenir des références aux paramètres de configuration au moment de l’exécution et au moment du Design. Ces méthodes utilisent la classe System. Configuration. Configuration comme type de retour. Vous pouvez utiliser la méthode GetSection statique de cette classe ou la méthode GetSection non statique de la classe System. Configuration. ConfigurationManager de manière interchangeable. Pour les configurations d’application Web, la classe System. Web. Configuration. WebConfigurationManager est recommandée à la place de la classe System. Configuration. ConfigurationManager. |
| Espace de noms [System. Configuration. Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) | Fournit un moyen de personnaliser et d’étendre le fournisseur de configuration. Il s’agit de la classe de base pour toutes les classes de fournisseur dans le système de configuration. |
| Espace de noms [System. Web. Management](https://msdn.microsoft.com/library/system.web.management.aspx) | Contient des classes et des interfaces pour la gestion et la surveillance de l’intégrité des applications Web. Strictement parlant, cet espace de noms n’est pas considéré comme faisant partie de l’API de configuration. Par exemple, le traçage et le déclenchement d’événements sont réalisés par les classes de cet espace de noms. |
| Espace de noms [System. Management. Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) | Fournit les classes nécessaires à l’instrumentation d’applications pour exposer leurs informations et événements de gestion via Windows Management Instrumentation (WMI) aux consommateurs potentiels. ASP.NET Health Monitoring utilise WMI pour remettre les événements. Strictement parlant, cet espace de noms n’est pas considéré comme faisant partie de l’API de configuration. |

## <a name="reading-from-aspnet-configuration-files"></a>Lecture à partir des fichiers de configuration ASP.NET

La classe WebConfigurationManager est la classe principale pour la lecture à partir des fichiers de configuration ASP.NET. Il existe trois étapes pour la lecture des fichiers de configuration ASP.NET :

1. Obtenir un objet de configuration à l’aide de la méthode OpenWebConfiguration.
2. Obtenir une référence à la section souhaitée dans le fichier de configuration.
3. Lisez les informations souhaitées dans le fichier de configuration.

L’objet de configuration représenté ne représente pas un fichier de configuration particulier. Au lieu de cela, il représente une vue fusionnée de la configuration d’un ordinateur, d’une application ou d’un site Web. L’exemple de code suivant instancie un objet de configuration représentant la configuration d’une application Web appelée *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Notez que si le chemin d’accès/ProductInfo n’existe pas, le code ci-dessus retourne la configuration par défaut telle qu’elle est spécifiée dans le fichier machine. config.

Une fois que vous disposez de l’objet de configuration, vous pouvez utiliser la méthode GetSection ou GetSectionGroup pour analyser les paramètres de configuration. L’exemple suivant obtient une référence aux paramètres d’emprunt d’identité pour l’application ProductInfo ci-dessus :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Écriture dans des fichiers de configuration ASP.NET

Comme pour la lecture à partir des fichiers de configuration, la classe WebConfigurationManager est le cœur de l’écriture dans les fichiers de configuration Asp.NET. Il y a également trois étapes pour écrire dans des fichiers de configuration ASP.NET.

1. Obtenir un objet de configuration à l’aide de la méthode OpenWebConfiguration.
2. Obtenir une référence à la section souhaitée dans le fichier de configuration.
3. Écrivez les informations souhaitées à partir du fichier de configuration à l’aide de la méthode Save ou SaveAs.

Le code suivant modifie l’attribut **Debug** de l’élément &lt;compilation&gt; en false :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Quand ce code est exécuté, l’attribut de **débogage** de l’élément de&gt; de compilation &lt;est défini sur false pour le fichier Web. config de l’application *webapp* .

## <a name="systemwebmanagement-namespace"></a>Espace de noms System. Web. Management

L’espace de noms System. Web. Management fournit les classes et les interfaces permettant de gérer et de surveiller l’intégrité des applications ASP.NET.

La journalisation s’effectue en définissant une règle qui associe des événements à un fournisseur. La règle définit le type des événements qui sont envoyés au fournisseur. Vous pouvez consigner les événements de base suivants :

| **WebBaseEvent** | Classe d’événements de base pour tous les événements. Contient les propriétés requises pour tous les événements tels que le code d’événement, le code de détail de l’événement, la date et l’heure de déclenchement de l’événement, le numéro de séquence, le message d’événement et les détails de l’événement. |
| --- | --- |
| **WebManagementEvent** | Classe d’événements de base pour les événements de gestion, tels que la durée de vie de l’application, les demandes, les erreurs et les événements d’audit. |
| **WebHeartbeatEvent** | Événement généré par l’application à intervalles réguliers pour capturer des informations utiles sur l’état d’exécution. |
| **WebAuditEvent** | Classe de base pour les événements d’audit de sécurité, qui sont utilisés pour marquer des conditions telles que l’échec de l’autorisation, l’échec du déchiffrement, *etc.* |
| **WebRequestEvent** | Classe de base pour tous les événements de requête d’information. |
| **WebBaseErrorEvent** | Classe de base pour tous les événements indiquant des conditions d’erreur. |

Les types de fournisseurs disponibles vous permettent d’envoyer une sortie d’événement à observateur d’événements, SQL Server, Windows Management Instrumentation (WMI) et à la messagerie électronique. Les fournisseurs et les mappages d’événements préconfigurés réduisent la quantité de travail nécessaire pour enregistrer la sortie de l’événement.

ASP.NET 2,0 utilise le fournisseur de journaux des événements prêt à l’emploi pour consigner les événements en fonction des domaines d’application qui démarrent et s’arrêtent, ainsi que la journalisation des exceptions non gérées. Cela permet de couvrir certains des scénarios de base. Par exemple, supposons que votre application lève une exception, mais que l’utilisateur n’enregistre pas l’erreur et que vous ne pouvez pas la reproduire. Avec la règle de journalisation des événements par défaut, vous pouvez collecter les informations sur les exceptions et les piles pour obtenir une meilleure idée du type d’erreur qui s’est produit. Un autre exemple s’applique si votre application perd l’état de session. Dans ce cas, vous pouvez consulter le journal des événements pour déterminer si le domaine d’application est en cours de recyclage et pourquoi le domaine d’application s’est arrêté en premier lieu.

En outre, le système de contrôle d’intégrité est extensible. Par exemple, vous pouvez définir des événements Web personnalisés, les activer dans votre application, puis définir une règle pour envoyer les informations d’événement à un fournisseur tel que votre adresse de messagerie. Cela vous permet de lier facilement votre instrumentation aux fournisseurs de contrôle d’intégrité. Autre exemple : vous pouvez déclencher un événement chaque fois qu’une commande est traitée et configurer une règle qui envoie chaque événement à la base de données SQL Server. Vous pouvez également déclencher un événement lorsqu’un utilisateur ne parvient pas à se connecter plusieurs fois sur une ligne et à configurer l’événement pour qu’il utilise les fournisseurs de messagerie.

La configuration des fournisseurs et des événements par défaut est stockée dans le fichier Web. config global. Le fichier Web. config global stocke tous les paramètres Web qui étaient stockés dans le fichier machine. config dans ASP.NET 1x. Le fichier Web. config global se trouve dans le répertoire suivant :

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

La section &lt;healthMonitoring&gt; du fichier Web. config global fournit des paramètres de configuration par défaut. Vous pouvez remplacer ces paramètres ou configurer vos propres paramètres en implémentant la section &lt;healthMonitoring&gt; dans le fichier Web. config de votre application.

La section &lt;healthMonitoring&gt; du fichier Web. config global contient les éléments suivants :

| **fournisseurs** | Contient les fournisseurs configurés pour le observateur d’événements, WMI et SQL Server. |
| --- | --- |
| **eventMappings** | Contient des mappages pour les différentes classes webbase. Vous pouvez étendre cette liste si vous générez votre propre classe d’événements. La génération de votre propre classe d’événements vous offre une granularité plus fine des fournisseurs auxquels vous envoyez des informations. Par exemple, vous pouvez configurer des exceptions non gérées à envoyer à SQL Server, tout en envoyant vos propres événements personnalisés à la messagerie. |
| **matière** | Lie le eventMappings au fournisseur. |
| **mise en tampon** | Utilisé avec SQL Server et les fournisseurs de messagerie pour déterminer la fréquence à laquelle les événements doivent être vidés sur le fournisseur. |

Vous trouverez ci-dessous un exemple de code issu du fichier Web. config global.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Comment stocker des événements dans observateur d’événements

Comme mentionné précédemment, le fournisseur de journalisation des événements dans le observateur d’événements est configuré pour vous dans le fichier Web. config global. Par défaut, tous les événements basés sur **WebBaseErrorEvent** et **WebFailureAuditEvent** sont journalisés. Vous pouvez ajouter des règles supplémentaires pour consigner des informations supplémentaires dans le journal des événements. Par exemple, si vous souhaitez consigner tous les événements (*c’est-à-dire*, tous les événements basés sur **WebBaseEvent**), vous pouvez ajouter la règle suivante à votre fichier Web. config :

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Cette règle lie la table d’événements **all events** au fournisseur de journaux des événements. EventMapping et le fournisseur sont tous les deux inclus dans le fichier Web. config global.

## <a name="how-to-store-events-to-sql-server"></a>Comment stocker des événements dans SQL Server

Cette méthode utilise la base de données **aspnetdb** , qui est générée par l’outil ASPNET\_RegSql. exe. Le fournisseur par défaut utilise la chaîne de connexion LocalSqlServer, qui utilise soit une base de données basée sur des fichiers dans le dossier de données de l’application\_, soit l’instance SQLExpress locale de SQL Server. La chaîne de connexion LocalSqlServer et la valeur SqlProvider sont configurées dans le fichier Web. config global.

La chaîne de connexion LocalSqlServer dans le fichier Web. config global ressemble à ceci :

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Si vous souhaitez utiliser une autre instance de SQL Server, vous devez utiliser l’outil ASPNET\_RegSql. exe, qui se trouve dans le%windir%\Microsoft.Net\Framework\v2.0. dossier de\*. Utilisez l’outil ASPNET\_RegSql. exe pour générer une base de données **aspnetdb** personnalisée sur l’instance de SQL Server, puis ajoutez la chaîne de connexion à votre fichier de configuration d’applications, puis ajoutez un fournisseur à l’aide de la nouvelle chaîne de connexion. Une fois que vous avez créé la base de données **aspnetdb** , vous devez définir une règle pour lier un EventMapping à SqlProvider.

Que vous utilisiez la valeur par défaut SqlProvider ou configurez votre propre fournisseur, vous devez ajouter une règle qui lie le fournisseur à une table d’événements. La règle suivante lie le nouveau fournisseur que vous avez créé ci-dessus à la table des événements **tous les événements** . Cette règle journalise tous les événements basés sur **WebBaseEvent** et les envoie au MySqlWebEventProvider qui utilisera la chaîne de connexion MYASPNETDB. Le code suivant ajoute une règle pour lier le fournisseur à une table d’événements :

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Si vous souhaitez envoyer uniquement des erreurs à SQL Server, vous pouvez ajouter la règle suivante :

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Comment transférer des événements à WMI

Vous pouvez également transférer les événements à WMI. Le fournisseur WMI est configuré pour vous dans le fichier Web. config global par défaut.

L’exemple de code suivant ajoute une règle pour transférer les événements à WMI :

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Vous devrez ajouter une règle pour associer un eventMapping au fournisseur, ainsi qu’une application d’écouteur WMI pour écouter les événements. L’exemple de code suivant ajoute une règle pour lier le fournisseur WMI à la table d’événements **all events** :

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Comment transférer des événements à un e-mail

Vous pouvez également transférer des événements à la messagerie. Soyez prudent quant aux règles d’événement que vous mappez à votre fournisseur de messagerie, car vous pouvez involontairement vous envoyer un grand nombre d’informations qui peuvent être mieux adaptées à SQL Server ou au journal des événements. Il existe deux fournisseurs de courrier électronique : SimpleMailWebEventProvider et TemplatedMailWebEventProvider. Chacun a les mêmes attributs de configuration, à l’exception des attributs « template » et « detailedTemplateErrors », qui sont tous deux uniquement disponibles sur le TemplatedMailWebEventProvider.

> [!NOTE]
> Aucun de ces fournisseurs de messagerie n’est configuré pour vous. Vous devez les ajouter à votre fichier Web. config.

La principale différence entre ces deux fournisseurs de courrier électronique est que SimpleMailWebEventProvider envoie des e-mails dans un modèle générique qui ne peuvent pas être modifiés. L’exemple de fichier Web. config ajoute ce fournisseur de messagerie à la liste des fournisseurs configurés à l’aide de la règle suivante :

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

La règle suivante est également ajoutée pour lier le fournisseur de messagerie à la table **des événements all events** :

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>Suivi ASP.NET 2,0

Il existe trois améliorations majeures apportées au suivi dans ASP.NET 2,0.

1. Fonctionnalités de suivi intégrées
2. Accès par programme aux messages de trace
3. Suivi amélioré au niveau de l’application

## <a name="integrated-tracing-functionality"></a>Fonctionnalités de suivi intégrées

Vous pouvez maintenant acheminer les messages émis par la classe System. Diagnostics. trace vers la sortie de traçage ASP.NET et router les messages émis par le suivi ASP.NET vers System. Diagnostics. Trace. Vous pouvez également transférer les événements d’instrumentation ASP.NET à System. Diagnostics. Trace. Cette fonctionnalité est fournie par le nouvel attribut **WriteToDiagnosticsTrace** de l’élément &lt;trace&gt;. Lorsque cette valeur booléenne est true, les messages de trace ASP.NET sont transférés à l’infrastructure de suivi System. Diagnostics pour être utilisés par tous les écouteurs inscrits pour afficher les messages de trace.

## <a name="programmatic-access-to-trace-messages"></a>Accès par programme aux messages de trace

ASP.NET 2,0 autorise l’accès par programmation à tous les messages de trace via la classe **TraceContextRecord** et la collection **TraceRecords** . La méthode la plus efficace pour accéder aux messages de trace consiste à inscrire un délégué **TraceContextEventHandler** (également nouveau dans ASP.NET 2,0) pour gérer le nouvel événement **TraceFinished** . Vous pouvez ensuite parcourir les messages de trace comme vous le souhaitez.

L’exemple de code suivant illustre ceci :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

Dans l’exemple ci-dessus, j’effectue une boucle dans la collection TraceRecords, puis j’écris chaque message dans le flux de réponse.

## <a name="improved-application-level-tracing"></a>Suivi amélioré au niveau de l’application

Le suivi au niveau de l’application est amélioré par le biais de l’introduction du nouvel attribut **mostRecent** de l’élément &lt;trace&gt;. Cet attribut spécifie si la sortie de traçage au niveau de l’application la plus récente est affichée et que les données de trace plus anciennes au-delà des limites indiquées par requestLimit sont ignorées. Si la valeur est false, les données de trace sont affichées pour les demandes jusqu’à ce que l’attribut requestLimit soit atteint.

## <a name="aspnet-command-line-tools"></a>Outils en ligne de commande ASP.NET

Il existe plusieurs outils en ligne de commande pour faciliter la configuration de ASP.NET. Les développeurs ASP.NET doivent être familiarisés avec l’outil ASPNET\_regiis. exe. ASP.NET 2,0 fournit trois autres outils en ligne de commande pour faciliter la configuration.

Les outils en ligne de commande suivants sont disponibles :

| **Outil** | **Utilisation** |
| --- | --- |
| **\_ASPNET regiis. exe** | Permet l’inscription de ASP.NET avec IIS. Deux versions de cet outil sont fournies avec ASP.NET 2,0, une pour les systèmes 32 bits (dans le dossier Framework) et l’autre pour les systèmes 64 bits (dans le dossier Framework64). La version 64 bits n’est pas installée sur un système d’exploitation 32 bits. |
| **\_ASPNET RegSql. exe** | L’outil d’inscription ASP.NET SQL Server permet de créer une base de données Microsoft SQL Server pour une utilisation par les fournisseurs d’SQL Server dans ASP.NET, ou d’ajouter ou de supprimer des options d’une base de données existante. Le fichier Aspnet\_RegSql. exe se trouve dans le dossier [Drive :] \WINDOWS\Microsoft.NET\Framework\versionNumber sur votre serveur Web. |
| **\_ASPNET RegBrowsers. exe** | L’outil d’inscription du navigateur ASP.NET analyse et compile toutes les définitions de navigateur à l’ensemble du système dans un assembly et installe l’assembly dans le Global Assembly Cache. L’outil utilise les fichiers de définition de navigateur (. BROWSER Files) à partir du sous-répertoire des navigateurs .NET Framework. L’outil se trouve dans le répertoire%SystemRoot%\Microsoft.NET\Framework\version\ |
| **\_ASPNET compiler. exe** | L’outil de compilation ASP.NET vous permet de compiler une application Web ASP.NET, soit sur place, soit pour le déploiement vers un emplacement cible tel qu’un serveur de production. La compilation sur place aide les performances de l’application, car les utilisateurs finaux ne rencontrent pas de délai lors de la première requête de l’application pendant la compilation de l’application. |

Étant donné que l’outil ASPNET\_regiis. exe n’est pas nouveau dans ASP.NET 2,0, nous ne l’aborderons pas ici.

## <a name="aspnet-sql-server-registration-tool---aspnet_regsqlexe"></a>Outil d’inscription ASP.NET SQL Server-ASPNET\_RegSql. exe

Vous pouvez définir plusieurs types d’options à l’aide de l’outil ASP.NET SQL Server Registration. Vous pouvez spécifier une connexion SQL, spécifier les services d’application ASP.NET à utiliser SQL Server pour gérer les informations, indiquer la base de données ou la table utilisée pour la dépendance de cache SQL, et ajouter ou supprimer la prise en charge de l’utilisation de SQL Server pour stocker des procédures et l’état de session.

Plusieurs services d’application ASP.NET s’appuient sur un fournisseur pour gérer le stockage et la récupération de données à partir d’une source de données. Chaque fournisseur est spécifique à la source de données. ASP.NET comprend un fournisseur SQL Server pour les fonctionnalités ASP.NET suivantes :

- Appartenance (classe [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) ).
- Gestion des rôles (classe [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) ).
- Profile (classe [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) ).
- Composants WebPart personnalisation (classe [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) ).
- Événements Web (classe [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) ).

Quand vous installez ASP.NET, le fichier machine. config de votre serveur comprend des éléments de configuration qui spécifient des fournisseurs de SQL Server pour chacune des fonctionnalités ASP.NET qui reposent sur un fournisseur. Ces fournisseurs sont configurés par défaut pour se connecter à une instance utilisateur locale de SQL Server Express 2005. Si vous modifiez la chaîne de connexion par défaut utilisée par les fournisseurs, avant de pouvoir utiliser l’une des fonctionnalités ASP.NET configurées dans la configuration de l’ordinateur, vous devez installer la base de données SQL Server et les éléments de base de données pour la fonctionnalité choisie à l’aide de ASPNET\_RegSql. exe. Si la base de données que vous spécifiez avec l’outil d’inscription SQL n’existe pas déjà (aspnetdb est la base de données par défaut si aucune n’est spécifiée sur la ligne de commande), l’utilisateur actuel doit disposer des droits nécessaires pour créer des bases de données dans SQL Server et pour créer le schéma e lements dans une base de données.

### <a name="sql-cache-dependency"></a>Dépendance de cache SQL

Une fonctionnalité avancée de la mise en cache de sortie ASP.NET est la dépendance de cache SQL. La dépendance de cache SQL prend en charge deux modes de fonctionnement différents : un qui utilise une implémentation ASP.NET de l’interrogation de table et un second mode qui utilise les fonctionnalités de notification de requête de SQL Server 2005. L’outil d’inscription SQL peut être utilisé pour configurer le mode d’interrogation de table de l’opération.

### <a name="session-state"></a>État de session

Par défaut, les valeurs et les informations d’état de session sont stockées en mémoire dans le processus ASP.NET. Vous pouvez également stocker les données de session dans une base de données SQL Server, où elles peuvent être partagées par plusieurs serveurs Web. Si la base de données que vous spécifiez pour l’état de session avec l’outil d’inscription SQL n’existe pas encore, l’utilisateur actuel doit disposer des droits nécessaires pour créer des bases de données dans SQL Server, ainsi que pour créer des éléments de schéma dans une base de données. Si la base de données existe, l’utilisateur actuel doit disposer des droits nécessaires pour créer des éléments de schéma dans la base de données existante.

Pour installer la base de données d’état de session sur SQL Server, exécutez l’outil ASPNET\_RegSql. exe et fournissez les informations suivantes avec la commande :

- Nom de l’instance de SQL Server, à l’aide de l’option **-S** .
- Les informations d’identification d’un compte qui a l’autorisation de créer une base de données sur un ordinateur exécutant SQL Server. Utilisez l’option **-E** pour utiliser l’utilisateur actuellement connecté, ou utilisez l’option **-U** pour spécifier un ID d’utilisateur avec l’option **-P** pour spécifier un mot de passe.
- Option de ligne de commande **-ssadd** pour ajouter la base de données d’état de session.

Par défaut, vous ne pouvez pas utiliser l’outil ASPNET\_RegSql. exe pour installer la base de données d’état de session sur un ordinateur exécutant SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnet_regbrowsersexe"></a>Outil d’inscription du navigateur ASP.NET-ASPNET\_RegBrowsers. exe

Dans ASP.NET version 1,1, le fichier machine. config contenait une section appelée &lt;browserCaps&gt;. Cette section contient une série d’entrées XML qui ont défini les configurations de différents navigateurs sur la base d’une expression régulière. Pour ASP.NET version 2,0, nouveau. Le fichier BROWSER définit les paramètres d’un navigateur particulier à l’aide d’entrées XML. Vous ajoutez des informations sur un nouveau navigateur en ajoutant un nouveau. Fichier BROWSER dans le dossier situé dans%SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers sur votre système.

Comme une application ne lit pas un fichier. config à chaque fois qu’elle a besoin d’informations de navigateur, vous pouvez créer un nouveau. Fichier BROWSER et exécutez ASPNET\_RegBrowsers. exe pour ajouter les modifications requises à l’assembly. Cela permet au serveur d’accéder immédiatement aux nouvelles informations du navigateur, de sorte que vous n’avez pas besoin d’arrêter l’une de vos applications pour récupérer les informations. Une application peut accéder aux fonctionnalités du navigateur via la propriété Browser de l’objet HttpRequest actuel.

Les options suivantes sont disponibles lors de l’exécution de ASPNET\_regbrowser. exe :

| **Option** | **Description** |
| --- | --- |
| **-?** | Affiche le texte d’aide\_regbbrowsers. exe de ASPNET dans la fenêtre de commande. |
| **-i** | Crée l’assembly de fonctionnalités du navigateur Runtime et l’installe dans le Global Assembly Cache. |
| **-u** | Désinstalle l’assembly des fonctionnalités du navigateur Runtime de la Global Assembly Cache. |

## <a name="the-aspnet-compilation-tool---aspnet_compilerexe"></a>Outil de compilation ASP.NET-ASPNET\_compiler. exe

L’outil de compilation ASP.NET peut être utilisé de deux manières générales : pour la compilation sur place et la compilation pour le déploiement, où un répertoire de sortie cible est spécifié.

### <a name="compiling-an-application-in-place"></a>[Compilation d’une application sur place](https://msdn.microsoft.com/library/ms229863.aspx)

L’outil de compilation ASP.NET peut compiler une application en place, autrement dit, il imite le comportement d’exécution de plusieurs demandes à l’application, provoquant ainsi la compilation normale. Les utilisateurs d’un site précompilé ne subiront pas de délai en raison de la compilation de la page lors de la première demande.

Lorsque vous précompilez un site sur place, les éléments suivants s’appliquent :

- Le site conserve ses fichiers et sa structure de répertoires.
- Vous devez avoir des compilateurs pour tous les langages de programmation utilisés par le site sur le serveur.
- Si la compilation d’un fichier échoue, la compilation du site entier échoue.

Vous pouvez également recompiler une application en place après y ajouter de nouveaux fichiers sources. L’outil compile uniquement les fichiers nouveaux ou modifiés, sauf si vous incluez l’option **-c** .

> [!NOTE]
> La compilation d’une application qui contient une application imbriquée ne compile pas l’application imbriquée. L’application imbriquée doit être compilée séparément.

### <a name="compiling-an-application-for-deployment"></a>[Compilation d’une application pour le déploiement](https://msdn.microsoft.com/library/ms229863.aspx)

Vous compilez une application pour le déploiement (compilation sur un emplacement cible) en spécifiant le paramètre targetDir. TargetDir peut être l’emplacement final de l’application Web, ou l’application compilée peut être déployée davantage. L’utilisation de l’option **-u** compile l’application de telle sorte que vous pouvez apporter des modifications à certains fichiers de l’application compilée sans la recompiler. ASPNET\_compiler. exe fait la distinction entre les types de fichiers statiques et dynamiques, et les gère différemment lors de la création de l’application résultante.

- Les types de fichiers statiques sont ceux qui n’ont pas de compilateur ou de fournisseur de générations associé, tels que les fichiers dont le nom possède des extensions telles que. CSS,. gif,. htm,. html,. jpg,. js, etc. Ces fichiers sont simplement copiés vers l’emplacement cible, avec leurs emplacements relatifs conservés dans la structure de répertoires.
- Les types de fichiers dynamiques sont ceux qui ont un compilateur ou un fournisseur de générations associé, y compris les fichiers avec des extensions de nom de fichier spécifiques à ASP.NET telles que. asax,. ascx,. ashx,. aspx,. Browser,. Master, et ainsi de suite. L’outil de compilation ASP.NET génère des assemblys à partir de ces fichiers. Si l’option **-u** est omise, l’outil crée également des fichiers avec l’extension de nom de fichier. COMPILÉ qui mappe les fichiers sources d’origine à leur assembly. Pour vous assurer que la structure de répertoires de la source de l’application est préservée, l’outil génère des fichiers d’espace réservé aux emplacements correspondants dans l’application cible.

Vous devez utiliser l’option **-u** pour indiquer que le contenu de l’application compilée peut être modifié. Sinon, les modifications suivantes sont ignorées ou provoquent des erreurs d’exécution.

Le tableau suivant décrit comment l’outil de compilation ASP.NET gère les différents types de fichiers lorsque l’option **-u** est incluse.

| **Type de fichier** | **Action du compilateur** |
| --- | --- |
| .ascx, .aspx, .master | Ces fichiers sont répartis dans le balisage et le code source, qui comprend à la fois les fichiers code-behind et tout code qui est inclus dans &lt;script runat = "Server"&gt; éléments. Le code source est compilé dans des assemblys, avec des noms dérivés d’un algorithme de hachage, et les assemblys sont placés dans le répertoire bin. Tout code inline, autrement dit, le code placé entre les **&lt;%** et **%&gt;** crochets, est inclus avec le balisage et n’est pas compilé. De nouveaux fichiers portant le même nom que les fichiers sources sont créés pour contenir le balisage et placés dans les répertoires de sortie correspondants. |
| .ashx, .asmx | Ces fichiers ne sont pas compilés et sont déplacés dans les répertoires de sortie en tant que, et ne sont pas compilés. Si vous souhaitez que le code du gestionnaire soit compilé, placez le code dans les fichiers de code source de l’application\_répertoire de code. |
| . cs,. vb,. jsl et. cpp (à l’exclusion des fichiers code-behind pour les types de fichiers répertoriés précédemment) | Ces fichiers sont compilés et inclus en tant que ressource dans les assemblys qui les référencent. Les fichiers sources ne sont pas copiés dans le répertoire de sortie. Si un fichier de code n’est pas référencé, il n’est pas compilé. |
| Types de fichiers personnalisés | Ces fichiers ne sont pas compilés. Ces fichiers sont copiés dans les répertoires de sortie correspondants. |
| Fichiers de code source dans l’application\_sous-répertoire de code | Ces fichiers sont compilés dans des assemblys et placés dans le répertoire bin. |
| fichiers. resx et. Resource dans l’application\_sous-répertoire GlobalResources | Ces fichiers sont compilés dans des assemblys et placés dans le répertoire bin. Aucune application\_sous-répertoire GlobalResources n’est créée sous le répertoire de sortie principal, et aucun fichier. resx ou. Resources situé dans le répertoire source n’est copié dans les répertoires de sortie. |
| fichiers. resx et. Resource dans l’application\_sous-répertoire LocalResources | Ces fichiers ne sont pas compilés et sont copiés dans les répertoires de sortie correspondants. |
| fichiers. Skin dans le sous-répertoire des thèmes de l’application\_ | Les fichiers. Skin et les fichiers de thème statiques ne sont pas compilés et sont copiés dans les répertoires de sortie correspondants. |
| . Browser types de fichiers statiques Web. config déjà présents dans le répertoire bin | Ces fichiers sont copiés tels quels dans les répertoires de sortie. |

Le tableau suivant décrit comment l’outil de compilation ASP.NET gère les différents types de fichiers lorsque l’option **-u** est omise.

| **Type de fichier** | **Action du compilateur** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Ces fichiers sont répartis dans le balisage et le code source, qui comprend à la fois les fichiers code-behind et tout code qui est inclus dans &lt;script runat = "Server"&gt; éléments. Le code source est compilé en assemblys, avec les noms dérivés d’un algorithme de hachage. Les assemblys résultants sont placés dans le répertoire bin. Tout code inline, autrement dit, le code placé entre les **&lt;%** et **%&gt;** crochets, est inclus avec le balisage et n’est pas compilé. Le compilateur crée des fichiers qui contiennent le balisage portant le même nom que les fichiers sources. Ces fichiers résultants sont placés dans le répertoire bin. Le compilateur crée également des fichiers portant le même nom que les fichiers sources, mais avec l’extension. COMPILÉ qui contient des informations de mappage. La. Les fichiers COMPILÉs sont placés dans les répertoires de sortie correspondant à l’emplacement d’origine des fichiers sources. |
| .ascx | Ces fichiers sont répartis dans le balisage et le code source. Le code source est compilé dans des assemblys et placé dans le répertoire bin, avec les noms dérivés d’un algorithme de hachage. Aucun fichier de balisage n’est généré. |
| . cs,. vb,. jsl et. cpp (à l’exclusion des fichiers code-behind pour les types de fichiers répertoriés précédemment) | Le code source qui est référencé par les assemblys générés à partir des fichiers. ascx,. ashx ou. aspx est compilé dans des assemblys et placé dans le répertoire bin. Aucun fichier source n’est copié. |
| Types de fichiers personnalisés | Ces fichiers sont compilés comme des fichiers dynamiques. Selon le type de fichier sur lequel ils sont basés, le compilateur peut placer des fichiers de mappage dans les répertoires de sortie. |
| Fichiers de l’application\_sous-répertoire de code | Les fichiers de code source de ce sous-répertoire sont compilés dans des assemblys et placés dans le répertoire bin. |
| Fichiers de l’application\_sous-répertoire GlobalResources | Ces fichiers sont compilés dans des assemblys et placés dans le répertoire bin. Aucune application\_sous-répertoire GlobalResources n’est créée sous le répertoire de sortie principal. Si le fichier de configuration spécifie appliesTo = « All », les fichiers. resx et. Resources sont copiés dans les répertoires de sortie. Ils ne sont pas copiés s’ils sont référencés par un [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| fichiers. resx et. Resource dans l’application\_sous-répertoire LocalResources | Ces fichiers sont compilés dans des assemblys avec des noms uniques et placés dans le répertoire bin. Aucun fichier. resx ou. Resource n’est copié dans les répertoires de sortie. |
| fichiers. Skin dans le sous-répertoire des thèmes de l’application\_ | Les thèmes sont compilés dans des assemblys et placés dans le répertoire bin. Les fichiers stub sont créés pour les fichiers. Skin et placés dans le répertoire de sortie correspondant. Les fichiers statiques (tels que. CSS) sont copiés dans les répertoires de sortie. |
| . Browser types de fichiers statiques Web. config déjà présents dans le répertoire bin | Ces fichiers sont copiés tels quels dans le répertoire de sortie. |

### <a name="fixed-assembly-names"></a>[Noms d’assemblys fixes](https://msdn.microsoft.com/library/ms229863.aspx##)

Certains scénarios, tels que le déploiement d’une application Web à l’aide de l’Windows Installer MSI, requièrent l’utilisation d’un nom de fichier et d’un contenu cohérents, ainsi que des structures de répertoire cohérentes pour identifier les assemblys ou les paramètres de configuration des mises à jour. Dans ce cas, vous pouvez utiliser l’option **-fixednames** pour spécifier que l’outil de compilation ASP.net doit compiler un assembly pour chaque fichier source au lieu d’utiliser le où plusieurs pages sont compilées dans des assemblys. Cela peut entraîner un grand nombre d’assemblys. par conséquent, si vous vous intéressez à l’évolutivité, vous devez utiliser cette option avec précaution.

### <a name="strong-name-compilation"></a>[Compilation avec nom fort](https://msdn.microsoft.com/library/ms229863.aspx##)

Les options **-APTCA**, **-delaysign**, **-keycontainer** et **-keyfile** sont fournies afin que vous puissiez utiliser ASPNET\_compiler. exe pour créer des assemblys à nom fort sans utiliser l' [outil Strong Name Tool (SN. exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) séparément. Ces options correspondent, respectivement, à **AllowPartiallyTrustedCallersAttribute**, à **AssemblyDelaySignAttribute**, à **AssemblyKeyNameAttribute**et à **AssemblyKeyFileAttribute**.

La discussion de ces attributs n’est pas abordée dans le cadre de ce cours.

## <a name="labs"></a>Laboratoires

Chacun des laboratoires suivants s’appuie sur les laboratoires précédents. Vous devrez les exécuter dans l’ordre.

## <a name="lab-1-using-the-configuration-api"></a>Atelier 1 : utilisation de l’API de configuration

1. Créez un nouveau site Web appelé *mod9lab*.
2. Ajoutez un nouveau fichier de configuration Web au site.
3. Ajoutez le code suivant au fichier Web. config :

[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Cela permet de s’assurer que vous avez l’autorisation d’enregistrer les modifications apportées au fichier Web. config.

1. Ajoutez un nouveau contrôle Label à default. aspx et remplacez l’ID par **lblDebugStatus**.
2. Ajoutez un nouveau contrôle Button à default. aspx.
3. Modifiez l’ID du contrôle Button en **btnToggleDebug** et le texte pour **basculer l’état de débogage**.
4. Ouvrez le mode code pour le fichier code-behind de default. aspx et ajoutez une instruction **using** pour **System. Web. Configuration** comme suit :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Ajoutez deux variables privées à la classe et une page\_méthode Init comme indiqué ci-dessous :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Ajoutez le code suivant à la page\_charge :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Enregistrez et parcourez default. aspx. Notez que le contrôle Label affiche l’état actuel du débogage.
2. Double-cliquez sur le contrôle Button dans le concepteur et ajoutez le code suivant à l’événement Click pour le contrôle Button :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Enregistrez et parcourez default. aspx, puis cliquez sur le bouton.
2. Ouvrez le fichier Web. config après chaque clic de bouton et observez l’attribut de **débogage** dans la section&gt; &lt;compilation.

## <a name="lab-2-logging-application-restarts"></a>Atelier 2 : redémarrage de l’application de journalisation

Dans ce laboratoire, vous allez créer du code qui vous permettra de basculer la journalisation des arrêts de l’application, des démarrages et des recompilations dans le observateur d’événements.

1. Ajoutez un DropDownList à default. aspx et remplacez l’ID par ddlLogAppEvents.
2. Affectez la valeur **true**à la propriété **AutoPostBack** du DropDownList.
3. Ajoutez trois éléments à la collection Items pour le DropDownList. Créez le **texte** du premier élément et *Sélectionnez* la valeur-1. Définissez le **texte** et la **valeur** du deuxième élément sur **true** et le **texte** et la **valeur** du troisième élément **false**.
4. Ajoutez une nouvelle étiquette à default. aspx. Remplacez l’ID par **lblLogAppEvents**.
5. Ouvrez la vue code-behind pour default. aspx et ajoutez une nouvelle déclaration pour une variable de type HealthMonitoringSection comme indiqué ci-dessous :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Ajoutez le code suivant au code existant dans la page\_init :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Double-cliquez sur DropDownList et ajoutez le code suivant à l’événement SelectedIndexChanged :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Parcourez default. aspx.
2. Définissez la liste déroulante sur **false**.
3. Effacez le journal des applications dans la observateur d’événements.
4. Cliquez sur le bouton pour modifier l’attribut de débogage de l’application.
5. Actualisez le journal des applications dans le observateur d’événements. 

    1. Des événements ont-ils été enregistrés ?
    2. Pourquoi ou pourquoi ?
6. Définissez la liste déroulante sur **true.**
7. Cliquez sur le bouton pour basculer l’attribut de débogage de l’application.
8. Actualisez la connexion de l’application à la observateur d’événements. 

    1. Des événements ont-ils été enregistrés ?
    2. Quelle était la raison de l’arrêt de l’application ?
9. Expérimentez l’activation et la désactivation de la journalisation et observez les modifications apportées au fichier Web. config.

## <a name="more-information"></a>Informations supplémentaires :

Le modèle de fournisseur de ASP.NET 2.0 vous permet de créer vos propres fournisseurs pour non seulement l’instrumentation d’application, mais aussi pour de nombreuses autres utilisations, telles que l’appartenance, les profils, etc. Pour plus d’informations sur l’écriture d’un fournisseur personnalisé pour enregistrer des événements d’application dans un fichier texte, visitez [ce lien](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
