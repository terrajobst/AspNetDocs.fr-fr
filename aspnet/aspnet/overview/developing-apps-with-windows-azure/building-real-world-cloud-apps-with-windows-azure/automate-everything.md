---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatiser tout (création d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: e741a753a36ebdaefbff8eee0b38911785c716ac
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457165"
---
# <a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatiser tout (création d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour obtenir une présentation de l’e-Book, consultez [le premier chapitre](introduction.md).

Les trois premiers modèles que nous allons examiner s’appliquent à tous les projets de développement de logiciels, mais surtout aux projets Cloud. Ce modèle concerne l’automatisation des tâches de développement. C’est un sujet important, car les processus manuels sont lents et sujets aux erreurs. l’automatisation de autant de fois que possible permet de configurer un flux de travail rapide, fiable et agile. Elle est particulièrement importante pour le développement Cloud, car vous pouvez facilement automatiser de nombreuses tâches qui sont difficiles ou impossibles à automatiser dans un environnement local. Par exemple, vous pouvez configurer l’ensemble des environnements de test, y compris le nouveau serveur Web et les machines virtuelles principales, les bases de données, le stockage d’objets BLOB (stockage de fichiers), les files d’attente, etc.

## <a name="devops-workflow"></a>Flux de travail DevOps

Vous entendez de plus en plus le terme « DevOps ». Terme développé par une reconnaissance qui vous permet d’intégrer les tâches de développement et d’exploitation afin de développer efficacement les logiciels. Le type de workflow que vous souhaitez activer est celui dans lequel vous pouvez développer une application, la déployer, tirer des enseignements de l’utilisation de la production, la modifier en réponse à ce que vous avez appris et répéter le cycle rapidement et de façon fiable.

Certaines équipes de développement Cloud réussies déploient plusieurs fois par jour dans un environnement actif. L’équipe Azure a utilisé pour déployer une mise à jour majeure tous les 2-3 mois, mais elle libère désormais des mises à jour mineures tous les 2-3 jours et toutes les versions majeures toutes les 2-3 semaines. La prise en main de cette cadence vous permet de répondre aux commentaires des clients.

Pour ce faire, vous devez activer un cycle de développement et de déploiement qui est reproductible, fiable, prévisible et dont la durée de cycle est faible.

![Flux de travail DevOps](automate-everything/_static/image1.png)

En d’autres termes, le laps de temps entre le moment où vous avez une idée pour une fonctionnalité et le moment où les clients l’utilisent et le fait de fournir des commentaires doivent être aussi courts que possible. Les trois premiers modèles, automatiser tout, le contrôle de code source et l’intégration et la remise continues, sont les meilleures pratiques que nous recommandons pour activer ce type de processus.

## <a name="azure-management-scripts"></a>Scripts de gestion Azure

Dans la [Présentation de ce livre électronique](introduction.md), vous avez vu la console Web, le portail de gestion Azure. Le portail de gestion vous permet de surveiller et de gérer toutes les ressources que vous avez déployées sur Azure. Il s’agit d’un moyen simple de créer et de supprimer des services tels que des applications Web et des machines virtuelles, de configurer ces services, de surveiller l’opération de service, etc. C’est un excellent outil, mais son utilisation est un processus manuel. Si vous envisagez de développer une application de production de toutes tailles, et en particulier dans un environnement d’équipe, nous vous recommandons de parcourir l’interface utilisateur du portail pour découvrir et Explorer Azure, puis d’automatiser les processus que vous allez effectuer de façon répétée.

Presque tout ce que vous pouvez effectuer manuellement dans le portail de gestion ou dans Visual Studio peut également être effectué en appelant l’API de gestion REST. Vous pouvez écrire des scripts à l’aide de [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), ou vous pouvez utiliser un Framework Open source comme [chef](http://www.opscode.com/chef/) ou [marionnette](http://puppetlabs.com/puppet/what-is-puppet). Vous pouvez également utiliser l’outil de ligne de commande bash dans un environnement Mac ou Linux. Azure possède des API de script pour tous les différents environnements, et possède une [API de gestion .net](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) au cas où vous souhaiteriez écrire du code au lieu d’un script.

Pour l’application Fix it, nous avons créé des scripts Windows PowerShell qui automatisent les processus de création d’un environnement de test et de déploiement du projet dans cet environnement, et nous allons examiner certains contenus de ces scripts.

## <a name="environment-creation-script"></a>Script de création d’environnement

Le premier script que nous allons examiner est appelé *New-AzureWebsiteEnv. ps1*. Il crée un environnement Azure sur lequel vous pouvez déployer l’application Fix it à des fins de test. Les principales tâches exécutées par ce script sont les suivantes :

- Créez une application web.
- Créez un compte de stockage. (Requis pour les objets BLOB et les files d’attente, comme vous le verrez dans les chapitres ultérieurs.)
- Créez un serveur SQL Database et deux bases de données : une base de données d’application et une base de données d’appartenance.
- Stockez les paramètres dans Azure que l’application utilisera pour accéder au compte de stockage et aux bases de données.
- Créer des fichiers de paramètres qui seront utilisés pour automatiser le déploiement.

### <a name="run-the-script"></a>Exécuter le script

> [!NOTE]
> Cette partie du chapitre présente des exemples de scripts et les commandes que vous entrez afin de les exécuter. Il s’agit d’une démonstration qui ne fournit pas tout ce que vous devez savoir pour exécuter les scripts. Pour obtenir des instructions pas à pas, consultez l' [annexe : l’exemple d’application Fix it](the-fix-it-sample-application.md#deploybase).

Pour exécuter un script PowerShell qui gère les services Azure, vous devez installer la console Azure PowerShell et la configurer pour qu’elle fonctionne avec votre abonnement Azure. Une fois que vous avez configuré, vous pouvez exécuter le script de création de l’environnement informatique avec une commande semblable à celle-ci :

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

Le paramètre `Name` spécifie le nom à utiliser lors de la création de la base de données et des comptes de stockage, et le paramètre `SqlDatabasePassword` spécifie le mot de passe du compte d’administrateur qui sera créé pour SQL Database. Vous pouvez utiliser d’autres paramètres que nous allons examiner ultérieurement.

![Fenêtre PowerShell](automate-everything/_static/image2.png)

Une fois le script terminé, vous pouvez voir dans le portail de gestion ce qui a été créé. Vous trouverez deux bases de données :

![Bases de données](automate-everything/_static/image3.png)

Un compte de stockage :

![Compte de stockage](automate-everything/_static/image4.png)

Et une application Web :

![Site Web](automate-everything/_static/image5.png)

Sous l’onglet **configurer** de l’application Web, vous pouvez voir que les paramètres du compte de stockage et les chaînes de connexion de la base de données SQL sont configurés pour l’application Fix it.

![appSettings et connectionStrings](automate-everything/_static/image6.png)

Le dossier *Automation* contient désormais également un fichier *&lt;WebSiteName&gt;. pubxml* . Ce fichier stocke les paramètres que MSBuild utilisera pour déployer l’application dans l’environnement Azure que vous venez de créer. Par exemple :

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Comme vous pouvez le voir, le script a créé un environnement de test complet, et l’ensemble du processus est effectué en environ 90 secondes.

Si un autre utilisateur de votre équipe souhaite créer un environnement de test, il peut simplement exécuter le script. Non seulement il s’agit d’un environnement rapide, mais il peut également être certain qu’il utilise un environnement identique à celui que vous utilisez. Vous ne pouviez pas être aussi sûr que si tout le monde était configuré manuellement à l’aide de l’interface utilisateur du portail de gestion.

### <a name="a-look-at-the-scripts"></a>Examiner les scripts

Il y a en fait trois scripts qui effectuent ce travail. Vous en appelez un à partir de la ligne de commande et il utilise automatiquement les deux autres pour effectuer certaines tâches :

- *New-AzureWebSiteEnv. ps1* est le principal script.

    - *New-AzureStorage. ps1* crée le compte de stockage.
    - *New-AzureSql. ps1* crée les bases de données.

### <a name="parameters-in-the-main-script"></a>Paramètres dans le script principal

Le script principal, *New-AzureWebSiteEnv. ps1*, définit plusieurs paramètres :

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Deux paramètres sont nécessaires :

- Nom de l’application Web créée par le script. (Cela est également utilisé pour l’URL : `<name>.azurewebsites.net`.)
- Mot de passe du nouvel utilisateur administratif du serveur de base de données créé par le script.

Les paramètres facultatifs vous permettent de spécifier l’emplacement du centre de données (par défaut, « États-Unis de l’Ouest »), le nom de l’administrateur du serveur de base de données (« dbuser » par défaut) et une règle de pare-feu pour le serveur de base de données.

### <a name="create-the-web-app"></a>Créer l’application web

La première chose à faire est de créer l’application Web en appelant l’applet de commande `New-AzureWebsite`, en lui transmettant le nom de l’application Web et les valeurs de paramètre d’emplacement :

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Créer le compte de stockage

Le script principal exécute ensuite le script *New-AzureStorage. ps1* , en spécifiant « *&lt;WebSiteName&gt;* Storage » pour le nom du compte de stockage et le même emplacement du centre de données que l’application Web.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*New-AzureStorage. ps1* appelle l’applet de commande `New-AzureStorageAccount` pour créer le compte de stockage et retourne les valeurs de nom de compte et de clé d’accès. L’application aura besoin de ces valeurs pour accéder aux objets BLOB et aux files d’attente dans le compte de stockage.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Il se peut que vous ne souhaitiez pas toujours créer un nouveau compte de stockage. vous pouvez améliorer le script en ajoutant un paramètre qui lui demande éventuellement d’utiliser un compte de stockage existant.

### <a name="create-the-databases"></a>Créer les bases de données

Le script principal exécute ensuite le script de création de base de données, *New-AzureSql. ps1*, après la configuration des noms de la base de données par défaut et des règles de pare-feu :

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Le script de création de base de données récupère l’adresse IP de l’ordinateur de développement et définit une règle de pare-feu pour que l’ordinateur de développement puisse se connecter au serveur et le gérer. Le script de création de base de données passe ensuite par plusieurs étapes pour configurer les bases de données :

- Crée le serveur à l’aide de l’applet de commande `New-AzureSqlDatabaseServer`.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Crée des règles de pare-feu pour permettre à l’ordinateur de développement de gérer le serveur et permettre à l’application Web de s’y connecter. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Crée un contexte de base de données qui comprend le nom du serveur et les informations d’identification, à l’aide de l’applet de commande `New-AzureSqlDatabaseServerContext`.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` est une fonction dans le script qui appelle l’applet de commande `ConvertTo-SecureString` pour chiffrer le mot de passe et retourne un objet `PSCredential`, le même type que celui retourné par l’applet de commande `Get-Credential`.
- Crée la base de données d’application et la base de données d’appartenance à l’aide de l’applet de commande `New-AzureSqlDatabase`.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Appelle une fonction définie localement pour créer une chaîne de connexion pour chaque base de données. L’application utilisera ces chaînes de connexion pour accéder aux bases de données. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    La fonction SQLAzureDatabaseConnectionString est une fonction définie dans le script qui crée la chaîne de connexion à partir des valeurs de paramètres qui lui sont fournies.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Retourne une table de hachage avec le nom du serveur de base de données et les chaînes de connexion.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

L’application Fix It utilise des bases de données d’appartenance et d’application distinctes. Il est également possible de placer l’appartenance et les données d’application dans une seule base de données.

### <a name="store-app-settings-and-connection-strings"></a>Stocker les paramètres de l’application et les chaînes de connexion

Azure dispose d’une fonctionnalité qui vous permet de stocker des paramètres et des chaînes de connexion qui remplacent automatiquement ce qui est renvoyé à l’application lorsqu’il tente de lire les collections `appSettings` ou `connectionStrings` dans le fichier Web. config. Il s’agit d’une alternative à l’application de [transformations Web. config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) lorsque vous déployez. Pour plus d’informations, consultez [stocker des données sensibles dans Azure](source-control.md#appsettings) plus loin dans ce livre électronique.

Le script de création d’environnement stocke dans Azure tous les `appSettings` et `connectionStrings` valeurs dont l’application a besoin pour accéder au compte de stockage et aux bases de données lorsqu’elle s’exécute dans Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) est un Framework de télémétrie que nous présentons dans le chapitre sur la [surveillance et la télémétrie](monitoring-and-telemetry.md) . Le script de création de l’environnement redémarre également l’application Web pour s’assurer qu’elle récupère les nouveaux paramètres Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Préparation du déploiement

À la fin du processus, le script de création de l’environnement appelle deux fonctions pour créer des fichiers qui seront utilisés par le script de déploiement.

L’une de ces fonctions crée un profil de publication *(&lt;websitename&gt;fichier. pubxml* ). Le code appelle l’API REST Azure pour obtenir les paramètres de publication et enregistre les informations dans un fichier *. publishsettings* . Ensuite, il utilise les informations de ce fichier, ainsi qu’un fichier de modèle (*pubxml. Template*) pour créer le fichier *. pubxml* qui contient le profil de publication. Ce processus en deux étapes simule ce que vous faites dans Visual Studio : téléchargez un fichier *. publishsettings* et importez-le pour créer un profil de publication.

L’autre fonction utilise un autre fichier de modèle (site Web-environnement. modèle) pour créer un fichier *website-Environment. xml* qui contient les paramètres que le script de déploiement utilisera avec le fichier *. pubxml* .

### <a name="troubleshooting-and-error-handling"></a>Résolution des problèmes et gestion des erreurs

Les scripts sont similaires aux programmes : ils peuvent échouer et, quand vous souhaitez en savoir plus sur la défaillance et la cause de l’échec. Pour cette raison, le script de création de l’environnement remplace la valeur de la variable `VerbosePreference` `SilentlyContinue` par `Continue` afin que tous les messages détaillés s’affichent. Elle modifie également la valeur de la variable `ErrorActionPreference` de `Continue` en `Stop`, afin que le script s’arrête même lorsqu’il rencontre des erreurs sans fin d’exécution :

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Avant qu’il ne fonctionne, le script stocke l’heure de début pour pouvoir calculer le temps écoulé lorsqu’il est terminé :

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Une fois son travail terminé, le script affiche le temps écoulé :

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

Et pour chaque opération de clé, le script écrit des messages détaillés, par exemple :

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Script de déploiement

Ce que fait le script *New-AzureWebsiteEnv. ps1* pour la création d’environnement, le script *Publish-AzureWebsite. ps1* fait pour le déploiement d’applications.

Le script de déploiement obtient le nom de l’application Web à partir du fichier *website-Environment. xml* créé par le script de création de l’environnement.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Il obtient le mot de passe de l’utilisateur du déploiement à partir du fichier *. publishsettings* :

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Il exécute la commande [MSBuild](http://msbuildbook.com/) qui génère et déploie le projet :

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

Et si vous avez spécifié le paramètre `Launch` sur la ligne de commande, il appelle l’applet de commande `Show-AzureWebsite` pour ouvrir votre navigateur par défaut sur l’URL du site Web.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Vous pouvez exécuter le script de déploiement avec une commande semblable à celle-ci :

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

Une fois l’opération terminée, le navigateur s’ouvre et affiche le site en cours d’exécution dans le Cloud au niveau de l’URL de `<websitename>.azurewebsites.net`.

![Réparer une application déployée sur Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Résumé

Avec ces scripts, vous pouvez être certain que les mêmes étapes seront toujours exécutées dans le même ordre en utilisant les mêmes options. Cela permet de s’assurer que chaque développeur de l’équipe ne manque pas de tâche ou de déployer un élément personnalisé sur son propre ordinateur qui ne fonctionnera pas de la même façon dans l’environnement d’un autre membre de l’équipe ou en production.

De la même façon, vous pouvez automatiser la plupart des fonctions de gestion Azure que vous pouvez effectuer dans le portail de gestion, à l’aide de l’API REST, de scripts Windows PowerShell, d’une API de langage .NET ou d’un utilitaire bash que vous pouvez exécuter sur Linux ou Mac.

Dans le [chapitre suivant](source-control.md) , nous allons examiner le code source et expliquer pourquoi il est important d’inclure vos scripts dans votre référentiel de code source.

## <a name="resources"></a>Ressources

- [Installez et configurez Windows PowerShell pour Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Explique comment installer les applets de commande Azure PowerShell et comment installer le certificat dont vous avez besoin sur votre ordinateur pour gérer votre compte Azure. Il s’agit d’un bon point de départ pour commencer, car il contient également des liens vers des ressources pour apprendre à utiliser PowerShell.
- [Centre de scripts Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Portail WindowsAzure.com vers des ressources pour le développement de scripts qui gèrent les services Azure, avec des liens vers des didacticiels de prise en main, une documentation de référence sur les applets de commande et du code source et des exemples de scripts
- [Scripteur du week-end : prise en main avec Azure et PowerShell](https://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). Dans un blog dédié à Windows PowerShell, cette publication fournit une introduction à l’utilisation de PowerShell pour les fonctions de gestion Azure.
- [Installez et configurez l’interface de ligne de commande multiplateforme Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Didacticiel de prise en main pour une infrastructure de script Azure qui fonctionne sur Mac et Linux, ainsi que sur les systèmes Windows.
- [Section outils en ligne de commande de la rubrique télécharger les outils et kits de développement logiciel (SDK) Azure](https://azure.microsoft.com/downloads/). Page du portail pour obtenir de la documentation et des téléchargements relatifs aux outils en ligne de commande pour Azure.
- [Tout automatiser avec les bibliothèques de gestion Azure et .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman présente l’API de gestion .NET pour Azure.
- [Utilisation des scripts Windows PowerShell pour la publication dans des environnements de développement et de test](https://msdn.microsoft.com/library/azure/dn642480.aspx). Documentation MSDN qui explique comment utiliser les scripts de publication générés automatiquement par Visual Studio pour les projets Web.
- [PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Extension Visual Studio qui ajoute la prise en charge linguistique pour Windows PowerShell dans Visual Studio.

> [!div class="step-by-step"]
> [Précédent](introduction.md)
> [Suivant](source-control.md)
