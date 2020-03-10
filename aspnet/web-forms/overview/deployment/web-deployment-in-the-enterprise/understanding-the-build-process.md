---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Fonctionnement du processus de génération | Microsoft Docs
author: jrjlee
description: Cette rubrique fournit une procédure pas à pas d’un processus de génération et de déploiement à l’échelle de l’entreprise. L’approche décrite dans cette rubrique utilise Microsoft Build engin personnalisé...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 802d93f7ca987d018967275bae68b8c56d883a25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641140"
---
# <a name="understanding-the-build-process"></a>Présentation du processus de génération

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique fournit une procédure pas à pas d’un processus de génération et de déploiement à l’échelle de l’entreprise. L’approche décrite dans cette rubrique utilise des fichiers projet Microsoft Build Engine (MSBuild) personnalisés pour fournir un contrôle précis sur chaque aspect du processus. Dans les fichiers projet, les cibles MSBuild personnalisées sont utilisées pour exécuter des utilitaires de déploiement tels que l’outil de déploiement Web Internet Information Services (IIS) (MSDeploy. exe) et l’utilitaire de déploiement de base de données VSDBCMD. exe.
> 
> > [!NOTE]
> > La rubrique précédente, [comprenant le fichier projet](understanding-the-project-file.md), décrit les composants clés d’un fichier projet MSBuild et a introduit le concept de fichiers projet fractionnés pour prendre en charge le déploiement dans plusieurs environnements cibles. Si vous n’êtes pas déjà familiarisé avec ces concepts, vous devez examiner [le fichier projet](understanding-the-project-file.md) avant d’utiliser cette rubrique.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](understanding-the-project-file.md), dans lequel le processus de génération est contrôlé&#x2014;par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="build-and-deployment-overview"></a>Présentation de Build and Deployment

Dans la [solution du gestionnaire de contacts](the-contact-manager-solution.md), trois fichiers contrôlent le processus de génération et de déploiement :

- *Fichier projet universel* (*Publish. proj*). Contient des instructions de génération et de déploiement qui ne changent pas entre les environnements de destination.
- *Fichier projet spécifique à l’environnement* (*env-dev. proj*). Contient des paramètres de build et de déploiement qui sont spécifiques à un environnement de destination particulier. Par exemple, vous pouvez utiliser le fichier *env-dev. proj* pour fournir les paramètres d’un environnement de développement ou de test et créer un autre fichier nommé *env-stage. proj* pour fournir les paramètres d’un environnement intermédiaire.
- Un *fichier de commandes* (*Publish-dev. cmd*). Contient une commande MSBuild. exe qui spécifie les fichiers projet que vous souhaitez exécuter. Vous pouvez créer un fichier de commandes pour chaque environnement de destination, où chaque fichier contient une commande MSBuild. exe qui spécifie un autre fichier projet spécifique à l’environnement. Cela permet au développeur de déployer dans différents environnements simplement en exécutant le fichier de commandes approprié.

Dans l’exemple de solution, vous pouvez trouver ces trois fichiers dans le dossier de la solution de publication.

![](understanding-the-build-process/_static/image1.png)

Avant de consulter ces fichiers plus en détail, jetons un coup d’œil sur le fonctionnement de l’ensemble du processus de génération lorsque vous utilisez cette approche. À un niveau élevé, le processus de génération et de déploiement se présente comme suit :

![](understanding-the-build-process/_static/image2.png)

La première chose qui se produit est que les deux fichiers&#x2014;projet qui contiennent des instructions de génération et de déploiement universelles et un qui&#x2014;contient des paramètres spécifiques à l’environnement sont fusionnés dans un seul fichier projet. MSBuild utilise ensuite les instructions du fichier projet. Il génère chacun des projets de la solution, en utilisant le fichier projet pour chaque projet. Il appelle ensuite d’autres outils, comme Web Deploy (MSDeploy. exe) et l’utilitaire VSDBCMD pour déployer le contenu et les bases de données Web dans l’environnement cible.

Du début à la fin, le processus de génération et de déploiement effectue les tâches suivantes :

1. Il supprime le contenu du répertoire de sortie, en vue d’une nouvelle Build.
2. Il génère chaque projet dans la solution :

    1. Pour les projets&#x2014;Web, dans ce cas, une application Web ASP.NET MVC et un service&#x2014;Web WCF, le processus de génération crée un package de déploiement Web pour chaque projet.
    2. Pour les projets de base de données, le processus de génération crée un manifeste de déploiement (fichier. DeployManifest) pour chaque projet.
3. Elle utilise l’utilitaire VSDBCMD. exe pour déployer chaque projet de base de données dans la solution, à l’aide de&#x2014;différentes propriétés issues des fichiers projet d’une&#x2014;chaîne de connexion cible et d’un nom de base de données, ainsi que du fichier. DeployManifest.
4. Elle utilise l’utilitaire MSDeploy. exe pour déployer chaque projet Web dans la solution, à l’aide de différentes propriétés des fichiers projet pour contrôler le processus de déploiement.

Vous pouvez utiliser l’exemple de solution pour suivre ce processus plus en détail.

> [!NOTE]
> Pour obtenir des conseils sur la personnalisation des fichiers projet spécifiques à l’environnement pour vos propres environnements de serveur, consultez [configurer les propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="invoking-the-build-and-deployment-process"></a>Appel du processus de Build and Deployment

Pour déployer la solution gestionnaire de contacts dans un environnement de test de développement, le développeur exécute le fichier de commandes *Publish-dev. cmd* . Ce code appelle MSBuild. exe, en spécifiant *Publish. proj* comme fichier projet à exécuter et *env-dev. proj* comme valeur de paramètre.

[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]

> [!NOTE]
> Le commutateur **/FL** (Short pour **/FileLogger**) enregistre la sortie de la génération dans un fichier nommé *MSBuild. log* dans le répertoire actif. Pour plus d’informations, consultez la référence de la [ligne de commande MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

À ce stade, MSBuild commence à s’exécuter, charge le fichier *Publish. proj* et commence à traiter les instructions qu’il contient. La première instruction indique à MSBuild d’importer le fichier de projet que le paramètre **TargetEnvPropsFile** spécifie.

[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]

Le paramètre **TargetEnvPropsFile** spécifie le fichier *env-dev. proj* , de sorte que MSBuild fusionne le contenu du fichier *env-dev. proj* dans le fichier *Publish. proj* .

Les éléments suivants que MSBuild rencontre dans le fichier projet fusionné sont des groupes de propriétés. Les propriétés sont traitées dans l’ordre dans lequel elles apparaissent dans le fichier. MSBuild crée une paire clé-valeur pour chaque propriété, en indiquant que toutes les conditions spécifiées sont remplies. Les propriétés définies ultérieurement dans le fichier remplacent toutes les propriétés portant le même nom et définies précédemment dans le fichier. Par exemple, considérez les propriétés **OutputRoot** .

[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]

Lorsque MSBuild traite le premier élément **OutputRoot** , le fait de fournir un paramètre nommé de la même manière n’a pas été fourni, il définit la valeur de la propriété **OutputRoot** sur **.. \Publish\Out**. Lorsqu’il rencontre le deuxième élément **OutputRoot** , si la condition est évaluée à **true**, il remplace la valeur de la propriété **OutputRoot** par la valeur du paramètre **OutDir** .

L’élément suivant que MSBuild rencontre est un groupe d’éléments unique, contenant un élément nommé **ProjectsToBuild**.

[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]

MSBuild traite cette instruction en générant une liste d’éléments nommée **ProjectsToBuild**. Dans ce cas, la liste d’éléments contient une valeur&#x2014;unique pour le chemin d’accès et le nom de fichier du fichier solution.

À ce stade, les éléments restants sont des cibles. Les cibles sont traitées différemment des propriétés et&#x2014;des éléments, mais les cibles ne sont pas traitées à moins qu’elles soient explicitement spécifiées par l’utilisateur ou appelées par une autre construction dans le fichier projet. Rappelez-vous que la balise de **projet** d’ouverture comprend un attribut **DefaultTargets** .

[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]

Cela indique à MSBuild d’appeler la cible **FullPublish** , si les cibles ne sont pas spécifiées lors de l’appel de MSBuild. exe. La cible **FullPublish** ne contient aucune tâche ; au lieu de cela, il spécifie simplement une liste de dépendances.

[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]

Cette dépendance indique à MSBuild que pour exécuter la cible **FullPublish** , il doit appeler cette liste de cibles dans l’ordre indiqué :

1. Il doit appeler la cible **Clean** .
2. Il doit appeler la cible **BuildProjects** .
3. Il doit appeler la cible **GatherPackagesForPublishing** .
4. Il doit appeler la cible **PublishDbPackages** .
5. Il doit appeler la cible **PublishWebPackages** .

### <a name="the-clean-target"></a>La cible de nettoyage

La cible **Clean** supprime fondamentalement le répertoire de sortie et tout son contenu, comme la préparation d’une nouvelle Build.

[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]

Notez que la cible comprend un élément **ItemGroup** . Lorsque vous définissez des propriétés ou des éléments dans un élément **cible** , vous créez des propriétés et des éléments *dynamiques* . En d’autres termes, les propriétés ou les éléments ne sont pas traités tant que la cible n’est pas exécutée. Le répertoire de sortie n’existe peut-être pas ou contient des fichiers tant que le processus de génération n’a pas commencé. vous ne pouvez donc pas générer la liste **\_FilesToDelete** comme élément statique ; vous devez patienter jusqu’à ce que l’exécution soit en cours. Ainsi, vous générez la liste en tant qu’élément dynamique dans la cible.

> [!NOTE]
> Dans ce cas, étant donné que la cible **Clean** est la première à être exécutée, il n’est pas vraiment nécessaire d’utiliser un groupe d’éléments dynamiques. Toutefois, il est conseillé d’utiliser des propriétés et des éléments dynamiques dans ce type de scénario, car vous pouvez souhaiter exécuter des cibles dans un ordre différent à un moment donné.  
> Vous devez également viser à éviter de déclarer des éléments qui ne seront jamais utilisés. Si vous avez des éléments qui seront utilisés uniquement par une cible spécifique, pensez à les placer à l’intérieur de la cible pour supprimer toute surcharge inutile sur le processus de génération.

Les éléments dynamiques de plus, la cible de **nettoyage** est relativement simple et utilise les tâches de **message**, de **suppression**et de **RemoveDir,** intégrées pour :

1. Envoie un message à l’enregistreur d’événements.
2. Générez une liste de fichiers à supprimer.
3. Supprimez les fichiers.
4. Supprimez le répertoire de sortie.

### <a name="the-buildprojects-target"></a>Cible BuildProjects

La cible **BuildProjects** génère fondamentalement tous les projets dans l’exemple de solution.

[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]

Cette cible a été décrite plus en détail dans la rubrique précédente, à [savoir le fichier projet](understanding-the-project-file.md), pour illustrer comment les tâches et les cibles référencent les propriétés et les éléments. À ce stade, vous êtes principalement intéressé par la tâche **MSBuild** . Vous pouvez utiliser cette tâche pour générer plusieurs projets. La tâche ne crée pas de nouvelle instance de MSBuild. exe ; elle utilise l’instance en cours d’exécution pour générer chaque projet. Les points clés intéressants dans cet exemple sont les propriétés de déploiement :

- La propriété **DeployOnBuild** indique à MSBuild d’exécuter toutes les instructions de déploiement dans les paramètres du projet lorsque la génération de chaque projet est terminée.
- La propriété **DeployTarget** identifie la cible que vous souhaitez appeler après la génération du projet. Dans ce cas, la cible du **package** génère la sortie du projet dans un package Web déployable.

> [!NOTE]
> La cible du **package** appelle le pipeline de publication Web (WPP), qui permet l’intégration entre MSBuild et Web Deploy. Si vous souhaitez jeter un coup d’œil aux cibles intégrées fournies par le WPP, examinez le fichier *Microsoft. Web. Publishing. targets* dans le dossier% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web

### <a name="the-gatherpackagesforpublishing-target"></a>Cible GatherPackagesForPublishing

Si vous étudiez la cible **GatherPackagesForPublishing** , vous remarquerez qu’elle ne contient pas réellement de tâches. Au lieu de cela, il contient un groupe d’éléments unique qui définit trois éléments dynamiques.

[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]

Ces éléments font référence aux packages de déploiement qui ont été créés lors de l’exécution de la cible **BuildProjects** . Vous ne pouvez pas définir ces éléments de manière statique dans le fichier projet, car les fichiers auxquels ils font référence n’existent pas tant que la cible **BuildProjects** n’est pas exécutée. Au lieu de cela, les éléments doivent être définis dynamiquement dans une cible qui n’est pas appelée avant l’exécution de la cible **BuildProjects** .

Les éléments ne sont pas utilisés dans cette&#x2014;cible. cette cible génère simplement les éléments et les métadonnées associées à chaque valeur d’élément. Une fois ces éléments traités, l’élément **PublishPackages** contiendra deux valeurs, le chemin d’accès au fichier *ContactManager. Mvc. deploy. cmd* et le chemin d’accès au fichier *ContactManager. service. deploy. cmd* . Web Deploy crée ces fichiers dans le cadre du package Web pour chaque projet, et il s’agit des fichiers que vous devez appeler sur le serveur de destination afin de déployer les packages. Si vous ouvrez l’un de ces fichiers, vous verrez fondamentalement une commande MSDeploy. exe avec différentes valeurs de paramètres spécifiques à la Build.

L’élément **DbPublishPackages** contient une valeur unique, le chemin d’accès au fichier *ContactManager. Database. DeployManifest* .

> [!NOTE]
> Un fichier. DeployManifest est généré quand vous générez un projet de base de données et utilise le même schéma qu’un fichier projet MSBuild. Il contient toutes les informations requises pour déployer une base de données, y compris l’emplacement du schéma de base de données (. dbschema) et les détails de tous les scripts de pré-déploiement et de prédéploiement. Pour plus d’informations, consultez [vue d’ensemble de la Build and Deployment de base de données](https://msdn.microsoft.com/library/aa833165.aspx).

Vous en apprendrez davantage sur la façon dont les packages de déploiement et les manifestes de déploiement de base de données sont créés et utilisés dans la [génération et l’empaquetage](building-and-packaging-web-application-projects.md) des projets d’application Web et du [déploiement de projets](deploying-database-projects.md)

### <a name="the-publishdbpackages-target"></a>Cible PublishDbPackages

En bref, la cible **PublishDbPackages** appelle l’utilitaire VSDBCMD pour déployer la base de données **ContactManager** dans un environnement cible. La configuration du déploiement de base de données implique un grand nombre de décisions et de nuances. vous en apprendrez davantage sur le déploiement des [projets de base de données](deploying-database-projects.md) et la [Personnalisation des déploiements de bases de données pour plusieurs environnements](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Dans cette rubrique, nous allons nous concentrer sur le fonctionnement de cette cible.

Tout d’abord, Notez que la balise d’ouverture comprend un attribut **Outputs** .

[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]

Il s’agit d’un exemple de *traitement par lots de cibles*. Dans les fichiers projet MSBuild, le traitement par lot est une technique qui consiste à itérer au sein des collections. La valeur de l’attribut **Outputs** , **« % (DbPublishPackages. Identity) »** , fait référence à la propriété des métadonnées d' **identité** de la liste d’éléments **DbPublishPackages** . Cette notation, **sorties =% * * * (ITEMLIST. ItemMetaDataName)* , est traduite comme suit :

- Fractionnez les éléments de **DbPublishPackages** en lots d’éléments qui contiennent la même valeur de métadonnées d' **identité** .
- Exécutez la cible une fois par lot.

> [!NOTE]
> L' **identité** est l’une des [valeurs de métadonnées intégrées](https://msdn.microsoft.com/library/ms164313.aspx) qui est assignée à chaque élément lors de la création. Elle fait référence à la valeur de l’attribut **include** dans l'&#x2014;élément **Item** en d’autres termes, le chemin d’accès et le nom de fichier de l’élément.

Dans ce cas, étant donné qu’il ne doit jamais y avoir plus d’un élément avec le même chemin d’accès et le même nom de fichier, nous travaillons essentiellement sur la taille des lots d’un. La cible est exécutée une fois pour chaque package de base de données.

Vous pouvez voir une notation similaire dans la propriété **\_cmd** , qui génère une commande VSDBCMD avec les commutateurs appropriés.

[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]

Dans ce cas, **% (DbPublishPackages. DatabaseConnectionString)** , **% (DbPublishPackages. TargetDatabase)** et **% (DbPublishPackages. FullPath)** font tous référence aux valeurs de métadonnées de la collection d’éléments **DbPublishPackages** . La propriété **Cmd\_** est utilisée par la tâche **Exec** , qui appelle la commande.

[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]

À la suite de cette notation, la tâche **Exec** créera des lots basés sur des combinaisons uniques des valeurs de métadonnées **DatabaseConnectionString**, **TargetDatabase**et **FullPath** , et la tâche s’exécutera une fois pour chaque lot. Il s’agit d’un exemple de *traitement par lot de tâches*. Toutefois, étant donné que le traitement par lot au niveau de la cible a déjà divisé notre collection d’éléments en lots à un seul élément, la tâche **Exec** s’exécutera une seule fois pour chaque itération de la cible. En d’autres termes, cette tâche appelle l’utilitaire VSDBCMD une fois pour chaque package de base de données de la solution.

> [!NOTE]
> Pour plus d’informations sur le traitement par lots de cibles et de tâches, consultez [traitement](https://msdn.microsoft.com/library/ms171473.aspx)par lot MSBuild, [métadonnées d’élément dans le traitement par lots de cibles](https://msdn.microsoft.com/library/ms228229.aspx)et [métadonnées d’élément dans le traitement par lot des tâches](https://msdn.microsoft.com/library/ms171474.aspx).

### <a name="the-publishwebpackages-target"></a>Cible PublishWebPackages

À ce stade, vous avez appelé la cible **BuildProjects** , qui génère un package de déploiement Web pour chaque projet de l’exemple de solution. L’accompagnement de chaque package est un fichier *Deploy. cmd* , qui contient les commandes msdeploy. exe requises pour déployer le package dans l’environnement cible, et un fichier *SetParameters. xml* qui spécifie les détails nécessaires de l’environnement cible. Vous avez également appelé la cible **GatherPackagesForPublishing** , qui génère une collection d’éléments contenant les fichiers *Deploy. cmd* qui vous intéressent. Fondamentalement, la cible **PublishWebPackages** exécute les fonctions suivantes :

- Il manipule le fichier *SetParameters. xml* de chaque package pour inclure les détails corrects de l’environnement cible, à l’aide de la tâche **XmlPoke** .
- Il appelle le fichier *Deploy. cmd* pour chaque package, à l’aide des commutateurs appropriés.

Tout comme la cible **PublishDbPackages** , la cible **PublishWebPackages** utilise le traitement par lots cible pour s’assurer que la cible est exécutée une fois pour chaque package Web.

[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]

Dans la cible, la tâche **Exec** est utilisée pour exécuter le fichier *Deploy. cmd* pour chaque package Web.

[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]

Pour plus d’informations sur la configuration du déploiement de packages Web, consultez [génération et empaquetage de projets d’application Web](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Conclusion

Cette rubrique a fourni une procédure pas à pas sur la façon dont les fichiers de projet fractionné sont utilisés pour contrôler le processus de génération et de déploiement du début à la fin de l’exemple de solution du gestionnaire de contacts. Cette approche vous permet d’exécuter des déploiements complexes à l’échelle de l’entreprise en une seule étape renouvelable, en exécutant simplement un fichier de commandes spécifique à l’environnement.

## <a name="further-reading"></a>informations supplémentaires

Pour une présentation plus approfondie des fichiers projet et du WPP, consultez à l' [intérieur du Microsoft Build Engine : utilisation de MSBuild et Team Foundation Build](http://amzn.com/0735645248) par Sayed Ibrahim Hashimi et William Bartholomew, ISBN : 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Précédent](understanding-the-project-file.md)
> [Suivant](building-and-packaging-web-application-projects.md)
