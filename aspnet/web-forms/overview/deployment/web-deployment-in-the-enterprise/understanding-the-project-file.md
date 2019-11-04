---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Fonctionnement du fichier projet | Microsoft Docs
author: jrjlee
description: Les fichiers de projet Microsoft Build Engine (MSBuild) se trouvent au cœur du processus de génération et de déploiement. Cette rubrique commence par une vue d’ensemble conceptuelle de MSBuild...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 419fe51aaf65bddcc2c50380f099f842a8d9439c
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445688"
---
# <a name="understanding-the-project-file"></a>Fonctionnement du fichier projet

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Les fichiers de projet Microsoft Build Engine (MSBuild) se trouvent au cœur du processus de génération et de déploiement. Cette rubrique commence par une vue d’ensemble conceptuelle de MSBuild et du fichier projet. Il décrit les principaux composants que vous allez rencontrer lorsque vous travaillez avec des fichiers de projet, et vous montre comment utiliser les fichiers projet pour déployer des applications réelles.
> 
> Ce que vous allez apprendre :
> 
> - Comment MSBuild utilise des fichiers projet MSBuild pour générer des projets.
> - Intégration de MSBuild aux technologies de déploiement, comme l’outil de déploiement Web de Internet Information Services (IIS) (Web Deploy).
> - Comment comprendre les composants clés d’un fichier projet.
> - Comment vous pouvez utiliser des fichiers projet pour générer et déployer des applications complexes.

## <a name="msbuild-and-the-project-file"></a>MSBuild et le fichier projet

Quand vous créez et générez des solutions dans Visual Studio, Visual Studio utilise MSBuild pour générer chaque projet dans votre solution. Chaque projet Visual Studio comprend un fichier projet MSBuild, avec une extension de fichier qui reflète le type de&#x2014;projet, par exemple C# un projet (. csproj), un projet Visual Basic.net (. vbproj) ou un projet de base de données (. dbproj). Pour générer un projet, MSBuild doit traiter le fichier projet associé au projet. Le fichier projet est un document XML qui contient toutes les informations et instructions dont MSBuild a besoin pour générer votre projet, comme le contenu à inclure, les spécifications de la plateforme, les informations de contrôle de version, les paramètres du serveur Web ou du serveur de base de données, ainsi que le tâches qui doivent être effectuées.

Les fichiers projet MSBuild sont basés sur le [schéma XML MSBuild](/visualstudio/msbuild/msbuild-project-file-schema-reference)et, par conséquent, le processus de génération est entièrement ouvert et transparent. En outre, vous n’avez pas besoin d’installer Visual Studio pour utiliser le moteur&#x2014;MSBuild. l’exécutable MSBuild. exe fait partie du .NET Framework et vous pouvez l’exécuter à partir d’une invite de commandes. En tant que développeur, vous pouvez créer vos propres fichiers projet MSBuild, à l’aide du schéma XML MSBuild, pour imposer un contrôle sophistiqué et affiné sur la façon dont vos projets sont générés et déployés. Ces fichiers projet personnalisés fonctionnent exactement de la même façon que les fichiers projet générés automatiquement par Visual Studio.

> [!NOTE]
> Vous pouvez également utiliser des fichiers projet MSBuild avec Team Build Service dans Team Foundation Server (TFS). Par exemple, vous pouvez utiliser des fichiers projet dans des scénarios d’intégration continue (CI) pour automatiser le déploiement dans un environnement de test lors de l’archivage d’un nouveau code. Pour plus d’informations, consultez [configuration de Team Foundation Server pour le déploiement Web automatisé](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).

### <a name="project-file-naming-conventions"></a>Conventions d’affectation des noms de fichiers projet

Lorsque vous créez vos propres fichiers projet, vous pouvez utiliser n’importe quelle extension de fichier de votre choix. Toutefois, pour faciliter la compréhension de vos solutions, vous devez utiliser les conventions communes suivantes :

- Utilisez l’extension. proj quand vous créez un fichier projet qui génère des projets.
- Utilisez l’extension. targets lorsque vous créez un fichier projet réutilisable à importer dans d’autres fichiers projet. En général, les fichiers avec une extension. targets ne génèrent rien, ils contiennent simplement des instructions que vous pouvez importer dans vos fichiers. proj.

### <a name="integration-with-deployment-technologies"></a>Intégration avec les technologies de déploiement

Si vous avez travaillé avec des projets d’application Web dans Visual Studio 2010, comme des applications Web ASP.NET et des applications Web ASP.NET MVC, vous savez que ces projets incluent une prise en charge intégrée de l’empaquetage et du déploiement de l’application Web dans un environnement cible. Les pages de **Propriétés** de ces projets incluent les onglets **Package/Publication Web** et **Package/Publication SQL** que vous pouvez utiliser pour configurer la façon dont les composants de votre application sont empaquetés et déployés. L’onglet **Package/Publication Web** s’affiche :

![](understanding-the-project-file/_static/image1.png)

La technologie sous-jacente qui sous-tend ces fonctionnalités est connue sous le nom de pipeline de publication Web (WPP). Le WPP apporte à MSBuild et [Web Deploy](https://go.microsoft.com/?linkid=9805122) ensemble pour fournir un processus complet de création, de package et de déploiement pour vos applications Web.

La bonne nouvelle, c’est que vous pouvez tirer parti des points d’intégration fournis par le WPP lorsque vous créez des fichiers de projet personnalisés pour des projets Web. Vous pouvez inclure des instructions de déploiement dans votre fichier projet, ce qui vous permet de générer vos projets, de créer des packages de déploiement Web et d’installer ces packages sur des serveurs distants via un seul fichier projet et un seul appel à MSBuild. Vous pouvez également appeler d’autres exécutables dans le cadre de votre processus de génération. Par exemple, vous pouvez exécuter l’outil en ligne de commande VSDBCMD. exe pour déployer une base de données à partir d’un fichier de schéma. Au cours de cette rubrique, vous découvrirez comment tirer parti de ces fonctionnalités pour répondre aux besoins de vos scénarios de déploiement d’entreprise.

> [!NOTE]
> Pour plus d’informations sur le fonctionnement du processus de déploiement d’application Web, consultez [vue d’ensemble du déploiement d’un projet d’application web ASP.net](https://msdn.microsoft.com/library/dd394698.aspx).

## <a name="the-anatomy-of-a-project-file"></a>Anatomie d’un fichier projet

Avant d’examiner le processus de génération plus en détail, il est judicieux de prendre quelques instants pour vous familiariser avec la structure de base d’un fichier projet MSBuild. Cette section fournit une vue d’ensemble des éléments les plus courants que vous pouvez rencontrer lorsque vous examinez, modifiez ou créez un fichier projet. En particulier, vous apprendrez à :

- Comment utiliser des *Propriétés* pour gérer des variables pour le processus de génération.
- Comment utiliser des *éléments* pour identifier les entrées dans le processus de génération, comme les fichiers de code.
- Comment utiliser des *cibles* et des *tâches* pour fournir des instructions d’exécution à MSBuild, à l’aide de *Propriétés* et d' *éléments* définis ailleurs dans le fichier projet.

Cela montre la relation entre les éléments clés dans un fichier projet MSBuild :

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Élément Project

L’élément de [projet](https://msdn.microsoft.com/library/bcxfsh87.aspx) est l’élément racine de chaque fichier projet. Outre l’identification du schéma XML pour le fichier projet, l’élément de **projet** peut inclure des attributs pour spécifier les points d’entrée pour le processus de génération. Par exemple, dans l' [exemple de solution du gestionnaire de contacts](the-contact-manager-solution.md), le fichier *Publish. proj* spécifie que la génération doit commencer en appelant la cible nommée **FullPublish**.

[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]

### <a name="properties-and-conditions"></a>Propriétés et conditions

Un fichier projet doit généralement fournir un grand nombre d’éléments d’informations pour créer et déployer vos projets avec succès. Ces informations peuvent inclure les noms de serveur, les chaînes de connexion, les informations d’identification, les configurations de build, les chemins d’accès aux fichiers source et de destination, ainsi que toute autre information que vous souhaitez inclure pour prendre en charge la personnalisation. Dans un fichier projet, les propriétés doivent être définies dans un élément [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) . Les propriétés MSBuild se composent de paires clé-valeur. Dans l’élément **PropertyGroup** , le nom d’élément définit la clé de propriété et le contenu de l’élément définit la valeur de la propriété. Par exemple, vous pouvez définir des propriétés nommées **ServerName** et **ConnectionString** pour stocker un nom de serveur statique et une chaîne de connexion.

[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]

Pour récupérer une valeur de propriété, utilisez le format *$ (PropertyName)* . Par exemple, pour récupérer la valeur de la propriété **ServerName** , vous devez taper :

[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]

> [!NOTE]
> Vous verrez des exemples d’utilisation des valeurs de propriété plus loin dans cette rubrique.

L’incorporation d’informations en tant que propriétés statiques dans un fichier projet n’est pas toujours l’approche idéale pour gérer le processus de génération. Dans de nombreux scénarios, vous souhaiterez obtenir des informations à partir d’autres sources ou permettre à l’utilisateur de fournir les informations à partir de l’invite de commandes. MSBuild vous permet de spécifier n’importe quelle valeur de propriété en tant que paramètre de ligne de commande. Par exemple, l’utilisateur peut fournir une valeur pour **ServerName** lorsqu’il exécute MSBuild. exe à partir de la ligne de commande.

[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]

> [!NOTE]
> Pour plus d’informations sur les arguments et les commutateurs que vous pouvez utiliser avec MSBuild. exe, consultez Référence de la [ligne de commande MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

Vous pouvez utiliser la même syntaxe de propriété pour obtenir les valeurs des variables d’environnement et des propriétés de projet intégrées. Un grand nombre de propriétés couramment utilisées sont définies pour vous, et vous pouvez les utiliser dans vos fichiers projet en incluant le nom de paramètre approprié. Par exemple, pour récupérer la&#x2014;plateforme de projet actuelle, par exemple, **x86** ou **AnyCpu**&#x2014;, vous pouvez inclure la référence de la propriété **$ (Platform)** dans votre fichier projet. Pour plus d’informations, consultez [macros pour les propriétés et les commandes de génération](https://msdn.microsoft.com/library/c02as0cs.aspx), [Propriétés de projet MSBuild communes](https://msdn.microsoft.com/library/bb629394.aspx)et [propriétés réservées](https://msdn.microsoft.com/library/ms164309.aspx).

Les propriétés sont souvent utilisées conjointement à des *conditions*. La plupart des éléments MSBuild prennent en charge l’attribut **condition** , qui vous permet de spécifier les critères sur lesquels MSBuild doit évaluer l’élément. Par exemple, considérez cette définition de propriété :

[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]

Lorsque MSBuild traite cette définition de propriété, il commence par vérifier si une valeur de propriété **$ (OutputRoot)** est disponible. Si la valeur de la propriété&#x2014;est vide en d’autres termes, l’utilisateur n’a pas fourni&#x2014;de valeur pour cette propriété. la condition prend la valeur **true** et la propriété a la valeur **. \Publish\Out**. Si l’utilisateur a fourni une valeur pour cette propriété, la condition prend la valeur **false** et la valeur de propriété statique n’est pas utilisée.

Pour plus d’informations sur les différentes façons dont vous pouvez spécifier des conditions, consultez [Conditions MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Éléments et groupes d’éléments

L’un des rôles importants du fichier projet consiste à définir les entrées dans le processus de génération. En général, ces entrées sont&#x2014;des fichiers de code fichiers, des fichiers de configuration, des fichiers de commandes et tout autre fichier que vous devez traiter ou copier dans le cadre du processus de génération. Dans le schéma de projet MSBuild, ces entrées sont représentées par des éléments [Item](https://msdn.microsoft.com/library/ms164283.aspx) . Dans un fichier projet, les éléments doivent être définis dans un élément [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) . Tout comme les éléments de **propriété** , vous pouvez nommer un élément **Item** comme vous le souhaitez. Toutefois, vous devez spécifier un attribut **include** pour identifier le fichier ou le caractère générique représenté par l’élément.

[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]

En spécifiant plusieurs éléments **Item** portant le même nom, vous créez en réalité une liste nommée de ressources. Un bon moyen de voir cela en action consiste à jeter un coup d’œil à l’un des fichiers projet créés par Visual Studio. Par exemple, le fichier *ContactManager. Mvc. csproj* dans l’exemple de solution comprend un grand nombre de groupes **d’éléments,** chacun avec plusieurs éléments de même nom.

[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]

De cette façon, le fichier projet demande à MSBuild de construire des listes de fichiers qui doivent être traités de la même façon&#x2014;que la liste de **références** comprend des assemblys qui doivent être en place pour une génération réussie, la liste de **compilation** comprend du code les fichiers qui doivent être compilés et la liste de **contenu** comprend les ressources qui doivent être copiées sans modification. Nous allons examiner comment le processus de génération référence et utilise ces éléments plus loin dans cette rubrique.

Les éléments Item peuvent également inclure des éléments enfants [ItemMetadata,](https://msdn.microsoft.com/library/ms164284.aspx) . Il s’agit de paires clé-valeur définies par l’utilisateur et qui représentent essentiellement des propriétés spécifiques à cet élément. Par exemple, un grand nombre d’éléments d’élément de **compilation** dans le fichier projet incluent des éléments enfants **DependentUpon** .

[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]

> [!NOTE]
> En plus des métadonnées d’élément créées par l’utilisateur, plusieurs métadonnées communes sont affectées à tous les éléments lors de la création. Pour plus d’informations, consultez l’article [Well-known Item Metadata (Métadonnées d’élément connues MSBuild)](https://msdn.microsoft.com/library/ms164313.aspx).

Vous pouvez créer des éléments **ItemGroup** dans l’élément de **projet** de niveau racine ou dans des éléments **cibles** spécifiques. Les éléments **ItemGroup** prennent également en charge les attributs de **condition** , ce qui vous permet d’adapter les entrées au processus de génération en fonction de conditions telles que la plateforme ou la configuration du projet.

### <a name="targets-and-tasks"></a>Cibles et tâches

Dans le schéma MSBuild, un élément [Task](https://msdn.microsoft.com/library/77f2hx1s.aspx) représente une instruction de build individuelle (ou une tâche). MSBuild comprend une multitude de tâches prédéfinies. Exemple :

- La tâche de **copie** copie les fichiers vers un nouvel emplacement.
- La tâche **CSC** appelle le compilateur visuel C# .
- La tâche **vbc** appelle le compilateur Visual Basic.
- La tâche **Exec** exécute un programme spécifié.
- La tâche **message** écrit un message dans un enregistreur d’événements.

> [!NOTE]
> Pour plus d’informations sur les tâches prêtes à l’emploi, consultez Référence des [tâches MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Pour plus d’informations sur les tâches, notamment sur la création de vos propres tâches personnalisées, consultez [tâches MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).

Les tâches doivent toujours être contenues dans les éléments [cibles](https://msdn.microsoft.com/library/t50z2hka.aspx) . Un élément **cible** est un ensemble d’une ou plusieurs tâches qui sont exécutées séquentiellement, et un fichier projet peut contenir plusieurs cibles. Lorsque vous souhaitez exécuter une tâche, ou un ensemble de tâches, vous appelez la cible qui les contient. Supposons, par exemple, que vous avez un fichier projet simple qui journalise un message.

[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]

Vous pouvez appeler la cible à partir de la ligne de commande, en utilisant le commutateur **/t** pour spécifier la cible.

[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]

Vous pouvez également ajouter un attribut **DefaultTargets** à l’élément de **projet** , pour spécifier les cibles que vous souhaitez appeler.

[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]

Dans ce cas, vous n’avez pas besoin de spécifier la cible à partir de la ligne de commande. Vous pouvez simplement spécifier le fichier projet, et MSBuild appellera la cible **FullPublish** pour vous.

[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]

Les cibles et les tâches peuvent inclure des attributs de **condition** . Par conséquent, vous pouvez choisir d’omettre des cibles entières ou des tâches individuelles si certaines conditions sont remplies.

En règle générale, lorsque vous créez des tâches et des cibles utiles, vous devez faire référence aux propriétés et aux éléments que vous avez définis ailleurs dans le fichier projet :

- Pour utiliser une valeur de propriété, tapez **$ (***PropertyName***)** , où *PropertyName* est le nom de l’élément de **propriété** ou le nom du paramètre.
- Pour utiliser un élément, tapez **@ (***ItemName***)** , où *ItemName* est le nom de l’élément **Item** .

> [!NOTE]
> N’oubliez pas que si vous créez plusieurs éléments portant le même nom, vous générez une liste. En revanche, si vous créez plusieurs propriétés portant le même nom, la dernière valeur de propriété que vous fournissez remplacera toutes les propriétés précédentes portant&#x2014;le même nom. une propriété ne peut contenir qu’une seule valeur.

Par exemple, dans le fichier *Publish. proj* de l’exemple de solution, jetez un coup d’œil à la cible **BuildProjects** .

[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]

Dans cet exemple, vous pouvez observer les points clés suivants :

- Si le paramètre **BuildingInTeamBuild** est spécifié et a la valeur **true**, aucune des tâches de cette cible ne sera exécutée.
- La cible contient une seule instance de la tâche [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) . Cette tâche vous permet de générer d’autres projets MSBuild.
- L’élément **ProjectsToBuild** est passé à la tâche. Cet élément peut représenter une liste de fichiers projet ou solution, tous définis par des éléments Item **ProjectsToBuild** dans un groupe d’éléments. Dans ce cas, l’élément **ProjectsToBuild** fait référence à un fichier solution unique.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Les valeurs de propriété passées à la tâche **MSBuild** incluent les paramètres nommés **OutputRoot** et **configuration**. Ils sont définis sur les valeurs de paramètre s’ils sont fournis, ou sur les valeurs de propriétés statiques si ce n’est pas le cas.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Vous pouvez également voir que la tâche **MSBuild** appelle une cible nommée **Build**. Il s’agit de l’une des différentes cibles intégrées qui sont largement utilisées dans les fichiers projet Visual Studio et qui sont disponibles dans vos fichiers projet personnalisés, tels que la **génération**, le **nettoyage**, la **régénération**et la **publication**. Vous en apprendrez davantage sur l’utilisation de cibles et de tâches pour contrôler le processus de génération et sur la tâche **MSBuild** en particulier, plus loin dans cette rubrique.

> [!NOTE]
> Pour plus d’informations sur les cibles, consultez [cibles MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).

## <a name="splitting-project-files-to-support-multiple-environments"></a>Fractionnement des fichiers projet pour prendre en charge plusieurs environnements

Supposons que vous souhaitiez être en mesure de déployer une solution dans plusieurs environnements, tels que des serveurs de test, des plateformes intermédiaires et des environnements de production. La configuration peut varier considérablement entre ces environnements&#x2014;, non seulement en termes de noms de serveurs, de chaînes de connexion, etc., mais également potentiellement en termes d’informations d’identification, de paramètres de sécurité et de nombreux autres facteurs. Si vous avez besoin de faire cela régulièrement, il n’est pas vraiment utile de modifier plusieurs propriétés dans votre fichier projet chaque fois que vous changez d’environnement cible. Il ne s’agit pas non plus d’une solution idéale pour exiger une liste sans fin de valeurs de propriété à fournir au processus de génération.

Heureusement, il existe une alternative. MSBuild vous permet de fractionner votre configuration de build dans plusieurs fichiers de projet. Pour voir comment cela fonctionne, dans l’exemple de solution, Notez qu’il existe deux fichiers projet personnalisés :

- *Publish. proj*, qui contient les propriétés, les éléments et les cibles qui sont communs à tous les environnements.
- *Env-dev. proj*, qui contient des propriétés spécifiques à un environnement de développement.

Maintenant, vous remarquerez que le fichier *Publish. proj* contient un élément [Import](https://msdn.microsoft.com/library/92x05xfs.aspx) , immédiatement sous la balise **Project** ouvrante.

[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]

L’élément **Import** est utilisé pour importer le contenu d’un autre fichier projet MSBuild dans le fichier projet MSBuild actuel. Dans ce cas, le paramètre **TargetEnvPropsFile** fournit le nom du fichier projet que vous souhaitez importer. Vous pouvez fournir une valeur pour ce paramètre lorsque vous exécutez MSBuild.

[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]

Cela a pour effet de fusionner le contenu des deux fichiers dans un seul fichier projet. À l’aide de cette approche, vous pouvez créer un fichier projet contenant votre configuration de build universelle et plusieurs fichiers projet supplémentaires contenant des propriétés spécifiques à l’environnement. En conséquence, l’exécution d’une commande avec une valeur de paramètre différente vous permet de déployer votre solution dans un environnement différent.

![](understanding-the-project-file/_static/image3.png)

Le fractionnement de vos fichiers de projet de cette façon est une bonne pratique à suivre. Il permet aux développeurs de déployer dans plusieurs environnements en exécutant une commande unique, tout en évitant la duplication des propriétés de génération universelle sur plusieurs fichiers de projet.

> [!NOTE]
> Pour obtenir des conseils sur la personnalisation des fichiers projet spécifiques à l’environnement pour vos propres environnements de serveur, consultez [Configuration des propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="conclusion"></a>Conclusion

Cette rubrique a fourni une introduction générale aux fichiers projet MSBuild et a expliqué comment vous pouvez créer vos propres fichiers projet personnalisés pour contrôler le processus de génération. Il a également introduit le concept de fractionnement des fichiers projet en instructions de génération universelle et en propriétés de génération spécifiques à l’environnement, pour faciliter la création et le déploiement de projets dans plusieurs destinations.

La rubrique suivante, [Présentation du processus de génération](understanding-the-build-process.md), fournit plus d’informations sur la façon dont vous pouvez utiliser les fichiers projet pour contrôler la génération et le déploiement en vous guidant tout au long du déploiement d’une solution avec un niveau de complexité réaliste.

## <a name="further-reading"></a>informations supplémentaires

Pour une présentation plus approfondie des fichiers projet et du WPP, consultez à l' [intérieur du Microsoft Build Engine : utilisation de MSBuild et Team Foundation Build](http://amzn.com/0735645248) par Sayed Ibrahim Hashimi et William Bartholomew, ISBN : 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Précédent](setting-up-the-contact-manager-solution.md)
> [Suivant](understanding-the-build-process.md)
